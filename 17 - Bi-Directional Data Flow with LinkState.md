# 17 - Bi-Directional Data Flow with LinkState

So far we have a way to add new inventory to our fishes, we have the ability to add that inventory to a specific order that we have, but we don't have any way to be able to manage that inventory.

We can't edit the name of the fish without having to open up the devtools or going to our Firebase, and we don't have any way that we can actually remove something from our order or delete an inventory item at all.

So that's what we're going to be doing here.  If we take a look at examples, we'll just check real quick what we're going to be building.  If I load some Sample Fishes and add a couple to my order you'll notice that I can just go ahead and type in here and it will update everything else. We've seen that over and over again throughout this app.

So that's what's called "bi-directional data flow", and by default React is uni-directional, which means that it'll only go a single way.  The idea is that you only edit your state, and when your state is changed it'll update it everywhere else, however we also want to be able to type in this box and be able to update our state just from this box.

That's what's called bi-directional data flow, and generally in React you don't do this. However, when you do some sort of inventory management system we do need to implement bi-directional data flow - or I guess tri-directional data flow is really what we're going with, because we also want to - when we edit it here we want to also update it on our Firebase.

React actually comes with something in their addons package that allows you to do this, because this is not an uncommon thing that you would want to do with React, and it's called `linkState`.  So if we just take a look at their "Two-Way Binding Helpers" here, they have this thing called  `ReactLink`, which allows you to link state, and essentially what it allows you to do is you can take something in your state, and link it up with an input, and whenever that input changes it will also update your listeners state - it does all of the listeners for `keyUp` and `change` and all of the regular form events that you're used to, however we don't have to bind any of those, we simply just call `linkState` on it.

Now, that's really great, however the problem with the `linkState` that comes with React is that it only works for things that are top level, so we've been looking at our `State` here, and what's top level here?  Well, `fishes` is top level and `order` is top level.

However inside of `fishes` we have all kinds of data that we're going to be using, so let me just open up React devtools here, and go to React, go for our `App` component and look at our `State` here.

So top level is `fishes`, but we don't want to listen for the change on the entire fish, we want to listen for the change on like `fish1.description`, `fish1.image`, `fish1.name`, so unfortunately it's not going to work for us, but that doesn't stop us from implementing it ourselves.  It's totally doable, and someone's already gone ahead and created this really nice package called React Catalyst.  What React Catalyst will do is it allows us to do `linkState`, but with nested objects.

So if I just took a quick search for React Catalyst here you'll see that it's a mixin which allows you to deep nest your listeners, where we can do things like `values.0.text`.  So that's what we're going to be doing here.

Again I've already gone ahead and installed React Catalyst for you, but you could do `npm install react-catalyst --save-dev`.  You pop that into your command line and it will save it to your `package.json`, but we already have it, so we need to go ahead and import it ourselves.

So `var Catalyst = require('react-catalyst');`, and that will load everything from that library on in there.

Now the way that React Catalyst works is we want this one here, this `linkState` mixin that we're really interested in here, and the way that it works is, it's a mixin.  When we previously used a mixin with the router on our `store` locator on our first page, now we're going to use a mixin on our `App` component.

So go on over to your `App` and we'll say `mixins`, and again it's always an array, and here we're going to type `Catalyst.LinkedStateMixin`.  And what that will do is it's going to take this little method called `linkState` and it's going to make it available to us anywhere inside our application.

So we've implemented that, now we need to scroll down and find our `inventory` component, because if we take a look at this, we're not - `App` is the entire thing, but we want to be able to put little edit boxes for each fish underneath this inventory one.

So let's scroll down and find our `inventory` component.  Here we go.  So let's just familiarise ourselves with what's going on here, we've got a heading, we've got an `<AddFishForm>` which is a separate component entirely to be able to edit this.  To add a new fish we have this button here that will load in some Sample Fishes for yourself.

Then we want to go ahead and build something like this where we've got our inputs for all of our different items, and then we also had a "Remove Fish" button which will remove our fish entirely from `State`.

So right here what we want to do is give yourself some curly brackets and we'll call `Object.keys` again on var `this.props.fishes`.  that will give us all the keys for our fishes, so `fish1`, `fish2`, `fish3`, and we will call `map()` on that, and we can create ourselves a method called `renderInventory`.

So this is going to grab us a list of all the keys, and then for each key it's going to run it against `this.renderInventory`, and `renderInventory` is going to be a function that we create which will return that little `render` block for us where we edit everything.

So we'll go inside of that `renderInventory` and we'll say `return`.  Because it will be multi-line we'll use parentheses, we have a `<div>`, that needs a `className` of `fish-edit`, and that's just because I've already gone ahead and done the styling for you.

Also one other thing we need is when you are returning multiple React elements we need to give that a unique key.  Remember we ran into issues with that in one of the past videos, so we'll give ourselves a key, and where does this key come from?  Well, this `renderInventory` is actually going to get the key that we need.

Then inside of that - let's just do the first one here, just the actual name here of the fish.  Let's just get that one working and then we'll fill in the rest of them.

So we just need a regular input, type of `text`, and normally you would say `value={}` something like `this.state.whatever`, but we need to go ahead and say `valueLink = {}` - and this is where the magic sauce comes in, if I just said `this.state.fishes.fish1`, that would prepopulate it, but when I changed it it wouldn't sync it up with our `State`, so we don't need to do that just yet.  We call `linkState()`, and `linkState` is that method that's available to us that was made through our mixin, and inside of that we want to say `fishes.fish1.name`.

So we're saying link the state of `fishes`, which is our high level one, `.currentfish.name`, and of course we can't hard code `.fish1` because it's going to repeat for every single one, so what we can go ahead and do is just concatenate the key inside of there, so that's going to be `'fishes.' + key + '.name'`.

Let's make sure that we put in a closing forwardslash in there, because all elements in JSX need to be self-closing, even if they are self-closing elements like `<input>` or `<img>`.

So give that a save, and we're going to hit a couple of errors here, and I purposely have them in here just because it's going to help us understand what's going on with state and scoping issues.

So give it a save, and it's going to refresh for us and nothing will load.  If we open up our console here we get an error that says:

> Uncaught TypeError: Cannot convert undefined or null to object.

So what is that doing?  If we click on the error it will show us?  Ok, the error is happening if we run `Object.keys`, and it's telling us it cannot convert `this`, which is `null` or `undefined`, to an object.

So that means that `this.props.fishes`, that doesn't exist in our inventory.  So if we take a look at our React devtools, open up our `App`, and we want to click on inventory, because here we said `this.props.fishes`.  `Inventory` has `this.props.addFish()` and `this.props.loadSamples`, but it doesn't have `this.props.fishes`.  And that's because where does the `fishes` state live?  The `fishes` state lives in our `App` component which is right here, and we haven't yet made the `fishes` accessible to the child component, so this is a source of a lot of confusion in React.  It's a little bit frustrating that it's not just magically available to you, but that's what helps you build really nice tidy modular components.

So the way that we can do that is scroll down to find your `<Inventory>` tag, which we've done in the `render` function of our `App` component, and you'll see that here's `addFish` and here's `loadSamples`, and they're available to us, however we need to pass along `fishes`, right?  So `fishes={this.state` - because they live in `state` in the `App` component - `.fishes}`, and I just want to go back to the `Inventory` component and make sure that they are showing up.

So let's go to our `Inventory` component... there we are.  `fishes`, and there's all the `fish`es that we're used to, great.  however it looks like we got another error here.  It says:

> Exception was thrown by the user callback.  ReferenceError: linkState is not defined

If you go down here, `linkState` is not defined, and this is where we're trying to find the `fishes.key.name`

So again we have this problem where - where did we get `linkState`?  We got `linkState` on our `App` because we used our mixedIn to make it.

And so if we were to use `this.linkState` inside of our `App` component it would totally work, however because this mixin is not on our inventory and the reason we do it on our `App` is because our `State` also lives on our `App`, we need to be able to pass the `linkState` method down to our `Inventory`. 

So we're going to do the exact same thing where we can go to `Inventory`, we can say `linkState = {this.linkState}`, and then we'll go down to our `Inventory`, and where we're calling it you'll see that If it refreshes you'll get the same error, and that's because in our `Inventory` we're calling `linkState`, but right before `return` you need to say `var linkState = this.props.linkState`.

And that's just for shortform, I could have also gone in here and said `this.props.linkState` but I find that to be a little bit long, especially  as we do it over and over.

So, we've got our `linkState` and our `fishes` being passed down to the `Inventory` component, so if all goes according to plan - alright! Look what we've got here!  We've got all these inputs lining up with this, and I'm willing to bet that if I change this to "Lake Scallops" - look at that!  It's changing immediately.  If I change "Pacific Halibut" to "Hey There", "Cool Shoes", you can see that immediately they're updating

So we've done bi-directional databinding to this input, and I'm willing to be if we open up our Firebase and go to our `catch-of-the-day` app, I'm willing to be that - where are we right now - `clean-panicky-analyses` - if we open that one on up, here it is, open up our fishes, open up `fish1`, whenever I type into here "Hey hello how are you"... look at that!  No extra work, we simply just link the `State` to `State`, and because this Firebase is also linked up it kind of all just works, which is great.

So I'm really happy with that, let's go ahead and build out the rest of them.  I'd probably encourage you right now to just pause the video and try to fill out the rest yourself, because most of them are going to be straightforward.

The one that's a little bit difficult is going to be the dropdown of changing it from fresh or unavailable to available.

So pause it right now, give it like five, ten minutes, try do it yourself and then come back and then you can follow along with me if you'd like to do that.

So I've got this `<input>` here, I'm just going to  copy/paste it, just because it's the exact same thing, except you take out `.name` and you add in the `.price`, and let's give that one a save and double-check that it's still working, and then we'll do the rest.

So we've got "Hello, how are you", and then we've also got the price, cool.  If I change the price of something it'll update immediately wherever else we have it.

Now, the next one is going to be, if you look at our example right here, it's not an `<input>`, it's `<select>` box.  So how do you make a `<select>` box in React?  I just go ahead and make a `<select>` and that `<select>` doesn't need a `name` or an `id` in that case, but we do need the value link, and that is going to be set to `linkState`, the same thing as before, `('fishes. + key + '.status')`, and all you have to do here is make two different options of what the possible options could be.

So I need to give myself an option, that value is going to be `"unavailable"`, so there's no weird value link or anything like this.  And here you can just say `Sold Out` or whatever you'd like it to say, and then `"available"` is going to be `"Fresh!"`.

So we'll give that a save, and React should be smart enough to look at the `<select>` and pre-select the one that actually is available to you.

So let's look here, we've got "Fresh", "Fresh", so "Lake Scallops" by default, because it comes off our data, that one by default is sold out.  You can see that by default that one's selected whereas the rest of them are ready, and then I can just go ahead and change them, and they'll automatically be updated both in our `State` as well as our Firebase.

So that's how you do `<select>`.  Next one up is a `<textarea>` for the description, so how do you do a `<textarea>`?  because if you know the data generally goes inside of the `<textarea>`, however for ours we can still use a `valueLink` just like we did before, you can just grab one of these ones, save yourself a bit of time, `valueLink` and it's not `.status`, it's `.desc`.

Then finally we've got one more `<input>` for the image, and that will have a value like `.image`.  So give that a save, let's see where we're at right now. It loads them all in, I can change it to "Sold Out", I can change the price of it, I can change the name of it, I can change the description of it, and I can also change the picture as well.

So one last thing is we want to give ourselves a button here that says "Remove Fish", and we're actually going to hook it up in the next video.