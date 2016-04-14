##5 - Creating our application layout with components

Alright, next up we want to make the app layout.

We made the store layout, which if you go to our thing here we see the store picker layout, and eventually we're going to be able to type in here and hit submit.`order` component

It's going to bring us to a screen like this - this is the fully completed app where we can go ahead and add things to our order, we can change the name of it, we can change the price and all of the stuff will immediately update.

So again we've got one big component here called `App`, and then I've got a whole bunch of mini-components inside of it that are going to be managing things like the header, the fish, the order and the inventory.

So let's go on to our Sublime Text and let's do this above `StorePicker`, because this `App` component is going to be probably the biggest one that we'll be working on.  So you want to make sure that's easy to get to.  So let's say `App` and say `var App = React.createClass({})`

And again, what's the one method that you actually need?  It's the `render` method, and inside that we are going to `return`, and again it's best practise to use parentheses, 'cause we're gonna have multiple lines.  So I'm just going to copy/paste this over, and let's take a look how we got here.

We've got a `<div>` with a `className` - remember you need a `className` for it to work - of `catch-of-the-day`.  That's just for styling.

Then I've got a `<div>` class of `"menu"`, and inside of that is going to go the `<Header>`, and then we are going to have all kinds of `<Fish>` here eventually.  We're going to say things like

```javascript
<Fish />
<Fish />
<Fish />
<Fish />
<Fish />
...
```

for each of the fish, but we don't have that just yet.

So we've got the Header, then we have our Order which we're going to shell off to another component, and then we have the Inventory which we're also going to let another component take care of.

So save that, check how everything is going.  Looks like it's compiling fine.  Now let's go to our `App` here and open it up, and there's no errors and we're not seeing any of it.

How come?  Well, because if we scroll down to the bottom here you'll notice that we have `React.render` `StorePicker`, not `React.render` `App`.  

So we don't want a `StorePicker` just yet.  Eventually we're going to get into what's called routing where you can show one or the other, but change `StorePicker` to `App`, because again the tag is going to be whatever we named the class here.

So save that and refresh.  Now we've got it, but if you open up your DevTools and go to the console it says:

> Uncaught ReferenceError: Header is not defined

on main.js on line 13.  And if you click it, it's telling us that the error is right here (`<Header>`).  I've got something called 'Source Map' setup.  It will show you the error in your original, because we're not actually running this code or running the compiled code, but it will still show you where the errors are in your uncompiled code.

So it's telling us:

>  Header is not defined.

Well, that's actually pretty easy to solve because we haven't created a component called `Header`.  

So you'll see here that we have an `App` component and we are nesting the `Header` component, nesting the `Order` component, and nesting the `Inventory` component.

So let's go ahead and  create those.  We don't need all these spaces.  Let's say:

```javascript
/*
  Header
*/
var Header = React.createClass({
render: function(){
return(
<p>Header</p>
  )
}

})
```

Now that's going to be very similar to the other ones, so let's just copy/paste that twice more, and under Header we'll say 'Order' is the first one, and then `Header` ... `Header` ... `Header` is going to be `Inventory`.

Cool.  So we've created our Inventory, our order and our header.  Maybe it's helpful to tell us what they're going to look like underneath in the comment, and we've got that there, there and there. 

So let's save it and go back and - alright!  Looks like we've got our `Header` component being pulled in, we've got our `Order` component being pulled in, we've got our `Inventory` component being pulled in.

We do have one error here:

> Warning: Unknown DOM property class.  Did you mean className?

So where did we use `class`?  We said `class="menu"` - it's `className="menu"`.

So I'll just let that refresh itself, and we're off to the races! Check our React devtools here.  If we open up `App` you'll see that we've got our `header` component, our `order` component and our `Inventory` component all nested inside of this `App` component.

So that's our layout of everything; we're going to be working with these components for the entire application here.  We're also going to create a couple more subcomponents, but in the next video we're going to talk about how do we get data to these components.