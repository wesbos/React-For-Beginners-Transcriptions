# 25 - Autobinding 'this' with ES7 Decorators

The other problem that we have has to do with the keyword `this`.  So when you're inside this method `goToStore`, `this` actually doesn't refer to anything.

however when you're inside of a `render` function, `this` refers to the actual class.  Now, when we are using the old React.createClass, what React does it it autobinds the keyword `this` inside of every single method to bind to the scope of your `React.Component`, and that's not automatically done for you when you use a ES6 component, which is a little bit confusing, but we're going to look at how we can fix it.

I'll just show you right now, if I try to go ahead and run this and click 'Go', we get this:

> cannot read property 'refs' of null

if we go ahead and opent that up, it's trying to read `refs` of `this`, and that means that `this` is not set to anything.

So how do we fix that?  Well, I'll just show you real quick.  If we do `console.log(this)` inside of the `goToStore`, but then I also do a `console.log(this)` inside of our `render` function. So when we load, `render` will get called and look: there's our `StorePicker` component.  You can clearly see that our `StorePicker` class is right there, however when I go ahead and click 'Submit', this `goToStore` method is going to run and we get that error, but we also see this as `null`.

So we're `console.log`ging `this`, `null` is set to it.

So essentially we want to do is get it to work like it did before where `this`, regardless of where you use `this` in any of these methods on the class, it will always refer to this React class, or to a React component. 

So there are two ways that we can do that.  Probably the simplest way, but maybe not the best looking way, is you can bind `this.goToStore` method in the context of `this`.  So what we say is `bind()`, and we pass it `this`, and that means when `goToStore` gets called it's going to run it against the scope of this `React.Component`.

So now when we `console.log` we see the `StorePicker` there, and when I go ahead and click submit it also logs the `StorePicker` there.  So we just changed what `this` equals to by simply calling `.bind(this)` against it.

Now, that's a little bit of a pain, because you have to add `.bind` to every single one, and I like to keep my code as clean as possible.

So you're welcome to choose that one, I just want to show you another way, and that's with something called an autobind decorator.

### Autobind Decorators

What a decorator is is sort of a way to extend the functionality of a class in ES7, and we're going to be able to use this because we're using babel and it's going to be able to compile it down to ES5 and work for us.

So a decorator is kind of cool, and it's actually a really nice solution to this.  All we have to do is just add `@autobind`, and that's sort of like a function that was going to extend whatever method that we have.

So I'm going to go ahead and install it for us, so `npm install` - what's the package called? `autobind-decorator --save-dev`.  So we'll save that, start it again with our Gulp task and we'll go back in here and say `import autobind from 'autobind-decorator'`.

Now we've got this sort of function called that, and we're going to go to the methods which we wish to have it decorated with.  So we'll say `@autobind` just right before the line where we have it, and give that a save.  You'll notice that I took that away.

Now we get a little bit of an error here.  Let's open up my terminal, and it doesn't totally understand the syntax.  The reason we have this is because this autobind decorator that we're using here is actually part of ES7, and by default this has not made it yet into Babel.  So what I have here is I've just pasted the code here, and when I have the experimental checkmark on it works fine.  When I take the experimental checkmark off we get that same syntax error.

So what we need to do is we need to change our build process to actually handle some of the ES6 features here, and I'm just looking at the Babelify, which is what I'm using for Browserify.

You have the ability to turn on things like sync functions or you can turn on all of the different experimental transforms right here, so we would want `es7.decorators`, or we could just turn on everything at once, depending on what you want.

So we go to our Gulp file here, and where you find the `transform: [babelify]` we'll call `.configure()` and that takes an object, and we can either give it an array of things that we want or in this case I'm just going to say `stage: 0`, and that will just turn them all on for us.

Give that a save, make sure we go back here, quit our Gulp and start it again.

Now you'll see that the `scripts` task has re-run, and it seems to be working automatically.  So I'm going back to my `StorePicker`, we've got this autobind here.  It's working properly.  Now if I click submit does the `StorePicker` work?  It works great!  I can take these `console.log`s out.

One other thing is React actually autobinds all of its methods right to the component, so what we could do is instead of just autobinding each method individually we can go ahead and autobind the entire class just by putting it above the class.

So give that a save, we'll go back to our `StorePicker` here and pick a new store, click submit, it still works great.

So I think that solution's a little bit cleaner, a little bit nicer, and you can tell people that you're writing ES7 and look cool like that.

