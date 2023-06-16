- Feature Name: Pronouns
- Start Date: (16/06/2023)
- RFC PR: None
- Tracking Issue: None
- Status: draft

# Summary

This change creates a pronoun field for revolt's backend.


# Motivation

Pronouns have been requested by the community and are high up on the RFC list.

# Guide-level explanation

Currently users display their pronouns in their Profile content (bio) but this provices a cleaner solution to the problem. The aim of this change is to ensure that pronouns for a user can be easily seen and clearly displayed seperately to a users bio.

# Reference-level explanation

This change will modify:
- core/models/users
- core/db/util/bridge
- quark/models/users
- core/db/models/users/ops/mongo
  And possibly more

For the backend, pronoun data will be stored as such.
```rust
    // User's displayed pronouns
        #[cfg_attr(feature = "validator", validate(length(min = 1, max = 5)))]
        #[serde(skip_serializing_if = "Option::is_none")]
        pub pronouns: Option<Vec<String>>,
```

Having pronouns on Revolt will be optional and this data will be editable from the Profile section of user settings on Revolt's main client. One of the major changes this could bring is pronoun specific language for bots interacting with users.

#### Input
```text
?/ whois info:roles @ToastXC
```
#### Response
```text
She has no roles!
```

# Drawbacks

Mocking users for displaying pronouns on their profile and creating fake pronouns as a joke is very common and seen by some as hateful, There must be a system built into the pronoun system to avoid this. That would take some time and may not be worth the effort.

# Rationale and alternatives

This feature is non essential, users can use their bio to display pronouns as well as their display name (as they have done in the past). The main alternative here is changing nothing

# Prior art

In discussion Instagram is mentioned frequently for their implementation of a pronouns system.

# Unresolved questions

There needs to be a proper system for validation of pronouns, perhaps a database for valid pronouns in each language or a blacklist, its unclear at this moment.

# Security concerns

N/A

# Future ideas

In future the Revolt frontend could refer to the user with their given pronouns (in a similar way to how facebook works).
