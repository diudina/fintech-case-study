# FlowPay Data Dictionary

## Overview

The FlowPay dataset contains five related tables:
`users`, `verifications`, `events`, `transfers`, and `support_contacts`.

## users

**Purpose:**  Stores registered users and their signup, acquisition, demographic, platform, and internal-account attributes.
**Grain:**  One row per registered user
**Primary key:**  user_id. No NULL or duplicate values were found during the initial data-quality audit.
**Foreign keys:**  None
**Important timestamps:**  signup_timestamp — date and time when the user registered.
**Nullable relationships:**  Not applicable.

### Columns

| Name | Type | Constraints | Notes|
|------|------|-------------|-----|
| `user_id` | `text` | Primary | Unique identifier of a registered user|
| `signup_timestamp` | `timestamptz` |  Nullable | Date and time when the user registered|
| `country` | `text` |  Nullable | User country recorded at signup|
| `acquisition_channel` | `text` |  Nullable | Channel through which the user was acquired|
| `platform` | `text` |  Nullable | Platform recorded for the user at signup|
| `signup_device` | `text` |  Nullable | Device category used during signup|
| `age_group` | `text` |  Nullable | Age-range category assigned to the user|
| `is_internal_user` | `int8` |  Nullable | Indicates whether the account belongs to an internal FlowPay user, 0 = external user; 1 = internal user|

## verifications

**Purpose:** Stores individual identity-verification attempts, including their
timing, outcome, method, failure reason, attempt context, and sequence number.

**Grain:** One row per verification attempt.

**Primary key:** `verification_id`. No NULL or duplicate values were
found during the initial data-quality audit.

**Foreign keys:** `user_id` references `users.user_id`. No verification records
with an unknown `user_id` were found during the initial audit.

**Important timestamps:**

- `started_at` — date and time when the verification attempt started.
- `ended_at` — date and time when the attempt ended. It may be NULL for attempts
  that have not reached an end state.

**Nullable relationships:** None expected. Every verification attempt should
belong to a registered user through `user_id`.

### Columns

| Name | Type | Constraints | Notes|
|------|------|-------------|-----|
| `verification_id` | `text` | Primary | Unique identifier of a verification attempt
| `user_id` | `text` |  Foreign | User who initiated the verification attempt |
| `started_at` | `timestamptz` |  Nullable | Date and time when the verification attempt started |
| `ended_at` | `timestamptz` |  Nullable | Date and time when the verification attempt finished |
| `status` | `text` |  Nullable | Current or final outcome of the verification attempt | 
| `verification_method` | `text` |  Nullable | Method used for the verification attempt |
| `failure_reason` | `text` |  Nullable | Reason recorded when a verification attempt failed | 
| `country` | `text` |  Nullable | Country associated with the verification attempt |
| `platform` | `text` |  Nullable | Platform used for the verification attempt |
| `app_version` | `text` |  Nullable | Application version used during the verification attempt |
| `attempt_number` | `int8` |  Nullable | Sequential number of the user’s verification attempt |

## events

**Purpose:** Stores timestamped product events generated during user activity,
including session, platform, device, location, and related business-entity
identifiers.

**Grain:** One row per recorded product event.

**Primary key:** `event_id`.

**Foreign keys:**

- `user_id` references `users.user_id`.
- `verification_id` references `verifications.verification_id` when the event
  relates to a verification attempt.
- `transfer_id` references `transfers.transfer_id` when the event relates to a
  transfer.

**Important timestamps:** `event_timestamp` — date and time when the event was
recorded.

**Nullable relationships:**

- `user_id` is nullable in the current schema. Whether anonymous or system
  events are expected should be confirmed.
- `verification_id` is expected to be NULL for events unrelated to verification.
- `transfer_id` is expected to be NULL for events unrelated to transfers.

### Columns

| Name | Type | Constraints | Notes |
|------|------|-------------|-------|
| `event_id` | `text` | Primary | Unique identifier of a recorded event |
| `user_id` | `text` |  Nullable | User associated with the event |
| `event_timestamp` | `timestamptz` |  Nullable | Date and time when the event was recorded |
| `event_name` | `text` |  Nullable | Name identifying the type of product event |
| `session_id` | `text` |  Nullable | Identifier grouping events generated during the same user session |
| `platform` | `text` |  Nullable |  Platform on which the event occurred |
| `app_version` | `text` |  Nullable | Application version recorded when the event occurred |
| `device_type` | `text` |  Nullable | Device category from which the event originated |
| `country` | `text` |  Nullable | Country associated with the event |
| `verification_id` | `text` |  Nullable | Verification attempt associated with the event |
| `transfer_id` | `text` |  Nullable | Transfer associated with the event |

### Data-quality considerations

- No NULL or duplicate `event_id` values were found during the initial audit.
- There are 30,201 `verification_completed` events but 30,167 verification
  records with `status = 'completed'`.
- The difference is associated with 34 `verification_completed` events where `verification_id` is NULL. These events cannot be linked to a specific verification attempt.

## transfers

**Purpose:** Stores individual money transfers, including their lifecycle timestamps, status, currencies, EUR-normalised amount recipient, payment method, failure information, and technical context.

**Grain:** One row per transfer.

**Primary key:** `transfer_id`.

**Foreign keys:** `user_id` references `users.user_id`.

**Important timestamps:**

- `created_at` — date and time when the transfer was created.
- `submitted_at` — date and time when the user submitted the transfer.
- `completed_at` — date and time when the transfer was successfully completed.

**Nullable relationships:** `user_id` is nullable in the current schema,
although every transfer is expected to belong to a registered user. Actual NULL
values should be checked before treating this as a business rule.

### Columns

| Name | Type | Constraints |
|------|------|-------------|
| `transfer_id` | `text` | Primary |
| `user_id` | `text` |  Nullable |
| `created_at` | `timestamptz` |  Nullable |
| `submitted_at` | `timestamptz` |  Nullable |
| `completed_at` | `timestamptz` |  Nullable |
| `status` | `text` |  Nullable |
| `source_currency` | `text` |  Nullable |
| `target_currency` | `text` |  Nullable |
| `amount_eur` | `float8` |  Nullable |
| `recipient_country` | `text` |  Nullable |
| `payment_method` | `text` |  Nullable |
| `failure_reason` | `text` |  Nullable |
| `transfer_number` | `int8` |  Nullable |
| `platform` | `text` |  Nullable |
| `app_version` | `text` |  Nullable |

### Data-quality considerations

- NULL lifecycle timestamps may be expected depending on the transfer status. For example, a transfer that was created but never submitted should have a NULL `submitted_at`, while a failed transfer should normally have a NULL `completed_at`.

## support_contacts

**Purpose:** Stores individual customer-support contacts, including when they
were created and resolved, why the user contacted support, the communication
channel, and the technical and geographical context of the contact.

**Grain:** One row per support contact.

**Primary key:** `contact_id`.

**Foreign keys:** `user_id` references `users.user_id`.

**Important timestamps:**

- `created_at` — date and time when the support contact was created.
- `resolved_at` — date and time when the support contact was resolved.

**Nullable relationships:** `user_id` is nullable in the current schema,
although a customer-support contact would normally be expected to belong to a
registered user. Actual NULL values should be checked.

### Columns

| Name | Type | Constraints | Notes |
|------|------|-------------|-------|
| `contact_id` | `text` | Primary | Unique identifier of a support contact |
| `user_id` | `text` |  Nullable | User associated with the support contact |
| `created_at` | `timestamptz` |  Nullable | Date and time when the support contact was created |
| `resolved_at` | `timestamptz` |  Nullable | Date and time when the support contact was resolved |
| `category` | `text` |  Nullable | Broad classification of the support contact |
| `contact_reason` | `text` |  Nullable | Specific reason why the user contacted support |
| `channel` | `text` |  Nullable | Communication channel used for the support contact |
| `country` | `text` |  Nullable | Country associated with the support contact |
| `platform` | `text` |  Nullable | Platform associated with the support contact |
| `app_version` | `float8` |  Nullable | Application version recorded for the support contact |
