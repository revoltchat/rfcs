- Feature Name: Discriminators
- Start Date: 06/05/2023
- RFC PR: https://github.com/revoltchat/rfcs/issues/0000
- Tracking Issue: https://github.com/revoltchat/revolt/issues/0000
- Status: draft

# Summary

Revolt should introduce discriminators which serve as part of a semi-hidden extension to
usernames which allows multiple users to have the same username, the discriminator would
not be visible in most circumstances except when viewing a person's profile and if you
were to add said person by their username.

# Motivation

This RFC aims to move forwards [prior discussion on the feature suggestions forum](https://github.com/orgs/revoltchat/discussions/61),
it stands to solve multiple problems when implemented correctly:

- Users should in most, if not all, cases be able to pick their chosen username.
- This should curb any incentive to sell / steal accounts for "rare usernames".

  Certain "rare" discriminators can also be prohibited.

- Allow users who do not want to be easily discoverable to stay hidden.

  Just a 4-digit discriminator provides 10,000 unique users.
  Expanding on this, we could for example, let users hide discriminators in servers and similar privacy features.

# Guide-level explanation

Revolt would be switching to a new username system made up of three parts:

```
Hello, World!#123456
^ your chosen name
             ^ separator between chosen name and discriminator
              ^ discriminator
```

Discriminators for new users are generated randomly on sign up while existing users will
get a randomly generated discriminators upon the server software being upgraded.

> Revolt treats usernames as case-insensitive so a full username such as "Jason#1234" is
> effectively the same as "jasON#1234" but only "Jason#1234" will display in the UI.

Users may:

- Change the case of their username at any moment, such as from "Jason" to "jasON".

  This will never affect their discriminator.

- Change their username, but may have their discriminator regenerated if there is a conflict.

  Such as if you are changing from "Jason#1234" to "Phil", but "Phil#1234" is taken.

### User Opinion

Prior discussions on the forums primarily seem to lean in favour of discriminators being
implemented into Revolt, the internal team has not raised any complaints against it, and
the primary community on Revolt ("Revolt Lounge") appears to be somewhat split on the
issue.

As a recent case study, Discord's change to unique usernames only has generated a lot of
backlash and criticism from the community, it appears to be overwhemingly negative but
these experiences are mostly coming from long-term users of Discord. The true opinion of
the average user has not been gauged yet.

### Usability

This may make it more difficult to understand how to add users on Revolt, however this
shouldn't be an issue as long as it's clearly communicated through the user interface.
Revolt already allows a wide range of Unicode in usernames so we are definitely not
making a significant UX hit towards the usability of the friend system.

> There are further considerations listed at the end of this document which must be
> addressed if this RFC were to be implemented.

# Reference-level explanation

WIP

This is the technical section of the RFC, it should go over in detail:

- Its interaction with other features
- How this will be implemented
- Corner or edge cases

This section should reference the examples in the previous section and disect them in more detail.

# Drawbacks

WIP

Why should this not be added.

# Rationale and alternatives

WIP

- Why is this design the best
- Are there alternative ways to solve this
- Could this be done with existing features or existing solutions

# Prior art

This has been implemented before on other platforms:

- Discord's discriminator system
- Blizzard's [BattleTag system](https://us.battle.net/support/en/article/75767)

# Unresolved questions

WIP: what should the discriminator look like, 4-digit, 6-digit, characters, etc

# Security concerns

This should not impact security, since this is almost entirely a cosmetic change to usernames.

# Future ideas

WIP: friend links, QR friend invites, anything to improve friend UX

WIP: additional privacy settings
