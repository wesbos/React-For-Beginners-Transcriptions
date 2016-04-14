# 7 - Routing with React Router

Alright, let's talk about routing.  So far we've got our two main components.  We have the `StorePicker` component and then we have this `App` component which encompasses our header, our menu, our order and our inventory.

So far I've just been switching them out manually.  We have `ReactDOM.render` and we swap out `App` with `StorePicker` and if I do that real quick I'll show you.  Watch this refresh, and if I go here we've got our `storePicker`, go back to `App`, give that a save, and that reloads our app.

So what we want - if I show you the finished version here, is that when I go to just `/`, I get the `storePicker`, and then I submit it or go to one of the urls like `/store/unsightly-sparkling-foci` - I'm going to show you how to get these funny store names in the next video - I want to be able to handle which one gets rendered without the page actually having to reload.

So if you've done any work with server-side languages before, maybe Express or maybe some PHP you have server-side routing.  However because almost everything in React happens in the client (this actually works on the server side as well - that's another story), we need to actually do the routing inside of React itself.

Now React doesn't come with any sort of routing, but there's a very popular package called react-router that seems to be the industry standard for handling exactly the use case that we have here.

So we're going to go ahead and show how to use our React Router.  Just a quick note, I'm using 1.0 in this screencast.  By the time you view it it should be the standard, however if you look up any documentation online make sure you're not viewing it for .13 or .14, just because that's an older version which is not one that we want to use because we're going to have to update it.

So we need to go ahead and install react-router.  I've already done, but should you ever need to do it yourself you can just temporarily go to Gulp and type `npm install react-router --save-dev` and then hit Enter.  That's going to install everything for you.  I don't need to do that because I've already installed it.

Then we head back to our JavaScript file and we need to go ahead and `require` it.  So let's say `var ReactRouter = require('react-router');`.  And that will give us the main library there, and there was a couple of things that we need - a couple of components inside of react-router that we need to be able to handle all the different routes that we have.  So we need the 'router' from react-router:

```
Router
```

And we need `Navigation` which is going to allow us to do what we saw here, where if we go to this one right here and we click submit it will change the url bar for us without actually having to reload the page.

So those are the three things that we need, and I'm just going to quickly put these on here:

```javascript
var Router = ReactRouter.Router
var Route = ReactRouter.Route
var Navigation = ReactRouter.Navigation
```


These are all related to loading in the `ReactRouter`.  Now let's scroll down to the bottom of - right before we do ReactDOM.render() we'll give ourselves a little comment and we'll say:

```javascript 
/*
  Routes
*/
```

And the way that you actually declare routes in `React Router` is just with JSX.  They are actually a component in themselves.  So if we take a look at the documentation for `React Router` they've got some pretty good examples.  Lots of documentation to show you all the different kinds that you'd want.

Ours is going to be pretty simple because we really just have two routes here.  We've got just `/` and then we've got `/store/the-store-name`, and that's going to show us that one, and then we're goin gto have a third one for when you've put something in that doesn't work like a 404.

So how do we handle all that?  Let's first look at what you can do here.  Here's the example that they have.  They have the router component here, and inside fo that you have a route and you supply each route with a path that should match, and then the component that should render when that paht is matched. 

And that's pretty straightforward, however if you want some `/about` you can also nest them inside.  `/about` will render `About`, `/users` will render out `users`.  Maybe `/user/:userId` - we'll go into what this little variable is in just a second - but you can nest them as far as you want and that allows you to group common routes together.

It's very similar to ExpressJS, the routing on that.

So we're going to go ahead and make a variable, just call it `routes`, and we're going to - because it's JSX that we're going to be routing you use two parentheses there - and we want to give ourselves a `<Router>` tag or a router component, that's going to be the top level one. 

And then inside of that we give ourselves `<Route>` tags.  Now if you're nesting routes inside of each other you'll need to use the `</Route>` tag.  In our case we're not going to be nesting anything so you can simply just use a self-closing `<Route />` tag. 

Now we need a couple of things here.  The `path=` and then our first case is going to be just `"/"` as that's going to be the home page.  Then the `component` to handle the home page is going to be what?  Well, the home page is not the `App`, it's the `storePicker`, right?  We want that to be able to handle inside there.  So we'll say `StorePicker` and then you can just go ahead and duplicate that.

The next one is going to be the individual store.  So if we take a look at this one we want to go to `store/clumsy-grumpy-leaves`.  I want this sort of layout.  However I can't predict all of the store names in the future, so we need to leave this part as kind of a placeholder or variable.

So `"/store/` and then this is what we do: `:storeId"`.  And that `:storeId"` is just a placeholder that whatever gets put in there, like in our case it's `clumsy-grumpy-leaves`, that's going to make a variable called `storeId` available to us inside our component, and we're going to able to use that to do specific things like order, editing, tracking which orders people have, tracking the inventory etc etc.

Then finally the component is not `StorePicker`, what's our other one called?  It's called `App`, because we want to render out the App component there.

So let's go ahead and give that a save, and let this refresh here - oh it's not refreshing because I forgot to start my - if you quit Gulp to install something make sure that you start Gulp again, otherwise your refreshes aren't going to be noticed.  

Now if I refresh that, you'll see that it's still rendering out the `App` for us, but didn't we just tell it that when we go to `/` it should render out `StorePicker`?  

That's because if you scroll down here we just created a variable here, we haven't done anything with it.  We need to actually instead of passing `App` to `ReactDOM.render()`, we need to pass `routes`.  

What that will do is pass this entire `Router` JSX.  You could also code that right inside there if you wanted.  And it's going to take the `Router` component and figure out, based on the URL, "should I render out the `StorePicker` or should I render out the `App` URL variable?".

So I'll give that a save and have it refresh, and there we go.  So now we're at "Please enter a store."  

A couple of things to note here.  First of all we get this little `_k=` ... some sort of unique id, and if I refresh that you'll see that I get this random one every time that I run it.  And that's because `React Router` is trying to maintain state for you, which essentially means that if you go from page to page to page, you should be able to use your browser's Back and Forward buttons and have the React components update.  Because we're technically not changing HTML pages, we're just changing the url bar, and `React Router` is smart enough to know that "oh, they changed the URL bar, they changed the page hash in this case, so let's go ahead and update the component that is needed".

Now, given all the stuff in the URL bar we can actually still check if it works.  After the hash (I'll talk about that in a second) we go to `/store/store123` and hit Enter.  You notice as soon as I hit Enter the `App` component gets rendered out.  If I were to take that away you'll see that the `StorePicker` component gets rendered out. 

So our routes are working just fine, however I'm not a big fan of how this looks, so I want to get rid of that.  The reason that we have the hash and the reason that I talked about that is because older browsers don't support something in HTML5 called "push state", and push state allows you to update the URL bar without having to refresh the page. 

So what we can go ahead and do is at the top of our document here we can say `var createBrowserHistory = require()` - and this is a separate module, you'd have to `npm-install history`, but once you have that installed (and I've installed it for you), you go `'history/lib/createBrowserHistory');` and what that will do is load in your required code to be able to do push state, and `React Router` is set up in such a way that you can just take this `createBrowserHistory`, scroll down to your `Router` component here, and say `<Router history={createBrowserHistory()}>` and that will pass in the history to the Router and we'll be off to the races.

So now I should be able to go to `localhost/store/hello` and hit Enter and it's immediately rendering out the Catch of the Day application.  Again if I go Back it's going to render that on out for you.  So much nicer, much cleaner way to do it and our URLs are looking good.

Now, the last thing we want to do is setup sort of like a 404 when you go to a route that doesn't exist.  So I'm just going to open up my DevTools here and go to a route, something like `not-there` and hit Enter. We're going to get some errors here in the console: 

> Warning: Location "/not-there" did not match any routes

And then we've got a whole bunch of errors following that, but that first one's the most important.  So what we want to do is sort of create a fallback route so that if any of these don't match then why don't we just send them to a Not Found component or something like that.

So let's go ahead and make a Not Found component, and we'll say `var NotFound = React.createClass()`.  It needs a `render` method and then what we'll do there is just return maybe like a `h1` tag, `"Not Found"`.  

Then we can go down to our routes and add a third route which will take a path of `"*"`, and that's because if neither of these last two match then `"*"` will for sure match it, and the component we tell it is `NotFound`.  Give that a save and when we refresh you'll see `NotFound` is being rendered to the screen here and all of those errors are gone.

One thing we can do is, if you go to your React DevTools here and open on up the router, routing contacts, you'll see that the `NotFound` component is being rendered.  I'm going to show you on the working one just so I can show you in real time, if I open up my `<Router>`, open up the `<RoutingContext>`, there's our `StorePicker`, but when I hit Submit you'll see it's automatically switched out with the `App` and then again if I were to go to something else that was not found you'll see our `NotFound` component is being rendered. 