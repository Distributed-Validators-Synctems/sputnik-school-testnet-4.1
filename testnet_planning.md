# **Testnet Planning**

## **Disclaimer**

Everything is subject to change and as close as possible to the realities of testnets that take place in various projects, so at this stage you will learn many useful things in practice. Pay attention to the discord validator announcement channel for announcements. We are testing open source beta software. Expect something to break. If the documentation is not enough or you find some kind of bug, make a PR.

## **Overview**

- 3 Phases
- Governance proposals
    - Enable Transfers
- Rewards: `To keep the competitive process going, we see the validators with the most points and they will be rewarded`
- Schedule: `14.09.2023 - 05.10.2023`
- For the Genesis and Celebrate sections please sign a transaction for each of the tasks and provide the txHash in a PR.

## **Phase - 1: Start chain (max 30 points)**

- GenTx Validator Address Submission : `14.09.2023 - 17.09.2023 17:00 UTC`. Up to `100% of the flow` will be accepted.

 `25 Points = 25 000 atom for successful wallet submissions.`

- Network start time:  `18.09.2023 15:45 UTC`

`20 Points = 20 000 atom for set up post genesis validators.`

- Provide peer

`5 points =  5000 atom for everyone who provides a peer.`

---

## **Phase - 2: Governance (max 30 points)**

Lead validator team will create a governance proposal on `21.09.2023 13:00 UTC`, to update the transfer parameter. You will need to study the proposals, find the correct one and vote.

In order to find correct proposal you will need to run query command using your network client binary (`gaiad` in our case). Please issue following command `gaiad query gov proposals`, and find proposal with `title:` ”Vote for me friend”.

**New to governance module?** Read [this](https://docs.cosmos.network/main/modules/gov)

**Voting Schedule:**

- Proposal: `Vote for me friend`
- Voting Period: `21.09.2023 13:00 UTC - 24.09.2023 13:00 UTC`

**What should validators do?**

- Review the parameter change proposal and cast your vote before voting period endtime.

`25 Points = 25 000 atom for successful wallet submissions.`

**Bonus challenge**

- Make a detailed thread on Twitter about incorrect props, what number you missed, what does it mean and make a PR to the `tweets` directory of this repo.

`5 points =  5000 atom for everyone who provides a tweet url.`

---

## **Phase-3: Transactions (max 30 points)**

- Send 1atom to DVS validator

`5 points =  5000 atom for everyone who do this task.`

- Delegate some DVS to others and redelegate to DVS validator

`5 points =  5000 atom for everyone who do this task.`

- Claim reward \ commission and redelegate to DVS validator

`5 points =  5000 atom for everyone do this task.`

- Tweet url linked in a memo (Come up with something cool to say about DVS Validator School and experience in Testnet)

`5 points =  5000 atom for everyone do this task.`

<aside>
📌 Participants need to raise a PR with the details onto the `transactions` directory of this repo

- Time: `25.09.2023 15:00 UTC - 28.09.2023 15:00 UTC`
(Only the txs in this time period are considered to be valid)
</aside>

---

## **Bonus Challenges**

- Uptime :
    - uptime >= 90%
    - 80% <= uptime >= 90%
    - 70% ≤ uptime ≥ 80%

`20 points =  20000 atom for everyone who have 90+% uptime`

`10 points =  10000 atom for everyone who have 80-90% uptime`

`5 points =  5000 atom for everyone who have 70-80% uptime`

- Never jailed validator :

`20 points =  20000 atom.`

- Top - 10 teams/individuals will receive points each for their contributions for the community. technical docs, helping/resolving community issues, etc.

`max 10 points =  10000 atom.`

- Send one transaction to two addresses at once and provide txHash

`max 5 points =  5000 atom.`

## **NOTE:**

Validator School is committed to building a strong community. Therefore, the testnet is built in such a way as to cover the most common tasks and situations that you will encounter as a validator.

In addition to this, in order to involve and encourage active members of the Validator School, those participants who score the most points will receive an additional delegation in the main network from DVS.

In addition to this, you will have passed the first testnet, which will be documented on github and twitter (provided that you complete all the testnet tasks) and this will give you a starting point in working with these services and simplify the development of your portfolio.
