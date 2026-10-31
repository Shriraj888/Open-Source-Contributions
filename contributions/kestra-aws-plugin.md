# PR #842 — Persist Kinesis Trigger Shard Positions (Kestra AWS Plugin)

Kestra's AWS plugin provides integrations for AWS services, including a polling
trigger for consuming records from Amazon Kinesis streams.

**Repository:** [kestra-io/plugin-aws](https://github.com/kestra-io/plugin-aws)  
**Pull Request:** [kestra-io/plugin-aws#842](https://github.com/kestra-io/plugin-aws/pull/842)  
**Issue:** [kestra-io/plugin-aws#820](https://github.com/kestra-io/plugin-aws/issues/820)  
**Status:** Merged (October 30, 2026)  
**Role:** Contributor (Author)  
**Contribution Program:** KestraHacktober

## Overview

This contribution fixed the Kinesis polling trigger so it persists the last
processed sequence number for each shard. Every subsequent poll can resume from
the saved position instead of starting from the originally configured iterator
position.

## Problem

`io.kestra.plugin.aws.kinesis.Trigger` computed the last sequence number for
each shard but did not persist it. As a result:

- `TRIM_HORIZON` replayed the same records on every polling interval.
- `LATEST` could skip records written between two polls.
- The trigger's documented checkpointing behavior did not match its runtime
  behavior.

The issue was reported and reproduced with a Kinesis stream containing records,
then assigned for implementation in
[kestra-io/plugin-aws#820](https://github.com/kestra-io/plugin-aws/issues/820).

## What I Implemented

- Persisted the last processed sequence number for each Kinesis shard.
- Resumed subsequent reads with `AFTER_SEQUENCE_NUMBER`.
- Kept the configured iterator type and starting sequence as the fallback for
  shards without a saved position.
- Added the optional `stateKey` setting to configure the persisted state key.
- Added the optional `stateTtl` setting to control the lifetime of persisted
  trigger state.
- Kept the latest per-shard positions available in the trigger output so flows
  can continue to inspect their polling progress.

## Technical Details

- **Core focus:** Stateful Kinesis polling and per-shard checkpoint recovery.
- **Files changed:**
  - `src/main/java/io/kestra/plugin/aws/kinesis/Consume.java`
  - `src/main/java/io/kestra/plugin/aws/kinesis/Trigger.java`
  - `src/test/java/io/kestra/plugin/aws/kinesis/ConsumeTest.java`
  - `src/test/java/io/kestra/plugin/aws/kinesis/TriggerTest.java`
  - `src/test/java/io/kestra/plugin/aws/s3/TriggerTest.java`
- **State behavior:** A saved shard position takes precedence on later polls;
  an unseen shard starts with the configured iterator and starting sequence.
- **Systems affected:** Kinesis record consumption, trigger state storage,
  polling continuity, and regression coverage.

## Validation

The PR was QAed against a Kinesis stream and polling trigger configured with
`TRIM_HORIZON`:

1. Confirmed that the first poll delivered existing records.
2. Confirmed that later polls did not emit already processed records again.
3. Added records between polls and confirmed that the next poll delivered them.
4. Confirmed that shards without saved positions used the configured iterator
   type and starting sequence.

Regression tests were added and updated around `Consume`, the Kinesis trigger,
and shared trigger-state behavior.

## Before vs After

| Before | After | Value |
|---|---|---|
| Each poll reused the configured starting position | Polls resume from a saved per-shard sequence | Prevents duplicate processing |
| `LATEST` could miss records written between polls | The next poll continues after the last processed record | Improves delivery reliability |
| Computed positions were not durable | State can be configured with `stateKey` and `stateTtl` | Makes checkpointing reusable and controllable |
| New shards had no explicit state fallback | New shards use the configured iterator and starting sequence | Handles shard discovery predictably |

## Impact

This change makes Kinesis polling stateful and reliable across intervals. It
prevents repeated workflow executions for the same records while ensuring
records arriving between polls are picked up. The configurable state key and
TTL also give Kestra users control over how trigger checkpoints are stored and
retained.

## Skills Demonstrated

- Java backend development
- AWS Kinesis stream and shard semantics
- Stateful polling and checkpoint design
- Regression testing and behavior-driven QA
- Open-source issue investigation and maintainer collaboration

## Contribution Outcome

- Fixed the bug reported in [kestra-io/plugin-aws#820](https://github.com/kestra-io/plugin-aws/issues/820).
- Closed the issue through
  [kestra-io/plugin-aws#842](https://github.com/kestra-io/plugin-aws/pull/842).
- Merged into `main` after maintainer review.

## Why This Matters

Polling integrations must preserve progress between runs to provide dependable
at-least-once stream consumption without uncontrolled replay. Persisting shard
positions fixes the root cause at the trigger boundary and makes the behavior
match Kestra's documentation and user expectations.