#  20 - Component Validation with PropTypes

One of the things that I really like about React is that it forces you to write really well structured code, and it makes you write your code in a way that's going to make you both a better developer as well as help your project succeed.

So while some of the things we've been doing so far may seem like they're a little bit harder than they should be, it's just because we're writing nice modular components that can be used over and over again.

Now, one of the tools that we can use in React to sort of help us along this way is called PropTypes.  You can think of PropTypes as sort of a validation for your component.  It's going to help us write a little bit more resilient components, given that you might not be the only one using your component.

So so far we've made all these different components, but the idea behind React is that you build all these components and you can share them off to other people in the React community, as well as you may be working on a team where five or six different people are writing a React app.  The person that authored the component might not be the person that ends up using it at the end time.

So PropTypes will essentially validate any properties that get put in to make sure that you're passing it the correct type of data.  So for example we've got this header here with a tag line, and I might not know that I need to pass in a tag line to it.  I might just give myself a header component, and rather than us getting a weird error or a layout issue when we don't pass it in, what can happen is React will sort of alert you to say 'hey, you actually forgot to pass in a tag line', or 'hey, you passed me in a number for the tag line and I was expecting a string'.

So I just took a look at a random JSX component that I found online, just to show you sort of how this works.  So this is a React component called "tabs", and if you ever need to implement tabbing into your application this is going to be a fantastic component for that.  And since I didn't author it, I might not pass in all the required properties that it needs.

It's sort of like when you have a JQuery plugin and you don't pass it in all the correct settings and it'll error out.  This will validate that you passed in the correct stuff.   So here we have a PropType.  This was written in ES6, which we're going to do in our next video, but we make sure that the name when it comes in, that it's a string and it's required.

We make sure that when someone passes a `clicked` property that it's a true function and not something like a string or a number that they pushed on etc etc.  So PropTypes is a fantastic way to make your component just a little bit more resilient.

So what I wanted to do with you is let's go to our application and add PropTypes in so that whenever you're using any of these components in the future they will throw a warning.  So remember the first component that we ever made, that was the `header` component?  That was a nice simple example as to how we could just pass in the tag line and have that change.  So if I look at my `App` right here and it says "Fresh Seafood Market", if I were to change that to "Wes is Cool", it's going to pull in a different string and put it into the header here.

Now what we want to do here is if someone else were to use this `header` component somewhere else on my website, I want to make sure that they're actually passing in a string and not some sort of number or a function, so let's go ahead and do that.

I've just got this `main.js` open in two different tabs just so that we can be able to see each of them and sort of visualize what's going on here.  We've got this header component here, so scroll down to where we've defined our header component.

Now we've only got the `header` component defined so far, and we need to go ahead and define something called `propType`.

So give yourself a comma, `propTypes` - it's got a capital 'T' on there - and `propTypes` is going to be an object, and we're just going to list all the different properties that we pass into our component.

So the best way to do that is you can either search inside your component for any time that you've used `this.props.`, or you can go back to where you're actually using the component, which in this case we've used it under the `render` method, under our `App` component, and here it is.

The only property that we're passing is the `tagline` property here, so we can say `tagline` is going to be set to `React.PropTypes`.  Now, `React.Proptypes`, that is a listing of all the different types of PropTypes, and if we go to the docs here on React you can see that there's all kinds of different ones.  There's `React.Proptypes` - you could make sure that it's an array, you can make sure it's a boolean, you can make sure it's a function, a number or an object, a string, a node - like something that's in your html - a different React element that you made, so maybe you can be checking if it can be passed another React component.  We've got `instanceOf`, `oneOf`, etc etc.  All kinds of great stuff.

You're probably going to get away with almost all of these most of the time, unless you have a bit more of an advanced example you can look into creating your own.

So we've got `tagline : React.PropTypes.` and you tell me, this one needs to be a `string`, so you can look here.  It's `string`, lower case.

And then we can also pass a `isRequired` on the end, and that's going to make sure that it is both a string as well as - if they omit it entirely then it's going to yell at them again.

So give that a save, and we go back to our application.  It's probably helpful to open up your consoles because that's where the errors are going to be.  We don't have any sort of errors, but let's go ahead and and go to our header and just remove the `tagline` entirely, and see if it starts to yell at us.

There we go:

> Warning: Failed propType: Required prop `tagline` was not specified in `Header`.  Check the render method of 'App'.

This is super helpful, because it's telling you exactly you forgot the `tagline` property.  It's on the `header` component and you're rendering it out inside of the `App` component.

So it gives you three really valuable pieces of information there, especially if this was not your component.  You might not necessarily know that you need to pass that along.

So let's go ahead and put that back in, and let's change it from - not to a string, but let's make it a number.  It's going to tell you 

> Warning: Failed propType: Invalid prop `tagline` of type `number` supplied to `Header`, expected 'string'. Check the render method of 'App'.

We didn't get a string, we got a number, so we have to pass it in as a string.

So that's really it, it's just pretty straight forward, super helpful.  It's a little bit of extra work to go through your application, but what I would recommend is every time that you bring in a new prop on to your component, just at the same time go ahead and add it to your PropTypes, it's going to make your code a little bit more resilient.

---

So now what I'd like to ask you to do is pause this video and go ahead and do it for the `Inventory` component as well as the `order` component, so we'll go look for `var Inventory`.

We used PropTypes and props all over.  So we've got the removeFish, that's going to be a function that we use there, as well as `order` component.

So pause it, try do them yourselves, make sure you get them all lining up and you have no errors, and then come back to this video if you want to figure out how to finish them off.

---

And we're back!  How did you do?  Hopefully you did pretty well.  What I want to do right now is go over our `Inventory` component  and show you how I would do PropTypes on it.

So we've got this `Inventory` component, and I'm not going to fish through here looking for `this.props.`, I'm just going to go right to where we use `Inventory` which is - we do it inside of our `App` component, so here we have `Inventory`, and I'm just going to go ahead and select them.

We've got one, two, three, four, five different `prop`s.

Now we go down to the actual `Inventory` component where we made it, and I'm just going to do this right under my `render` function, but it doesn't matter where you put them, it's the same for everything that you put inside of a component.

It's going to be `propTypes`, and that is going to be an object.  sometimes you see people type `function` right here and it doesn't work.  That's just because it's not a function, it should be an object.

Inside of that we can put all of our different ones that we need.  Now you tell me what are all of these going to be:

```
addFish
loadSamples
fishes
linkState
removeFish
```

All of those are functions, except for `fishes` which is a... object.  Good.  So we're going to say `React.Proptypes.`, then we'll just put them all as `func.`.

The reason why it's not `function` is because `function` is a reserved word in JavaScript, so we need to use `func.isRequired,`

Now, this `fishes` here, that is an `object`.  There we go, and take the trailing comma off.

Now we'll give it a save and see if it all works for us.  Just go back to your app here, and here we see it's yelling at us right now: 

> Warning: Failed propType: Invalid prop `addFish` of type `function`

That means `addFish` got a function, which is actually what we want, but we were expecting a number because we told it in PropTypes that it should be a number.  So change that back to `func` and we're good to go.

Let's do the `order` one as well where we're passing it `fishes`, we're passing it `order` and we're passing it the `removeFromOrder`.  `fishes` is going to be an object, `order` will be an object because those two things are coming from our `State`, and then finally `removeFromOrder` is going to be a function, so let's go down and look where we find our `order` component.  Here we are.

I'm just going to go down here and we're going to have our `propTypes` set to an object.  Those three things here are going to be `React.Proptypes.func.isRequired,` and `fishes` and `order` are going to be an object, and `removeFromOrder` is a function.

So we'll give that a save, open up our devtools, and it doesn't look like we have any errors, so we're off to the races.  PropTypes, really nice way to validate the details that are coming in from your properties.  It makes your components more resilient as well as helps those who haven't necessarily built the component pass in the data that they need.