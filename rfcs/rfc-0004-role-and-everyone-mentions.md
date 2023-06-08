- Feature Name: Role and Everyone mentions.
- Start Date: 08/06/2023
- RFC PR: https://github.com/revoltchat/rfcs/pull/4
- Tracking Issue: https://github.com/revoltchat/revolt/issues/0000
- Status: draft

# Summary

Everyone and Role specific mentions.

# Motivation

The issue this RFC aims to solve is that servers sometimes want to notify users of something like a major event or announcement. 

However without specifically mentioning everyone (an example 50+ people) Or only mentioning a specific role (for example Beta Testers).

# Guide-level explanation

## Role and Everyone mentions
In Revolt you can mention a set of users or just everyone in your server. To do this is as simple as adding a @ and then selecting the appropriate role using the arrow keys.
```
@Everyone
```
```
@RoleName
```
## Disabling
to disable role specific or everyone notifications simply either edit your notification settings and disable the mentions by ticking the boxes appropriately named "Role Mentions", "Everyone Mentions".

 Or on a per server basis simply right click on the server name or channel name and untick either "Role Mentions" or "Everyone Mentions". It's as easy as that!

## Permitting
Of course you shouldn't just trust *anyone* with that power that's why you can enable or disable the ability for users to mention Roles or Everyone with the permissions "Can Mention Everyone" or "Can Mention Role".


# Reference-level explanation

This RFC aims to enable users permitted to do so, to mention a subset of users or the entire server. the mention in the message would look like this:
```text
<r:@01H2D2VBG4D2W4FTYM5G0T33WA>
    ^ role id
 ^ role type mention
```
and for everyone mentions it'd look like
```text
<*:@>
```
of course in the frontend it'd have to be similar to the already existing mention feature where along with users server roles appear visibly separated from the users. 

As for Everyone Mentions they'd appear at the top of the autocomplete selection to stop accidental everyone's. 

Additionally when mentioning a large number of people (100+) it will show a confirmation dialogue that will ask for confirmation before the mention is sent. 

## Corner Cases
- Users named either with a role name or everyone. This is not a problem due to the fact that we separate out role and everyone mentions with special syntax instead of the standard user mention syntax.
# Drawbacks

- annoying users with notifications they did not want to be apart of
- being abused by power-moderators
- would add parsing time as it'd require the back-end to parse it further and do a complex selection over the affected users.
# Rationale and alternatives

- This solutions works the best as it's simple and we already have frontend components for mentions, and back-end infrastructure delivering notifications dynamically. It only requires additional parsing of content and easy to add permissions to happen. 
- Adding multiple mentions by selecting affected roles or users using a table of some kind, this approach could work however there would need to be large changes done to the messaging API to facilitate such a solution.
- A bot that sends a couple messages mentioning all of the affected usernames, I did not consider this to be a good solution as it practically DDoSes our servers and if used frequently could fill up chats with useless messages.

# Prior art

Discord, Slack and telegram solve this issue similarly and it's worked for them so far.

# Unresolved questions

- Specific UI design. while this RFC brushes over the potential UI implementation of the change it does not specify the design in full.
- Opinions of the user-base about this. The author of this RFC doesn't quite know how the user-base of Revolt feels about this.
- Server Load, this RFC does not address the inherent load that this'd put on the back-end especially with loads of mentions happening.
