# 3 - Writing your first Component

Ok, let's write our first component.

Before we write our first component, and before we can write some React code we actually need the React library first, similarly to before you can write JQuery you need to load in the JQuery library.

So you might think let's go to our HTML file and load in react.js like that and then it'll be magically available for us.  Hower writing modern JavaScript applications we use something called commonjs where you can use a package manager to install your dependencies (and when I say dependencies I mean a JQuery plugin or something like that).

And we'll use commonjs to 'require' them.  So the way that works is you go into your terminal - and you don't have to do this, because I've already installed all the dependencies for you - but let's say you wanted to include some sort of new dependencies that you want.

You go into your Terminal, and I'm just going to quit Gulp here with `Ctrl+c`, and the way we do it is type `npm-install react --save-dev`.  

And what that will do when we install it is it's going to add it to the list of dependencies in our - if you open up `package.json`, this is a list of all the dependencies that we need in order to build our application.

So similarly if you had five or six different plugins you'd also install them.  And you might be saying 'why are we using npm to install client-side applications?'.  While npm was initially created for Node for things that are on the server side, it's now also being - see?  "The package manager for javascript".  It's also for client-side things.

So if you want a JQuery in your project, instead of going to get the JQuery link from Google's CDN, you'd simply just type `npm-install jquery --save-dev`.  I'm not going to do it because we don't actually need it here.

And then in your application you simply just say `var $ = require('jquery');` and that will just automatically assign whatever is in the JQuery package to `$`. 

So similarly we've installed React, so we can go ahead and say `var React =`  - and note that I've put a capital 'R' on that, and that's just because we're going to be using that to create multiple components, sort of best practise.


```javascript
var React = require('react');
```

And what that's going to do is it's going to take whatever is in that `node_modules/react.js` package.  
You never really have to look in here, but that will export the entire React library for us.  Once we give it a Save, go back to your iTerm here.  Make sure that you're running your Gulp task by just typing `gulp`.  

You'll notice that every time I change it - let me just clear it and give this a Save again.

You'll notice that every time I save it it rebundles and exports our `main.js`.  So where does that go?  Well this is our `main.js`, this is our source.  This is kind of where it goes.  However if you open up `build` and go to `main.js`, if you look at that file, you'll notice that - holy smokes!  There's all kind of code in here!  Where did that come from??

Well, this code in here, this is actually the React - this is the entire React libary that's being included for us.  So that one little line here is going to include it for us and bundle it into our `build/main.js`. 

So now that we have that we can start to go ahead to make our different componenents.  

The component that we want to build is this entry: "Please Enter A Store".  So before we even show that sort of three columns with our menu or order and the inventory, we need to make a component which allows people to enter in which store would you like to visit, and manage and add to your order from.

So we can go ahead and say - I'm just going to give myself a little comment, "`StorePicker`" component:


```javascript
StorePicker

```

we'd say `var StorePicker =` - again this has got a capital on the front.  You may not be used to having capitals on the front, but it's sort of a best practise because it will be a class.  Then we say `React.createClass()`, and inside of that we pass it an object which takes in a whole bunch of different options.  

So throughout this video course what we're going to do is I'm going to show you all of the different options and when you use them and why you use them etc, but the one option that every single React component needs is the `render` method.

So I'll say `render :` and se that to a `function`.  Now the `render` method is going to get called - essentially means "what HTML do you want me to display?".  So we can simply go ahead and say `return`, and you can go ahead and start returning some HTML right away.  I'm just going to say <p>hi</p>, and this is called JSX.  In the next video I'm going to talk to you all about it.  However, it's the best practise when writing JSX that you put some open and closed parentheses - put them on their own line and then inside of that you can put all your html, and what that allows you to do is to do multiple-line HTML, and you can write your nested HTML just as you're used to it.  

So what we just did there is we've created a `StorePicker` component, and let's put a little comment:

```javascript
  /*
  StorePicker
  This will let us make <StorePicker/>
  */
```

So it's going to give us this little StorePicker element, and I can pop that anywhere I want inside of our page, and it's going to render out whatever is exactly inside of here.  

We're going to have other methods in here that's going to be used for calculating data, syncing data etc etc, but the `render` method is what shows us what HTML is going to be showsn.

So if I give that a save now and I go back to our app and refresh, you notice that -it jus trefreshed there becauase the refresh was on automatically.  We still don't see aything that's on the page, and that's because while we created the StorePicker, we haven't yet mounted it to the page.