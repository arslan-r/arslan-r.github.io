---
layout: essay
type: essay
title: How to use meter-template
# All dates must be YYYY-MM-DD format!
date: 2020-08-15
labels:
  - Software Engineering
  - Career
  - Concepts
---

  Here are some basic functions of the template to get you up and running. We will get through how to add a page, access it, request data fromt he user, add it to the database, and then retrieve it.
  
  
  TO DISPLAY DATA
To list data, you need to gather data from the user through a form.
We will create a page that asks the user for certain data in a certain format and store it in a database. 
We will format the data through "Schemas"
Example can be found on IMPORTS > UI > PAGES > ADD PAGE
Its way easier to copy paste code and modify as needed



## LETS ADD A SINGLE PAGE:
Its generally a good idea to add the page + button to your site, even if its empty or if it links to a copy of another page.

Lets add a new page in the PAGES directory.
Create a new file. I named my file AddProfile.jsx.
Copy the contents of AddStuff.jsx, and paste it into the new file. 
At the bottom of AddProfile.jsx, change the `export default AddStuff;` to `export default AddProfile;`

Next we will give this new page a url
Go to to App.jsx file in UI > Layouts > App.jsx
At the top of App.jsx , import your new page
`import AddProfile from '../pages/AddProfile';`

- copy `<ProtectedRoute path="/add" component={AddStuff}/>` and paste it again right underneath it so it looks like this
```
<Route exact path="/" component={Landing}/>
              <Route path="/signin" component={Signin}/>
              <Route path="/signup" component={Signup}/>
              <ProtectedRoute path="/list" component={ListStuff}/>
              <ProtectedRoute path="/add" component={AddStuff}/>
              <ProtectedRoute path="/add" component={AddStuff}/>
              <ProtectedRoute path="/edit/:_id" component={EditStuff}/>
              <AdminProtectedRoute path="/admin" component={ListStuffAdmin}/>
              <ProtectedRoute path="/signout" component={Signout}/>
```
edit the replica so the app routes traffic to your new page
`<ProtectedRoute path="/AddProfile" component={AddProfile}/>`



NEXT: Lets create a button in the top nav bar to access our new page
Go to IMPORTS > COMPONNENTS > NAVBAR. This is the navbar that holds the buttons at the top of the page
- To add a button, make a copy of the existing buttons that are labaled "Add Stuff" or "List Stuff". Link it to your new AddProfile page
Your new entry should look like
`<Menu.Item as={NavLink} activeClassName="active" exact to="/addProfile" key='addProfile'>Add Profile</Menu.Item>,`


Check to make sure that it shows up on the website, and leads to the exact same page as the Add Stuff button




## ADDING A NEW LIST PAGE, where we display something from a database! (eventually)
With a MongoDB data base, info is stored in "Collections"
Lets make a new page int he PAGES directory, and make it identical to "ListStuff.jsx"
Ill name it ListProfile.jsx, because we will list user data eventually
Rename the class, the .proptypes, and the export at the very bottom of the page to the class name
```
class ListProfile extends React.Component {
...
...
ListProfile.propTypes = {
...
...
})(ListProfile);
```

Then we add to the App.jsx route and make a button for it. It should display a page identical to the List List Stuff, because its pulling data from the same collection. Refer to how to do that up top! Double check to make sure its working



Great! 
Now, lets make a new collection (its where mongodb stores data!) and add things to it. 
Lets go to IMPORTS > API > STUFF and create a new page. Ill name my page "Profile.js"
Copy paste everything from "Stuff.js" into it
Change the class, collection name, and the export so it reflects "Profile", like so
```
class ProfilesCollection {
...
...
this.name = 'ProfilesCollection';
...
...
export const Stuffs = new ProfilesCollection();
```

For simplicity purpose, it will take in the exact same parameters as the original StuffsCollection. We will change this later



NEXT: Go to  IMPORTS > STARTUP > SERVER > Publications.js 
We will add function here that will return things from the Profiles collection that are associated with the logged in user
First, import your collection at the top of the page
```
import { Profiles } from '../../api/stuff/Profile';
```

Right under the `Meteor.publish(Stuffs.userPublicationName, function () {` function, 
make another copy of it, so it looks like this
```
Meteor.publish(Stuffs.userPublicationName, function () {
  if (this.userId) {
    const username = Meteor.users.findOne(this.userId).username;
    return Stuffs.collection.find({ owner: username });
  }
  return this.ready();
});

Meteor.publish(Stuffs.userPublicationName, function () {
  if (this.userId) {
    const username = Meteor.users.findOne(this.userId).username;
    return Stuffs.collection.find({ owner: username });
  }
  return this.ready();
});
```


Edit the new function so it publishes things from your new Profiles collection, like so
```
Meteor.publish(Profiles.userPublicationName, function () {
  if (this.userId) {
    const username = Meteor.users.findOne(this.userId).username;
    return Profiles.collection.find({ owner: username });
  }
  return this.ready();
});
```



## Change the code so it saves to your new collection
Go to AddProfile.jsx
import your profiles collection at the top of the page
```
import { Profiles } from '../../api/stuff/Profile';
```

In the "Submit" section, we want to change from submitting to the `Stuffs.collection.insert({ name, quantity, condition, owner },`
to submitting to the Profiles collection. `Profiles.collection.insert({ name, quantity, condition, owner },`


Lets check our work. If you go to the "Add Profile" page of your website and create a submission, you should get an alert saying stuff was added,
but it would not show up on the List Profiles page beacause we havent linked it yet

Lets do that now


## Showing your profile data. 
This is a two part thing. PART 1.

Lets create a component for displaying user data. 
Imagine this whole thing as a function definition. We will pass in a user into this function, and get their name, and other stuff from it. In this case, we will put their data in a table

The `this.props.stuff.name` will take whatever is passed into this function "stuff", and try to access the ".name" property of it


Go to IMPORTS > UI > COMPONENTS
Make a new file. I called my file "ProfileData.jsx"
Copy everything from StuffItem.jsx into ProfileData.jsx
Change the class, .proptypes, and export to reflect the page name
```
class ProfileData extends React.Component {
...
...
ProfileData.propTypes = {
...
...
export default withRouter(ProfileData);
```


We will later be using a map function. Map is kinda like a super smart loop, it takes an array of elements and a function, and then performs that function on every element of that array. In our case, lets say the user makes a bunch of profiles (we shouldnt allow them this, but for the sake of this example we will!). We will pass this array of profiles to the map function, and the function will store each profile into a row of a table.


PART 2
Now that we desiganted how we want the table displayed,
go to ListProfile.jsx in IMPORTS > UI > PAGES

lets import our profiles collection at the top
```
import { Profiles } from '../../api/stuff/Profile';
```
and edit the export function to subscribe to your Profiles collection
It should look like this
```
export default withTracker(() => {
  // Get access to Stuff documents.
  const subscription = Meteor.subscribe(Profiles.userPublicationName);
  return {
    stuffs: Profiles.collection.find({}).fetch(),
    ready: subscription.ready(),
  };
})(ListProfile);

```

Edit the table body element so it accesses your ProfileData
Change it from
```
{this.props.stuffs.map((stuff) => <StuffItem key={stuff._id} stuff={stuff} />)}
```
to
```
{this.props.stuffs.map((stuff) => <ProfileData key={stuff._id} stuff={stuff} />)}
```

The List Profile page on your site should list your new profile!

