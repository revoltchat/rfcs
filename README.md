# rfcs
RFCs for changes to Revolt. This is a process for adding or changing anything in revolt, in a formalised and clear maner, so that everyone understands clearly what is needed.

## When you need an rfc

You must create an RFC if you intend to make a substantial change to Revolt, or any part of Revolt, this includes:

- Any new features
- Changes to existing ones which have a large impact
- Removing features

What doesn't need an RFC:
- Rewording, reorgnizing or refactoring
- Additions which improve in ways which do not add new features - speedups, bug fixes, better user experience in regards to errors
- Additions which do not affect users of revolt but only the developers of revolt.

If you attempt to make a pull request which would require an RFC without an RFC you will be turned down and be asked to submit an RFC before the PR is looked at.

If you are able to, you are allowed to make an implementation for a draft or still in progress RFC if applicable but, if the RFC is denied your pull request will be denied as well.

## Before creating an rfc

It is good to discuss what you want to make an RFC on first before creating an rfc, this means if the outlook on the proposal is bad then the time will not be wasted creating the rfc, you can discuss in issues or discussions in the Revoltchat Github organisation or on Revolt.

## Creating an rfc

To get a feature added to revolt, you must get your RFC merged into this repository first, at this point the RFC is "active" and can be discussed.

- [Fork the RFC Repo](https://github.com/revoltchat/rfcs/fork)
- Copy the `rfc-0000-template.md` into `rfcs/rfc-0000-name-of-feature.md`, dont give it a number yet, this will be assignee once the RFC is made.
- Fill in the RFC. Make sure to include detail, any ambiguties will slow down the RFC being accepted and will likely need to be corrected.
- Submit the RFC, make sure to name the PR to the RFCs name or something similar which conveys a similar message.
- Now that the PR is made you can update the RFC number to the pull request number
- The RFC will be labelled by the team with the relevent labels.
- The RFC will be discussed and commented on with queries or requests for changes by the team and community members, you can make edits in-line with what has been said, do not squash or edit existing commits as that can lead to confusion.
- Once the RFC is at a final stage, a member of the team will assign the RFC the Final Comment Period label, at this point the RFC should not be edited, this stage will last 10 days and after that the team is in a position where the final disposition can be decided (merged, closed or postphoned), this outcome does not need to the concensus of all participants of the discussion, however most if not all opinions will be taken into account. During the 10 days any final comments can be made to lodge any objections before a decision is made.
- During the Final Comment Period if any new substantial arguments or ideas are raised, the Final Comment Period can be cancelled and it can go back into development if the author or the team agrees.

## Active rfc life-cycle

Once the RFC is merged it becomes "active", you can still make updates to the RFC via pull requests however the workflow above must be followed.

Now that it is active a developer can be assigned to implement the RFC, the author does not need to implement it themself, however it is the most effective way for the RFC to be implemented. Furthermore, the fact that the RFC is active does not mean it it garenteed to be implemented or accepted, if after this stage it is found that RFC is no longer deemed nessessary or a hole is found in it, it can be still denied and closed.

## Implementing the rfc

Depending on the RFC an implemention must be made, A tracking issue can be made to track the status of the implemention and link to the currently open pull requests (if any), this can also serve as a discussion page where the implemention or queries about the RFC can be made. If you would like to implement the RFC comment on the tracking issue where a member of the team can assign you to the tracking issue, if you are unsure on if there is already someone working on the implemention, fele free to comment on the comment

## RFC postponement

RFCs can be postphoned (they will be labelled with the "Postphoned" label), this can be for varies issues, if they are not being currently worked on, or are blocked by other RFCs and will take a while for it to be unblocked.
