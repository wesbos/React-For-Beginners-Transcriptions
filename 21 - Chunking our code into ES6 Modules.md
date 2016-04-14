# 21 - Chunking our code into ES6 Modules

As we finish up this application we're starting to get really comfortable with all the different components and methods and everything and how it all works together.

Now that we're comfortable with that, let's look at moving it over to ES6.

ES6 is the new version of JavaScript.  It's not completely replacing the current version of JavaScript which is ES5, it's sort of adding on a bunch of new features, sort of like when we went from CSS to CSS3, where we just got a bunch of nice new stuff on top of it.

So why would we want to do this?  A lot of the examples that you're going to see online are built with ES6, and React , when you're building a React application it's generally recommended that you build it in ES6, which will make a couple of things available to us. 

Specifically we're going to be looking at making modules, chunking your code up into multiple files, as well as using ES6 classes which makes writing them a little bit easier.

Now, let's take a look at our application we've got right here.  You're starting to notice it's getting really long and it's getting a bit unruly to have to chunk out into all of the different parts.  We're at about 350 lines.

So ideally what I'd do is each little component that I have here will be in its own entirely own class, and then we'll just pull in the components as we need them.

Now not all the features in ES6 work in the browser work just yet, however we have this really cool thing called Babel, and what it does is takes your ES6 code, puts it through Babel and will spit out ES5 code which will work in all of our modern browsers.

It's really exciting because we don't have to worry about does this feature work in that browser or does this feature work in this browser, you just write it in pure ES6 and everything will be figured out for you.

So [babeljs.io](http://www.babeljs.io) is a great little website, and what you can do is go to their 'Try it out' one, and it works a little something like this:  you put in your code here, like in this case I'm going to look at our `<fish>` component, and we're going to make it into something like a class, which looks a little something like this, and we'll have our methods and our `render` method there, and it'lll just kick out some code.

It's quite a bit longer, it'll look a little bit different, but that's really not what we're interested in looking at.  We want to look at the nice clean ES6 code, and it's going to put it in there for us.

So Babel is sort of a standard.  Facebook actually employs the lead developer that works on Babel, so the entire facebook.com is actually compiled by Babel as well.

Now I'm not going to make you copy/paste our code into this little thing, I've already set up the `gulpfile.js`.

If we take a look at our bundle one right here, we see that we have this `transform:`, and I've said `[babelify]`.

Now, what all of our code is doing is it's going through Babel and it's being converted from any ES6 stuff that we use into ES5.  As an added benefit it's also compiling our JSX into regular old JavaScript.

So we're going to start with modules in this one, and we've already been using modules up here where we've been `require`, `require`, `require` - these are all just different chunks of code that live in different files, and we're using node.js `require` to import them into the specific file that we want.

Now we want to take that one step further and not just require the packages that we need and then import the rest, we want to go ahead and make every single component in its own folder.

So what we're going to need is in our `scripts` folder we're going to need a new folder and we're going to call that `components`, and inside of that `components` folder is where all of the different components are going to go.

So let's start things off easy and move a couple of components that don't have any dependencies on over, and then we'll finally move some of the bigger ones like our `App` and our `inventory`.

### NotFound

So probably the simplest one that we have is the `NotFound` component.  Remember this one?  This one is super simple, all it does is returns each one not found, and if we go to our app and go to a page that doesn't exist like 'whatever', you get this 404 'not found'.

So what we'll do is we'll create ourselves a new file in our components folder and we'll call it `notfound.js`.  It's generally best practise to name the component name exactly as you have it in there, and we'll put it in there.

Now I want you to go ahead and just take all of this code here, cut it out of our `main.js` and paste it into your `NotFound.js`, and we'll go back into our `main.js`, and now when we're using this `NotFound` right here it's not being referenced anywhere.  So we need to import `NotFound` into our `main.js`.

So where do we do our imports?  We do them at the top of our application.  Maybe we'll just give ourselves a little comment here and say `Import Components`, and we'll say:

```javascript
import NotFound from './components/NotFound';
```

So a couple of things to go over what's going on right here, is we've used this thing called `import`, then we say `NotFound from`, and then you say the path to the actual one before.

Before we've been doing `require`, and `require` is sort of the Node.js way of doing the imports, but this is the JavaScript ES6 way, and ideally we want to move everything over to using this import.

If we try and use it it's not going to work, we don't see anything loading on our page here, and while we've imported the module `NotFound` we actually haven't exported anything from the module file

So that's how exports work, is that your file itself needs to do a whole bunch of calculations and export something.  In this case we're exporting `React.createClass`, we're exporting a component, but it could be an object, could be a string, could be a number, could be an object with all kinds of methods on it, and we need to import it into there.

So the way that we can export it is we say `export default NotFound;`, and that's going to take the `NotFound` and export it from that file.

We'll give that a save and go back to our `main.js`.  This is going to refresh, and it's still not working, so we open up our devtools here, and it's going to say:

> Uncaught ReferenceError: React is not defined

And you may be kind of kicking yourself being like 'well, I clearly have imported React here', but the problem is this file, this component right here, this component has one dependency and that dependency is what?  It's React.

So you have to import React into every single file.  Don't worry, it's not going to include React eight times into your final `main.js` file, it's going to only import it once.

So we say `import React from 'react';` and that's lowercase.  The reason that's lowercase is if we go into our `node_modules` folder you're going to see `react` is right here, and when you don't specify a path it's going to look inside of your `node_modules` folder for the actual React.

So go ahead and give that a save and see where we're at.  Beautiful, our `NotFound` module is working perfectly. Now any time someone wanted to work on this, if you wanted to give this off to a designer or front-end dev, say 'hey, could you work on the 404 page for me?' all you have to do is work on this `NotFound.js` and they don't have to touch any of the code in there.

### StorePicker

So that's one, let's do another one.  Probably the other smaller one is the `StorePicker` app.  Let's just go ahead and do the same thing that we did last time, so I'm going to go into my `components` folder, I'm going to make myself a new file, I'm going to name it `StorePicker.js`, it's going to keep the naming consistent, and go ahead and take this `StorePicker` code out and put it into here.

Now we still have to go ahead and export this, we we'll say `export default StorePicker;` because `StorePicker` is the name of the class, so we exported it there and then we can go back into our `main.js`, because if you take a look at it right now you're going to see `StorePicker is not defined`.  Why?  because we're trying to reference it in our routes here but we haven't yet imported that `StorePicker`.  

So we can just duplicate this and say `StorePicker` and give that a save.  Now, that still isn't going to work, we're going to get that same error we had last time, `React is not defined`.  And this is sort of - if you're ever moving something over to ES6, if you're not exactly sure what to move over this is sort of your debuging process for that.

So we need to import React so we're going to `import React from 'react';` and that's the same thing as saying `var React = require('react');`, except the way that we're doing it here, that's the ES6 way of doing it.
So that's eventually the way everything is going to be in JavaScript, so let's go ahead and do that.

So save it, and we're going to get another issue, which is `Navigation is not defined`.  So you're starting to get the idea that these modules, even if something is defined in your main file here, when you're building a module if it's not imported into there then it just won't work, even though it's in here.  That's the sort of idea about modules is that they're all totally independent and they just pull in the things that they need.

So we'll go into here, and we need this `Navigation = ReactRouter.Navigation;` so we can go ahead and pull that in.

Now, it normally looks a little something like this, where you go `Navigation = ReactRouter.Navigation;`, and in order to even do this one we'd have to import `ReactRouter` initially, but one of the benefits to ES6 is you can sort of just reach into the package directly without having to import the entire package.

So we can write this in ES6 as `import` and we can use curly bracket destructuring to say `{ Navigation} from 'react-router'`, and that will just pull in the `ReactRouter` dot.notation directly, we don't need `ReactRouter` itself in this one, so that's good for us, and we can actually go ahead and delete it from our `main.js`, and that's sort of the idea here is that we can slowly remove it as we go, and it looks like this loaded as well, and we go back to my `StorePicker` and see if that works.

We've got one more error here which is the `h`, `h is not defined` on line 22 of `StorePicker`, which is right here.  We're trying to use the default value of the `getFunName`, and we're trying to say `h`.  What's `h` again?  `h` is our helper file that we had here.

So we actually need to go ahead and import this helper file, because it's not available inside of this one.  So we can say `import h from` - now.  How do you import a module that's not part of our `node_modules`?  This is just some module that we made up.  What we can do is we reference it directly so `'../helpers.js'` and we actually don't need the `.js` when you're importing them there.

So that's kind of cool.  Why do we do `..`?  We went up one level to get into our `helpers.js`.

So give that a save and click `Submit`, and it looks like this component is working.  Great!

---

Now that we've done it twice together I want you to pause this video and try move over the rest of the components yourself to using ES6 modules.  So I'd probably recommend next do the header, then go ahead and try do the `fish` component.  Then you can move over the `fish` form, the `order` form, the `inventory`, the `App` sort of as you go along.

So go ahead and give that a shot yourself.  I'm going to stay here on the videos and do the rest of them.  You're welcome to reference them, but if you feel like you've got a pretty good handle on that you're welcome to skip the rest of it.  I'm just going to be moving each of them on over to it and sort of explaining my process as it happens.

