# 9.2 - All About React Event Listeners

Events in React are exactly the same as events in regular JavaScript, so if you're familiar with JavaScript events, or even just JQuery `onClick`, `onSubmit` etc etc, you're going to be totally fine here.

The only difference here between regular JavaScript events and React events is that React has wrapped them in this cross-browser wrapper which allows it to work consistently across all of the different browsers out there.

So the one thing we need to know about events in React is that they all happen inline.  So we don't go - if you're used to JQuery you might think "oh, we'll have a separate file somewhere and we'll listen for the DOM node and we'll listen for a click and when someone clicks that we'll go ahead and do something.  

We're still going to have the same idea where we have some sort of element, we listen for some sort of event, and when that event happens we're going to go ahead and do something.  However we aren't entirely doing it in a separate file, we're doing it right inline.

So the event that we're going to listen for here is when someone goes ahead and clicks "Submit".  So we don't listen for a click on the Submit button, because some people maybe just type in here and click Enter, the event that we actually listen for is the `submit` on the form.

So we go ahead and just right inside here as an attribute we hit `onSubmit=`, and in curly brackets we're going to tell it what function to run.  Now, we don't have a function just yet, but it's going to be something called `this.goToStore`. 

So what does `this` refer to?  We've got a couple of videos coming up that will dive into this a little bit more, but in our case `this` refers to the actual component that we've made.  It doesn't refer to this function, which it usually would, it refers to the component.

So we're saying `StorePicker.goToStore`.  Now, we don't have a `goToStore` function yet so let's go ahead and make it: `goToStore`, and that's going to be a `function`, and inside of that let's just do `console.log('Ya submitted it!')`.

Let's just make sure - before I do anything I like to make sure that my events are actually listening and firing, so I'll open up my console here, going to go ahead and click Submit and - oh, I saw it there for a split second but a couple of things happened.

First of all I've got this weird question mark in my URL bar, and secondly it reloaded the page.  So if I hit it again watch, it reloads the page really quickly.  So what we need to do is not use that right there, and inside of this thing we need to pass it the event, which is what happened, and we need to stop this form from actually submitting because we're going to take over from here and handle it ourselves.

So we say `event.preventDefault();` so if you're familiar with any sort of JavaScript or JQuery before, you'll know that `preventDefault()` stops the default from happening.  In our case a `form`'s default is to actually refresh the page and send the data along with it.

So we're going to stop it, give that a save and going to refresh.  There we go, and now when I click Submit we get "Ya submitted it" and every time I click it it's going to go ahead and submit it.

So that's not actually what I want to do but it's good to know that our events are working.  We can go ahead and delete that, but a couple of things we need to do is first we need to get the data from the input, which is right here, and the second thing that we need to do is we need to transition from `StorePicker` to `App`, right, because we have these components here where we want to move from the `StorePicker` to that.  We actually want to change the URL bar from just `/` to `/store/mysterious-plain-lives`.  

So how do we get data from an input with React?  If you're used to JQuery or something like that you might think well, we can say `var storeId =` and we can go ahead and select the `input` and call `val` on it and we can get that data no problem.

However we're not using JQuery in here, and a lot of the stuff in React is really - it sort of steers away from actually using the DOM.  We'd like to just manage our data and let React update the DOM for us, but in this case we actually need to get some data out of the DOM.  So what we do there, instead of using the dollar sign: 

```javascript
this.refs.storeId.value;
```

Now let's break that down.  `this` equals the component itself, so that's `StorePicker`, `.refs`. Now what is refs?  So earlier when we did our render of our form we put a `ref` on this input here, the one with `grumpy-plain-knives` in it.  What that does is allows us to reference that input anywhere inside of the component.

So if I were to say `console.log(this.refs);` and give it a save, let that refresh, and when I go ahead and submit it you're going to see here that I get an object that I `console.log`ged on line 84, and open that up.  `storeID`, you see that it's actually an input.  So `this.refs.storeID`, that's the actual input right here, and this is just plain old JavaScript from here on out.  We want to be able to grab the value out of that, because it's an input.

So `this.refs.storeID.value` will gets us whatever is in that box, and I can go ahead and console.log the storeId and give that a refresh we get `embarrassed-embarrassed-diagnoses`, `jealous-magnificent-mice`.

So I'm getting whatever is out of that input there, but now that we have that I need to actually put it in the URL bar.  So rather than actually doing something like - you might think `window.location.hash =` and then you can pop in a `'#'` there and `+storeId` and you're off to the races, but you can't do that because `ReactRouter` actually implements all of the routing for us, and provides us with the ability to use Back and Forwards buttons as well as the ability to just scroll back and forth to where we used to be.

So `ReactRouter` takes care of all of this, we just have to make sure that we use the `ReactRouter` api in order to switch our pages.

Now, `ReactRouter` actually uses a package called `history`, and all we have to do is say `this.history.pushstate`, and if you haven't heard of `pushstate` before, it will change the URL without having to use hashbangs or anything like that, and most importantly not refreshing the page, so we're going to call `.pushState` on that, `null`, and then we pass it the link to the actual store.  So you want to say `/store/store123`, but we don't want to hard code that, we can just concatenate in the actual `storeId` which we just got right there.

So I'm going to put this in and it's going to error out on us, and I'm going to show you exactly why.  So let this refresh for us and going to go ahead and click submit, we get this error:

> could not read property pushState of undefined

And what's happening is we don't have that `pushState` method available to us, and the way we can get access to it is via something called a mixin.

So I'll go to the top here, and previously I was importing this Navigation mixin, and that's not what we want anymore, because `React Router` changes in `React Router` 1.0.  What we need to do is say `var History = ReactRouter.History;`, and that will give us the History mixin available for us.  

Then we can go down to our `StorePicker` here and define our mixins, and our mixins are always going to be an array.  What mixins will do is sort of... mix in!  It will populate our `StorePicker` class with those methods that we need, so we say:

```javascript
mixins : [History], 
``` 

Give that a save, now when this refreshes I can go ahead and click on submit and it will actually automatically move me over to `store/itchy-nervous-mice`.  If I go back, click submit again it's doing a `pushState` on the URL there.