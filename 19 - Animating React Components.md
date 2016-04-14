# 19 - Animating React Components

Alright, who's ready to have some fun?  The last couple of videos have been pretty heavy on scope and state and managing all of our JavaScript and everything like that, and I feel like we need a little bit of a break.

Let's move into doing some CSS and adding some animation to our actual application here.  So I've got three different animations on this app here.  I've got this 'Fold' button here, and when you click it it gives you this kind of cheesy 'menu folds out' look.

If you're curious as to how that works, you can just open up your `style.style`.  If you've never seen this before, this is just something called 'stylus'.  It's just another way of writing CSS.  It's like SASS, pretty much exactly the same except it doesn't have the colons or the semi-colons - you can actually add them if you want.

So if you want to look for the fold you can see right here I'm just using `translateX` and `rotateY` to give it a little but of a 3D look.  It actually has nothing to do with React, it's just kind of a cool way.

The other thing that I'm more interested in showing everyone and learning how to do is built right into React, and it's when you change your state it's going to animate itself in, as well as the old state out.

So what does that mean?  Well, when I have this number '12' right here, that is part of `State` that gets changed.  That's called the `count`.  whenever I go from 12 to 13, 12 is going to animate up and 13 is going to animate in, and watch 13 now, it goes up and the new one comes on in.  So React actually does all of the creation behind the scenes, and then we can use CSS to animate them in and out.  We'll talk all about that.

The other two animations here is that when you delete an item it'll animate it out to the right, it kind of moves away.  Then when you add it back in it'll animate it in from the left to the right.  So let's go ahead and learn how all of that works.

---

So go back to your `main.js`.  While animations in React are officially supported, which means that it's officially part of the React library, it's not a third-party addon, we actually have to install and require them totally separately.

So we're starting to see React just put the core into React so that if you're using React for Canvas or React Native for Android or iOS, then you really only have to include this React.  Then when you want things like `reactDOM`, or we talked about `linkState`, or animations, those are going to be separate React packages where they sort of split them out.  That's sort of the beauty of Browserify, is you just require the tasty little bits that you need and your app is not going to be any bigger than it needs to be.

So we need to go ahead and first - I've already installed it for you, but if you haven't installed it you need to go into another one.

The way that you do that is you `npm install`, and the name of this one is pretty long, it's `react-addons-css-transition-group --save-dev`.  There we go - that would install the entire thing for you.  I've already done it.

And we go to the top of it and - I'm just going to go underneath React - because this is a core part of React I'm going to keep it along with it. Then in a future video we're going to chunk all these parts - this app is starting to get a little bit big and chunky - into a separate module.

So we'll say `var CSSTransitionGroup = require('react-addons-css-transition-group');` and now we have this thing called `CSSTransitionGroup`, and we'll go along to all the different parts that we need.

So let's start by doing the animation in and the animation out.  So if I look at this one here. it animates in.  When I remove it it'll animate it on out.  We'll sort of understand how it works, and then we'll go ahead and do a secondary one which is animating this sucker up and down.

So where do we go?  We go to our `order` component, and here we are, and we'll go down to the `render` method, and we'll find this right here which is our `<ul>`.  The reason we're looking for this `<ul>` is because the React transition group needs to be - it's actually a component - it needs to be a parent of all the things that are going to be animated.

In our case what's going to be animated?  Well each of the these blocks right here, that is an `<li>`.  So we need to take this `<ul>` and turn it into a React `CSSTransitionGroup`.

So the way that you do that is it's actually a component in itself.  So we'll remove the `<ul>`, and replace the `<ul>` with `<CSSTransitionGroup>` tag.  We can keep the class name of `order`, and the other thing we need to say is the `component="ul"`.

So what that's going to do is turn itself into an unordered list. It's going to keep the class name, but it's going to add in the magic that we need to be able to transition it.  So I'm going to give that a save and make sure that nothing broke, that it's still rendering properly.

So if I inspect it here you'll see that it's still an unordered list in our DOM, but if I go to my React tab here you'll seet that this is a React `CSSTransitionGroup`.  So that's not all we need to do there just yet, we need to add a couple more properties there.

The first one is the transition name, and we're going to need that for our CSS.  I'm going to call it `order`.

We also need to specify how long the transitions take to enter as well as leave.  If we look at the answer right here, when I add one it takes half a second to animate in (500ms), and it takes half a second (500ms) to leave.  It's getting a little bit long for me, especially as I'm on a small screen here, so I'm just going to move those each on to their own line.

Now, the two other `props` that we need to say is `transitionEnterTimeout=` - and that's going to be set to 500, so I'm using curly brackets here because that's a number, and then the other one is a `transitionLeaveTimeout`, and that's also going to be set to 500ms.  You can make one of these really long and one of them really quick depending on the experience that you want.

Now if we go and try and add something to our item, you'll see that nothing really happens here, and whenever I add something new here it just immediately shows up.  However the secret sauce is in the classes that React is adding to your elements, so what I would like to see you do is open up your devtools and just go ahead and look at these different `<li>`s here, because remember this `<ul>` is our CSS transition group, therefore these `<li>`s, these are our CSS transition items, and those are all going to be animated in and out.

So when I go ahead and remove one or - let me just remove them all and show you when one gets added on in.  So here's my `<ul>`, we have this `<li>` for the total but when I add a new one you'll see that what happened there really quickly is that React added a `class` of `order-enter` and `order-enter-active`.  Now you might have missed it, so what I'd like to see us do here is, let's make this 5000ms or 5 seconds, and that should give us enough time to really see what's going on under the hood.

So I've got this `<li>`, I'm going to close that down.  I'm going to add "Atlantic Salmon".  Keep your eyes down here. You see - look at this.  It adds `order-enter` and `order-enter-active`, and then after five seconds it takes them away.

Same thing when I delete it, it adds `order-leave` and `order-leave-active`.  Now, what do those mean?  That's all that React is going to do.  The rest of it leans on CSS transitions to do all of our animation.

So what I want you to do is open up `animations.style`, and we're going to show how it works.

So we have `.order-enter` and we have `.order-enter-active`, and you may be asking 'why do they add `order-enter` and `order-enter-active`, and it's because with CSS transitions you need to be able to transition from A to B.  So if in A you have `padding` of 0, and in B you have `padding` of 20px, what CSS transitions will do is it'll say 'ok, I need to go from here to here over five seconds or half a second, and I'll figure out how I get in between those two things'.  Or 'if I have a `background red` and I'm going to a `background blue` over five seconds, I'm going to figure out how do I go through the rainbow of colours between red and blue in orderer to animate those colours for you.'

So we need an A and a B for us to be able to actually work, and React - what it does it adds `.order-enter`, and then a split second later it adds `.order-enter-active` to it which means that with `.order-enter` we're able to set A, we're able to set our starting point, and then at `.order-enter-active` which is B we're able to set our sort of `enter` state.

So if we look at my example right here, immediately what it does when you add a new item to it is that it starts at zero - let me show you here with this padding - it starts at zero padding, and then slowly over the animation I animate in the padding from zero `rem`s up to `2rem`.  So I need an A to get to a B.  That's sort of why `.order-enter` and `.order-enter-active` are in there.

So let's actually do it and maybe it'll start to click as to how this all works.

So you can calculate all this.  So I have `.order-enter`, and underneath that I'm just going to use colors for now just so we can see exactly what happens.  I'm just going to say `background red`, and underneath that `&.order-enter-active`
`background orange`

So I've got these two colors where it's going to be transitioning from red immediately to orange over the five seconds, and if you're wondering what this ampersand is, this is just a fancy way to write CSS with `stylus`.  If you're writing regular CSS it would look like this:

```css
.order-enter{
background: red;
}


.order-enter.order-enter-active{
background: orange;
}
```

So if you're familiar with SASS or anything like that it works exactly the same way, but I find it easier to write it in this way.

So let's give that a save, and when we add a new item - I'm going to add "Atlantic Salmon" - see what it did there?  Immediately it turned it red, and then it turned orange, and then it got rid of it because the default state that I've styled somewhere here in `style.style` is `background: none`.

So if I add again "Jumbo Prawns", it goes from red immediately to orange.  You may be asking 'why isn't it animating?'.  Well, we have to add a `transition` to it, and we can say `0.5s`.  Make sure that you line up this transition with the amount that you set here, so in our case it's not 0.5 seconds, it's 5 seconds.

Now I'll show you - let me just remove these here - they'll take five seconds to remove because we're delaying it with our transition.  So I'm going to add "Jumbo Prawns", it's red and you can see it's animating, animating, animating over five seconds, over to orange and then once it gets there it's done.

So obviously you probably don't want to animate the background color, but it's a nice way to visualize going from A to B.

So what we actually need to do is - by default I want to put it off screen as well as animate the padding and the height, and that's exactly what we're doing in this one where first of all it's offscreen, you can imagine it to the left here, and when you add one it's animating the height as well as the padding and where it is going from left to right.

So the way we go ahead and do that - we get rid of these colors here - is we say `transform translateX-(120%)`, and what that will do is put it 120% off the screen, so it'll be just enough to hide it behind this one right here.  And then when we are in the active state we will say `translateX(0)`, so we started offscreen, and then when it's added it'll animate itself onscreen.

So let's go ahead and try that.  So let's go and add in some more fishes here, I'm running out.  There we go: "Pacific Halibut"... 

There it goes!I Look, look, look it animates itself on in.  We didn't get the height animated or the padding animated, but when we add and the new item immediately snaps and then it's only animating the `translateX` from left to right. However we can also go ahead and say `max-height 0`, and we don't want to use `height` here, because you can't transition a height from 0 to `auto`.  Here we can say `max-height 60px`, that's just a guess.  That's a bit of a CSS hack to be able to transition from 0 to auto.  Let's see where that gets us.

"Mahi Mahi"... so there's the height slowly animating, and then what's left is it's a bit jerky, and that's just because we have to say by default our `padding` is 0, and I just pop a quick `important!` tag on there, just because we're dealing with animations here.  we need to make sure that they override any of the default styles that we have inside of here.  So this `ul.order`, I've got some padding on this but it needs to overwrite that.

You can think of this as the default state, while the stuff in animation is the `entering` and `leaving` state, and then when the order is active we want to go ahead and say `padding 2rem 0` and pop that important tag, and I'm just matching that with what we have here.

---

So let's give that a save and see where we're at.  Let's go ahead and delete some of these as well.  So I add "Mahi Mahi", you see over five seconds it's animating the height , the padding, as well as the X value from left to right, and another one it's animating the height and everything.

So we can slowly see that working.  Let's just add it in our DOM here.  we're going to add one more Oyster.   See `order-enter`, `.order-enter-active`.  It's going a little bit slow, so we can speed that up by taking these five seconds to 0.5 seconds, go back to our `main.js` and take these down to 500ms.  Let's add a sentence of "Jumbo Prawns".  Ooh, nice little animation there.  Now when I take it away - you see we have a 500ms delay and then it's gone, and that's we haven't animated the `leave` yet.

So a really nice way you can do it is just go here and say `.order-leave`, and we basically do the opposite here.  We put a transition on that, and we'll say `all 0.5s`.  Then I want to say `transform translateX(0)`, and the reason why I'm doing that is because we're going to animate it offscreen, we're going to animate it 120% off the screen, so we need to be able to go from A to B again, right?

So the `.order-leave` is going to be the A, and `&.order-leave-active`, that is where we put all the other stuff.

So I'm just going to grab this from my answer here and put it in there. We set the `max-height` to zero, we set the `padding` to zero, and we set the `transform` to be X, 120% and that should animate it off the screen.

So give that a save, let's give it a shot.  "Add", go ahead and delete, oof - very nice.  It just swoops them right off the screen and we can still add them on in with that.

---

I want to show you that you don't just have to transform heights and padding and translate X, you can transform any CSS property to any CSS property that is translatable, and that is almost all of them.  So just as an example right here we could say `rotate(2000deg)` and we could say `scale(0)`, and we could give ourselves a starting point which is just `rotate(0)` and `scale(1)`, and let's see what happens.  Add a couple in... so they still go in normally but now what happens when we remove it is - whoo!  Batman!  out!  And it will just spin around and go away from us.

Have some fun with it, and every time that you see some sort of interaction animation on a website, think about 'how could I do that with two A to B CSS transitions that I'm doing in there.

---

Next what I'd like to do is when I add a second item to order, see where I'm adding "Atlantic Salmon" over and over and the number is climbing as I go, I'd like to animate that old one out and the new one in.  So it's a little bit different but let's see how we can do that.

Head back to your `main.js`, and we're going to look for our `order`.  This count here is what we want to animate in, however it's not an element itself, so we need to make it an element before we can go ahead and animate it.

So put that 'lbs' online there and you put the name beside it, and I believe in the answer I also have the removebutton on that line, so let's just move that around so it looks like that.  That'll help our formatting issues that we have there as well.  And we want to take this count here and wrap it in a `<span>`, and when you animate elements in and out, they each need a unique key so React knows which ones to take out and which ones to put in.  So you can actually go ahead and use the count as a key here, just because if the count's going from 1-2 to 2-3 to 3-4, the key is always going to be different and we're not going to end up with any duplicate keys.

I've got that `<span>` there.  Now we need to go ahead and give ourselves a `<CSSTransitionGroup>`, which is just a tag that we have, and put that `<span>` inside of it.  Because while we are only ever going to have one element in here, what's going to happen is React is going to make a  duplicate element so we're going to have `count=12`, and then animate in the new one, so that's why we have to have only one item inside of a `<CSSTransitionGroup>`.

Then `component=` - let's just make that a `span` as well, and we'll need a `transitionName="count"`, and again remember last when we did `transitionName="order"`, that's because we've prefixed all of our CSS animations with `order`, and the next one's gonna be all prefixed with `count`.  Like before we need a `transitionLeaveTimeout=` and we'll do this one at - let's do it at 250ms.  I want it to be nice and quick - and we want to transition `transitionEnterTimeout=` and we'll do also 250ms.

Ok, so there we go.  Let's give it a save and open up our devtools here. go ahead and and just inspect that count that we have here, and you'll see that this is what we got with 11, so I'm going to go ahead and add another "Atlantic Salmon", and just keep your eyes down here.

Oh, see what it did there?  It duplicated the element and added it on in.  So if you didn't catch that, maybe again I'll make that five seconds so we can slowly see it go in and out.  Go to the 17 here and going to add in the "Atlantic Salmon", and you see this one?  This one has `enter count-enter-active`, this one has `leave leave-enter-active`.  

Let me show you again, this one's got the `enter` and the `leave`, so it duplicates the element.  We temporarily have both.  You'll see here again.  I've got a 20 and a span of 19, but after five seconds it deletes the old one and keeps the new one as the remaining child.

So that's a little bit weird because we have two of them, but  what we're going to do is overlay them on top of each other and animate one all the way up and one all the way in.

So head on over to your `.animations`, and we'll say this is the `count` animation.  This can be really similar to our `.order-leave` except this one has a transition name of `.count-enter` and `.count-leave`.  So let's go ahead and do `.count-enter` first.

We'll put a transition on it for 5s, and the reason I'm doing 5s is because we've set it to 5s just so we can see what's going on, then we'll make it a little bit shorter after that.

Then I'm going to set a `transform` of a `translateY` of 100%, and the reason that I'm doing 100% is by default, before it starts when it enters I want it to start below the existing number and I want to animate it up to where it should be, and then the '2', we're going to animate it from where it should be all the way on up outside of that.

So that's why we do that, and then the active is we're going to animate it to where it should be, and where should it be?  It should just `translateY(0)`, and zero is just where it lies nately in our CSS.

Then under `leave` we're going to give ourselves a transition.  It should be `all 5s`, and then that should be five seconds as well.

Now this one is going to be positioned absolute, because we need to overlap them on top of each other in order to get the proper slide up technique, and that's going to have a `left` of zero and a `bottom` of zero just so that it overlaps, and then finally we want to have a `translateY`.  we want to start it where it is.  Maybe that's a good comment that we can give ourselves:

```
// start it where it is
```

and then with this one you can say 

```
// start it just off screen
```

Then when it is active, let's go ahead and say `-100%`.  we're just going to take it from where the `to` is now up whole amount of however much it is because of the negative 100%.

So let's go ahead and give that a save here, I'm going to go find "King Crab" and watch this '2' here... see it's slowly going up and the '3' sort of comes into place.

If I inspect that here we'll see that here's just the '1', it's got the number three in it, but when I click it it duplicates number 3, puts it at the bottom, puts a '4' right before it, and it's going to animate one in and animate the other out.

So that's that.  We should make that a little bit quicker though, so let's take this 5 seconds and make this a 0.25 seconds, as well as go into our `main.js` and make that 250ms, and we should get a little bit of a nicer animation here.

Add one, there we go.  Nice and snappy.  Every time we add one it will animate it on up, and again you could use anything.  I'm just using `translate` to move them up and over, but you could change the color when something's added, you could make it bigger, you could make it shake, whatever really is up to you.

---

Finally we've just got a little bit of styling issue with the 'X' here and the 'lbs' not being beside it.  That's just because there is nothing surrounding all of these here, so I'm just going to go ahead and and give myself a quick `<span>`.

That's just because I'm using FlexBox here to outline it, and eventually what I want is one element to the left, which includes that, and then the other element on to the right.

So give that a save and there we go. we've got our "23lbs Atlantic Salmon", however if I go ahead and add one or two "Halibut", it will animate nicely and cleanly on up.