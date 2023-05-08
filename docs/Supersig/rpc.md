---
sidebar_position: 4
---

# RPC

## RPC functions

- `superSig_getUserSupersigs` 
  - Find what supersigs your associated to.
  - Parameter(s):`who: AccountId` the AccountId you'd like to check
- `superSig_listMembers`
  - Get list of members related to supersig. 
  - Parameter(s): `SupersigId` (nonce)
- `superSig_listProposals`
  - Get list of proposals (calls) connected to a supersig. 
  - Parameter(s): `SupersigId` (nonce)
- `superSig_getProposalState`
  - Get the state of votes after youve submitted a call for voting. 
  - Parameter(s): `SupersigId` (nonce)


## cURL

Use cURL to make rpc calls:

### Example

**List Members**

`superSig_listMembers`

From our example we make a jsonrpc call through cURL, (assuming that your chain is running on port 9933).

```bash
curl -sS -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "superSig_listMembers", "params": ["INSERT ACCOUNTID OF SUPERSIG"]}' http://localhost:9933/
```

Result:

```json
{
  "jsonrpc":"2.0","result":
  [
    ["5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY","Standard"], //Alice
    ["5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty","Standard"], //Bob
    ["5FLSigC9HGRKVhB9FiEo4Y3koPsNmBmLJbpXg2mp1hXcS59Y","Standard"]  //Charlie
  ],
  "id":1
} //Alice, Bob and Charlie's accounts related to Supersig[0]
```
## Use RPC functions for your own chain

- Fork a node template containing `pallet_supersig` and RPC functions [here](https://github.com/decentration/substrate-supersig-template)
- View the RPC section of the pallet [here](https://github.com/kabocha-network/pallet_supersig/tree/polkadot-v0.9.28)
- See how the RPC functions are added to the runtime [here](https://github.com/decentration/substrate-supersig-template/blob/6fbce881471ef6b5730bb8bf4b68f2ee20f58025/runtime/src/lib.rs#L518)
