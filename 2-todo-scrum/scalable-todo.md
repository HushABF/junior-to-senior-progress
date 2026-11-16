# Scaling a To-Do App, Scrum Style

> Scenario: a simple to-do app just grew from 100 users to 100,000, and the site keeps slowing down.

## 1. The Problem

<!-- 2-3 sentences: what is slowing the app down? -->

the initial design of one application server and one database instance with low specs was enough to handle 100 users, when the user count grew into 100,000 , neither the application server nor db instance were able to handle this much read and write requests or possibly the db doesn't have enough storage capacity to save data for this number of users.

## 2. The Scaling Decision

<!-- Vertical, horizontal, or a mix? Justify in 3-4 sentences. -->

First we need to measure and define the bottleneck, is it the cpu, memory or storage that is maxing out. is the problem in application server or database instance or both.
If we need to scale the database, we start with horizontal scaling by moving it into a bigger machine until the next upgrade is not possible or too expensive for our needs.
If we need to scale the application server, we also start with moving it into a bigger machine to see if bigger spec solves the problem.
If either of them still max out or we need to solve the single point of failue, we introduce more servers and a load balancer for application layer, and Replicas or Shards to databse layer

### Trade-off I'm Accepting

<!-- One trade-off: cost, complexity, or a single point of failure. -->

at the start, if vertical scaling only solves the problem, the trade off for simplicity is failure handling, if the single instance fail the app is down.
after that if we go with horizontal , we need to hadle routing and balancing the request between instances etc...

## 3. The Sprint Plan (1 week)

### Sprint Goal

<!-- One sentence. -->

Scale the systen to handle 100,000 users.

### Sprint Backlog

<!-- 3-4 items. Mark the one you'd do first. -->

1. Use metrics and analyze to identify the bottleneck in the system. \*
2. Implement vertical scaling techniques \*\*
3. Implement horizontal scaling techniques \*\*\*
4. Analyze the final results to see if it worked or not

## 4. The Scrum Master's Role `

<!-- 2-3 sentences: removing blockers, running the daily standup (not assigning technical work). -->
the scrum master manages daily standup meetings and ensures that each meeting start and finish on time (e.g only 15 minutes), 
and steers the conversation to be about 3 questions: yesterday's work, today's work and if there is any blocker,
also he makes sure to solve blockers outside of code, like contacting external departments and make sure to reduce distractions.
---

## Self-check

<!-- Without looking: why is horizontal scaling harder to implement than vertical, yet has a higher ceiling? -->
vertical scaling is simpler to implement since it mainly focuses on keeping the same archeticture but give it more resources like cpu cores, ram , and storage, but this approach has a limit where for example the next upgrade is too expensive to handle.
horizontal scaling is about balancing the load on components by adding more web servers or add replicas or shard the database, there is no limit on this approach since you can add or remove servers as needed, but its adds more complexity since you need to manage several servers and make sure all componenets are in sync and handle failure correctly.

## Notes:

1- Viertical scaling is simpler but limited by hardware capacity and may require downtime during upgrades.
2- metrics is the key for deciding, Maybe the CPU is maxed out. Maybe memory is full. Maybe disk, network, database locks, GPU memory, or a shared service is the real limit. Once you know what is limiting the system, there are two basic ways to add capacity.
3- For an application tier, this usually means running more instances behind a load balancer. For workers, it means adding more consumers to a queue.
For storage, it may mean replicas, partitions, or shards.
4- If the database primary can handle 20,000 writes per second, adding 50 more web servers does not raise that limit. It may only make the database fail faster.
5- Stateless API servers and background workers usually scale out cleanly,
6- if scaling out, differ between read-heavy and write-heavy instances (masters and slaves) in case of replicas
