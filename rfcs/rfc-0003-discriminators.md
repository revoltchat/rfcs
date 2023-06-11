- Feature Name: Discriminators
- Start Date: 06/05/2023
- RFC PR: https://github.com/revoltchat/rfcs/pull/3
- Tracking Issue: https://github.com/revoltchat/backend/issues/247
- Status: accepted

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
myusername#1234
^ your chosen name (restricted character set)
          ^ separator between chosen name and discriminator
           ^ discriminator (4-digits in range 0001 to 9999)
```

Users will also be able to set an optional display name which overrides their username
in various parts of the user interface.

Usernames use a restricted character set of any Unicode alphabet excluding some lookalike
characters from the Cyrillic alphabet, if you username does not currently fit the specification,
then it will be automatically santised upon upgrade and copied to your display name.

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

A new `discriminator` string field shall be added to the User object, as such the
`UNIQUE IGNORE-CASE USERNAME` index will be replaced with a
`UNIQUE (IGNORE-CASE USERNAME + DISCRIMINATOR)` index.

A new `display_name` string field shall be added to the User object, this will be subject
to all existing username restrictions.

Existing clients may continue to display usernames but will no longer have the guarantee
any one username is unique and must implement the discriminator field to distinguish them
to end users, and they must also show the display name instead of username in the chat
interface and include it in profiles.

Discriminators shall be 4-digit identifiers (this may be expanded in the future if we reach
a point at which they are no longer sustainable, or otherwise at least one username is becoming
saturated).

The friend add route, `POST /users/friend` will be updated to support parsing a discriminator,
this will provide backwards compatibility for older clients to some limited extent.

```json
{
  "username": "abc#1234"
}
```

# Drawbacks

The main drawbacks are:

- Additional information to remember about your username

  Although this works both ways in the sense that, you can have a much simpler username.

- Users may no longer have a completely 'unique' username on the platform

  But we're approaching this from the standpoint that usernames on platforms such as these should not have to compete for 'uniqueness'.

I want to take this section to also discuss the case study of Discord's removal of the discriminator system and their [cited drawbacks](https://discord.com/blog/usernames#heading-3),
it is worth noting however that Discord has not really specified how they've generated these statistics or this information,
although for the sake of the following they are taken at face value:

- > More than 40% of you either don’t remember your discriminator or don’t even know what a discriminator is. That’s a big problem when discriminators are required to add a new friend.

  This RFC cannot address this issue however further work can be done to ease these interactions:

  - R&D to determine how to build the UI to intuitively tell the user how to share their username with their discriminator
  - Implementation of friend links / codes to ease connecting users to each other

    This is not something that Discord has solved and to this day there is no way to create a friend request link.

  - Implementation of nearby find (prior art: Discord) / QR friend codes (prior art: Snapchat) / add by contact book (prior art: Signal, WhatsApp)

    These may help streamline IRL interactions by providing users with a simple flow to follow.

  - Implementation of add by connections

    Allow adding other people by common social media connections.

  - Global user search (prior art: Steam)

    Allow users to opt-in to a global username search / or otherwise also search through mutual members on servers, to avoid needing the discriminator altogether.

- > Across Discord, almost half of all friend requests fail to connect the user with the person they wanted to match with, mostly because users enter an incorrect or invalid username due to a combination of missing discriminator and incorrect casing.

  - Issues with ease of adding are addressed in the point above, however incorrect casing is irrelevant to Revolt as-is because Revolt's usernames are already case-insensitive and this will not change.

    If we look at the given example:

    > You meet someone IRL that you want to talk to on Discord, and they say “I’m Phibi Eight Nine Three Six!” You go home and add “phibi#8936” only to find out you added the wrong “Phibi” because your new friend’s username is actually “PhIBI#8936”.

    Revolt does not permit a registration of both "PhIBI" and "phibi".

- > You want to use a common name like “Mike” or “Jane” but there are already 9,999 Mikes or Janes so you’re blocked from that name altogether.

  We are not restricted to just 4-digit discriminators in our implementation.

- > You like to change your username a lot and get rate limited.

  If we were also to add more stringent rate limits, this may be solved by also including display names.

- > Your friend says they changed their name to “vernacular” but actually it’s “𝖛𝖊𝖗𝖓𝖆𝖈𝖚𝖑𝖆𝖗” and you have trouble finding them.

  While this is a valid concern, I would personally put this down as the user's own issue.

# Rationale and alternatives

Discriminators (with display names and a restricted character set) appear to show the least disadvantages out of all of the solutions discussed so far.

| Solution                                                                | Description                                                                                                           | Users have desired username | Selling disincentivized | Privacy | Usability | Difficulty |
| ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | :-------------------------: | :---------------------: | :-----: | :-------: | :--------: |
| **Discriminators**                                                      | Proposed in this RFC                                                                                                  |             ✅              |           ✅            |   ✅    |    ⚠️     |    Low     |
| **Discriminators**<br/>w/ display names                                 | Show display names with greater priority to username / discriminator combinations.<br/>\* May be proposed in this RFC |             ✅              |           ✅            |   ✅    |    ⚠️     |    Low     |
| **Discriminators**<br/>w/ display names<br/>w/ restricted character set | Also restrict the characters that you can use in the username itself.<br/>\* May be proposed in this RFC              |             ✅              |           ✅            |   ✅    |    ✅     |  Medium †  |
| Unique usernames                                                        | Current system on Revolt<br/>(but any unicode username is allowed)                                                    |             ❌              |           ❌            |   ❌    |    ⚠️     |    Low     |
| Unique usernames<br/>w/ restricted character set                        | Alphanumeric unique global usernames                                                                                  |             ❌              |           ❌            |   ❌    |    ✅     |  Medium †  |
| Unique usernames<br/>w/ display names                                   | Either unique username solution but with added display names that show instead of the username                        |             ⚠️              |           ❌            |   ⚠️    |    ✅     |    Low     |
| Remove usernames altogether<br/>(only display names)                    | Resort to using friend codes, invite links, and the like exclusively.                                                 |             ✅              |           ✅            |   ✅    |    ⚠️     |   High ¶   |
| Unique cryptographic user identifiers with display names                | Resort to using friend codes, invite links, and the like exclusively.                                                 |             ⚠️              |           ✅            |   ⚠️    |    ⚠️     |   Medium   |

In regards to this table,

- Privacy means whether users can avoid being discovered based on their common names.
- Usability is whether users can practically and quickly add each other.
- † Restricting existing usernames further would potentially require some users to change username.
- ¶ Removing usernames altogether would require a complete redesign of how friends work on Revolt.

Discriminators also help solve a couple of issues with regards to username abandonment, users may sign up and:

1. Forget their account credentials or otherwise get locked out, and never recover their account.
2. Forget that their account even exists.
3. Decide they don't want to keep using their account but never delete it.

Revolt does not currently automatically delete old accounts with no activity which is why this poses an issue.

Given Revolt's current size, 4-digit numeric discriminators currently pose the least issues.

| Solution       |      Example       | Description                                   | Usability | Quantity | Safe |
| -------------- | :----------------: | --------------------------------------------- | :-------: | :------: | :--: |
| 4-digit        |      `#1234`       | Any 4 digits                                  |    ✅     |   Low    |  ✅  |
| N-digit        |     `#123456`      | Any N digits                                  |    ⚠️     |  Medium  |  ⚠️  |
| Variable Digit | `#123` ... `#1234` | Scale between n and N digits depending on use |    ⚠️     | Infinite |  ⚠️  |
| 4-hex          |      `#12AF`       | Any 4 hex characters                          |    ✅     |  Medium  |  ⚠️  |
| 4-char         |      `#1ABZ`       | Any 4 alphanumeric characters                 |   ❌ †    |   High   |  ❌  |

In regards to this table:

- Usability is whether the solution is practical, i.e. reasonably sized and simple.
- Quantity is how many possible discriminators may be housed under one username.
- Safe is whether the solution is not susceptible to generating undesired combinations and phrases.
- † Allowing any alphanumeric characters may cause confusion between similar charactres using certain fonts, e.g. `O` and `0`.

We also choose to restrict usernames to any Unicode alphabet rather than the full range given this has the best compromise between reasonable usernames, users affected, and localisation:

| Permissible Format                                |                 Regex                 | Users affected by change | Supports regional dialects | Potential for abuse |
| ------------------------------------------------- | :-----------------------------------: | :----------------------: | :------------------------: | ------------------- |
| Alphanumeric                                      |            `^[a-zA-Z\d]+$`            |           18%            |             ❌             | Low                 |
| Any alphabet ¶ or digit                           |          `^(\p{L} \| \d)+$`           |           17%            |             ✅             | Medium ¶            |
| Alphanumeric with special characters †            |         `^([a-zA-Z\d_.-])+$`          |           11%            |             ❌             | Low                 |
| Any alphabet ¶ or digit with special characters † |        `^(\p{L} \| [\d_.-])+$`        |            9%            |             ✅             | Medium ¶            |
| Current format                                    | `^[^\u200BА-Яа-яΑ-Ωα-ω@#:\n\r\[\]]+$` |            0%            |             ✅             | High                |

- † Special characters include underscore, period and dash.
- ¶ Certain lookalike characters will continue to be blocked, such as those from the cyrillic alphabet.

# Prior art

This has been implemented before on other platforms:

- Discord's discriminator system
- Blizzard's [BattleTag system](https://us.battle.net/support/en/article/75767)

# Unresolved questions

No currently unresolved questions.

# Security concerns

This should not impact security, since this is almost entirely a cosmetic change to usernames.

This should not have any adverse effects for functionality such as blocking users as this is entirely handled using internal IDs.

# Future ideas

As discussed previously, we may look into implementing:

- Friend links / codes
- Nearby find / QR friend codes / add by contact book
- Add users by social connections
- Global user search
- Better friend flow UX

We may also want to look into implementing additional privacy settings for adding users.

As per [discussion comment](https://github.com/revoltchat/rfcs/pull/3#discussion_r1199818589), we may want to warn users that
their username contains weird unicode or other characters that may look similar or weird, that may prevent them from adding
other people on the platform.
