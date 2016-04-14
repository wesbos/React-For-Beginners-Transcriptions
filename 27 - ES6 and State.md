# 27 - ES6 and State

Let's move over our `App` component, which is the entire thing that everything goes inside of.

There's a couple of things you need to be aware of.  We've got a mixin, we have all kinds of things like `this.addToOrder`, `this.renderFish`, all of those need to be bound properly with our autobind decorator.  Then finally when you use ES6 classes with React you need to handl `props` and `State` a little bit differently.

So we're going to move those on over.  Let's get started.

The first thing I'll probably do here is I'll say `class App extends React.Component`.  That's going to open up, and then I'm going to go to the end of this entire thing and delete that.

So that's our block there which we've opened and closed.  Now mixins we'll deal with in just a second. `getInitialState`, that is our function here so we'll get rid of that.  `componentDidMount`, that's also another function. 

Open/close, delete that.

`componentWillMount`, that's good, `addToOrder`, we'll get that one, we'll get all these in one go here.  Delete that closing one.  I just like to put a space in between for readability.

That one

Good.

`loadSamples`.

`renderFish`.

And the `render`, I'll make sure to take care of these.

So there's a lot of little things you need to take care of when you're moving this on over.

And cool.  So that looks like we've done the syntax of moving over to the class, which is great.

We can get rid of this `var App = ` because we're creating the `class App`.

Now we need to deal with the mixins, so we need to do the import.  `import reactMixin from 'react-mixin'`.  we've already installed it, so we don't need it.  And we will go to the bottom here, and we will say `reactMixin.onClass(App, )` we want to add in - what is our component called here?  `[Catalyst.LinkedStateMixin]`.  So get rid of it in here, and put it on in there.

So that handles our mixin.

So we've done mixin, now we'll go ahead and do the autobind, and we can import the `import autobind from 'autobind-decorator';`.  If we take this, and I'm just going to autobind the entire class in one go, so we can `@autobind` that class, give that a save, and that will take care of that for us.

So give that a refresh.  Let's see if we've got any errors going on here.  we've got this right here:

> Warning: getInitialState was defined on App, a plain JavaScript class.  This is only supported for classes created using React.createClass.  Did you mean to define a state property instead?

And that's exactly what - I really like the errors in React, because they tell you exactly what's going on here, is that I've tried to say `getInitialState` and we can't actually do that. what we need to do is we need to define the `constructor` function.

So we go ahead and say `constructor()` and that going to be a method in there.  The `constructor` method is going to run every single time that we create the `App` class, and we need to go ahead and say `super()`, which is in ES6, we need to call that and that's going to call the parent constructor which is `React.Component`.

Then we'll go ahead and say `this.state = ` and you just set it to an object and put your `fishes` and `order` right in there.

So instead of running `getInitialState` like we have been right here, we can remove that.  We simply just say `this.state`, and when it's created it will set `this.state.fishes` and `this.state.order`.

So I'm going to give that a save, and we've got one more thing to fix here, it's a little bit of a gotcha. 

So it seems to work and then we get this error here:

> Cannot read property 'fishes' of undefined

And it's actually in our `linkState` mixin.  This threw me at first, but I finally figured it out, is that every time we're using `this` it refers to the `react` component, and we use `this` all throughout.

However down here where we say `this.linkState`, when the autobind - this one right here - when this is run, `this.linkState` doesn't exist.  So it's linking it to nothing.

Why doesn't it exist?  Because `linkState` doesn't show up until we run the mixin.

So it's a little bit of a weird thing where the autobind does all the binding and then we add in the mixin, so we actually need to explicitly bind it to `this`.

So go and find where it says `this.linkState` and bind it to `this`, and then - `this.bind(this)` is what's happening with the autobind decorator, but because we added it in after the fact we need to actually just manually bind it ourselves, so give that a save and everything will be perfectly back to normal.

So there we go, I can add in my thing and everything's working.

