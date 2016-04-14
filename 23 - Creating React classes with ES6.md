# 23 - Creating React classes with ES6

One of the new features we have in ES6 is the ability to have real classes.

So far we've been going ahead and saying `React.createClass`, which returns an object.  It sort of acts like a class, however in React now we can actually use real ES6 classes because we are rewriting this in ES6.

So there's some benefits to using classes, it's more future-friendly, it's looking forward.  We have this ability to have a bit of a shorter syntax when we're creating our methods.

The downside is that we can't use mixins and we can't use autobind.  So actually that's right here.  First of all, when we need to reference the keyword `this`, like we do something like `this`, `this`, `this`, or we say `onSubmit={this.createFish}` and it refers to the `AddFishForm.createFish` method.

So we're going to have to do some binding to make sure that it actually refers to the component and not the actual `render` function that we have there.

The second thing is that we don't have the ability to add mixins in, which is easily solved by either rewriting your code in a way that doesn't require mixins, or more easily we can just use a plugin that will sort of replicate the mixins functionality.

So you're welcome to skip this video as well.  If you find yourself like 'I really like this part but I'm not totally sold on using React classes just yet', just because of these two issues which are - they're not minor issues, and we'll show you how to fix them.

You're totally welcome to keep using `React.createClass` in all of them.  It's really up to you.  I've talked to a lot of people, and the consensus seems to be like 'yeah, I'm using a lot of the ES6 stuff but I'm not going to be using it to do my `React.createClass`'.

---

So let's get started.  I'm going to go ahead and convert the `NotFound` one first, just to show you a simple example, and then we'll step it up and we'll run into these issues of autobinding and mixins together.

So let's go ahead and instead of saying `var NotFound = React.createClass` we say `class NotFound extends React.Component`, and make sure you have a capital 'C' on there.

And you open up your block right there.  Now, we want to be able to put all of the methods that we have inside of here.  So right now it's only `render`, and the way that we say that is - let me just take this one here, and I'll paste it and show you how you can convert it.

It's not `render:function`, it's just `render()` and then you open your block and close it.

Now, if you had a secondary one - like maybe you had an `add()` - you can just go ahead and explicitly state `return 1+1`.  You'll notice that I'm not doing a comma here, because when you state your methods inside of a ES6 class you simply just type it and you put all of your stuff inside of it.

But we only have one here so I'm going to take that back.  Now that's my `NotFound` one.  I'm going to go ahead and remove this one here, it's a little bit of a simpler syntax which is nice.

Give that a save, let it refresh, let's see if we have any errors - no.  Let's actually go to this component by trying to type in something that we can't go to, and 'Not Found!' is rendering on out.

Let's step it up a little bit and go through what our next one is going to be.