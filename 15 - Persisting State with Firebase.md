## 15 - Persisting State with Firebase

In this video we're going to talk about hooking our React app up to a backend.  we're going to talk about how do we get our state to persist when we refresh the page, and how do we get people who are maybe on another computer that are also visiting the same store - how do we get them to see the exact same information?

Because so far when I go ahead and add an item, and I go ahead and add it to my order or load in some sample fishes here, when I refresh they're just gone.  And that's because our state is just stored in JavaScript, and as soon as you refresh the page all of that state is reloaded.

So we need a backend, some sort of cloud service, some sort of database that's going to let us save this data, and then when we refresh the page we'll be able to fetch that data and restore our state.

So we're going to be using Firebase in this case, and Firebase is a product from Google.  If you haven't heard of it, it essentially lets you do away with your backend entirely.  It lets you store your data up in the cloud and the handle all of the authentication, all of your security and all of the syncing across all of your different databases which is pretty, pretty cool.

It's really easy to use, it's nice and fast, and probably the coolest thing about it is it uses something called HTML5 websockets, which means that as soon as data is updated anywhere in the database it sends out an event to everywhere else that's connected to that database and says 'hey, there's an update', and using JavaScript we can immediately update those things on the page.

So let's just take a look here.  I've got my 'Catch of the Day' database right here, and Firebase databases, they're essentially just one big JavaScript object, which is perfect for working with React because our state is really just one big JavaScript object, and let me go to my one right here.

I'm going to go to a store, so I'm going to hit Submit here.  Keep your eye on the left here, and as soon as I hit submit you'll see that right away this turns yellow & green.  yellow means it's been updated, green means it's been created.  And if I open it up you'll see it says `owner: "github:17601"`.  That's just my login.  we're going to learn all about how we can log in with Github or Facebook in the authentication video.

And I can go ahead and click on "Load Sample Fishes", and again watch on the left here, whoa!  Look what happened here, is immediately I see: these are all of our fishes that we're used to.  This is just an object which has all of the fishes, one through nine, that we have created, and if you open it up there's a description, image... everything that we're used to.

Now the really cool thing is you can actually edit your content right from the Firebase dashboard, and if I change that to "Atlantic", hit Enter, and look - whoa!  Immediately it updated it here as well as updated it in here.  Anywhere I've referenced it it's updated it.

This is totally remote, this is not part of React at all, however it's all synced with our state and will allow anyone who has this URL - if I open it up in Safari real quick, if we can all watch it at once, and I go ahead and change it and call it "Wes Halibut", hit Enter, and everything is immediately updated. 

If I change the price of it to increase quite a bit, it updates every single place that we have it.  So pretty cool, and it's surprisingly easy because Firebase is just one big object, because React uses a big object for state it's actually fairly straightforward to sync them.

So let's go ahead and get started on that.

The first thing we need to do is actually sign up for a Firebase account, so go to [firebase.com](http://www.firebase.com), they have a free account.  So you can go ahead and create yourself an Application Name and an Application URL, click 'Create New App'.

I've already gone ahead and done mine with the `catch-of-the-day`, and what we need to do is install some sort of tool that's going to help us sync our state.  Now the people at Firebase have created us one called 'ReactFire', however the problem with it is that you still have to use all of the Firebase APIs.  You still have to update your state as well as make sure that you're properly syncing your Firebase and you're kind of managing the two things at the same time, which is not always ideal when you're working with state, and even the flux pattern in React.

So I'm not going to be using ReactFire, I'm going to be using one by Tyler McGinnis, and he's created this awesome one called 're-base', which essentially allows you to set it up with your Firebase database, and after that you can just sort of forget about it.  It's going to work in the background for you.  You just have to keep managing your state, and whenever you change your state it's all going to be reflected in your Firebase and vice-versa.

So we want to go ahead and install that, so open up your terminal here.  I've already installed it for you, just like all of the dependencies, but if we do need to install a dependency for ourself we would do `npm install re-base --save-dev` and hit enter, and that will go ahead and install everything for us, but make sure that you start up your Gulp again.

Then we want to head back to the top where we include all of our dependencies, and you might just want to give yourself a little comment, `// Firebase`, and we can require it, so `var Rebase = require(re-base);` and that will load the library into the variable `Rebase`, and then we need to hook it up with Firebase, and those are called the `bases` in Rebase, so we say `var base = Rebase.createClass()` - and this is very similar to just regular React - and all I need to do right now is pass it the URL to your Firebase database.

So what is that URL?  Well go back to your Firebase, and you simply just take the URL out of the browser here, and in my case it's `https://catch-of-the-day.firebaseio.com`.  Yours is going to be a little bit different, and what that's going to do is that base, that's going to be called a reference to the Firebase database.

Now we need to go ahead and sync our Firebase database up with state of `fishes` in our `App` component.  That way whenever any one of them changes, the rest of them will be all updated.  So for that we need to dive into something called the React Component Lifecycle.  And you can think of them as different hooks that we can use to get into our component in different times of its life.

So we already looked at the function that gets called whenever we need to render out the HTML, and we also talked about `getInitialState`, which is right before the component is mounted, which means that right before the component is put on to the screen we need to go ahead and create some initial state.

However we also need another one that when the component is put on to the page in the HTML we need to go ahead and go to Firebase, and if I'm just loading this fresh here I need to go to Firebase and grab everything from Firebase and dump that into my state.

So that's called 'syncing state', and the one that we actually want here, if you scroll down here to the lifecycle methods, is the `componentDidMount`, and this is one - it's invoked only once on the client immediately after initial rendering occurs, so what that means is once it's put on to the page it's going to call `componentDidMount`, and in this function you can do things like going and fetching some data that needs to be inputted into our state.

So `componentDidMount` - if you're not working on Firebase but maybe you're working with like your regular AJAX API or you're going back and forth to grab some initial data, you would also put your AJAX request right inside there.

So let's going to go ahead and find our `App` component, and right below `getInitialState` we'll give ourselves the `componentDidMount` function.

And you'll notice - this might be a little confusing, but this page is kind of helpful to know that - these things like `addToOrder` and `addFish`, we just made those up.  Those are custom methods that we've gone ahead and made.

However things like `getInitialState` and `componentDidMount`, if you start seeing them in some React applications, just go ahead and check out the component specs page in the React docs and you can read through exactly what they do, and this is pretty much all you need to know about the different things that happen in React, and it's actually a pretty small document.

One of the beauties of React is that although it's super powerful, the API is fairly simple.  So `componentDidMount` - let's just do a `console.log("The component did mount");`.

I just want to make sure this is running and see when that will run for us.  So let's go to our app here and open up the thing - we see it there.  Let me just refresh, and you see that it logs it pretty much immediately, and that is because my application component is being mounted immediately, but if I go back to our home page and - you don't see it right there, but if I click submit you then see `"The component did mount"`.

So that should give you an idea as to when this is being called.  And now what we want to do is not just `console.log` inside of it, is we want to grab our `base` variable that we selected before, and that's just a reference to our Firebase.  We can call `syncState()` on it, and what `syncState` will do is take our our React state, if we open up our `Router`, our `RoutingContext`, `App` we've got our `State` right here.  

It's going to take our `State`, specifically our fishes we'll tell it, and sync it with the one in Firebase, which is going to be `catch-of-the-day/whatever-the-storename-is/fishes`, and by the way the reason I'm saying "forwardslash" is you can click on any one of these objects here, and you'll see that the reference to that specific object then updates the URL.  

So we need some way to tell React we want to sync our fishes, that is related to the current store. You'll notice that I'm on `sparkling-bewildered-women` and this is on `glamorous-long-teeth`.  We don't want to sync `sparkling-bewildered-women` with `glamorous-long-teeth`, we want to sync them with the right one.

So we need to tell it to sync this fishes with the one that's associated with that.  So head back to your code editor, and `syncState()` takes two arguments.  The first one is, what is the path that we're going to be syncing with, because we already did the `https://firebase-whatever.com`, but we didn't do the `/glamorous-long-teeth`, and then we're actually going to go one further which is `fishes`.

So we want to tell it `glamorous-long-teeth/fishes`, however we can't hardcode `glamorous-long-teeth` because that needs to work regardless of what store that is, what we can do is we need to access this parameter here, this thing from the URL bar that we need, and luckily `ReactRouter` - remember we used `ReactRouter` to handle the URL bars?  What it does it gives us a `props` called `params`, and if you open that up you'll see - oh!  `storeId`!

So all we need to do is say `this.props` - which is all the `props` of the `App` component - `.params.storeId` and then we'll just concatenate on `/fishes`, so that's going to make sure that it's syncing with the proper store `/fishes`, and then the second argument that we have is an object which just has a couple of settings that we need to have in there.

The first one is the `context`, which allows us to say `this` that will sync it with our `App` component, and then the other thing is the `state`, which part of `state`.

So we don't actually want to sync `order`, we'll talk about that in the next video, we want to sync only the `fishes` state, so we can say `fishes`.

And that should - give that a save and head back to your React here.  I'm going to back this one up all the way to `catch-of-the-day`.  If you ever have a lot of stuff here you can always just click the `X` on here and it will just delete everything for you.

So I've got this `glamorous-long-teeth`, if I load in some Sample Fishes we see `sparkling-bewildered-women` is immediately filled.  We've got our fishes and all of the details that are available to us.  If I go ahead and change one of these - let's do a different one now, `fish2`, change "Lobster" to "Cool Guy" and hit Enter.  You'll see immediately it's updated here.

That's it.  it's pretty surprising that - what have we got here, 1-2-3-4-5-6 lines of code, 7-8 lines of code.  To be able to immediately sync our data and now any time someone has this application open they're going to be able to refresh this page, and all of a sudden my "Pacific Halibut" and "Cool Guy" is there.

We don't have to keep clicking this "Load Sample Fishes" every single time, it's going to persist in the database.