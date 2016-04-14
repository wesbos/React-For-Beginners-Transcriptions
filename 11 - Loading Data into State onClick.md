# 11 - Loading Data into State onClick

Now, to make it a little bit easier on ourselves so we don't have to fill out this form every time we want some sample fishes, what I've done is I have a `sample-fishes.js` file which includes a whole bunch of fish.  I've pre-given them keys so they don't have timestamps on there.  It's not a big deal as long as they're unique.  Then each one has a name, an image, a description, a price and a status.

So what we can do is load in the `sample-fishes.js` and pre-populate it whenever someone clicks a button.  So this is also a good review for our `onClick` button.

So right below the `addFishForm` let's just create ourselves a button that says `Load Sample Fishes`, and when someone clicks that - so we're going to use an `onClick` handler, we're going to say `this.loadSamples`.

Now again you may think `this.loadSamples` - well we could go ahead and make a `loadSamples()` method inside the `Inventory`, but we don't want to load the samples into the `Inventory` state, we want to load the samples into the `App` state just like we did when we filled out this form.

So what we need to do is go back to our `App` component, and right under `addFish`, let's say `loadSamples`, and what that function would do is - we'll just go ahead and call `this.setState`, and we're going to tell it `fishes`, and we're going to `require` the `./sample-fishes`.

What that will do is it'll go to `sample-fishes`, grab this entire `fish` object, and then immediately set the state on that.

So we put a semi-colon there and that will load them on in for us, but now the question is: how do I call the `loadSamples` method from another nested component?

Because again, `loadSamples` belongs to `App`, however our button is inside of the `Inventory` component.  Well, again we pass the `loadSamples` via props.

So again simply go right beside where we said `addFish`.   Say `loadSamples={this.loadSamples}`, and what that will do is make the method available to us, we can go down to our `Inventory` component, and we actually don't want to say `this.loadSamples`, we want to say `this.props.loadSamples`, because the method is going to be living in the props, not right on the component.

So give that a save, and I'm going to open up our app here, and another way you can quickly find one - instead of opening these arrows every time you can simply just search for "app", and you'll see that our `State` is empty, but when I click on "LOAD SAMPLE FISHES" immediately it's populated with all of the fishes.

So in the next video what we're going to do is now that we have these fishes loaded, how do we get to displaying them under our menu.