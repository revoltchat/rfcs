- Feature Name: (API route to look up user by username)
- Start Date: (15/03/2023)
- RFC PR: https://github.com/revoltchat/rfcs/issues/1
- Tracking Issue: https://github.com/revoltchat/revolt/issues/0000
- Status: draft

# Summary

Add an API route to look up users by username instead of ID

# Motivation

Currently, bots need to keep a cache of every user if they want to allow commands to target users by username (e.g. /ban @insert).
I am proposing an API route that allows clients to look up users by username.

# Guide-level explanation

Send a `GET` request to `/users/by-name/insert` (no idea how to name the route) to get an array of users matching that username.

- Lookup should be case insensitive
- The response should be an array, in case multiple users managed to get the same username assigned somehow and to future proof for when discriminators will be added

# Reference-level explanation

I don't know

# Drawbacks

- Might require another index in MongoDB
- Since the backend needs to make sure the returned users are mutual, there might be more processing overhead involved, but the logic for this can probably be copied from the /users/:id route

# Rationale and alternatives

- Caching large amounts of users becomes unfeasable once bots scale to a certain size. AutoMod is using 3-4gb of memory right now.
- Bots could technically keep a key-value cache for usernames and IDs, but implementing an API route to look up users is more feasable and doesn't require a bunch of API requests at startup to fetch all members from every server.

# Prior art

No idea how Discord bots solve it

# Unresolved questions

- Not sure about the API route and whether to put the username to look up in the path or the request body

# Security concerns

The backend will need to ensure to only return users the client has access to (e.g. friends, mutual servers or groups)

# Future ideas

Can I just delete empty segments or do I have to put text here
