- Feature Name: Interactions
- Start Date: 06/04/2023
- RFC PR: https://github.com/revoltchat/rfcs/issues/0000
- Tracking Issue: https://github.com/revoltchat/revolt/issues/0000
- Status: draft

# Summary

Interactions are a way of interacting with bots in a more high level with better intergration
with revolt and the UI. This will include, slash commands, buttons, selects, context menu commands and
a customizable settings page for users to edit the bots config.

# Motivation

A large part of developing a bot is handling a lot underlying parts like command handling and
a way to let users configure the bot for there server, as both of these can be done in a variety
of ways it can cause a lot of fragmentation in the ways its done, different bots could parse
commands in different ways making it hard for the user to remember how each bot handles it,
increasing cognitive load on the user, along with this is the fact that there is no UI to support
it as its all handled by the bot. In a similar case configuring the bot can be either done via
commands or via a website which requires the user to leave revolt.

# Guide-level explanation

Multiple things are introduced in this RFC, slash commands, buttons and context menu commands are
all classed under the "interactions" type.

## Slash command

A defined command users can invoke to trigger actions from a bot, the parameters are known and
is intergrated into the UI in a way that the user can understand what the command takes before
they run it and while writing it. Invoking a slash commands requires typing "/" into the message
box. Slash commands are never sent as a message and are instead directly sent as a seperate request,
other people will not see the command being sent unless the response is a message.

Slash commands are able to have parameters with a variety of types and subcommands to group commands
together.

parameters are able to have autocomplete to allow entering certain types of infomation easier.

## Context Menu Commands

Context menu commands are actions you can invoke on any message or user from the context menu
(right click or click and hold on mobile), these allow users to run actions on a message or user.

member context menu commands are not attached to any specific channel and are invoked at a server level,
message context menu commands have the message's channel attached.

## Ephermeral Messages

Ephermeral messages are messages which only one user can see, they can be sent as response to interactions,
they are not stored in the server and are only sent to the user.

## Message Components

Message components are interactions which are attached to messages, this currently includes buttons
and selects.

Message components are layed out in a grid on messages, currently the grid size is of 5 x 5 however
this can be exapnded in the future. each component takes up a different size on the grid.

### Buttons

Similar to reactions they are clickable actions on a message which can do different actions
from a bot.

buttons take up a 1x1 area.

### Selects

Selects are dropdown menus which allow users to select a value from a preexisting list of values.
They are able to select one or multiple values depending on the configuration on the select.

selects take up a 1x5 area, an entire row.

## Bot Configuration

Because most bots require a way to change the configuration for them, there will be a centralized
system to be able to create, edit and view the configs for each server, this will include both a
server wide config and per channel.

The configuration can be customized using a variety of different input types, such as:
- Text
- Integer
- Role
- Channel
- Dropdown
- Switch
- Radio Buttons
- Colour Picker

This list could be expanded in the future to allow more types

## Renders

https://www.figma.com/file/cDSUvArKH2pG0zv35na9El/Interactions?type=design&node-id=0%3A1&t=8eDl3jrNkGiHez7L-1

<!-- Explain the proposal as if its already in Revolt and you were teaching it to new users.
- Introduce new concepts
- Explain the feature with examples
- What this fixes or adds and what users should think of the feature
- Discuss how this impacts using revolt, does it make it harder or easier to use.

For internal oriented RFCs such as internal code changes, this should largely talk about how contributors should think about the change. and give examples on the impacts. -->

# Reference-level explanation

## Slash Commands

### Parameter Types

### Checks

### Cooldowns

## Components

## Responding To Interactions

## Config Panel

## Receiving Interactions

Bots can receive interactions either via their websocket connection or via the interactions URL, if the
interactions URL is set then the interaction will not be sent via websockets.

[graph here]

## Routes

- GET -  Get interactions
- POST - Sync interaction

- GET - Get server/channel config
- PATCH - Update server/channel config
- PATCH - Set server/channel config options

- POST - interaction response
- PATCH - edit interaction response
- DELETE - delete interaction response

Webhook for sending followup messages, doesnt work with context menu commands.

## API Reference

<!-- This is the technical section of the RFC, it should go over in detail:
- Its interaction with other features
- How this will be implemented
- Corner or edge cases

This section should reference the examples in the previous section and disect them in more detail. -->

# Drawbacks

<!-- Why should this not be added. -->

# Rationale and alternatives

<!-- - Why is this design the best
- Are there alternative ways to solve this
- Could this be done with existing features or existing solutions -->

# Prior art

- Discord
- Slack

<!-- This should include both good and bad outlooks on the the proposal, this could include how other platforms, software and hardware solve similar issues if relevent or how any existing proposals have tried to solve the same problem. -->

# Unresolved questions

- is this worse in any way to message commands

<!-- - Are there any parts which are not yet designed or you believe need further discussion
- Do you expect any part of this proposal to change and you wish to draw attention to
- Are there any related issues which you belive are out of the scope of this RFC that could be addressed in a seperate RFC? -->

# Security concerns

- Sending requests to unsecure bot routes

<!-- How does this RFC impact security, this section might not always be applicable and if you believe it is not please write in this section why you believe that. -->

# Future ideas

- more components and interaction types

<!-- Are there any features or changes that this proposal could enable, or how this proposal impacts the future of Revolt. -->
