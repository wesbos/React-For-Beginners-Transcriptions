# 12 - Displaying State with JSX

Now that we have data in our state we want to go ahead and display it.  The way we display it is by using JSX in our render function.

So take a look at our example here, I'm going to go to a new store.  In the last video when we click "LOAD SAMPLE FISHES", that will put some fishes in our state, and as soon as the state is populated you notice that our HTML is immediately updated with all of the list of fishes and photos and everything that goes along with it.

So with ours, if we just go to one right here and open up our React DevTools again.  Go to our `App` component because that's where our state lives.  

If we open up our `fishes` here you'll see it's an empty object.  I click "LOAD SAMPLE FISHES" and that's immediately populated with all the fishes.  You'll notice this is an object, but we need a way to display them inside of here.

So we can go to our `App` component, go to the `render()` method and right underneath `<Header>` we're going to create an unordered list with the classname of `"list-of-fishes"`.  Then inside of that we need some sort of way to loop over every single available fish.

Now, JSX doesn't have any sort of special templating logic built in.  We don't have any sort of loops in JSX, we just have plain old JavaScript, which is totally fine for us because we can go ahead and give ourselves two curly brackets which tells JSX that we're going to be running some JavaScript here, and we can say - generally the way to loop over data in JSX is to use JavaScript's `map`, however `map` only works on an array, and if we look here we have an object.

So we can say `Object.keys`, and what `Object.keys` will do is it will give us an array of all of the keys that we have here, so `fish1`, `fish2` and `fish3`, and I can show you here if you go to console and type `$r` and hit Enter, it's going to show you the actual component here, so `$r.state.fishes`, that gives you the object, but if I put `Object.keys` on there it returns us an array of all the keys, so `fish1`, `fish2` - that's what we want to work off of, because as long as we have that key we're going to be able to reference state and get the rest of the information about the fish.

So `Object.keys(this.state.fishes)`, (because that's the object), and then on that we'll call `.map()`. If you've never used `.map()` before, essentially what it will do is take in an array and allow you to use the data in that array to return a new array.  In our case it's just going to be a whole bunch of JSX.

Now instead of writing the `map()` function which will run once for every fish inside of here, what we can do is pass it off to another method that lives inside of our component.

So we'll say `this.renderFish`, and that will run `renderFish()` function once for every single fish that we have in our state, and we need to go ahead and create that, so we'll say `renderFish` is a `function` that takes a `key` in, and it'll return some sort of JSX.

So let's just use a `listItem` now, and inside of that `listItem` we'll just say `Welcome` and then we'll do the `key`, so this is - remember curly brackets will give us an actual variable in there.

So give that a save and when I click on "LOAD SAMPLE FISHES" you're going to see:

```javascript
Welcome fish1
Welcome fish2
Welcome fish3
...
```

That's because once for every fish we have it's running this `renderFish` function.

Now we can take that even a step further and make our app really modular and really componentised, which will allow us to do something like - we should just create a `<fish>` tag, where any time I want to make this little display here, like I want to show the photo, I want to have the "Add to Order" button, I want to have the title and the price, I should be able to just pop in a `<Fish>` tag and pass it a little information about the fish and it will display itself on out there.

Because, chances are - maybe I want to display this exact component somewhere else on the website, maybe I want to pop it into a popup where I say "hey, thanks for checking out.  Would you like to add some lobster, it's on sale?" and I could just render out that `Fish` component there.

So instead of using a `<li>` let's give ourselves a `Fish` component, and that `Fish` component needs a couple of things here.  We're going to pass it a `key`.

Now, whenever you render out a component or whenever you render out an element in React you need to give it a `key` property, and that `key` property needs to be unique.  Why do we do that?  It's because React needs a unique key to be able to track it, so that when there's a change to this particular fish, so when there's a change to just `Lobster` it knows which element on the page to update and render out itself while leaving the rest of them untouched.  So the `key` will be just `key`. 

We need to also say `index={key}`, and that's just because you can't access this `key` value when you're inside of the `Fish` component, and I'm going to need that later - you'll see why - and then finally we'lll need the `details` of the fish, and how do we get the details that we need?  What we can do is while we already know that all of the details live on `this.state.fishes`, and we want the specific details for this fish, so we can do `[key]`, and what that essentially will give us is `this.state.fishes.fish1`, `.fish2`, `.fish3`.

So I've gone ahead and made this `Fish` component, however we haven't made - I've used it but I haven't made it yet.

So let's go just below our app and make it.  We'll call it the `Fish` component.  I always like to show how you use the `Fish` component - look it even looks like a fish with a little tail on it! - and we'll go ahead and create it, so `var Fish = React.createClass()`, and you tell me what's the one method that every single component needs?  It's a `render` method.

And that will need to take a function, and we will return some sort of JSX.  I'm going to give ourselves some - why do we use these parentheses sometimes and sometimes not?  When you want to do multiple line JSX you need the parentheses, and inside of that we'll give ourselves a `<li>` item, and `this.props.index`.  Remember earlier I gave it an index here?  That's because I want to be able to just output `fish1`, `fish2`, `fish3`.  We also pass it the detail, so we could say `this.props.index` or `this.props.details`.  We can't exactly do that because that is a full object full of information.

But let's go ahead and use `index` and try it out.  Load in some sample fishes and immediately we're writing out `fish1`, `fish2`, `fish3` - that's exactly what we had before, but if we go to our React console and open up our `App` component, you'll see now that instead of having just `<li>` items we have a `Fish` component.

What does that `Fish` component have?  It's got an `index` as well as a `details`, and you can see in here we can open up the `props` showing the `details` and it gives us all the information that we're actually looking for.

So from here on out it's really just a templating issue for us, which we can have a little fun with.  So we've got a `<li>` item inside of there, and that `<li>` item just needs a class with `menu-fish`.  This is just for CSS styling.

Now we want to give ourselves an `<img>` tag. The source is going to be set to - now you might be tempted to put your template tags right inside of the quotes, but when it is simply just a variable you don't need the quotes and you can say `this.props.details.image`.  Now that might seem a little bit long.  Let's double check it works and then we'll move that on out a little bit.

We've got an error in our console here, and it's telling us:

> Expected corresponding JSX closing tag for <img>.

It's expecting a closing `<img>` tag.  Now, we all know `<img>` tags don't have closing tags, but React is funny so we need to actually self-close that `<img>` tag there.

So give that a save, there we go - it's working for us.  Load the sample fishes - whoa!  Right away, we're loading in all of these images for ourselves.  Now you can get the point where we say `this.props.details` dot-whatever else we need from there.

One kind of thing that we could do is before our return statement say `var details = this.props.details`, and then that allows you to just use the `details` variable.  It makes our templating a little bit cleaner.

Same with the `alt`, we can go ahead and say `details.name`.  Underneath that we need an `<h3>` that has a `className` of `"fish-name"`.  I also want to give this one a `className`, not a `class` - you can't use `class`.

Inside of that `<h3>` we need `details.name`. We also need a `<span>` that has a className of `"price"` on it, because if we're looking at what we're working towards, this little `<span>` right here is inside of the `<h3>` and it's styled a little bit differently, and inside of that `<h3>` we have a `<p>` button and it'll give us a `details.desc`.

So, give it a save, cross your fingers.  We'll see what it does when we load something into our state.  Load the sample fishes... alright!  This looks really good.  We've got our image, we've got our title, we've got our description.  

However the only weird thing here is that we're pulling in the raw number of cents that this item is, and we don't want to do that.  If you remember that we have our `helpers.js`, we've got this nice little function called `formatPrice`.  You pass it in cents, it returns a formatted string of currency. 

So we already have pulled it in via `h`, so all we need to go ahead and do is right around `details.price` say `h.formatPrice(details.price)`.  Give it a save, and that should give us a nice formatted dollar value.  So load 'em in... beautiful, got those.