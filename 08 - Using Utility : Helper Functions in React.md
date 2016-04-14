# 8 - Using Utility / Helper Functions in React

Before we get into Events I wanted to talk real quick about Helpers. This is not specific to React, this is actually just something that you do quite a bit in JavaScript, and it's essentially when you have some utility functions where - for example in this app right here I'm just randomly filling in this `Store` box with some kind of random human-readable text, so I don't have to always mash my fingers into this box.  And it's also fun, makes me laugh.  And when you submit it, I load in some sample fishes and I add in a whole bunch of stuff.  You'll see that throughout the app I'm formatting the dollar value all over the place, and the amount that's stored is the `cents` value.  So if I put in 2000 cents it's going to format it as $20.00.

So these are just small little utility functions that I like to use throughout the app, and they're not really associated with any one component, so what I like to do is create a file called `helpers.js` and this is just full of functions that help me.

So for example I've got a `formatPrice` function here which will take in the amount of cents and output the formatted price here.  I've got one called `slugify` which we're going to use as well in just a second.  It will take any string of text and make it URL-friendly, and then I've got one here called `getFunName`.  Essentially it's going to take a list of adjectives like `'adorable', 'beautiful', 'clean'`, and a list of nouns like `'women', 'men', 'children', 'teeth', 'feet'`, and it's going to come up with sort of a fun store name here.  Every time we reload the page it's going to give us a random one.

So how do we actually use these functions here?  Never mind this `export default helpers`, we'll talk about that when we refactor for ES6, but the way that we get these functions - like if I wanted to call `helpers.rando` or `helpers.slugify`, we need to import this `helpers.js` into our `main.js`. 

So go to the top here and give ourselves - we'll call it `h`, so we could call this `helpers`, `wes`, we could call it `cool`, we can call it whatever you want, however whatever you call it here is going to be - all of it is going to be `helpers.slugify` or `h.slugify`, or you can even call it `$` if you wanted, but I'm going to stay with `h`, and so far when we've used `require` we've been requiring npm packages:

```javascript
require('react');
require('react-router');
```

So how do you require helpers?  Do you just type `require('helpers')`?

No, unfortunately not.  What you do is require these JavaScript files directly.  So we are in `main.js`, and we want `helpers.js` which is a sibling of that, so we can do `./` - and what the dot means is the current directory - `/helpers`.  And you can say `helpers.js` but the `.js` is not necessary when using commonjs it will figure it out for you, and then put a semicolon on there.

Now, this `h` is going to be supercharged with all of the methods like `formatPrice`, `slugify` and `getFunName` and we're going to be able to use them in our app right here.  So let's go down to our `StorePicker` element here, our `StorePicker` component.  And what I want to do is this input right here - if I go to this right here, I want this to be automatically filled up. 

So if I say `value="testing123"`, give it a save.  I'm going to open my console here so we can see something.  It will fill it in for us, but again React gives us a little bit of a warning here which is kind of cool:

> Warning: Failed propType: You provided a `value` prop to a form field without an `onChange` handler.  This will render a read-only field...

Essentially what it's telling us is use `defaultValue` attribute instead of the `value` one if you want to set a default value.

So we're going to change this to `defaultValue`, give that a save and let's see if it refreshes for us.  So that's `123` but I want it to be dynamic.  I want it to be set to - well, if we take the quotes out of here and use curly brackets.  If we use curly brackets in JSX that means we're running some JavaScript, and we're simply going to call `h`, which is our helpers, `.getFunName()`.  

And that `getFunName()` is going to go in here, figure out a fun name based on these adjectives and the nouns, and it should populate for us.  So give it a save, refresh, there we go.  Every time I refresh now - "grumpy-nervous-teeth" [laughs] - we're getting all kinds of hilarious names here.  Cool!