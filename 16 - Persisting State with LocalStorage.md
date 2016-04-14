# 16 - Persisting State with LocalStorage

Now we need to work with the other part of the state which is the `order`.  We've done `fishes` and we have that syncing up with Firebase, however when someone adds something to their order and you refresh the page, it doesn't stay, and we don't really want to store that data in Firebase because it's specific to the user who is using this website and about to check out.

So what we actually need to do is, we could save that in a cookie or we could use HTML5 local storage, which will allow us to - every time we update the order we update the local storage, and then when we refresh the page, when we load the page or when we load this component, what we will do is we'll check local storage to check if there is anything in local storage that we can then update our order, and that will allow us to move around the application as well as refresh the page while still maintaining our order state.

So what we need to do is go on into our `App` component again, and in the last video we talked about the different life cycle events of a React component, and we talked about how `componentDidMount` will run immediately once it is loaded.

However we don't need it just yet when it's loaded, we need to save it any time some data is updated.  So if we take a look at some of the other ones like here, we'll look for one called `componentWillUpdate` - let's just take a look here:

> Updating: componentWillUpdate
> Invoked immediately before rendering when new props or state are being received.  This method is not called for the initial render.
> Use this as an opportunity to perform preparation before an update occurs.

So what that means is that any time you change `props`, which is - let's take a look at our `render` - any time any of these `props` changes, like the number of fishes or the order, or any time state changes like what is in our order or what is in our fishes, then this is going to run and then it's going to pass us the new `props` in the new state.

So we can use that to sort of hook in and run this function every time the data is changing, sort of like an event listener for when the data is changed.

So we'll take `componentWillUpdate` and we'll put it right below `componentDidMount`, and that is a function, and what did we say that it takes?  It takes two things - or what is it going to give us?  It's going to give us the `nextProps` and it's going to give us the `nextState`.

Let's just `console.log` the `nextState`; we're not really concerned with the `props` in this case.

So let's give that a save and go back to our app, open up our devtools, and we already see an object being logged here which is the state, however any time I add one to our state you'll see that it's logging us a new object which contains our new state.  It has the fishes on it as well as the order.

But let's hold on a second, we've got a bit of an error here.  Let's just debug what's going on here:

> Warning: Each child in an array or iterator should have a unique "key" prop.  Check the render method order

So it looks like - remember I told you earlier every time that you repeat over a child with React it needs a key in order to work on it, so I bet that we forgot that in our application here.

So here's our `order`, and here we have `<li>` which has a key for `Sorry, fish is no longer available!`, but it looks like when we do have the fish we're not actually giving a key, so let's say `<li> key={key}` and let's see if that fixes it for us.

There we go, that's working great.  So we can see that every time I add something to order it `console.log`s our new order state and it'll tell us how many fishes of each that we have, which is great.

So we don't want to `console.log` it each time, what we want to do is save it to local storage.  Now, if you haven't worked with local storage before, it's just like a giant object.

You're probably starting to see a pattern here, Firebase is a giant object, React state is a giant object, and local storage is also a giant object in which you can store stuff, refresh the page, and you can get it at a later time.

So what we can go ahead and do is say `localStorage` - and if you've never used local storage before, it's just one big object, and we can use it just like an object, however there are also a set of methods you can use on it like `.setItem()`.

Now we need to - first we need to give it a key of where to store it, and then second we actually need to give it the day that we want to store. So if you go on over to your resources tab in your devtools, you'll see that we've got a local storage here and you can click on the domain name that's loaded, and you'll see that I actually have a couple here, and this is what I want to store.  I want to store the `fish` key as well as the number of fish that the person uses, and that's exactly our order state that's just stored in local storage here.

So we want to say `order` dash the actual state dash the actual storename, so `sparkling-bewildered-women` in this case.

So let's go ahead and say `'order-` - now, we want to say `sparkling-bewildered-women` but we can't hardcode that so we have to say `this.props.params.storeId`.  remember we used that in the last video to get the current `storeId` out of the URL bar.  `ReactRouter` makes that available to us, and then the second value is what we actually need to store, so you could say `nextState.order`, and that will give us the order.

Now, we're going to get a bit of an error here, but I'm going to go ahead and show you exactly why we can't just do this directly, so let's let this refresh and give it a shot.

I'm going to add something to my state.  You'll see that we have this new `order-sparkling-bewildered-women`, however the value is `{object Object}`, and if you've ever seen that it's when you're trying to display an object as a string.

local storage is pretty simple, you can't store anything but a string in it, so if we want to be able to store an entire object in local storage what we can use is JSON.  So we can just wrap this `nextState.order` in `JSON.stringify()`, give that a save, now when I go ahead and add things to my order - here it is, I have it selected, so I'm just going to go ahead and add a bunch and then every time I add one now it's storing in my local storage.  When I add another one, `fish2`, I go ahead and add a whole bunch of oysters it's going to load in `fish7` for me.

Now I'm going to refresh the page here, and you'll notice that really temporarily it was there, however then it gets overwritten as soon as it writes.

So what we need to do is when the page loads or when the component is mounted to the browser, we need to go ahead and take this out of here and restore this state.

So that's another one of these lifecycle ones, and it's actually the exact same one that we used before, it's the `componentDidMount`.  Now, we need to do this underneath the `base.syncState`, just because we need to wait until all these fishes are loaded into state before we can actually restore the order, otherwise we won't have any information about the title and the price of it.

So apparently this rebase is getting a promises API, but it doesn't have it just yet, so we have to do with putting it underneath for us, so we'll go ahead and say `var localStorageRef` - I'm just going to make a reference to `localStorage`, that's just a variable - and then it equals `localStorage.getItem`, so we used `setItem` in the last one.  In this one we say `getItem`, and we want to get the `order-` and again we can say `this.props.params.storeId`. 

Then we have to first check if we have something in there, because if this is a brand new store, this is still going to run and we won't have anything in our `localStorage` to update.  So we say `if` there is a `localStorageRef`, we need to go ahead and update our state.


So we say `this.setState()`, and what state do we want to update?  We don't want to update all of our state, we just want to update our `order` state, which will be - you can say `localStorageRef` in there, but again that's just going to be a string, so how do you take - since we stringified it how do you unstringify it?  You say `JSON.parse` around that.

So that should go - when the component loads it should go into our `localStorage`.  Check if we have one of these.  If we do have one that matches that current store, it will put it on into our state.

So give it a save, let's try it out... and there we go.

You'll notice that temporarily when I load the page you'll see "Sorry, fishes no longer available", and some of you may see that, some of you may not see that.  It's really just how quickly your internet connection is running.  What's happening here is that this `base.syncState` is happening after we actually restore the `localStorage`, so when we go and render out our order it doesn't have a fish to show.  So that's a little bit of a downside of this library, there's no - essentially I'd like to say `on('connected')` and then "run the following code", so only once they've connected it, but it doesn't have that just yet, it's going to be added, so I'm going to leave that as is just for simplicity's sake.

What you could do - there is another component lifecycle called `shouldComponentUpdate` in React, and what it does is when this `App` runs, or - we don't need to update the entire app - when the `order` component is going to run you can run the `shouldComponentUpdate` and do a quick check to say is the Firebase loaded.  If it is, then return `true` and it will update itself, otherwise the `render` method if you return false on `shouldComponentUpdate` the render method is not going to run, and it's just going to come around the next time.

So that one's handy if you don't want to re-run the render three our four times as you're getting stuff in, like maybe you're getting four pieces of data in and you want to wait until all four pieces of data have come back, you could run the `shouldComponentUpdate` and only return true once all of them are back.