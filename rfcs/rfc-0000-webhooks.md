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

This RFC adds the ability to create, edit, delete webhooks and send messages with the
webhook, webhooks have a name, id, channel id, avatar and token. The token and id are
used to send messages with the webhook.

Fetching the webhook can be done with or without authorization, if you fetch with
authorization the token will be returned, as the token is secret it should not be
given to people without permissions.

There will be a built-in way to use GitHub webhooks from revolt as this is a very
commonly used type of webhook, no other types of webhooks have been considered yet.

Creating a webhook will give it a default name of "Webhook" and default to the first
channel in the channel list, users can then edit the name, channel and avatar.

# Reference-level explanation

The database will have an additional document to store the webhooks

```rust
struct Webhoook {
  // Unique Id
  id: String,

  // The name of the webhook - 1 to 32 chars
  name: String,

  // The avatar of the webhook, it it has one
  avatar: Option<File>,

  // The ID of the channel which this webhook belongs to
  channel: String,

  // The token of the webhook
  token: String
}
```

Addition routes must be added to the API, this includes:

### Channel Routes
```
POST /channel/<target>/webhooks - Create webhook in the channel
GET /channel/<target>/webhooks - Gets all webhooks in the channel
```

### Webhook Routes

```
GET /webhooks/<target>/<token> - Gets the webhook with a token, does not require permissions
GET /webhooks/<target> - Gets the wehook, requires permissions
PATCH /webhooks/<target>/<token> - Edits the webhook with a token, does not require permissions
PATCH /webhooks/<target> - Edits the webhooks, requires permissions
DELETE /webhooks/<target>/<token> - Deletes the webhook with a token, does not require permissions
DELETE /webhooks/<target> - Deletes the wehook, requires permissions
POST /webhooks/<target>/<token> - Sends a message with the webhook
POST /webhooks/<target>/<token>/github - GitHub compatible route for sending a message with the webhook
```

The PATCH routes take a json body with the data for editing the webhook:

```rust
struct WebhookEditBody {
  // The new name for the webhook
  name: Option<String>,

  // The new avatar for the webhook - Autumn ID
  avatar: Option<String>,

  // The fields to remove
  remove: Option<Vec<RemoveFields>>
}

pub enum RemoveFields {
  Avatar
}
```

The POST routes take the same json body as the message send route. The data the GitHub compatible
route takes can be seen on the [GitHub docs](https://docs.github.com/en/webhooks-and-events/webhooks/webhook-events-and-payloads)

The message structure must be changed to acomodate these changes, this requires a breaking change
of making `Message.author` optional, as no author will exist if a webhook sends the message,
when a webhook sends a message a `Message.webhook` field will instead exist containing the webhook
ID. These two fields are mutually exlusive.

```diff
- author: String
+ author: Option<String>,
+ webhook: Option<String>
```

This will require an update to existing programs which use the API to ensure they do not break
with this change.

To get infomation about the webhook which send the message, you are able to use the `GET /webhooks/<target>` route which returns all public infomation about the webhook, as this does not require any permissions anyone is able to use the route, you are encoraged to cache the webhook info to avoid repeated fetches.

There will be three new events to go along side this, these events do not contain the token as these events are
sent to all people who have access to the channel and not just people with permissions

```rust
enum Events {
  WebhookCreate(Webhook),  // Contains all the info about the webhook which was created

  WebhookUpdate {
    id: String,  // The ID of the updated webhook
    data: PartialWebhook,  // Contains the data which was updated
    remove: Vec<RemoveFields>  // A vec of the fields which where cleared
  },

  WebhookDelete {
    id: String  // The ID of the webhook which was deleted
  },

  // existing events
  ...
}
```

# Prior art

[Discord](https://discord.com) and [Slack](https://slack.com) both have webhooks as well both with
similar implementation,
both work well and have wide adoption

# Unresolved questions

Currently this RFC includes a breaking change, if possible this would like to be avoided however
no solution has been found yet.

# Security concerns

No security concerns have been found yet, however this does allow third-parties to send messages to revolt,
this could be used maliciously however the affected scope is small as it can only send messages.

# Future ideas

Currently there is only a Github compatible route for pre-existing formats, in the future this could be
expanded to support more formats such as slack for example.