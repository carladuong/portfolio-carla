---
title: Assignment 4
layout: doc
---

# Assignment 4

## Data Modeling
**Name**: Posting [Author] \
**State**: 
* posts: set Post
* author: posts -> one Author
* content: posts -> one string


**Name**: Labeling [Item] \
**State**:
* labels: set Label
* labelName: labels -> one string
* items: labels -> set Item


**Name**: Messaging [Entity] \
**State**: 
* messages: set Message
* from: messages -> one Entity
* to: messages -> one Entity
* content: messages -> one string
* chats: set Chat
* chatters: chats -> two Entity


**Name**: Commenting [Author, Item] \
**State**: 
* comments: set Comment
* parent: comments -> one Item
* content: comments -> one string
* author: comments -> one Author


**Name**: Authenticating \
**State**:
* registered: set User
* username: registered -> one string
* password: registered -> one string


**Name**: Sessioning [Entity] \
**State**:
* active: set Session
* entity: active -> one Entity


**Name**: Matching [Entity] \
**State**:
* matches: set Match
* entities: matches -> two Entity
* matchable: set Entity


app **Ginger** \
include Posting [User] \
include Labeling [Post] \
include Labeling [User] \
include Labeling [Label] \
include Messaging [User] \
include Commenting [User, Post] \
include Commenting [User, Comment] \
include Authenticating \
include Sessiong [User] \
include Matching [User] \


![Data Diagram](/../assets/images/data_diagram.png)


## Code and Deployment
The GitHub repo with my backend code can be found [here](https://github.com/carladuong/ginger), and the deployment can be found [here](ginger-kappa.vercel.app). If the deployment link doesn't work, please try going to this website: ginger-kappa.vercel.app


## Design Reflection
While implementing my backend service, I realized that the abstract data models that I had originally designed for some of my concepts were too complex or infeasible in practice. For example, my original Messaging concept involved a type called Chat, which would map to a set of objects of type Message. These nested types were clunky to implement in practice, and because of this I had to simplify my abstract data model so that the Messaging concept was responsible for a set of Chats and a separate set of Messages. 

Another example was my Matching concept. I originally planned to implement the matching behavior as an action within the Matching concept, but because users are matched based on the communities that they are a part of (i.e. the Labels they assign themselves), I realized that the matching behavior would be more feasible to implement if I could use methods that belonged to the Labeling concept. Because of this, I simplified the Matching concept so that it just handled the pairing of two entities, and I synchronized Matching and Labeling to implement the matching behavior.

Perhaps the most drastic change I made was removing the Grouping concept that I had originally planned for. While I was implementing it, I realized that the code for Grouping and Labeling were very similar. Because of this, I ended up removing the Grouping concept and absorbing the functionality into the Labeling concept. Though this did save a lot of repetitive code, I might have kept Grouping as a separate concept if I had more time for a project with a larger scope, as I believe it is more intuitive to think about associating a set of users this way, and it would be easier to store data such as traits of the group’s users.
