---
name: book-designing-data-intensive-applications
description: >-
  Rules for scalable, reliable, maintainable data systems distilled from Martin
  Kleppmann's book "Designing Data-Intensive Applications": storage, data
  modeling, partitioning, replication, consistency, transactions, streaming,
  and fault tolerance. Use only when the user explicitly asks for "Designing
  Data-Intensive Applications", "DDIA", or Martin Kleppmann's guidance.
---

# Designing Data-Intensive Applications — Martin Kleppmann

Rules distilled from *Designing Data-Intensive Applications* by Martin Kleppmann. Apply them when designing systems whose primary challenge is data: its volume, complexity, or rate of change.

## Architecture

- **Shared-Nothing**: Prefer shared-nothing architectures over shared-disk or shared-memory. Independent nodes connected via a network manage their own CPU, memory, and disks to ensure horizontal scalability and fault isolation.
- **Separate Compute from State**: Decouple stateless application logic from durable state management. Treat the database as a specialized service, not a mutable shared variable, to enable independent scaling and rolling upgrades.
- **Specialized Components**: Compose complex systems from purpose-built tools (OLTP databases for writes, search indexes for keyword search, caches for performance) rather than forcing a single tool to serve every access pattern.
- **Single System of Record**: Funnel all system inputs through one primary system of record that determines a total ordering for writes before deriving secondary representations.
- **Data Outlives Code**: Choose encoding schemes (Avro, Protobuf) that support both forward and backward compatibility so old and new code can coexist during rolling deployments.

## Data Modeling

- **Document Model**: Use document models for data that is mostly self-contained (a tree) and typically loaded as a single unit.
- **Relational Model**: Use relational models when data is highly interconnected and requires frequent joins or many-to-many relationships.
- **Graph Model**: Use graph models when relationships are complex and query paths are variable-length or recursive (e.g., social networks, transitive closure).
- **Schema-on-Read vs Schema-on-Write**: Prefer schema-on-read for heterogeneous data from external systems you do not control; prefer schema-on-write to enforce structure and document shared datasets.
- **Derived Denormalization**: Denormalize into read-optimized views (caches, indexes, timelines) only when they are derived from a source of truth through a repeatable, automated process.

## Consistency & Transactions

- **Read-After-Write**: Guarantee users see their own updates by routing their reads to the leader or tracking their last write timestamp.
- **Monotonic Reads**: Ensure a given user always queries the same replica, or one at least as up-to-date as their previous read, to avoid moving backward in time.
- **Prevent Write Skew**: Use serializable isolation or explicit row-level locking (`SELECT FOR UPDATE`) when a write depends on a precondition checked earlier in the same transaction.
- **Avoid Weak Isolation for Coupled Writes**: Do not rely on weak isolation levels (e.g., Read Committed) for multi-object updates that must stay in sync, such as denormalized counters.
- **Linearizability Where Required**: Assume linearizability is required for distributed locks, leader election, and uniqueness constraints (e.g., usernames) to prevent split-brain and corruption.

## Scalability

- **Identify Load First**: Measure load parameters (requests/sec, read/write ratio, cache hit rate) before attempting to scale.
- **Partition by Hash**: Partition data by hash of key to spread load evenly and avoid hot spots, unless range queries on the primary key are a core requirement.
- **Split Hot Keys**: Prefix a disproportionately active key with a random value (and strip it on read) to spread its write load across multiple partitions.
- **Rebalancing-Aware Routing**: Route requests through a dedicated tier (e.g., ZooKeeper-backed) that tracks partition rebalancing so clients never query stale nodes.

## Performance

- **Optimize Tail Latency**: Optimize for tail latency (P95, P99, P99.9) rather than averages, since slow backend requests dominate aggregate user experience.
- **Columnar for Analytics**: Use vectorized processing and column-oriented storage for analytical workloads to minimize disk I/O and maximize CPU cache efficiency.
- **Sequential over Random Writes**: Prefer sequential writes—log-structured (LSM) storage for write-heavy workloads, write-ahead logs for crash recovery. Benchmark LSM vs B-tree for your write amplification profile.
- **Avoid Synchronous Critical-Path Calls**: Eliminate synchronous network requests in the critical path by subscribing to change streams and maintaining local state where possible.

## Fault Tolerance

- **Assume Faults**: Assume hardware and network faults are inevitable; design software to tolerate partial failures rather than trying to prevent all of them.
- **Fencing Tokens**: Grant locks and leases with monotonically increasing fencing tokens so a process paused by GC or network delay cannot act after its lease expires.
- **Tolerant Timeouts**: Detect failures with timeouts, but configure them to tolerate expected network jitter and process pauses.
- **Manual Override**: Always provide a manual override for automatic failover and rebalancing to prevent cascading failures and split-brain scenarios.

## Streaming & Batch

- **Immutable Inputs, Append-Only Outputs**: Treat batch inputs as immutable and outputs as append-only to enable safe retries and "human fault tolerance" (re-running after fixing buggy code).
- **Event Time, Not Processing Time**: Use event-time timestamps embedded in the data for all windowing and aggregation to ensure deterministic results during reprocessing.
- **Session Windows**: Use session windows for analytics to group user events that occur closely together in time.
- **Co-Partition Joins**: Partition stream joins by the same key so related events from different streams reach the same operator instance.

## State & Idempotency

- **Idempotent Side Effects**: Make all side-effecting operations idempotent so they can be safely retried after network or process failures.
- **Deduplicate with Request IDs**: Suppress duplicate requests by passing a unique request ID from the client all the way to a database uniqueness constraint.
- **Log Compaction**: Use log compaction in event logs so derived state can be rebuilt without replaying the entire history.
- **Separate Commands from Events**: Validate a command synchronously, then append the successful result as an immutable event to a log.

## Observability

- **Invariant Telemetry**: Set up detailed metrics and error-rate telemetry to detect invariant violations and give early warning of degradation.
- **Log Causal Reads**: Log the results of read queries that influence subsequent user decisions to enable reconstruction of causal dependencies during debugging.
- **Continuous Integrity Audits**: Continually compare replicas and verify checksums with background processes rather than blindly trusting database guarantees.

## Security

- **No LWW for Sensitive Data**: Never use "Last Write Wins" with time-of-day clocks for security-sensitive data (e.g., permissions), because clock skew can silently drop writes.
- **Encrypt Across the Public Internet**: Assume a non-Byzantine environment inside a private datacenter, but implement end-to-end encryption and authentication for any data crossing the public internet.

## Operational Tradeoffs

- **Manual Rebalancing for Stateful Systems**: Prefer manual over fully automatic rebalancing for stateful systems to avoid unpredictable performance drops under high load.
- **Handle Replication Lag**: Use asynchronous replication for cross-datacenter availability, but explicitly account for replication lag in application logic.

## Explicit Anti-Patterns

- **No Unordered Dual Writes**: Never write to multiple systems (e.g., DB and cache) without a global ordering mechanism; use Change Data Capture (CDC) instead.
- **No Heterogeneous Distributed Transactions**: Avoid distributed transactions (XA) across heterogeneous technologies unless strictly required by legacy constraints; prefer log-based asynchronous integration.
- **Never Ignore Commit Results**: Never ignore the return value of a commit. If the network fails before the response arrives, assume the transaction could have either succeeded or failed and handle both cases.
- **No Unguarded Check-Then-Insert**: Never rely on application-level "check-then-insert" logic without serializable isolation or a database-level uniqueness constraint.
