- Feature Name: Webhooks
- Start Date: 10/03/2023
- RFC PR: https://github.com/revoltchat/rfcs/issues/0000
- Tracking Issue: https://github.com/revoltchat/revolt/issues/0000

# Summary

This RFC adds the ability to be able to send messages from external applications and
services through webhooks, this allows intergration with other services.

# Motivation

Webhooks make it easy to intergrate with other services, this allows other services
to take advantage of Revolt's to send infomation through revolt. A common example is
GitHub Webhooks which allow you to send messages when a repository changes.

# Guide-level explanation

<!--
Explain the propsal as if its already in revolt and you where teaching it to new users.
- Introduce new concepts
- Explain the feature with exampls
- What this fixes or adds and what users should think of the feature
- Discuss how this impacts using revolt, does it make it harder or easier to use.

For internal oriented RFCs such as internal code changes, this should largely talk about
how contributors should think about the change. and give examples on the impacts. -->

This RFC adds the ability to create, edit, delete webhooks and send messages with the
webhook, webhooks have a name, id, channel id, avatar and token. The token and id are used to send
messages with the webhook.

Fetching the webhook can be done with or without authorization, if you fetch with
authorization the token will be returned, as the token is secret it should not be
given to people without permissions.

There will be a built-in way to use GitHub webhooks from revolt as this is a very
commonly used type of webhook, no other types of webhooks have been considered yet.

Creating a webhook will give it a default name of "Webhook" and default to the first channel in the channel list, users can then edit the name, channel and avatar

# Reference-level explanation

<!-- This is the technical section of the RFC, it should go over in detail:
- Its interaction with other features
- How this will be implemented
- Corner or edge cases

This section should reference the examples in the previous section and disect them in
more detail. -->

# Drawbacks

<!-- Why should this not be added. -->

# Rationale and alternatives

<!-- - Why is this design the best
- Are there alternative ways to solve this
- Could this be done with existing features or existing solutions -->

# Prior art

<!-- This should include both good and bad outlooks on the the proposal, this could include
how other platforms, software and hardware solve similar issues if relevent or how any
existing proposals have tried to solve the same problem. -->

# Unresolved questions

<!-- - Are there any parts which are not yet designed or you believe need further discussion
- Do you expect any part of this proposal to change and you wish to draw attention to
- Are there any related issues which you belive are out of the scope of this RFC that
  could be addressed in a seperate RFC? -->

# Security concerns

<!-- How does this RFC impact security, this section might not always be applicable and if you
believe it is not please write in this section why you believe that. -->

# Future ideas

<!--
Are there any features or changes that this proposal could enable, or how this proposal
impacts the future of Revolt. -->