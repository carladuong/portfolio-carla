---
title: Assignment 5
layout: doc
---

# Assignment 5

## Heuristic Evaluation

### Usability Criteria
* *Efficiency*: I believe that a user would be able to efficiently accomplish their goals once they know how to use the app. The navigation bar automatically stores the communities that a user has added themselves to, so they can easily find them once they become a member. Also, the chat page stores the user’s recent chats with other users. However, this does not necessarily imply that the most used chats are stored closer to the top (e.g. a user could be sending one-time messages to many users in a community about an event). In spite of this, I will keep my original design, as I believe that ordering chats from most recently used to least recently used is consistent with the convention established by other messaging apps and is therefore more intuitive.
* *Error tolerance*: If a user accidentally creates a post, my design is not very tolerant, as I have not included a way to delete it in my wireframes, since post components consist of only the post itself and no ways to interact with it. I might add a menu button to each post where a user can opt to delete it. I could have a delete button directly on the post box (outside of a menu), which would be more efficient and discoverable, but it would introduce another weak point in terms of error tolerance. If a user accidentally hits the “delete post” button, there won’t be a way to recover it. Although this is still true if the delete option were inside of a menu, the user would have to deliberately look for the option inside of the menu, which makes it less likely for it to be a mistake.

### Physical Heuristics
* *Fitt’s Law*: Given that the navigation bar is always docked on the far left side of the screen, users can very easily reach the different pages linked on it. However, I haven’t used the top bar for anything except for the app name and the profile, which can both be accessed via the side navigation bar. To further take advantage of Fitt’s Law, I could reserve the side navigation bar for the user’s communities and move all of the other links to the top bar where they can still be easily accessed.
* *Situational context*: In my wireframes, the user can’t always easily tell what page they are on just by looking at the website. For example, the chat page displaying the user’s recent chats has no indication that that is what the user is looking at. To fix this, I can bold or underline the name of the page that the user is on in the navigation bar, changing the highlighted item as the user switches screens. 

### Linguistic Level
* *Speak a user’s language*: In its current state, my app is lacking user-facing error messages, which could make it difficult for the user to know how to react when something goes wrong. When an invalid input is given, I would like my app to display a message informing the user of the problem. For example, if an empty input is given for a username, password, or post, the message will say something like “Please provide a value for [relevant field].”
* *Consistency*: Although they take the user to the same page, the profile button on the navigation bar and the profile button in the top right have different icons. Using the fix I proposed in the Fitt’s Law observation, I will likely remove the profile link that was previously on the navigation bar to remove this ambiguity and to avoid having two links to the same page next to each other. It is possible that removing the profile page from the group of icons in the top bar could make it less discoverable, but since many social media apps allow a user to access their profile by clicking a profile picture in the top right corner, I believe that this is still an intuitive flow.

## Visual Design Study
![Font visual design study](/../assets/images/font_visual_study.png)
![Color visual design study](/../assets/images/color_visual_study.png)

