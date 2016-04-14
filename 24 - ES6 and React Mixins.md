# 24 - ES6 and React Mixins

`StorePicker`.  So let's open up the `StorePicker` component.

`class storePicker extends React.Component`.  Open up your block and we can go ahead and - so we've got the mixins here, we'll deal with that in a second.  We've got our `goToStore`, we've got a `render` function.

So I'm just going to move all of these on up inside of here.  Get rid of that.

Now let's go one by one.  I'm just going to keep the mixins there, and `goToStore` is now a function.  It still passes the `event`, that's fine.

Now I don't need that comma - `render`.  Don't need the `function`, and we are good.

Now, that is somewhat helpful, except I said we cannot use mixins.

So, to use a mixin with ES6 classes we need a package called `react-mixin`, and it works very similar to all of the other packages that we've been using.

First you go ahead and install it, so you type `npm install react-mixin --save-dev`, and I've already gone ahead and installed that for you, but if you are on another project you'd have to do this.  Make sure you start your Gulp up after you've installed it.

Then we're going to go on into our actual `StorePicker` component here and say `import reactMixin from 'react-mixin';`, and that will give us this  `from 'react-mixin'` function available.

Then we go down to after we've created the `StorePicker` class, and go ahead and say `reactMixin`, and you call the `onClass` method and you pass it two things.  First of all you pass it which class you actually want it to mix in the thing, which in our case it's `StorePicker`.

Then the second thing that you pass is the actual name of the mixin, and you want to - here we're going to be removing this mixins navigation array, and we're going to be passing it the navigation there.

So if you had multiple mixins you would just go ahead and like put your second mixin in there, but we only have one in this case.

So that solves our mixin problem.