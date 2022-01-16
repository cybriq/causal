# causal

Causal is a protocol for distributed gathering and ordering transactions
received by a peer to peer network, without a consensus, but a guarantee of the
total transaction ordering as a consensus from every node.

The protocol borrows from the songs of insects, who are constantly gossiping,
and giving data about their current state.

Nodes are therefore randomly reporting to each other the latest seen, or when
they see new, they send it to everyone who didn't already tell them they have
it.

## Attrition Sort

In order to define a 'first' row, using a distributed version of a bubble 
sort, nodes gossip their hashes to each other, and nodes that have been 
beaten wait for the final node.

In N/2 rounds, you can find the 'first' item in the list. Each round is one 
duplex status report, counting as one message cycle.

In the other half of the rounds (odd numbers only cost one extra round), 
each node keeps looking forward a competitor to step in front of.

Thus, the algorithm has a linear growth in time to execute with the number 
of nodes, plus one message cycle for odd numbers of nodes. 

## Completion

Because the odds are that only half of the subjective event orderings are 
required find a stable result, the algorithm can be switched to a pipeline 
pattern, and in theory, the 5 zipped items with no change of ordering 
criteria may sometimes go over or under. Likely it's got half a chance of 
going over or under. So out of two samples, it will average to half the 
steps of the number of nodes.

If we can say that half a message cycle eliminates one node, then average 
200ms one way transit, 1000 nodes will cost 20 seconds propagation and 10 
seconds for the winner to be found.

Thus, the ideal, maximum node count for a Causal Ordering Cluster would be 
around 200-300, and it would be simple to cap it this way with a issue role 
token, and until this level.

