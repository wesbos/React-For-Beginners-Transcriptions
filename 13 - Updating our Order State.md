# 13 - Updating our Order State

Alright let's move on to the next one , which is adding fish to our order.

We've got the fish displaying properly, which you can see right here.  However we don't have this little button, which when we click it it adds it to the order here, when you click another one it adds it to the order, when you click it multiple times it's going to increase the amount that we have to the order.

As well as that we have this kind of cool "Sold Out" button.  Whenever I change the state of one of these it switches from "Fresh", which is available, to "Sold Out" which is unavailable.

So we need to be able to do all of that in this video.  So go on over to your `Fish` component, and we're going to create a variable called `isAvailable`, and that's going to be a boolean that tells us 'is it available?' so it's `true`, otherwise it's false. 

So we can use a shorthand `if` statement in JavaScript to do that.  we'll Say `details.status`, and if the `details.status` is set to `'available'` then it's going to be `true`, otherwise it's going to be `false`.

Whoops, I put that backwards there.  So this just a quick shorthand `if` statement where `isAvailable` equals `true` or `false`.

Then the next one, what we need is the button text, because the text of this button is going to be dynamic.  It's either going to say "Add to Order", or when it's unavailable it's going to say "Sold Out!".

Now, I don't want to create two buttons for that case.  I'd rather just have one button and that allows me to do this animation that we have right here. 

So what we can do is make the text that goes in the button dynamic.  So I'll say `var buttonText = ` - and here we'll use another `if` statement - `isAvailable`.  So if it's available, then we will set the `buttonText` to be `'Add To Order'`, otherwise we'll call it `'Sold Out'`.

Then we're going to use these two variables while we just go down right below this `<p>` here and create this `<button>`.  Inside of that button goes the `buttonText`, because we just created it there.  We also have the `disabled` attribute, which is kind of cool if you've never used the `disabled` attribute.

If I inspect this right here, this is simply just disabled which doesn't let your user click it, and then I've also used the `disabled` attribute to jus tstyle the CSS a little bit differently.  

So we want to say `disabled={!isAvailable}`, and what that will do is if it's not available it's going to set `disabled` attribute.  Otherwise React would just take out the `disabled` attribute altogether which is a nice clean way to go ahead and do that for us.

Let's see that, we'll give that a save here, go back to ours, go ahead and load in the sample fishes and good, so we've got the `"Sold Out"` button, not available to us there, and we have got these `Add to Order`.

We don't have the ability to edit the inventory just yet, but you tell me how do we edit the inventory?  We can just go to React, we can find our `App` component, we can look at one of our fishes.

Let's do `fish1`, and we'lll just change the status from `available` to `unAvailable`, and as soon as state changes React will just immediately update this button right there.  So see how we did that?  We don't have to worry about anything sort of HTML.  So we did all of that in our JSX, and it's going to update automatically for us.  We just have to update the actual text, or the actual state here and it'll go for us.

So that's it, if I go ahead and click this it doesn't do anything, and that's where we need to actually code our method that's going to add it to the order.  So we're going to say `onClick = {this.onButtonClick}`.  

So we're going to call a method called `onButtonClick`.  And I'm just 
calling it `onButtonClick` just to show you that it doesn't matter what you're calling these methods.  If we're referring to `this`, in our case `this` refers to `React.createClass`, so it refers to the `Fish` class.

So we can say `onButtonClick` is a function, and inside of that we can say `console.log("Going to add the fish: ")`, and how do you access the current fish that's being clicked?  Well it's `this.props.index`. 

Inside of any of these functions that's related to your `Fish` component, you could always access `this.props.whatever-it-is` as well as  `this.state.whatever-it-is`.

So let's give that a save and open up your JavaScript console here.  Load in some sample fishes and click 'Add to Order'.  You see that we're triggering the `onClick`.  We're triggering our `onButtonClick` event.

Again I could name this `addFish` - I'm not going to name it that as it's a little bit confusing with some of our other methods, but if I rename it there as well as here, load up some sample fishes I can always call it from there.  So I'm gonna bring that back, the `onButtonClick`.

Now what we actually need to do here is add this index, this key to our `order` state.  Now you're asking me 'where is our `order` state?  Is it attached to `Fish`?' 

it's actually not, it's attached to our `App` component, right?  we've been working with the `fishes : {}` state so far, but now it's time to put that aside and work with the `order : {}` state.

So we need a way to get this index into the parent state, and the way that we do that is we create ourselves a method on our `App`, and we'll pass it down to the `Fish` component via a `prop`.  So let's go ahead and do that, we'll say `addToOrder` is a function, and that function there is going to take in a `key`, which is going to be `fish1`, `fish2`, `fish3` etc., and inside of `addToOrder` we'll say `this.state.order[key]` - in our case because `key` is a variable we need to use square bracket notation.

Now before I go and update the state I actualy want to show you sort of what we're gunning for here so it makes a little bit more sense.  So I'm going to figure out the answer, I'm just going to open up the React devtools, I'm going to find the `App` component - there it is.

If you open up our `State` and go to `order`, you're going to see that `order` is simply just a list of keys - it's an object -and the value of how many pounds we're ordering, so when I add additional pounds you'll see that `fish1` is added, and then when I add something new like - we don't have mussels in our order yet, when I add it it's going to add the `key` for mussels which is `fish8`, as well as when you add another one it's going to update that to `2`.

So that's kind of what we're going for, so what we need to say is `this.state.order[key]` equals itself plus one - this is a little bit confusing - or one.

So what's going on here?  we're going to set it to itself plus one if there already is a value there, otherwise it's going to be set to one.  So in the case of 'Pacific Halibut' we already have five, so when I click 'ADD TO ORDER" it's going to set it to itself (which is five) plus one (which is six), and then when I add something new like 'Jumbo Prawns', I don't have that yet, it's not going to set it to itself plus one, it's going to set it to one.  That's kind of a neat little trick you can do with the `or`, you don't have to do any sort of `if` statement there.

So that will update our `state` object of `order`, but again `state` doesn't actually update the HTML until you then go ahead and call `setState`.  So we call `this.setState` and you're going to pass it an `order`, where we're telling it we're updating `order`.  We say `this.state.order`.

Now before we can go ahead and use it we need to make this `addToOrder` method available to us inside of the `Fish` function.  Right now it's only available inside of our `App` component, however the clicking happens where?  It happens right inside of our `Fish` component.   So we're run into this issue before where we need to be able to access methods and data inside of a child component, and the way you can do that is you can pass it via `props`.

So let's go down to this `Fish` component.  We're already passing `key`, `index` and `details`, so we can also pass the `addToOrder` method and set that to `this` - in our case `this` equals the `App` component - `.addToOrder`.

What that will do is make this `addToOrder` method available to us inside of our `<Fish>` method.  Then we scroll down to our `Fish` component - sorry that's the `Fish` component here - and inside of our `onButtonClick` we can say `this.props.addToOrder()` and that `addToOrder` extracts one thing, a key, which is like `fish1`, `fish2`, `fish3`, so how do we get access to that?  Well it's actually stored in `this.props.index`, and you can put it in there.

If you'd rather have a variable called `key` what you can do is just create a little variable called `key` in front of it, set it to that and pass it in like that.

So give that a save and let's try it out.  I'd like for you to open up your React Devtools, and open up these arrows here.  Click on `App`.  Now open up your `state`.  There's nothing in there when I load in some sample fishes immediately our `state` is updated, one through nine.

Open up our `order`, there's nothing in there but when we add to order here it should update it for us.  There we go, `fish1` - one of them. Add it again; two. Three, four.  Try and add `Mussels`, there we go, `fish8`: one yadda yadda.  Every time you click it it's going to update it.

So in the next video what we will do is take that data from our `order` state and be able to display it in here.