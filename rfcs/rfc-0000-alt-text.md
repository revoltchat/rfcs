- Feature Name: Alt Text for Media
- Start Date: 23/02/2024
- RFC PR: <https://github.com/revoltchat/rfcs/pull/10>
- Tracking Issue: <https://github.com/orgs/revoltchat/discussions/664>
- Status: draft

# Summary
Alt text on media is important for enhancing the accessibility on Revolt and overall user experience with users that are blind or low-vision.

# Motivation
Adding the ability to let users add alt text to media will greatly improve user experience, mainly targeting users that are blind and low-vision. The default alt text on Revolt is the media's file name (e.g. `image.png`), which is problematic, because it doesn't describe the media properly, and is highly discouraged.

# Guide-level explanation
<!--
Explain the proposal as if it's already in Revolt and you were teaching it to new users.

- Introduce new concepts
- Explain the feature with examples
- What this fixes or adds and what users should think of the feature
- Discuss how this impacts using Revolt, how it makes it harder or easier to use

For internal oriented RFCs such as internal code changes, this should largely talk about how contributors should think about the change and give examples on the impacts.
-->

Alt texts are a way to make your media accessible to more users on Revolt.

# Reference-level explanation
<!--
This is the technical section of the RFC, it should go over in detail:

- Its interaction with other features
- How this will be implemented
- Corner or edge cases

This section should reference the examples in the previous section and disect them in more detail.
-->

# Drawbacks
<!-- Why should this not be added. -->

# Rationale and alternatives
<!--
- Why is this design the best
- Are there alternative ways to solve this
- Could this be done with existing features or existing solutions
-->

# Prior art
<!--
This should include both good and bad outlooks on the proposal. This could include how other platforms, software and hardware solve similar issues if relevent or how any existing proposals have tried to solve the same problem.
-->

A few alt text systems have been created on other websites before Revolt:

## Sharkey
Sharkey has an alt text system labelled as "captions." To access the captions modal, you would need to click on the image, then click on "Add caption." Doing this will display a modal with the image on top, then a text field for entering alt text.

![Screenshot from 2024-03-24 11-28-09](https://github.com/theycallhermax/revolt-rfcs/assets/67456566/3e2e0b72-e505-4f1a-90e4-a95039b51546)

If you have not set an alt text on the image, Sharkey will display an confirmation modal, reminding you that you haven't set an alt text. This functionality can be enabled with the "Warn you when you forget to put alt text" setting.

![Screenshot from 2024-03-24 11-34-33](https://github.com/theycallhermax/revolt-rfcs/assets/67456566/eca1f301-4e71-45d4-a91d-73f6835a18a4)

When the image is expanded, the alt text will show below the image inside a small box.

![Screenshot from 2024-03-24 11-43-38](https://github.com/theycallhermax/revolt-rfcs/assets/67456566/0c8caba0-1260-499e-93ba-f97399033c10)

# Unresolved questions
<!--
- Are there any parts which are not yet designed or you believe need further discussion?
- Do you expect any part of this proposal to change?
- Are there any related issues which you believe are out of the scope of this RFC that could be addressed in a seperate RFC?
-->

# Security concerns
<!--
How does this RFC impact security - This section might not always be applicable and if you believe it is not, please write your reasoning in this section.
-->

# Future ideas
<!--
Are there any features or changes that this proposal could enable? How does this proposal impact the future of Revolt?
-->
