# 10 - Understanding State

This is the first video where we're going to be talking about talking about state, and state is a pretty fundamental concept in React.  

It's actually a concept that's shared in all the frameworks like Backbone and Angular and Ember, so if you're going to take some time to go over a video a couple of times it would definitely be this one.

So what is state?  You can think of state as a representation of all of your components' data.  It's one big object that holds all the information that's related to your component.

In the case of our application here we have two things that need to be stored in state.  We've got a order which contains what fish the person has bought as well as how many of those fish the person has bought.

So one great way to look at it and one nice way to think about it is sort of - state is your master copy of all of your data, and if you've ever used JQuery or something like that you may be used to storing your data in the actual DOM, in the HTML, and in React (and all of these frameworks) we don't actually do that at all, it stays away from that entirely.  We store all of our data in an object, and the HTML is all based off that.

So if we ever want to make a change to the title of a fish to the price of a fish, to the number that we have, we don't go and change the HTML, we change our state and React will update our HTML accordingly.  That's what the next two videos are going to be about.

So I'm going to open up my `<Router>`, `<RoutingContext>` and click on `App` just for my example here, because on our `App` that's where most of our state will be held.  You go over here and you see `State`, we've got `fishes`, and `order`. 

If I open up `fishes` you'll see this is a list of all of the fishes.  Again this is just one big object, and if I open that up it shows me the description, the name, the image and the price.  So if I were to go ahead and just edit one of those, just call it "Pacific Hali", and hit Enter, you'll notice that right here as well as right here those both were updated.  

The same goes for - so I've got some fish in my cart, it's $17.24/lb, so the price is being pulled in here.  The price is being pulled in here but multiplied by how many pounds we've bought, and then the price is being pulled in this Inventory Management as well.

So if I went ahead and changed that from $17.24 to $19.24, watch when I hit Enter: that changes, the total updates and that one changes.

So wherever I've referenced `State` in my HTML, it's automatically going to be immediately updated.  I don't have to go through all of my HTML and make sure that I've properly updated every single one.  We've all done that with some JQuery applications, and while it might be the way we have learned it's really just a pain to do so.

So there's all of my `fishes`, and if I open up the `order` you can see that we're keeping log of - I've ordered four of `fish1`, which is the key for the "Pacific Halibut", and five of `fish2`, and then if I add more you'll see that it just turns green because I've totally changed that amount. Same goes for here, we've built a little inventory system here, because you can't tell your users to open up the console, but every time I change it here you'll see that it gets updated down in `State` here.

So again, `State` is one big golden master digital representation of your data, and your HTML and all your application is based off that.

So the first thing that I want to do is create the form that's at the bottom here, where I'm able to add a new Fish.  So I can add the Fish name, the price, whether it's available or not, the description, and I can go ahead and add it to my `State`.

Let me show you an example: If I add a `Trout`, that is $10, it's fresh, "Great Fish" and `:fishpic1` - I've just got a random `fishpic` there, and when I go ahead and add that, it adds it to `State` and automatically my HTML will be updated.  I can add that to my order.

So I want to build that, we're going to add it to `State` and in the next video we'll take everything in our `State` and display it under the menu right here.

So head back to your `main.js` and scroll down just below the `App` component.  We'll keep these sort of together.  In a further video we're going to refactor this so we don't have as many components in the same file.

We will create ourselves one called `AddFishForm`, and that will just be the form to add a fish.  The reason I'm not going directly into the `Inventory` component right here is because this `AddFishForm` is in the inventory right now, but what if I'd like to put it somewhere else.  

So ideally we're going to have a little component that says `AddFishForm` and you'd be able to pop that component wherever you'd like to Add Fish Form.  So we'll create it:

```javascript
var AddFishForm = React.createClass()
```

You're probably getting the hang of this!  See how everything in React is a component?  And that of course needs a `render` method, and let's just do `return()` - I always like to make sure everything is working before I actually build it, so `<p>Testing this is the fish form</p>`. 

So give that a save.  Now, it's not going to show up anywhere on our page because we just created the component, we haven't actually shown it. So how do I get this `AddFishForm` to show up?

Well, I can just go to our `Inventory` component which is right here and delete that `<p>` where there's that `Inventory`, we'll give ourselves an `<h2>` instead that says `Inventory`, and then below that give yourself your component, `<AddFishForm />`.  You make sure it's self-closing, you have to do that with all components in React.

Then you give that a save, go back to our - here.  Oh, I got a little error.  Let's debug that debug that together, it's kind of interesting to see.

It says:

> adjacent JSX elements must be wrapped in an enclosing tag while parsing file: ...

So what happened here is that you can't return two sibling elements.  You can only ever return a single element, however if you have multiple elements you can simply just wrap them in a `<div>` or any element and then just return that.

So there we go, let's see - yeah it's not yelling at me, I'm in good shape now.  We'll submit it and there we go: "Testing this is the fish form".

If I open up our React DevTools, open up the `<Router>`, open up our `App`, open up our `Inventory` component, see how you can see just the `App` component, the `Inventory` component and you open that on up there's our `<h2>`, our `<div>`, but here, `addFishForm`, there's our React component.

So now we don't have to worry about doing too much work in this little `Inventory` component, we'll come back to it in just a second, but we can go back on up to the `addFishForm`.  

I'm not going to make you sit here and watch me type up HTML for the next five minutes, I'm just going to copy/paste that in from one of our answer keys, and I'm just going to walk you through exactly what we've got going on here.

So we've got a `<form>` tag, and we're giving it a `className` and that's just for styling.  We have an `onSubmit` which we will be talking about in just a second.  Then we've got our `input`, type of `"text"`, `ref` of `"name"`.  Remember we used `ref` a couple of videos ago in order for us to be able to reference each of these elements and their values.

We've got a placeholder, we've got a `select`.  It's either going to be `Available` or not, like `Fresh` or `Sold Out`. We've got a `textarea` for the description, we've got an input for the URL, and we've got a button which when you click it it'll submit the entire form.

So give that a save and let's open this up.  And you see we've got Fish Name, Fish Price, you can put whatever you want in there, Description, URL to image, and then we've got our "Add Item" button.

Good, so the HTML is rendering properly, however if we open up our console and click `Add Item` you'll see that it just refreshes the page for us.  That's because when the form is submitted we actually need to run a function called `this.createFish`.  Remember when we had the `StorePicker` we had an `onSubmit` which said `goToStore`, now we want to have another event that says `this.createFish` 

So just go right above your `render` here and say `createFish`, it's a  `function()`, and that function should take in `event`.

Now we want to do three things.  First we want to stop the form from submitting.  We want to take the data from the form and create an object.  Then finally we want to "3. Add the fish to the App State."

So that's going to be a little bit tricky because we're not really concerned with the state of the `addFishForm` component, we're actually concerned with the state of the app.  So how are we going to get our data from `addFishForm` to `Inventory` to `App`?  Three levels higher, but we'll cross that bridge when we get to it.

First of all, stop the form from submitting.  That's pretty easy, it's `event.PreventDefault();`

Then `// 2. Take the data from the form and create an object`, so say `var fish = ` and that fish needs a `name`, a `price`, a `status`, a `description` and an `image`.  So all of those are just going to be properties on the object.

Now where to we get that object?  Let's do the first one first with `name`.  Input type is that, `ref` is `"name"`.  So remember we did `this.refs.name.value`, because `this.refs` gives us a list of all of the inputs, and `.name` will give us the input itself and `.value` will give us whatever the user had typed into that.

So that's the first one, and then you can probably extrapolate that for the rest of them: `this.refs.whatever.value`, and let's just go ahead and once that's done, `console.log` the fish. 

So let's see if we have got everything working properly.  Fish Name: "trout", "1000", "testing", I'll just say "123" here, "hi.jpg".  It won't actually work, so go ahead and submit.

What have we got here?  We've got an object where the name, the price, the status, the description and the image are all available to us.

So that's the object, now we need to take that Fish object and put that into the `State`.

I said earlier that the `State` is not living in the `addFishForm`, it's actually living in our `App`, so we actually need an initial state, something to start off with.

That's generally a blank state, and then we can add into it.  So go to your `App` component and type `getInitialState`, and what that is is a function that runs, and that will return the initial `State`, which is just an object and inside of that we have a `fishes` object which is also a blank object and an `order` object which is also blank.

Those are going to be populated once we start clicking the buttons and loading.

So `getInitialState`, that's just part of what's called the React lifecycle.  Before React creates the component what it will do is run `getInitialState` and it will populate itself with anything that is in here, so we don't put anything in there but we at least need it there for us to be able to push it into there.

Then we also need a method that's going to add our new fish to the `State`, and we can't do that from the `addFishForm` directly, we need to create the method on `App`.  We're going to be doing this quite a bit.

So the method that we're going to create is called `addFish`, and that is going to be set up as a function. We should expect one argument and we're going to pass it the `fish` that we want.

We need to give each `fish` a unique key, so if we go to our answer right here, and I'll open up our app and look at our `fishes`, you can see that each of these `fishes` have a unique key. `fish1`, `fish2`, `fish3`, `fish4`, all the way through - those are the default ones.

What I've done here is I've just used a timestamp to ensure that all the `fishes` have a unique key.  Another benefit to doing that is they will always be in the right order.  They'll show the newest `fish`es at the top, so the timestamps are incremental.

So what we can do is say `var timestamp = (new Date()).getTime();` and if you've never done that before, what `new Date()).getTime()` does - if I click on my console here - every time I run that it just tells me how many milliseconds since January 1, 1970.  That number is always going to be unique unless you're creating two `fish`es in the same millisecond.

So I have that there, that's just a nice easy way for me to get a unique timestamp.

Then the way that we update and set - so we're going to say `// update the state object` and then we're going to say `this.state`, and that refers to - `state` is this entire thing right here - `.fishes`, because we don't want to overwrite `order`, and then we're going to set it to `'fish-'` (we put that in quotes because it's a string).  

Normally it'd be `fish1`, `fish2`, `fish3`, but because I have this unique timestamp in order to get unique keys, we'll say `['fish-' + timestamp] = ` - well what does it equal?  We passed in that `fish` object that we made earlier, put it right there, so what that does it it updates the `State` object but that alone is not enough for React to re-render it, because you could be doing a couple of things on here.

You actually have to go ahead and call `this.setState()` on React, and again you can't just call that as well.  You have to pass it an object of what has changed inside of the `State`.

So you shouldn't just pass `this.state`, because what that will do is it passes React the entire `State`, and it has to check 'alright, here's my old state and here's my new state'.

For example if I went ahead and changed this to "Rainbow Trout", what it's doing is saying 'okay, my last state just said "Trout", and my new state says "Rainbow", and it's going to compare the two and figure out where in the DOM, where in your HTML it needs to update.  That's the whole virtual DOM that you may have heard about in React.

So you don't want to pass it the entire state because that's going to slow it down, that's going to be more work than it needs.  We know that I didn't change `order`, so we can just say I specifically changed the `fishes` object, and it's going to be `this.state.fishes`.

So I thought that was kind of weird at first, that you have to explicitly tell it to set `State` and pass it itself, but after really reading into it and talking to a few people as to how that works and why it works that way it's done entirely for performance so that it can do as little comparison as possible and keep your app nice and snappy.

Now, if I fill out some data here and click "Add Item" you'll notice that nothing happens, and that's because what we're doing here is when the form is submitted, we're creating a `fish` object but we still haven't done this third thing called `// 3. Add the fish to the App State`.

So this is where when I was first learning this I found it to be a little frustrating because I can't just go ahead and call - you might be thinking like "okay, so why don't you just call `App.addFish` and pass it `fish` right?  That would make a lot of sense, you've got this `fish` object, you pass it, however the method `addFish` is not in `addFishForm`, it's in `App`, and it's called `addFish`.

So unfortunately it doesn't work that way, and in order to get to the `addFish`  - `something.addFish` method, we have to pass this method of `addFish` down from its parent down to the child.

So if we look at the component we're in right now, we are - the method is on `App`, and if you open on `App` there's `Inventory` and inside the `Inventory` there's `addFishForm`.

So we need to pass the method from `App` to `Inventory`, and then `Inventory` needs to pass that method from itself down to the `addFishForm`.

So what we can do is use `props` to pass things on down, so `Inventory` is going to get one called `addFish=(this.addFish)`.

Now give that a save and I'll go down to my `Inventory`, and inside the `Inventory` it will now have the `addFish` function, but we still want to pass the `addFish` down to `addFishForm`, so we could say `addFish=(this.addFish)`, and that's essentially what we're going to be doing here is passing it down and down.

It may seem kind of repetitive, and I agree that it seems repetitive, but that's just the React way of doing things, of keeping things nice and module.  If you do want to share you have to use `props`.

You can imagine if `Inventory` had three or four props that I wanted to pass down to `addFishForm`, or maybe you want to pass down things from `App` down to `Inventory` down to `addFishForm`, and who knows - you might go three or four levels deep with your components - they can become really cumbersome to have to maintain a list of four or five different props that you passed down from each one over and over again.

So the way we can get around that is by using what's called a `spread`.  In a `spread` you just put two curly brackets, ..., `this.props`, and essentially what that will do is take all the props from the `Inventory` component and just pass them on down to the `addFishForm` component.

You want to be careful with that, it's tempting just to put that everything so everything has everything on everything, but you really only want to pass down what you need.

Now, `addFishForm` will have all of my methods, and we can go back to our `addFishForm`.  It's `this.props.addFish`, and you pass it the fish that you were creating.

So give it a save, fill this out, "Trout", and click `addItem`, and nothing happens.  However, let's go ahead and open up the `<Router>` open up that.  Click on `App`. Here's our state and there it is, here's the fish that we created with the "testing", "testing.jpg".

If I go and change this to "Salmon", add the item, you'll see immediately it's added on to there, and one other thing I wanted to show you is if we click on `Open App`, we click on `Inventory` you see that there's props, it has `addFish`.  That's the method we passed down from app, we'd pass it on to `Inventory` and if you open that up `addFishForm` also has `addFish` available to itself, because we used that `spread` to pass it on down.

Now, you'll now see when we add the item, the form doesn't clear.  We can easily fix that. If we go to our `addFishForm` component, and in the form we can give it a ref of - let's just call it `fishForm` - and we can -- after we add the fish we say `this.refs.fishForm.reset()`, and what that will do is reset the form and clear all the stuff out.

So if I put some stuff in here, add the item, it will clear it out after it adds it to the `State`.