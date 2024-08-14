# **Testnet Planning**

## **Disclaimer**

Everything is subject to change and as close as possible to the realities of testnets that take place in various projects, so at this stage you will learn many useful things in practice. Pay attention to the telegram group for announcements. We are testing open source beta software. Expect something to break. If the documentation is not enough or you find some kind of bug, make a PR.

## **Overview**

- 3 Phases
- Governance proposals
    - Enable Transfers
- Rewards: `To keep the competitive process going, we see the validators with the most points and they will be rewarded.`
- Schedule: `14.08.2024 - xx.08.2024`

## **Phase - 1: Start chain (max 30 points)**

- GenTx Validator Address Submission : `14.08.2024 - 15.08.2024 10:00 UTC`. Up to `100% of the flow` will be accepted.

`25 Points = 25 000 sputnik for successful wallet submissions.`

- Network start time:  `15.08.2024 17:30 UTC`

`20 Points = 20 000 sputnik for set up post genesis validators.`

- Provide peer

`5 points =  5000 sputnik for everyone who provides a peer. You can provide a peer by sending a PR to the `peers` directory of this repo.`

---

## **Phase - 2: Governance (max 30 points)**

Lead validator team will create a governance proposal on `15.08.2024 17:30 UTC`, to update the transfer parameter. You will need to study the proposals, find the correct one and vote.

In order to find correct proposal you will need to run query command using your network client binary (`sputnikd` in our case). Please issue following command `sputnikd query gov proposals`, and find proposal with `title:` "Enable transfers".

**New to governance module?** Read [this](https://docs.cosmos.network/v0.50/build/modules/gov)

**Voting Schedule:**

- Proposal: `Enable transfers`
- Voting Period: `15.08.2024 17:30 UTC - 16.08.2024 17:30 UTC`

**What should validators do?**

- Review the parameter change proposal and cast your vote before voting period end time.

`10 Points = 10 000 sputnik for successful vote tx.`

---

## **Phase-3: Transactions (max 30 points)**

- Send 1sputnik to DVS validator

`5 points =  5000 sputnik for everyone who do this task.`

- Delegate some sputnik to DVS validator

`5 points =  5000 sputnik for everyone who do this task.`

- Claim rewards and commission

`5 points =  5000 sputnik for everyone do this task.`

- Tweet url linked in a memo (Come up with something cool to say about DVS Validator School and experience in Testnet)

`5 points =  5000 sputnik for everyone do this task.`

<aside>
📌 Participants need to raise a PR with the details into the `transactions` directory of this repo

- Time: `xx.08.2024 17:00 UTC - xx.08.2024 17:00 UTC`
(Only the txs in this time period are considered to be valid)
</aside>

---

## **Bonus Challenges**

- Uptime :
    - uptime >= 90%
    - 80% <= uptime >= 90%
    - 70% ≤ uptime ≥ 80%

`20 points =  20000 sputnik for everyone who have 90+% uptime`

`10 points =  10000 sputnik for everyone who have 80-90% uptime`

`5 points =  5000 sputnik for everyone who have 70-80% uptime`

- Never jailed validator :

`20 points =  20000 sputnik.`

- Top - 10 teams/individuals will receive points each for their contributions for the community. technical docs, helping/resolving community issues, etc.

`max 10 points =  10000 sputnik.`

- Send one transaction to two addresses at once and provide txHash

`max 5 points =  5000 sputnik.`

## **NOTE:**

Validator School is committed to building a strong community. Therefore, the testnet is built in such a way as to cover the most common tasks and situations that you will encounter as a validator.

In addition to this, in order to involve and encourage active members of the Validator School, those participants who score the most points will receive an additional delegation in the main network from DVS.

In addition to this, you will have passed the first testnet, which will be documented on github and twitter (provided that you complete all the testnet tasks) and this will give you a starting point in working with these services and simplify the development of your portfolio.
