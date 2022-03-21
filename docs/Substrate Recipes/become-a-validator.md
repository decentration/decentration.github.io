---
sidebar_position: 2
---

# Become a Validator on Pop-Art

![alt text](https://github.com/Kabocha-Network/polkadot/blob/master/images/pop-art.jpg?raw=true?raw=true)

**Repo**: https://github.com/kabocha-network/relay-chain/edit/v0.9.16-n1/  

**Branch**: v0.9.16-n1

This branch contains Pop-Art, a custom Rococo relay staging network. It is intended for projects in the Substrate ecosytem (and Edgeware/Kabocha community), so that people can test their parachain integrations, and get experiecne as a validator in a shared network. 


## Launch a Validator (summary)

To launch a validator you will need to:


- Make an account on Pop-Art relay, get some POP tokens and get that account registered as a validator by an admin.
- Submit BABE and GRANDPA keys to your node keystore.
- Rotate keys then submit keys via an extrinsic.

Then you should start to be included to participate in validation on Pop-Art.

Below is a more detailed guide:
## Launch a node

First you need to compile and launch your node with the --validator flag and the correct chain spec in order to make sure it is peering with the correct network, then you will be able to convert this node into a working validator through a few steps shared below: 

### Boot to the correct network
Make sure nodes are peering, and do that through running the correct chain spec and booting through an node in that network. 

Example of a command:

```bash
./target/release/polkadot \
-- validator \
-- base-path /tmp/relay/MyVal1 \ specify your db path
-- chain ./specs/pop-art-3-val.json \
-- port 30333 \
-- ws-port 9944 \
-- rpc-port 9933 \
-- rpc-methods=Unsafe \
-- name <INSERT CUSTOM NAME> \
-- telemetry-url 'wss://telemetry.polkadot.io/submit/ 0' \
-- node-key <INSERT-KEY> optional
```

_In this instance, our chain spec contains bootnodes, but if you come across a chain spec without any bootnodes, ask someone who is running a node to provide you with a bootnode address and then add the `— bootnodes` tag to your command._


## Register new validators

### Get some POP tokens
Ask in the [Kabocha Technical Chat](https://matrix.to/#/#kabocha.technical:matrix.org) for some POP so that you can add "existential deposit" to your (stash) AccountIds of their validators.

### Ask Sudo to register you
 Ask Sudo to register your AccountIds as Validators
Ask the sudo to register your validators as via the `sudo > validatorManager > registerValidators`




## Rotate Keys
Now they are registered you (and your partner) can “rotate keys”, so that new keys are generated and populated in all the session key fields for your validators.

Submitting calls via RPC can be long winded, so a neat trick is to submit the BABE and GRANDPA so the chain produces and finalizes blocks, then you can run author_rotateKeys for each of your validators, which will then generate all your other keys automatically.

```
curl -H ‘Content-Type: application/json’ --data ‘{ “jsonrpc”:”2.0", “method”:”author_rotateKeys”, “id”:1 }’ http://127.0.0.1:9933 
```

Make the RPC call in the terminal of your where your validator’s node is located, which should look like this:

![3 author_rotateKeys calls for my 3 validators. If you have one validator you only need to make the call once.](https://miro.medium.com/max/1400/1*9TxE-iVRz7qxgi3xD_VgWw.png)

Once you have generated the returned hex result you need to submit them as an extrinsic for all the validators you’ve done that for.

`session > setKeys(keys, proof)`

![The UI for the setKeys extrinsic call](https://miro.medium.com/max/1400/0*pWs9X_HF_OcuLWJt.png)

- Be conscious of the account you are using to set the keys.
- In “proof” just add 0x00 (not guaranteed to be secure).
- Submit transaction

_Wait for an epoch to see the changes, and other validators._

A guide for people who forked this relay and need a workflow to add validators.


## Fork this relay chain and launch youre own network 

//ToDo
### Submit keys

- Create BABE and GRANDPA keys.

This guide assumes you have the sudo account, you've launched your validators, have submitted your babe and grandpa keys and are producing and finalizing blocks.

### Make sure validators are working
Make sure your nodes are producing blocks and finalizing, if they are not, restart nodes, and add keys again, (or use the author_hasKey RPC method to check they have the correct keys).



### Create your first Post

Create a file at `blog/2021-02-28-greetings.md`:

```md title="blog/2021-02-28-greetings.md"
---
slug: greetings
title: Greetings!
authors:
  - name: Joel Marcey
    title: Co-creator of Docusaurus 1
    url: https://github.com/JoelMarcey
    image_url: https://github.com/JoelMarcey.png
  - name: Sébastien Lorber
    title: Docusaurus maintainer
    url: https://sebastienlorber.com
    image_url: https://github.com/slorber.png
tags: [greetings]
---

Congratulations, you have made your first post!

Feel free to play around and edit this post as much you like.
```

A new blog post is now available at `http://localhost:3000/blog/greetings`.
