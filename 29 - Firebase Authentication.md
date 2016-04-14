# 29 - Firebase Authentication

Up until now anyone's been able to add or edit any of the fish in any of the stores.  There's no authentication built in, and by default Firebase actually comes with no authentication turned on.  That's why there's the little exclamation mark right here, it's because it's easy to get up and running, however once you start having your app and you're going to be putting it online you need some sort of authentication to log in.

So how do you handle authentication when everything is done client side?  That's where Firebase is going to come in handy, because it allows you to handle all the authentication.  It allows you to handle the login with Github, Facebook or even a username and password with this.

So in this case the Firebase is actually sort of our back-end.

So what we want to do is when someone goes ahead and goes to any store, what we want to do is allow the first person to create that store to own it, and then whenever anyone goes to this store URL it's either going to show you the inventory if you're logged in properly or it's going to say 'sorry, you're not allowed to view this one because you are not the owner of this store'.

So you'd still be able to view the fish, you'd still be able to add them to your order, but the ability to manage your inventory is only via the store owner.

So if I go here to my data, you see here that I've got one of them right here, and in addition to the fishes, which we are comfortable with, we also now are going to have an `owner` property on each store, and that's going to be what's called a 'User ID' or a 'UID' that's associated with whoever has logged in.

### Setting up auth

So first thing's first.  we need to go ahead and go to our `catch-of-the-day` Firebase, and go to `Login & Auth`.

Now, this is the different types of authentication that Firebase allows you to have.  I recommend jacking up your session length to be something long like 24 months or something like that so it will never log you out without having to worry.

Then we can go ahead and turn on - in this case I'm going to show you how to do Twitter, Facebook and Github login, however you could also use Google and a couple of other different ones that we have right here.

In order to log in with these you have to create an application with Twitter, Facebook and Github.  So I went ahead and I went to 'Developer Applications' on my Github.  I went ahead and created this application called 'Catch of the Day'.

Homepage URL doesn't really matter, the really important thing here that I sort of banged my head against the wall is the callback URL is not your application.  So I had `localhost` in here for a while.  The callback URL for your application is going to be `auth.firebase.com/blah-blah-blah`, and then `/your-actual-one`, so `catch-of-the-day` is the URL in my Firebase, that's why `catch-of-the-day` shows up here, `/auth/github/callback`.

Cool.  Same goes for Facebook as well.  When you create your app make sure you do that.  It's going to give you some tokens.  Same goes for Twitter.  The callback URL is going to be `firebase.com`.

So what you can do there is it's going to give you some keys.  When you create your application you go ahead and put the id and the secret, the api key and the secret, and the Github ID and the secret - and by the way, do not ever show these to people.  I'm going to have to regenerate these after this screencast, just because no-one should ever have to see these ones.

Then once you have them turned on and saved in, we can actually go back and start writing some code.

### Adding Auth Buttons

So what we're going to be doing here is we're not going to be working really in our `app.js`, because we're not locking down the entire application, we're really just locking down this `inventory` component which allows us to edit and manage all of our content, so let's get going.

The first thing that we want is some HTML.  We want three buttons to log in with Github, Facebook and Twitter.  To do that we can do two things.  We could use a separate component to render this on out, which would be nice, but just to keep things simple I'm going keep it in a separate method in here.

We're going to call it `renderLogin`, and inside of that method we're going to just return a navigation with the three buttons for Github, Facebook and Twitter.

Now, these aren't hooked up to anything just yet but we're going to add some event listeners to that.

Now, essentially what we want to do is this `render` method here, we want it to only render out the fishes in the `addFishForm`, and this button to load the sample fishes if they are logged in.

So first of all someone has to be logged in to manage the inventory, and then second of all we want to check that they actually own the current store before they can actually render it out.

So in order to do that we need to keep track of the user's user id once they're logged in.  How do we keep track of stuff like that? we're going to use state.  Now, we're not going to be using the app state because this is not specific.  It's specific to the `Inventory` component, so we'll use the state on here.

So let's go on up, and how do you set state on ES6 class?  Well, we use the `constructor` function, and in that we have to call - if you want to set state or anything on this we have to call `super()` first, which will inherit the parent.

We'll say `this.state = ` and then we're going to say `uid` - this might not make a tonne of sense just yet, but you'll see how we're going to come back and update that.  We just need to have it there.

Now we'll go down to our `render` function inside of here, and the first thing that we'll do is we will:

`//first check if they aren't logged in`

We'll say `if(!this.state.uid)`, so if the current state doesn't have a user ID, then go ahead and return some JSX which in our case is just going to be a `<div>`, and inside of that `<div>` we're going to say `this.renderLogin`.

So if they are not logged in - put that on one line - then go ahead and render the login, which should be true.  I'll give that a save, we should see our buttons.

Good, there's our three buttons showing up instead of the `renderInventory`, but then next we need to:

`//then check if they aren't the owner of the current store`.

So how do you check?  we're going to say `if(this.state.uid !== this.state.owner)`.  So we don't even have the `owner` just yet, but when we have the owner we're going to check - if they're logged in, we have to check if they are the owner of the current store.  because anyone can log in, but we shouldn't be able to let them edit it.

So if they are logged in but not the owner of the store, we'll `return` ourselves a `<div>`, and inside of that `<div>` we'll say `<p>Sorry, you aren't the owner of this store</p>`.

### Logout Button

Good.  Now, last thing I want to do is give ourselves a Logout button, because if someone has logged in with the wrong account they should be able to log out when they go into it.

So I'm just going to pop that into a variable here.  Say `let` - `let` is a part of ES6, and it allows us to create a variable, but it's scoped to the block instead of the function - `let logoutButton = ` and we can just use some JSX here, which is a `<button>` that says "Log Out!".  And we're going to hook that up, we'll have an `onClick` in there in the future.

We'll take that `logoutButton` and we'll put it in here.  if they aren't we'll say `logoutButton`.  What we'll also need to do is take that button and also put it inside the inventory somewhere, and if they're logged in and able to manage the inventory.  If they want to log out they have a button to log out.

So that's why I've put it in a variable, because if I use it more than once I don't want to have to rewrite the html to do it each time.

### Communicating with Firebase

Now we need to actually start communicating with Firebase in order to handle all the authentication and the updating of the owner of each store.

So what I'm going to paste in here is - first of all we `import Firebase from 'firebase'`, and what that will do is it will include the Firebase script library which will allow us to communicate with it, and then second we're going to establish the connection between our application and actual Firebase.

So we're going to store that in a variable called `ref`.  I'm using `const` here, which is part of ES6.  What that does is allows us to create a variable that cannot be overwritten, which is really nice for a reference like this.  so this reference right here is sort of a reference to our object, but will also contain a whole bunch of methods that will allow us to communicate and listen for when data changes.

We'll say `new Firebase` and we'll pass it the URL to our actual Firebase.  you can see that matches the one right here.

Now, if you're noticing that that URL is the same one that we pasted here, and if you change the URL you have to update two different files.  So maybe a little extra work that you could do here is make a separate file called `config.js`, make a module that will contain all the configuration, similar to how we use helpers, but create a config one, and then ideally you could just import the config and say something like `config.firebaseurl` and pass it in there so that whenever you change it you just change it in that one file.

But I'm going to keep it hard coded for simplicity's sake.

### Hooking up the buttons

Now let's go down to these buttons, and when someone clicks any one of these buttons, what we want to happen is to call a `authenticate` method which will communicate with Firebase and do all of that good stuff for us, and we're yet to write that method but let's hook up the event handler first.

So we'll say `onClick={this.authenticate}`, and we want to be able to pass it where we want to authenticate with.  It's either Github, Facebook or Twitter, and you might think 'ok, we could just pass `github` directly, but what that will do is it will actually run the `authenticate` method on page load and return the event handler.

So we don't want to do that, we want to do is call `.bind` and pass it `this`, because we want to bind it to the component, and then we pass it `github` as our parameter, and then we'll do the exact same thing with the other two, which is our `facebook` and our `twitter`.  Paste that in there and we take our `facebook` and our `twitter` and we'll place the `github` with that one.

Cool.  Then let's go and write that `authenticate` method, so `authenticate`, it will take the `provider`, I'll call that and that's going to pass us either `github`, `facebook` or `twitter`, and inside of that we'll say `console.log("Trying to auth with" + provider);`.

We'll just double check that these click handlers are working for us.  "Log in with Github".  There we go, try to log in with Github, Facebook and Twitter, so those buttons are working.

Then we actually can go ahead and say `ref` - remember `ref` is a reference to our Firebase - `.authwithOAuthPopup()` and you pass it which `provider` you want, so sometimes you might hard code something like `'facebook'` here, but we don't want to do that, we want to pass it the `provider`, which is just a string, it's going to say `facebook`, `twitter` or `github`.

And then we have a function which is going to be sort of the authentication handler or what's called the callback here.

So let's just call back - it's going to pass us some data which is the error and the `authData`, and I can just say `console.log(authData);`.  I'm going to log in with Github, it's going to pop up.  since I've already said - I've done the initial 'it's okay, I accept that - I'm allowing this application, which is `catch-of-the-day`, to access my Github stuff', it immediately goes ahead.

Then you see in here I have this object which is `console.log`ged, which is this `authData`, and inside of that is all kinds of information about my profile.  Most importantly I have a user ID and a token. we're going to talk about that in a second.  If I were to also similarly do that with Facebook it would go ahead and try and log me in, and it would give me all the information about my Facebook.  So I can open that up, it'll give me my picture, my name etc etc.

We are also going to do that on page load, because when I refresh it doesn't persist just yet.  So rather than putting this callback right inside of here, what we can do is say `this.authHandler`, and we're going to make a new method called - you probably guessed it - `authHandler`, which is going to take the error and the `authData`.  It's going to give us that, and inside of that let's just say `console.log("I'm in the auth handler");`.

So log in with Github - there we go: "I'm in the auth handler".  So now that we know we are in here, we can say if there's an error - first we're going to just check if there's an error, then we'll say `console.err(err);` - you can handle that however you want, and then we'll say `return`, which will stop the rest of the code from running.

Now we need to reference the actual specific store that we're on, because right now we have that `ref` variable which is referencing the entire database which is called `catch-of-the-day`, however now we need to reference the individual store of whatever the person is on.  So I'm on `elegant-magnificent-nuclei` right now, and I want to get the data from this specific one and say  'are you the actual owner of this store?  If you are I'll allow you to edit and see it, otherwise I won't allow you to edit and see it.'

So we need to go in here, and we'll make another `const` variable, and we'll call that `storeRef`, which will take - it's our `ref.child()`, and we're going to say `ref.child()`, and ideally we want to pass it like the name of the store that we're currently on.  We want to pass it something like that.  But it needs to be dynamic, so we can pass it the `this.props.params.storeID`, and that should give us the storeID that we have here, and let's go ahead and `console.log` this just to make sure that we have it.

Log in with Github, and it says right here `uncaught TypeError: 'storeID' of undefined'`, and it's telling us that `this.props.params` is not available to us.  And that's because the params that tell us about the `storeID`, they're not part of this component.

What component are they a part of?  Well we can go to our React devtools here and open up our `Router` and click on our `App`.  you'll see that we have our `params` here, and inside of that we have the `storeID`.  That's the data that we want, but it's not available on our `Inventory` component.  You'll see it's not in there.

So how do we get data from a parent down to a child?  Well, we use `params` to pass it down.  So we can go to our `App` here and find where we use the `Inventory` right here, and instead of again doing - it feels like we're getting a lot of these `fishes`, `linkState`, `removeFish`, etc etc.  One little trick that we can do is we can use something called a spread, which will take everything from the parent and pass it down to the child.

So if you just do two curly brackets and you just say `...this.props`, and that will take all the `props` that are given to the `App`, and one of the props that we want is the `route`, and it will pass them down to the `inventory` so we don't have to explicitly pass every single one of them on down.

Make sure that this 'd' is lower case, `Id` is proper camel-case.

Now when we click on "Login with Github" it does `console.log` the actual one, and we have this variable `storeRef`, which is now a reference to the current - sort of like the child object that we are looking for.

Now what we want to do is fetch the data about that specific store, and we're going to check does that store already have an owner? If it doesn't we can claim that store as our own, and then we can also update our internal state of this component to tell about the current user as well as the owner.

So what we'll do is we'll say `storeRef.on('value')`, and this is like a listener for when the data about this store comes back to us, and it's going to pass us a `snapshot`, and I'm going to use an arrow function here, and this is part of ES6 as well.  Because I'm now entering into another function - and this is the same as doing a function like this - however you know that when you have a function like this, the keyword `this` is actually changed inside of that function.

So one of the benefits to ES6 is if you use an arrow function the keyword `this` does not change inside of the function.  So we can still reference `this`, which is referring to our actual component, which is great.

So we're using ES6 right there, and we'll say `var data` either equals the `snapshot.val`, and that will take all of the data out of the child ref, which is going to give us all the information about the `fishes`, as well as if there's a possible owner, and sometimes there's not any data there because we're the first ones to the store and we're actually claiming it.  So we'll just say `or` it's equal to a blank object.

We can say if there is no owner of that store, then we can claim it as our own. `storeRef.set()`, and we're going to update the data in the database, and we're going to say the `owner` is equal to - well how do we get the current user's user ID?

Well, remember we have this `authData` that we've been working with right here?  So we can say `authData.uid`, and that's the user id of the actual one.  And that's going to give us something, like `"github:176013"', that's my Github user id, which is great.

So we've got that.  If there is no - here we can put a little comment here: `//claim it as our own if there is no owner already`, and then we can go ahead and `update our state to reflect the current store owner and user`.  So we'll say `this.setState`, and we want to update the `uid`, which is going to be exactly the same as we did before.

So the current user, regardless of if they own the store or not, is going to be equal to `authData.uid`, and then the `owner` is going to be the `data.owner`, so the data from - that came back, or if there's nobody it's going to be equal to the current user id.

So the reason we do that is because we have a `uid` which is a current person and the owner, which is the person that owns this current store, and they might be the same person, and that's exactly why if we go down to our `render` function here, we said `if(this.state.uid` is not the same as `this.state.owner`, "Sorry, you aren't the owner of this store".

However, if they are then this one will go ahead and render and will actually see it.

So we'll give that a save, and I'm going to log in with Github, and then you can see right away when I'm logged in the state updated and that caused the `Inventory` component to trigger and update, and now I'm able to go ahead and edit all of my stuff.

### Persisting Authentication

Now, one big issue we had here is that your authentication does not persist, meaning that I'm logged in right now, but if I go ahead and refresh the page I'm no longer logged in and able to edit it, so I'd have to go and click "Log in with Github" once more, and log in.

So that's kind of a pain, and how applications manage this is either with cookies or some sort of local storage.  They need to save something in the browser so that when you go from page to page or when you refresh or when you come back three days later, it still remembers that 'oh, this person was logged in and we're going to be able to log them in.

So the first thing that we need to do is save the login token in the browse somehow.  So we'll go up to this `authHandler` function, and right below the error we will say `// save the login token in the browser` so that when I reload it'll be able to fetch it again.

And the way that I'm going to save iti here is with local storage, we've been using local storage so far.  You could also use a cookie for this.  So I'm going to say `localStore.setItem('')`, and we're going to save it as `token`, and that `token` is going to be the `authData.token`. 

Now when I go ahead and log in with Github I can open up my `localStorage`.  Now when I go ahead and log in with Github I can go to my `resources\localstorage` localhost.

You'll see that I've got - in order - as well as we've got information about which fish I'm trying to buy, I also have the token which will allow me to do it.  But now when I refresh it still doesn't log us in, and that's because when - right before this component renders, what we need to do is communicate back with Firebase and say 'hey, this person has been here before, let's re-authenticate them'.

The way that we do that is with the `componentWillMount` method, so that's part of React, and the `componentWillMount` method will run when the component will mount.  So I'll say the `componentWillMount`, and this will run right before the component is about to run.  We can just say `console.log("Checking to see if we can log them in");`

`var token  = localStorage.getItem('token');`.   We'll say if there Is a token, then we can call `ref` - which again `ref` is a reference to our Firebase - `.authWithCustomToken()` and we'll pass it the token, and then what happens when there's  a successfull login?  Well, `this.authHandler`, right?  Remember? 

So we either call the `authHandler` when someone clicks it for the first time, or when the page loads we are also going to call the `authHandler`, which is going to do all of the checking to see if they're in that one, and then I'm going to do this whole song and dance once again.

So give that a save, refresh and you'll see that it logs me right away in.  We see the buttons real quick when the page loads, and there's things we can do with another one called `componentShouldRender`, but for simplicity's sake I'll keep that out for now.

### Logging Out

The final thing that we want to be able to do is hook up this "Log Out" button, which will when you click it it will log us out and allow us to sign in with another one.  So in order to do that we will create ourselves a `logout` method inside of our class, and inside of that we'll go ahead and do a couple of things.

First one will be unauth them with Firebase, so we'll say `ref.unauth()`, and then we'll go and remove the local storage token, because if they come back and refresh the page that token shouldn't be stored there.  So we'll say `localStorage.removeItem('token');` and then finally what we want to do is update the state of our application, which is going to re-render the buttons for us, so we can say `this.setState()` and we want to set the `uid` to `null`.

So give that a save and we will also call this function in our "Log Out" button.

So go down to our `render` function here where we have our `logoutButton`, and we'll say `onClick={this.logout}`.  So click "Log Out!" and it logs us out.  I can log in with Facebook - oh, that's cool!  "Sorry, you aren't the owner of this store."  Why? Because I created this store here, `elegant-magnificent-nuclei`, with my Github login, and now I'm logged in with Facebook it's not the same one, so I can go ahead and log out, log in with my Github and I am able to edit it.

Similarly I can go to another one called `wes-is-so-cool`, and I'm not logged in to any one, there is no store owner here at all, so let's try log in with Facebook, and I can load in some Sample Fishes, I can call this the "Wes Cool Fish", and that's good, I can refresh and it still logs me in with Facebook, but if I log out and log in with Github I'm not able to access it.

Why?  because I created the `wes-is-so-cool` store with my Facebook one.  So some basic auth that we have there, there's a lot more that you can do with it and you can take it in whatever order that you want, but that is the basic authentication.

### Server-side authentication

The last, last last thing that we want to do here is - this is really the only - it's pretty weak, because it's only showing the `inventory` editor if they log in, so anyone could figure out that 'oh, I just couldn't open up my devtools here and gett the `addFishForm` in the `object.keys.fish` to render out even if I'm not the owner'.

So we need a little bit of authentication on the server side as well, which - if you go to your Firebase and you go to 'Securities and Rules', and by default `read` and `write` of everything is `true`, which means that anyone can read it, anyone can write it, and anybody with a little bit of a brain can figure out that you can find the `ref` in `call.remove` which will delete your entire Firebase asset, which is not what you want.

So what we have here is I've got a little security `rules.json`, I'm just going to paste that in here and explain that to you.

Right here we have `//won't let people delete an existing room`, so as soon as a room is create someone can't just go ahead and delete everything, if the `data.exists()`, that's just part of the Firebase rules.

Then `$room`, so whenever someone goes to the next level, I'm calling that `room`, and that's because our top level is `catch-of-the-day`, but every chilid here - `wes-is-cool`, `wes-is-so-cool`, and this one, these are the rooms.

So, the access restrictions for an individual room is anybody can read it, `read` is `true`.  However, who can write to it?  Well, only the people that can write to it are - if they're sending new data, if the owner of that is the same person that is sending it on over.  Because we want to check - if we go to this `owner` here, if the `owner` of `wes-is-so-cool` is `facebook`, and it's the same person that's sending the updated data to it, then we'll allow it.  Otherwise we won't.

So there's a lot more you can go into here, and you can read all the documentation as to how it works, but it's pretty simple. This is like my entire application's authentication strategy in eleven lines of JSON.