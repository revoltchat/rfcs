- Feature Name: Pronouns
- Start Date: (16/06/2023)
- RFC PR: None
- Tracking Issue: None
- Status: draft

# Summary

This change creates a pronoun field for Revolt.chat, this field will be attached to a users public profile information.


# Motivation

Pronouns have been requested by the community and are high up on the RFC list.

# Guide-level explanation

Currently users display their pronouns in their Profile content (bio) but this provides a cleaner solution to the problem. The aim of this change is to ensure that pronouns for a user can be easily seen and displayed inline with a users displayname / username.

# Reference-level explanation

The field `pronouns` will be added to the main `User` object, as well as the `PartialUser` and `FieldsUser` objects. (In practise this means that the pronouns field will be fetched for the `fetch_user` API route, and pronouns can be edited or removed from the `edit_user` API route.

The pronoun field (on a `User` will be an optional array of strings. 

### Data representation in Rustlang
```rust
{
pronouns: Option<Vec<String>>
}
```

### Pronouns stored in JSON
```json
{
	"pronouns": ["she", "her"]
}
```
For no pronouns the object will not be serialised 
```json

```

Additionally, there will be database changes for pronouns to be fully implemented. The `Collection` shall contain an entry of every supported language (represented by an abbreviation) with an array of every valid pronoun for the language. This collection must also be accessible as an API route.

```bson
{
  "language": "<string>",
  "pronouns": ["<string>", "<string>", ...]
}
```


# Drawbacks

As pointed out by users this system is prone to abuse, not only from users assigning themselves fake pronouns but for automated attacks. (e.g. bots scanning users accounts for neopronouns and harassing based on that data). And it has even been suggested that bots should not be allowed to access this data. However there is no clear answer on that yet.

# Rationale and alternatives

For the new web clients pronouns could be displayed as a field of the their profile similar to the new display for badges on Revolt, another option is to show a users pronouns after their displayname/username. 

The former offers a more 'minimal' display for users messages while the latter is more effective as a tool, for ensuring that a user's pronouns are easily visible and more importantly; used and respected.


# Prior art

Pronoun systems have been created tested on many different platforms before Revolt.chat, here is a sample of those
- Instagram
	Instagram has a very high regarded pronoun system as it validates 		pronouns before applying them for a user. 
- Facebook
	Facebook's UI changes if a user adds pronouns to their profile, (changing from gender neutral to gender specific language), and this information is only made public if a user chooses for it to be so.
	

# Unresolved questions

The specifics for how to best represent and store data for pronouns in a way that is both simplistic and effective for MongoDB has not yet been confirmed.

Should access to pronouns be restricted to non bot accounts?
Should every user be able to see every other users pronouns or should this feature be restricted to those who are friended? 

# Security concerns

N/A

# Future ideas

For Revolt clients in the future, there may be options for how pronouns are displayed on each user and perhaps multiple choices for a more customisable UI.
