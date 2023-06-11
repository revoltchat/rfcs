- Feature Name: (API route to look up user by username)
- Start Date: (15/03/2023)
- RFC PR: https://github.com/revoltchat/rfcs/pull/2
- Tracking Issue: https://github.com/revoltchat/revolt/issues/0000
- Status: draft

# Summary

Add an API route to look up users by username instead of ID

# Motivation

Currently, bots need to keep a cache of every user if they want to allow commands to target users by username (e.g. /ban @insert).
I am proposing an API route that allows clients to look up server members by username.

# Guide-level explanation

The client sends a `POST` request to `/servers/{server}/members/search`. The search query is specified in the request body:
```json
{
    "username": "insert",
    "discriminator": "1234",
    "exact": true,
    "limit": 10
}
```

If the `discriminator` field is omitted, the result returns all users with a matching username, otherwise it returns only the user with matching discriminator. `limit` can be an optional integer between 1..100 and limits the maximum number of results.

The `exact` field should default to true and indicates whether we want an exact match (still case insensitive) or if we want to receive all users starting with the search term, similar to Discord's user search - See [below](#prior-art). \
Setting this to `false` will make this endpoint feasable as a replacement for the current temporary [member search route](https://github.com/revoltchat/backend/commit/d81d08f1ce222a9fe05986e15c4e3748bdd5d4ae).

If this is implemented before discriminators are, the `discriminator` field should be supported as a placeholder and functionality can be added at a later point.

The response should be an array of users and server members, like so:
```json
{
    "users": [
        {
            "_id": "first user",
            "username": "...",
            "...": "..."
        },
        {
            "_id": "second user..."
        }
    ],
    "members": [
        {
            "_id": {
                "user": "first user",
                "server": ".."
            },
            "..": "..."
        },
        {
            "_id": {...}
        }
    ]
}
```

TODO If no user is found, should we simply return empty arrays or send HTTP 404?

# Reference-level explanation

TODO I'm not experienced with Revolt's backend or database structure, so I don't know exactly how the implementation would work.

# Drawbacks

- Might require another index in MongoDB
- Unsure about database query efficiency

# Rationale and alternatives

- Caching large amounts of users becomes unfeasable once bots scale to a certain size. AutoMod is using 3-4gb of memory right now.
- My current solution to this is looping through every server the bot is in, fetching every user and caching them. This results in super slow startup times and obviously doesnt scale well - Every time AutoMod disconnects (which sometimes happens every couple minutes), it sends thousands of requests to Revolt.

# Prior art

Discord has a [similar API endpoint](https://discord.com/developers/docs/resources/guild#search-guild-members) which also lets you search users by username. The result returns all users who start with the query.

# Unresolved questions

- If `exact` is false, how should the `discriminator` field behave?
- Is this feasable in terms of database queries? I believe MongoDB has advanced text search features, but I don't know the limitations of those.

# Security concerns

Since this is scoped to server members, we don't need to worry about exposing users that should not normally be visible to the client.

# Future ideas

Besides improving bot development, this change could also allow clients to display @mention autocompletion results without having to cache every member. The current route which lets clients fetch all members could be deprecated or removed entirely.
