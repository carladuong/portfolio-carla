---
title: Assignment 3
layout: doc
---

# Assignment 3

## Pitch
Ginger is an app where people with disabilities and mental illnesses can connect with and build communities with people who get it. The app is divided into community pages, and a user can join any number of community groups that they feel align with their identity. On a community page for a certain chronic condition, a user may interact with posts made by other users containing stories, advice, resources, and more by commenting and replying to existing dialogue. The user may also add their own posts to the community page, or cross-post in multiple communities about experiences with intersecting disabilities. Through chatting, users can also interact directly with other users. A user may also use the Buddy Matching feature, which will pair them with another user in the same community or communities.

On Ginger, people with disabilities and mental illnesses can find others who share similar experiences, which is often difficult in person-- especially for those who have rare conditions or intersecting disabilities, or for those who live in small towns. Ginger is necessary because everybody deserves to have people in their lives who truly understand them.

## Functional Design 

### Concepts
**Name**: Posting [Author] \
**Purpose**: users can post items for other users \
**Operational principle**: after making a post, that post is available to other users \
**State**: 
* posts: set Post
* author: posts -> one Author
* content: posts -> one string
* time: posts -> one string

**Actions**: \
*createPost* (a: Author , c: string, t: string, out post: Post) \
    post.author := a; \
 	post.content := c; \
 	post.time := t; \
    posts += post;

*deletePost* (post: Post) \
	post in posts; posts -= post

*editPost*(p: Post, c: string, t: time) \
    post in posts; post.content := c; post.time := t

\
**Name**: Labeling [Item] \
**Purpose**: organize items into categories \
**Operational principle**: after marking items with a label, those items will be filterable by that label \
**State**:
* items: set Item
* labels: items -> set string
**Actions**: \
*addItem* (item: Item) \
	item not in items; items += item

*affix* (item: Item, label: string) \
	item in items; label in existingLabels; item.labels += label

*remove* (item: Item, label: string) \
	item in items; label in item.labels; item.labels -= label

*findItemsByLabel* (label: string, out matching: set Item) \
    item in items \
        if label in item.labels \
            matching += item

*getLabels* (item: Item, out itemLabels: set string) \
	item in items; return item.labels


**Name**: Chatting [Entity] \
**Purpose**: allow users to chat with each other \
**Operational principle**: when a user sends a message to another user, that other user can view and respond to that message \
**State**: 
* chats: set Chat
* chatters: chats -> set Entity
* messages: chats -> set Message
* content: messages -> one string
* time: messages -> one string
* sender: messages -> one Entity 

**Actions**: \
*startChat* (chatters: set Entity) \
	newChat.chatters = chatters \
	if newChat not in chats \
		chats += newChat

*getChat* (chatters: set Entity, out matchingChat: Chat) \
	chat in chats \
		if chat.chatters == chatters \
			return chat 

*sendMessage* (chat: Chat, from: Entity, content: string, time: string) \
	chat in chats; chatter in chat.chatters \
	message.content = content \
	message.sender = from \
	message.time = string \
	chat.messages += message


**Name**: Commenting [Author, Item] \
**Purpose**: users can leave comments in response to items \
**Operational principle**: when a user leaves a comment on an item, another user viewing that item can also view the comment \
**State**: 
* items: set Item
* comments: items -> set Comment
* content: comments -> one String
* author: comments -> one Author
* time: comments -> one string
* replies: comments -> set Comment

**Actions**: \
*addComment* (p: Item, c: string, a: Author, t: string) \
	comment.content = c \ 
	comment.author = a \
	comment.time = t \
	p.comments += comment

*replyToComment* (com: Comment, c: string, a: Author, t: string) \
	reply.content = c \
    reply.author = a \
	reply.time = t \
	com.replies += reply

*deleteComment* (p: Item, com: Comment) \
	com in p.comments; p.comments -= com


**Name**: Grouping [Entity] \
**Purpose**: entities can join groups with other like-minded entities \
**Operational principle**: when an entity joins a group, they can interact with other similar entities \
**State**:
* groups: set Group
* name: groups -> one string 
* traits: groups -> set String
* members: groups -> set Entity

**Actions**: \
*createGroup* (n: string, c: Entity, traits: set string, out group: Group) \
	group in Groups \
		if group.name == n \
			throw error \
	newGroup.name = n \
	newGroup.members = c \
	newGroup.traits = traits \

*joinGroup* (u: Entity, g: Group) \
	u not in g.members; g.members += u

*leaveGroup* (u: Entity, g: Group) \
	u in g.members; g.members -= u


**Name**: Authenticating \
**Purpose**: authenticate users so that app users correspond to people \
**Operational principle**: after a user registers with a username and password pair, they can authenticate as that user by providing that pair \
**State**:
* registered: set User
* username: registered -> one string
* password: registered -> one string

**Actions**: \
*register* (u: string, p: string, out u: User) \
	existing in registered \
		if existing.username == u \
			throw error \
	user.username = u \
	user.password = p \
    registered += user

*authenticate* (u: string, p: string, out status: string) \
	existing in registered \
		if existing.username == u and existing.password == p \
			return ‘success’


**Name**: Matching [Entity, Trait] \
**Purpose**: match items with similar traits \
**Operational principle**: after looking for an object with similar traits to another object, the user will be presented with matching objects \
**State**:
* traits: set set Trait
* entity: traits -> zero or one entities

**Actions**: \
*addTraits* (t: set Trait, e: Entity | None) \
	traits += t \
	traits.t.entity = e

*findMatch* (t: set Trait, out matches: set Entity) \
	traitList in traits \
		if t is subset of traitList \
			matches += traitList.entity \
	throw error

### App-level Synchronizations

app **Ginger** \
include Posting [User] \
include Labeling [Post] \
include Labeling [User] \
include Chatting [User] \
include Commenting [User, Post] \
include Grouping [User] \
include Authenticating \
include Matching [User, string] \
include Matching [Community, string] \

*sync* register (username: string, password: string) \
	Authenticating.register (username, password, user) \
	Matching.addEntity (user) \
	Labeling.addItem (user)

*sync* createCommunity (name: string, creator: User, symptoms: set string, out com: Group) \
	Grouping.createGroup (name, creator, symptoms, out com) \
	Matching.addTraits (symptoms, com)

*sync* joinCommunity (user: User, com: Group) \
	Group.joinGroup (user, com) \
	Labeling.affix (user, com.name)

*sync* postToCommunity (author: User, content: string, time: string, com: Group) \
	Posting.createPost (author, content, time, post) \
    Labeling.affix (post, com.name)

*sync* crossPostToCommunities (author: User, content: string, time: string, coms: set Group) \
	Posting.createPost (author, content, time, post) \
	for com in coms \
        Labeling.affix (post, com.name)

*sync* addComment (post: Post, content: string, user: User, t: string) \
	addComment (post, content, user, t)

*sync* buddyMatch (toMatch: User, out matches: set User) \
	Labeling.getLabels (toMatch, conditions) \
    Matching.findMatch (conditions, out matches) \
    Chatting.startChat ([toMatch, matches[0]])

*sync* symptomSearch (symptoms: set string, out matches: set Group) \
	Matching.findMatch (symptoms, out matches)

*sync* startChat (chatters: set User) \
	Chatting.startChat (chatters)

*sync* sendMessage (chat: Chat, from: User, content: string, time: string) \
	Chatting.sendMessage (chat, from, content, string)

### Dependency Diagram
![Dependency Diagram](/../assets/images/dependency_diagram.png)

## Wireframes

https://www.figma.com/design/t7FoVfEjN8hIkl42vZf2sy/Ginger-Wireframe?node-id=0-1&t=xkGYN4IOZMrRdBdZ-1

## Design Tradeoffs

**Infinite Replies**
One problem I encountered was working with replies to comments. In the Commenting concept, a possible action is replying to a Comment with an object that is itself a Comment, and can therefore be replied to as well. The problem that this may cause is that there is no limit to the length of a chain of replies (i.e. a reply to a reply to a reply...), which could end up being difficult to store and hard to read. However, onr main goal of this app is to provide a space for dialogue, and I didn't want to limit that by making it impossible to reply to a reply. I also reasoned that long discussions in the replies are unlikely given that a chat feature exists.

**Labeling Users**
A potential ethical concern comes from "labeling" users with the communities they are part of (i.e. the disabilities or mental health conditions they identify with). These labels will appear on their profiles, which is something I could have chosen not to do. One problem with this is that it could be seen as a way to reduce someone to the condition(s) they have. However, in a space dedicated to users with disabilities or mental health conditions, I hope that this will not be a source of alienation, but a way for a user to find and connect with others who have similar experiences.

**Generalizing Matching**
In terms of functional design, one option I considered was generalizing the Matching concept so that it can also be used to match chat messages with content that the recipient does not want to see to implement my "Safe Chat" feature. I ultimately decided that this application of Matching isn't "user-facing" enough and makes the concept of Matching too nebulous, and that the Safe Chat could instead be implemented through filtering later on.




