# RFCs
RFCs for changes to Revolt. This is a process for adding or changing anything in Revolt, in a formalised and clear manner, so that everyone understands clearly what is needed.

## When you need an RFC

You must create an RFC if you intend to make a substantial change to Revolt, or any part of Revolt. This includes:

- Any new features
- Changes to existing ones which have a large impact
- Removing features

Some things don't need an RFC - these include:
- Rewording, reorganising or refactoring text or code
- Additions which improve the app  in ways which do not add new features  (e.g. performance improvements, bug fixes, better user experience in regards to errors)
- Additions which do not affect users of Revolt but only the developers of Revolt

If you attempt to open a pull request which would require an RFC without opening an RFC first, your pull request will be closed and you will be asked to submit an RFC before the pull request can be looked at.

If applicable, you're allowed to start working on an implementation for a draft or in-progress RFC - however, if the RFC is denied, your pull request will be denied as well.

## Before creating an RFC

Before submitting an RFC, it's a good idea to discuss your idea first - if your idea is declined, you won't have wasted any time writing an RFC. You can discuss in issues or discussions in Revolt's GitHub organisation or, preferably, [the Revolt API server](https://rvlt.gg/API).

## Creating an RFC

To get a feature added to Revolt, you must get your RFC merged into this repository first. At this point, the RFC is "active" and can be discussed.

- [Fork the RFC repo](https://github.com/revoltchat/rfcs/fork)
- Copy the `rfc-0000-template.md` file into `rfcs/rfc-0000-name-of-feature.md`. **Don't give it a number yet** - this will be assigned once the RFC is opened.
- Fill in the new RFC file. **Make sure to include as much detail as possible** - any ambiguties will slow down the RFC process and will likely need to be corrected.
- Submit the RFC. Make sure to give the PR a relevant name - use the RFC's name or something similar which conveys a similar message.
- Once the PR is open, you can update the RFC number to match the pull request's number - for example, if the pull request is `#36`, your RFC will be `0036`.
- The RFC will be labelled by the team.
- The RFC will then be discussed and commented on with queries or requests for changes by the team and community members. You can make inline edits to incoporate suggestions - **do not squash or edit existing commits**, as this can lead to confusion.
- Once the RFC is close to being finalised, a member of the team will assign the RFC the Final Comment Period label. At this point, the RFC **should not be edited**. This stage will last 10 days; once it ends, the team will be in a position where a final decision on the RFC can be made (i.e. whether to merge it, close it or postpone it). This outcome does not need to match the concensus of all participants of the discussion - however, most (if not all) opinions expressed in the RFC's discussion will be taken into account. During the Final Comment Period, any final comments can be made to lodge any objections before a decision is made.
- During the Final Comment Period, if any new substantial arguments or ideas are raised, the team or the RFC's author can cancel then Final Comment Period can be cancelled - the RFC will then go back into development.

## Active RFC lifecycle

Once the RFC is merged, it will become "active". You can still make updates to the RFC via pull requests; however, the workflow above must be followed.

Now that it is active, a developer can be assigned to implement the RFC. The author does **not** need to implement it themselves; however, this is the most effective way for the RFC to be implemented. Furthermore, the fact that the RFC is active does not mean it is guaranteed to be implemented or accepted. If, after this stage, it is found that the RFC is no longer deemed necessary or any major flaws are found in it, it can be still denied and closed.

## Implementing the RFC

Depending on the RFC, an implemention must be made. Once the RFC is merged and active, a tracking issue can be made to track the status of the implemention and link to any currently open pull requests. This can also serve as a discussion area where the implemention or queries about the RFC can be made. If you would like to implement the RFC, comment on the tracking issue so a member of the team can assign you to it. If you are unsure as to whether someone is already working on implementing the RFC, feel free to leave a comment.

## RFC postponement

RFCs can be postponed - if so, they will be labelled with the "postponed" label. This can be for various issues - for example, if the RFC is not being currently worked on, or if it is blocked by other RFCs that are not expected to be finished soon.
