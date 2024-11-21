---
title: Project Phase 3
layout: doc
---

# Project Phase 3: Convergent Design
November 20, 2024. By Carla, Adriana, Arzy, Arli

## Functional Design

### Concepts:

#### Listing \[Author\]

  - **Purpose:** users can list items that don’t need and want to get rid of

  - **Operational principle:** When a user posts a listing, it becomes available for others 

  - **State:**

    * listings: set Listing  
    * name: listings \-\> one string  
    * author: listings \-\> one Author  
    * meetup\_location: listings \-\> one string  
    * image: listings \-\> one file  
    * quantity: listings \-\> one number  
    * remaining: listings \-\> one number  
    * hide: listings \-\> one Boolean  
- **Actions:**
    * *add*( author: Author, name: string, quantity: integer,  meetup_location: string, img: file)   
        * adds a new listing containing an author, the name of the item, the quantity of the given item, the meetup location, and an associated image is added to listings; hide is set to false and remaining is set to quantity  
    * *delete*(listing: Listing)  
      * listing gets removed from listings  
    * *edit*(listing: Listing, name: string, quantity: integer, img: file)  
      * listing in listings is edited (either new name, quantity, or image)  
    * *getAllListings*()  
      * returns all items in listings  
    * *getListingsOfAuthor*(author: Author)  
      * returns all listings where author \== author  
    * *getRemainingQuantity*(listing: Listing)  
      * returns remaining of listing  
    * *updateRemainingQuantity*(subtract: number)  
      * update remaining \= remaining \- subtract
      * if quantity == 0 set hide to true

			

#### Claiming \[Item, Claimer\]

  - **Purpose:** users can claim an item they are interested in

  - **Operational principle:** if the user claims an item, it becomes unavailable to other users

  - **State:** 

    * claims: set Claim  
    * claimer: claims \-\> one Claimer  
    * quantity: claims \-\> one number  
    * item: claims \-\> one Item  
- **Actions:**  
    * *claim*(claimer: Claimer, item: Item, quantity: number)  
      * a new claim is added to claims with the claimer, item name and quantity  
    * d*eleleAllClaimsOnItem*(item: Item)  
      * all claims where item \== item are removed from claims  
    * *unclaim*(claim: Claim)  
      * claim gets removed from claims  
    * *checkIfClaimed*(item: Item)   
      * returns true if item is in claims  


#### Requesting \[Requester\] 

- **Purpose:** users can request items they need

- **Operational principle:** if the user requests something, other users can see the request

- **State:** 
    * requests: set Request  
    * requester: requests \-\> one Requester  
    * quantity: requests \-\> one number  
    * image: requests \-\> one file  
    * hide: listings \-\> one Boolean  

- **Actions:** 
    * *add*(author: Requester, Item: string, Qt: integer)  
        * a request is added to requests containing the requester, a given item, its quantity and (opt) its image  
        * hide is set to false  
    * *delete*(request: Request)  
        * request is removed from requests  
    * *edit*:(request: Request, Qt: integer)  
        * request’s quantity if updated to Qt  


#### Offering \[Item, Offerer\]

- **Purpose:** users can offer an item that another user needs.

- **Operational Principle:** if they offer the item needed the user can 

- **State:**
    * offers: set Offer  
    * offerer: offers \-\> one Offerer  
    * accepted: offers \-\> one Boolean  

- **Actions:** 
    * offer( offerer: Offerer, item: Item, img: file, location: string, optional message: string)  
      * offer containing offerer, item, img, location and (opt) message) is added to offers  
      * accepted is set to false  
    * accept(offer: Offer)   
      * offer’s accepted property is set to true  
    * editOffer(offer: Offer, opt img: file, opt message: string)  
      * offer’s image or message is set to new value  
    * removeOffer(offer: Offer)  
      * offer in the list of offers is removed   
    * removeOffersOfItem(item: Item)  
      * all offers whose item==item are removed from offers  

#### Expiring \[Item\] 
(stolen from class slides 🙂)

- **Purpose:** handle expiration of short-lived resources/items

- **Operational principle:** when an item is allocated an expiration date, it expires at that time

- **State:** 
    * active: set Item  
    * expiry: active \-\> one Date  
- **Actions:** 
    * **allocate**(rsc: Item, time: int)  
      * rsrc not in active  
      * active \+- rscr  
      * rsrc.expiry := time sec after now  
    * **editExpiration**(rsc, Item, time: int)  
    * system **expire**(out rsc: Item)  
      * rsrc is active: rsrc.expiry is before now  
      * active \-= rsrc: rsrc.expiry:=none  

#### Labeling \[Item\]

- **Purpose:** users can add labels to items

- **Operational principle:** when a user lables an item, it can be retrieved with the given label

- **State:** 
    * labels: set Label  
    * name: labels \-\> one string  
    * labeledItems: labels \-\> set Item  

- **Actions:**
    * *create*(name: string)  
      * label is created and added to the list of labels  
    * *labelItem*(item: Item, label: Label)  
      * item is added to the in labels   
    * *getItemsWithLabel*(label: Label)  
      * finds label in labels, then returns labeledItems for that label  
      
#### Reviewing \[Entity\]

- **Purpose:** entities can review other entities

- **Operational principle:** after an entity gives a rating to another entity, it can be seen by other entities

- **State:**
    * reviews: set Review  
    * text: reviews \-\> one string  
    * subject: reviews \-\> one Entity  
    * author: reviews \-\> one Entity  
    * rating: reviews-\> one integer  

- **Actions:**  
    * *addReview*(subject: Entity, author: Entity, rating: int, optional text: string)  
      * Add a Review to reviews with subject, author, and text, and 1-5 rating.   
    * *delete*(review: Review)  
      * Remove review from reviews  
    * *edit*(review: Review, optional text:string, optional rating: int)  
      * Find review in reviews and edit its text or rating  
    * *getAverageRating*(subject: Entity)  
      * find all of the reviews in reviews with subject as the subject, get their ratings, and calculate the average from these.  

#### Reporting \[Entity\]

- **Purpose:** entities can report other entities for misbehavior

- **Operational principle:** when an entity is reported by at least two other entities, they are marked as reported

- **State:** 
    * reports: set Reports   
    * reporter: reports-\> one Entity  
    * subject: reports \-\> one Entity  
    * reported: set Entity  

- **Actions:**   
    * *report*(reporter: Entity, subject: Entity)  
      * adds a new Report to reports, with reporter and subject. If getNumberOfReports(subject) \>= 2 and subject isn’t already in reported, add subject to reported.  
    * *checkIfUserReported*(subject: Entity)  
      * returns true if subject is in reported, false otherwise  
    * *getNumberOfReports*(subject: Entity)  
      * gets the Reports from reports where subject == subject and counts them


**Nice to have:** Bookmarking, Mapping, Tracking (if the item was received/given)

### Concept Diagram
![](/../assets/images/project-images/concept-diagram.png)


### Synchronizations
```javascript 
Authenticating
Listing[User]
Requesting[User]
Claiming[Listing, User]
Offering[Request, User]
Labeling[Listing]
Reporting[User]
Reviewing[User]
Expiring[Listing]
Expiring[Request]


sync listItem(author: user, name: string, qt: integer,  location: string, img: file, labels: set Label, expirationDate: Date){
	listing = Listing.add(author, name, qt, location, img)
    labels.forEach(label => (){
        Labeling.labelItem(item, label)
    })
	Expiring.allocate(listing, expirationDate)
}

sync deleteListing(item: Listing) {
      Listing.delete(item)
      Claiming.delete(item)
}

//add system action that deletes expired listings and requests 

sync editListing(listing: Listing, name: string, qt: number, IMG: file, exp: Date) {
      Listing.edit(listing, name, qt, IMG)
      Expiring.editExpiration(listing, exp)
}

sync getAllListings() {
      Listing.getAllListings()
}

sync getListingsOfAuthor(author: User) {
      Listing.getListingsOfAuthor(author)
}

sync claimItem(claimer: User, item: Listing, quantity: number){
	Reporting.checkIfUserReported(claimer)
	Claiming.claim(claimer, item, quantity)
      Listing.updateRemainingQuantity(quantity)
}

sync unclaimItem(item: Listing) {
      Claiming.unclaim(item)
}

sync request(author: User, item: string, qt: number, deadline: Date){
	request = Requesting.add(author, item, qt)
	Expiring.allocate(request, deadline)
}

sync deleteRequest(req: Request) {
     Requesting.delete(req)
     Offering.deleteOffersOfItem(req)
}

sync editRequest(req: Request, item: string, qt: number, deadline: Date) {
      Requesting.edit(req, item, qt)
      Expiring.editExpiration(req, deadline)
}

sync offer(offerer: User, item: Request, IMG: file, location: string, message: string)
	Offering.offer(offerer, item, IMG, location, message)


sync acceptOffer(item: string, offer: OfferObject) {
      Offering.accept(offer)
      Requesting.delete(item)
}

sync editOffer(offer: Offer, item: string, IMG: file, message: string) {
      Offering.edit(offer, item, IMG, message)
}

sync removeOffer(offer: OfferObject) {
      Offering.checkIfAuthor(user, offer)
      Offering.remove(offer)
}


Sync review(author: user, item: string, rating: int, message) {	 
     Claiming.checkIfClaimed(item)
     Reviewing.addReview(rating, Message)
       
Sync report(reporter: user, ){
      Reporting.checkIfUserReported(reporter, user)
      Reporting.report(reporter: string, reported: string, reasoning:string)
}      

Sync addReview(subject: User, author: User, rating: number, optional message: string) {
     Reviewing.addReview(subject, author, rating, message)
}

Synch deleteReview(review: Review, author: User){
     Reviewing.delete(review)
}

Sync editReview(review: Review, optional message: string, optional rating: int) {
      Reviewing.edit(review, message, rating)
}

Sync getAverageRating(subject: User) {
      Reviewing.getAverageRating(subject)
}

```

## Wireframes

App flows can be accessed in our [Figma project](https://www.figma.com/design/FaBJNUYngwMU4bETc9Va7Z/Replate?node-id=0-1&t=uDOAmpZVozMpb7MN-1).

![](/../assets/images/project-images/wireframes.png)

## Heuristic Evaluation

#### Usability Criteria
* **Efficiency:** once you know how to use an interface, can you use it to quickly and efficiently accomplish your goals?  
  *  We use a navigation menu at the top of our website that clearly outlines and makes it easy to find the main features like listing and requesting.  
  *  Add a back button because in some cases it has taken users several steps to get to a certain posting or section. For example, right now you can only navigate to the main page through the logo and it takes you to the “All” feed and refreshing. This could help the user not have to scroll or try to remember the title of an item that caught their attention at one point.  
* **Error tolerance:** how easily can a user recover from making mistakes?  
  * Throughout our platform, we need the user’s input to have content, for instance while they are in the process of posting or requesting something their content only gets posted when they click submit.   
  * On the other hand if they have made a mistake in a Listing they can go and edit and update it or completely remove it from the platform.  
  * It would be helpful to have a drafts section where the users can save their progress of making a post, this may come in handy when a user plans to request an item but the user needs to first get home and check if they actually need that product.

#### Physical level
* **Gestalt principles:** 
  * The feeds separated between listed and requested items helps users understand that there are two types of actions and roles that they can partake in, giving and receiving.   
  * The organization of posts based on expiry date (closest expiry dates first) makes users focus more on making the most out of available resources.  
* **Situational context:** 
  * To help the user understand where they are scrolling and what products they are seeing we keep our feed filtering options displayed and highlight the one where the user is scrolling. This can be observed in our home page where the landing option shows all products and it marks the “All” products button. We similarly remind them about the page where they’re at through titles, in this case, we have two similar post creation screens “List” and “Request”, where users can clearly understand based on the title.   
  * It would be helpful to have the menu icons highlighted when the user is in that page of the website to give them a better sense of location and learn where to find certain features in the app.

#### Linguistic level
* **Consistency:** 
  * The app uses the same familiar symbols that other apps use. For example, the plus sign opens a menu to add a type of post (listing or request), the bell represents notifications, and the circle with the person redirects to the profile page.   
  * For the most part, these symbols only appear once (they all only appear in the header), so there is little risk of confusion. One exception is that the plus sign is also used to add pictures when a user is listing an item. While something is being added each time, perhaps there could be a different way to communicate this for one of these cases to avoid confusion.  
  * Request and list are app-specific concepts that reappear across the app  
* **Speak a user’s language:** 
  * Verbs are used for actions, such as the “more” menu on a user profile, which shows a screen that lists the actions “share,” “report,” and “cancel”  
  * On the home page, user can select one of the categories at the top of the page (nouns) to query for only listings that belong to that category  
  * There is an opportunity to add simple, informative messages when an error is encountered in the final app. For example, if a user claims a quantity of an item on a listing that exceeds what is available, they can be shown an error message informing them that that was the problem.


  ## Design Iteration
  As we integrated feedback and continued implementing our app, we idetified several issues:

    1. **Meeting Locations:** Suggesting meeting locations for each transaction proved complex, so we decided to let the user listing the item choose the location. While this improves efficiency, it can create accessibility issues if the requester cannot reach the chosen spot. This also introduces reliance on trust between users, similar to challenges seen in Facebook Marketplace.  
    2. **Splitting Post Types:** We separated Posting into Requesting and Listing to reflect their distinct storage needs and associated actions, as well as distinct response actions such as Claiming and Offering, respectively.  
    3. **Post Handling:** When a listing or request is claimed or fulfilled, it is not deleted but hidden from the browsing page. Users can access these posts when looking at their transaction history, or when opening another user’s profile to see what they have sold in the past and how others reviewed these transactions. This is similar to what Depop does because they also focus on one-time listings as opposed to products that can be bought over again. To implement that, we use hide boolean in the state of Lisitng and Requesting concepts.  
    4. **Category Navigation:** Inspired by grocery delivery apps, we decided to have both vertical scrolling between categories and horizontal within a category. However, after wireframe testing, we replaced horizontal scrolling within categories with a filter menu on the desktop for better accessibility and ease of use. Yet, horizontal scrolling remains on mobile for convenience and reduced clicking.  
    5. **Exchange Tracking (Future Iterations):** We need a way to track when exchanges are completed. A potential solution is a confirmation feature where the poster marks the exchange as finalized, similar to Uber's start/end ride functionality. This needs to be done on both sides to ensure an equal distribution of power.  
    6. **Time Estimation (Future Iterations):** Time estimation for reaching meetup locations could enhance user planning before committing to the transaction. We may implement this through our mapping concept or external tools like Google Maps.

## Visual Design Study

### Color Study
![](/../assets/images/project-images/color_study_inspo_map.png)
![](/../assets/images/project-images/color_study_app.png)


### Typography Study
![](/../assets/images/project-images/typography_study.png)


## Planning Implementation
| Task | Build/ Deadline | Status  | Assigned  | Comments |
| :---- | :---- | :---- | :---- | :---- |
| Setting up repo, and github (Vue/Vercel) | Thu 11/21 | Not started | Carla |  |
| Authing backend | Fri 11/22 | Not started | Carla |  |
| Listing backend | Sun 11/24 | Not started | Adriana |  |
| Requesting backend | Sun 11/24 | Not started | Arzy |  |
| Listing View Page \+ Edit Page | Sun 11/24 | Not started | Arli | if user=author: edit button, else just view; if editing: EditListingComponent |
| Listing Form Page  | Tue 11/26 | Not started | Arli  |  |
| Requesting View Page \+ Edit Page | Tue 11/26 | Not started | Arzy | if user=author: edit button, else just view; if editing: EditRequestComponent |
| Requesting Form page | Tue 11/26 | Not started | Carla |  |
| Claiming backend | Tue 11/26 | Not started | Carla |  |
| Offering backend | Tue 11/26 | Not started | Carla |  |
| Expiring backend | Tue 11/26 | Not started | Arzy |  |
| Tagging backend | Tue 11/26 | Not started | Arli  |  |
| Reviewing backend | Tue 11/26 | Not started | Arzy |  |
| Reporting backend | Tue 11/26 | Not started | Arli  |  |
| Home page | Tue 11/26 | Not started | Arzy | \+ filter |
| Profile page | Tue 12/3 | Not started | Arli | if user=me: MyProfileComponent |
| Claim Form page | Tue 12/3 | Not started | Arli |  |
| Offer Form page | Tue 12/3 | Not started | Adriana |  |
| Offer View page  | Tue 12/3 | Not started | Adriana | if user=author: edit button elif user=requester: accept button |
| Reviewing page  | Tue 12/3 | Not started | Arzy |  |
| Reporting page | Tue 12/3 | Not started | Adriana |  |
| User testing plan | Thu 12/5 | Not started | Adriana |  |
|  |  |  |  |  |