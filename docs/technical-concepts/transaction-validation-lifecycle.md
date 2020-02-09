# Transaction Validation Lifecycle

Transactions are at the heart of the CKB’s operation. When you interact with the CKB, you are submitting state transitions through transactions. This document will explain the lifecycle of CKB transaction validation.

![Image](未命名文件%20(1).jpg "Transaction Validation")

## RPC

First, a sender constructs a transaction, then submits it through RPC. The transaction will be validated by the `outputs_validator`  (introduced in v0.27.0) it has been submitted to. 

The default validation logic involves checking various things:

```
transaction.outputs.all{ |output|
    let script = output.script
    (script.code_hash == secp256k1_blake160_sighash_all && script.hash_type == "type" && script.args.size == 20) ||
    (script.code_hash == secp256k1_blake160_multisig_all && script.hash_type == "type" && （script.args.size == 20 || (script.args.size == 28 && script.args[20..28].is_valid_since_format))
}
transaction.outputs.all{ |output|
    let script = output.type
    script.is_null || script.code_hash == dao && script.hash_type == "type"
    || (script.has_lock_period() && since.is_absolute())
}
```

This validation is intended to prevent improperly formed transactions, such as mentioned in *https://github.com/nervosnetwork/ckb/wiki/Common-Gotchas#nervos-dao* 

Although the node can be* *configured to `passthrough` to skip this validation.

Once the transaction has been submitted to your local node, the node also outputs the transaction id, which can then be used to track transaction status.

## Verification

Before a transaction is broadcasted and enters the mempool, the transaction will be verified and executed locally.

### STEP 1 — Resolve

Essentially, transaction inputs are just pointers, as shown here:

```
struct OutPoint {
    tx_hash:        Byte32,
    index:          Uint32,
}
```

We gather the referenced data through the pointer prior to transaction execution, this process is called “resolve transaction”. We will also need to check that all inputs of this transaction are valid (no* *duplicate or double spends).

### STEP 2 — Verification

Verification involves checking a number of things:       

1. version (currently must be 0) 
2. serialized_size is less than limit 
    1. pub fn serialized_size(&self) -> usize {
            // the offset in TransactionVec header is u32
            self.as_slice().len() + molecule::NUMBER_SIZE
            // molecule::NUMBER_SIZE = `size_of::<u32>() 4`
        }
3. inputs are not empty
    1. inputs().is_empty() || outputs().is_empty()
4. inputs are mature
    1. For each input and dep, if the referenced output transaction is a cellbase, it must have at least 4 epoch confirmations
5. capacity
    1. sum of inputs’ capacity must less than or equal to outputs’ capacity
6. duplicate_deps
    1. deps should not be duplicated
7. outputs_data_verifier
    1. number of ‘output data' fields must equal number of outputs
8. since     
    1. ‘since’ value must follow the rules described in https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0017-tx-valid-since/0017-tx-valid-since.md

CKB VM will then execute the transaction script and output the number of cycles consumed.

## Broadcast to the network

If the verification is successful, the current node broadcasts the transaction (with cycles value) to all of its peer nodes (nodes it is connected to). 

If verification fails, the transaction is not broadcasted any further. The transaction flows through various “full nodes”, which repeat the verification process described in the previous step, and check that the cycles value matches the actual cycles consumed when they verified the transaction.

## Tx-pool (mempool)

![Image](未命名文件%20(1).jpg "Transaction Propagation")

CKB uses a two-step process for transaction confirmation. Transactions will be divided into different status (pending and proposed) in the tx-pool. The status of transactions will change as a block is added to the chain. When the latest block changes, all transactions in the tx-pool will be re-scanned to ensure they are still valid.

`BlockAssembler` will fetch proposals and transactions from the pending pool and proposed pool for block template.

More information about this two-step transaction confirmation process can be found here:
https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0020-ckb-consensus-protocol/0020-ckb-consensus-protocol.md#two-step-transaction-confirmation
