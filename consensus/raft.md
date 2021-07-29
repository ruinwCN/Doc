# Note. Raft

## Understandable Distributed Consensus



### problem of *distributed consensus*.

## *Raft* is a protocol for implementing distributed consensus.



the *Follower* state,

the *Candidate* state,

the *Leader* state.



## *Leader Election*.

1.All our nodes start in the follower state.

2.If followers don't hear from a leader then they can become a candidate.

3.The candidate then requests votes from other nodes.

4.Nodes will reply with their vote.

5.The candidate becomes the leader if it gets votes from a majority of nodes.

6.This process is called *Leader Election*.



## *Log Replication*.

All changes to the system now go through the leader.

Each change is added as an entry in the node's log.

This log entry is currently uncommitted so it won't update the node's value.

To commit the entry the node first replicates it to the follower nodes...

then the leader waits until a majority of nodes have written the entry.

The entry is now committed on the leader node and the node state is "5".

The leader then notifies the followers that the entry is committed.

The cluster has now come to consensus about the system state.

This process is called *Log Replication*.



## Leader Election

In Raft there are two timeout settings which control elections.

First is the **election timeout.**

The election timeout is the amount of time a follower waits until becoming a candidate.

The election timeout is randomized to be between 150ms and 300ms.

After the election timeout the follower becomes a candidate and starts a new *election term*...
...votes for itself...
...and sends out *Request Vote* messages to other nodes.

If the receiving node hasn't voted yet in this term then it votes for the candidate...
...and the node resets its election timeout.

Once a candidate has a majority of votes it becomes leader.

The leader begins sending out *Append Entries* messages to its followers.

These messages are sent in intervals specified by the **heartbeat timeout.**

These messages are sent in intervals specified by the heartbeat timeout.



## Log Replication

Once we have a leader elected we need to replicate all changes to our system to all nodes.

This is done by using the same *Append Entries* message that was used for heartbeats.



> noteWith: http://thesecretlivesofdata.com/raft/
