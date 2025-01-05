# Common Concurrency

In order to think about concurrent applications, here are some things to keep top of mind:

- CAP Theorem
- Idempotency
- BigO
- Heaps and HashMaps
- Read in constant time
- Common messaging patterns

## CAP Theorem

It is impossible to simultaneously achieve all three of the following guarantees:

- Consistency: Every read receives the most recent write or an error.
- Availability: Every request receives a (non-error) response, without the guarantee that it contains the most recent write.
- Partition Tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.

Common solutions, and their CAP Theorem implications.

## Idempotency

Performing the same operation more than once does not run it more than once.

In a large distributed system, this can mean choosing some structure to guarantee idempotency on writes:

- Unique Identifiers: use a UUID or GUID, store in a hash map or distributed hash table.
- Idempotency Token: hash the record, store in a heap (O(log(n))) or hash map (O(1))
- Use a write-ahead log: write to the log, if the record doesn't exist, write it. Insertion: O(1). Lookup: O(1).
- CRDT: if the data is associative and commutative...
- Per-record Version History: Put a version in a hash map, if a version is newer, write. Lookup and update: O(1).
- Distributed Locks: Use Redis, Zookeeper, or etcd to enforce a single write per record. Acquire and release: O(1). Has a potential for deadlock.

A DHT requires stable node membership, so piggybacking a tool like this on the web nodes would not work.

