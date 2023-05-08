---
sidebar_position: 2
---

# Pallet Overview

## The pallet

The Supersig pallet provide function for:

- Creating a supersig,
- Adding and removing members,
- Leaving the supersig,
- Submit transaction to a supersig,
- Vote for the transaction,
- Remove a pending transaction,
- Delete a supersig,


## Dispatchable Functions

- `create_supersig` - create a supersig, with specified members. The creator will have to
  deposit an existencial balance and a deposit that depend on the number of members, in the
  supersig account. This last amount will be reserved on the supersig

  /!!\ note of caution /!!\ the creator of the supersig will NOT be added by default, he will
  have to pass his adress into the list of added users.

- `submit_call` - make a proposal on the specified supersig. an amount corresponding to the
  length of the encoded call will be reserved.

- `approve_call` - give a positive vote to a call. if the number of vote >= SimpleMajority, the
  call is executed. An user can only approve a call once.

- `remove_call` - remove a call from the poll. The reserved amount of the proposer will be
  unreserved

- `add_members` - add new members to the supersig. In case some user are already in the
  supersig, they will be ignored.

- `remove_members` - remove members from the supersig. In case some user are not in the
  supersig, they will be ignored.

- `remove_supersig` - remove the supersig and all the associated data. Funds will be unreserved
  and transfered to specified beneficiary.

- `leave_supersig` - remove the caller from the supersig.

## Get a node started and use the functions

- `cargo build --release`

- `./target/release/node-template --dev`

- Then go to view your node from Polkadot JS Apps development > local node https://polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9944#/addresses


---

