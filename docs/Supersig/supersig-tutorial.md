---
sidebar_position: 3
---

# Supersig Tutorial

:::info Walk Through Blog Post

Follow a blog post guide about the quick set up of a substrate node template containing supersig, and polkadot-js apps fork [here](https://decentration.medium.com/setup-for-running-and-testing-supersig-m3-bc1ddfc25f43).

:::

In this tutorial you require a node-template with supersig pallet (and rpc functions) added.

Repo: [substrate-supersig-template](https://github.com/decentration/substrate-supersig-template.git)


## Video Tutorial

[![How to interact with a supersig 01 - Watch Video](/img/screenshots/video-image-supersig.png)](https://www.loom.com/share/dbcaa6319b1a4644aacb709aa0e38783)




## Create a supersig account

Go to `Governance > Supersig > Create/Approve > supersig > createSupersig(members)`

_Alternatively Go to `Developer > Extrinsics > supersig > createSupersig(members)`_

![createSupersig](/img/screenshots/createSupersig.png)
_Notice how if you are the creator of the supersig, you must also add yourself as a member._ 

## Name the Supersig Address (and fund it)

![SupersigCreated](/img/screenshots/SupersigCreated.png)

- Give the supersig a name.
- Fund the supersig account from any account that has funds. 

:::info Register your Supersig's identity with registrar

If the blockchain you are creating your supersig has the identity pallet (like Kabocha) then you can make a proposal to register your supersig to have an identity so it becomes a known entity. 

:::


## Make a call from your Supersig

Now that your supersig is funded and has members, you can create a proposal. Thereafter the members of the supersig can vote for it.  

![submitCall](/img/screenshots/submitCall.png)

- You can click the _Propose_ button from the dashboard. 

- Or you can go to `Governance > Supersig > Create/Approve > submitCall(supersigAccount, call)`

:::note The Proposer is not automatically a Voter

If you created a proposal you will also need to vote on it. Votes are not automatically counted by the proposer. 

:::
## Members vote/sign transactions

Go to `Governance > Supersig > Dashboard > Vote`

![ApproveCall](/img/screenshots/ApproveCall.png)
_A simpleMajority of members sign a call for the supersig account._ 

- Notice that Alice created the call, but she also has to approve the call. 
- The callId here is `0` which is a call nonce. This is the first ever call from this supersig so we know it is zero. 
- You can also view the call nonce from the event log or from  `> chain_state`. 
- Remeber to approve a call, you need to be a member with a sufficiently funded balance. 

![CallVoted](/img/screenshots/CallVoted.png)
_Alice has voted on Call with nonce of `0`. Now we just need one of the 2 other members to make a simpleMajority._


![CallExecution](/img/screenshots/CallExecution.png)
_Bob voted and then the simpleMajority threshold was reached and the Call was executed. Ferdie now receives his balance of 500._


## Add/remove members

Go to `Developer > Extrinsics > supersig > addMembers(newMembers)`

**A common mistake**

Here is a common way to submit a call. In this example we want to add a member, but just because the call is a supersig call it doesn't mean we can skip starting with `submitCall`
![AddMemberIncorrect](/img/screenshots/AddMemberIncorrect.png)
_In this example we add Dave as a member, which also requires simpleMajority vote. But wait, it did not work because we need to submitCall then addMember within the call._

**The correct way**
![AddMember](/img/screenshots/AddMember.png)
_Now we have wrapped the addMember call correctly within submitCall, and have selected the Supersig we want to interact with._

- Add a member (Dave)
- In this example we add Dave as `master`
  - This means he will have 50% voting power. And no matter how many members there are, if he votes then only one other person is required to create a `simpleMajority`.

![AddMember](/img/screenshots/AddMemberCallSubmitted.png)
_This is the second ever call for the Supersig. So the `callId` is now `1`._

Supersig members need to vote in order to accept Dave as a member. 

![AddMember](/img/screenshots/ApproveCallNewId.png)
_Dont't forget to add the correct callId when voting for the call._

![CallExecutionDave](/img/screenshots/CallExecutionDave.png)
_...Alice and Bob vote and Dave is now a member of the Supersig._ 


## Get Information about your Supersig

**Find your AccountNonce from your AccountId**

Go to `Developer > Runtime Calls > accountNonceApi > accountNonce(accountId)`

![AccountNonce](/img/screenshots/AccountNonce.png)
_select your AccountId to get your AccountNonce_

Your AccountNonce is the number of your Supersig that is used to represent your supersig. Here we find the nonce is `0`.

In our exmaple, there are now 4 members in the Supersig, 3 `Standard` members, and 1 `Master` member. But if we lose track of this let's check from the chain state. 

Go to `Developer > chain state > supersig > members(u128, AccountId32): PalletSupersigRole`

![ChainStateMembers](/img/screenshots/ChainStateMembers.png)

- Select the id of the supersig. In this case we know theres only one, so it's `0`
- For the second parameter `Option<AccountId32>` untick the box so that we can get a list of all the `Members` of the selected supersig.
- As we can see from the screenshot there are 4 accounts, each with their Member type (standard or master). 

---
