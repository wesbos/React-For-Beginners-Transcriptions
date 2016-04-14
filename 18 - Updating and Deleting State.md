# 18 - Updating and Deleting State 

If you think of a lot of popular web applications out there today, they all follow principles where they have CRUD.

If you've never heard of CRUD before, it means Create, Read, Update and Delete.  Those are sort of the big four operations that we need to be able to do with our data.

So we have the ability to create a fish, we've gone down here and we can fill out this form.  We're able to read it, which is what we're doing here, reading the `State` and displaying them.  We're able to update it be going into here and typing in the fish name.

However we aren't able to delete our fish, and we're also not able to delete any of the items from our order, because we're used to having an 'X' right there that would delete it.

So if you look at the answer right here, if I add something, add some Mahi-Mahi, I should be able to both delete the lobster as well as delete the Mahi-Mahi from the fishes, and it will delete it from our inventory and it'll tell us that fish is no longer available.

So that's what we want to be able to do next, is have the ability to delete both the fish as well as something from the order.

So let's go back to our application here, and we're going to look for our `App` component.  And we've got all these nice methods, we've got the mixins, `getInitialState`, `componentDidMount`, `componentWillUpdate`.

Then we have these two right here, `addToOrder` and `addFish`.  Those are sort of the two that allow us to create an item.  You'll see that in both of them we are calling `setState`, because what that does is goes ahead and pushes our items into `State`.

However each of these `addToOrder` and `addFish`, they need a complementary one which will be `removeFromOrder` and `removeFish`, which will allow us to update the `State` with the deleted one.  So let's go ahead and write the function up first and then we'll wire it up to be able to click this "Remove Fish" button.  So let's go ahead and make ourselves a `removeFish` function, and inside of that let's go ahead and code it first and I'll show you how you get this little popup so we can cancel it.

So `removeFish`, we'll say `this.state.fishes`, and we need to be able to specify which actual fish is getting deleted, and how do we do that? Well right here inside of this `removeFish` function we don't have the key.  So we're going to have to pass the key so we'll expect it to come and when we wire it up we'll make sure that we get that, and `fishes[key]`, and that `key` will be like `fish1`, `fish2` etc, but since this is a variable we'll say `key`, and you just set it to `null`, and what `null` will do is simply just delete the actual item from there, and that will update our actual state, but it won't actually sync it, it won't trigger the re-render, which is really what we want here.

So we need to call `this.setState()`, and what are we updating?  We're updating `fishes` in this case, and we can pass `this.state.fishes`.  So that may seem a little bit redundant to you, because why doesn't it just work right here?  However that's how React knows - if you had to do a couple of things in here you might want to update all of those things before you call `this.setState`, otherwise you'll have `render` running more times than you actually need it.

So that should work for us.  Let's give it a save and give it a shot on our actual application here.  When I try to click "Remove Fish" nothing happens, and why is that?  Well, we only coded the method here and we haven't actually wired it on up to our button.

So what we need to do is make this method available to our `Inventory` component, just like we did twice before in the last video.  So we'll go down to our `Inventory` and we'll say `removeFish={this.removeFish}`, similarly to how we did `this.addFish`, we're making the `removeFish` method available to it.
`Inventory`
Then we can go down to our `Inventory` component here, and here's our button.  Right now it doesn't do anything, but we can attach an event listener to it.  We can say `onClick={this.props}` - because when something gets passed down to it it's passed down via `props` - `this.props.removeFish`.

However you'll know that we need to actually pass `removeFish` the actual key that we want to remove. So it's not as easy as just calling `this.props.removeFish`, because it doesn't know which fish we actually need to edit.

So the way that you can pass - you would think you could just pass it that key, however it doesn't work exactly that way.  we need to call `.bind` on it, and what `bind` will do is it will run this function in the scope.  In the scope we have to put `null`, and the reason we put `null` there generally in something like that you pass it like this, but the reason you put `null` there is because React has a whole bunch of sort of magic behind the scenes of binding the keyword `this`.

So we pass it `null`, and the first argument that it gets is calleI the `key`, and that's going to supply the key to our `removeFish` function.

So give that a save and let's see if it works.  We might hit some errors but let's doublecheck.  So I'm going to go go ahead and remove this fish right here, click it - bam! It's gone immediately.  The one thing I'd like to do is have a doublecheck, because if you click "Remove Fish", "Cool Shoes" is just gone immediately.  I'd like to be able to pop it up and say 'are you sure you want to do it?'

So we'll go back up to our `removeFish` function, and we'll just wrap that entire thing in an `if()` statement, and then inside of that `if()` statement we can say `confirm` - and if you don't know what `confirm` is it's the old school popup where you say `"Are you sure you want to remove this fish?!"`, and what that should do is if they click 'Ok' it'll return `true`, and it'll run this code.  If they click 'Cancel' it'll return false and none of that code will run.

So let's go ahead and remove "Lake Scallops", removed; gone.

You'll notice one thing here is we get "Sorry, fish no longer available!".  That's because if someone has added something to their order like "Mahi Mahi", and then you immediately remove it, that's sort of the cool thing about this app is it's realtime.  If a customer has it in their cart but they haven't done anything with it and all of a sudden it's not available anymore, we've removed it from it, we can click it away and this will re-render out to "Sorry, fish no longer available!".

Now, the next thing we need to do is when I've added all these things to my cart here, how do I remove it from my order?  And that's where we're going to need - first we're going to need to add a little 'Exit' button, and then secondly we're going to need to write ourselves another method that's called `removeFromOrder`.

So let's go ahead and find this.  Now, you tell me where are we going to find this actual little piece right here?  It's in which component?  It's in the `order` component, so let's do a quick search for `order`.  Here it is, and `Sorry, fish no longer available`, and then - so we need to put an `X` to get rid of it in there, but we also need to put an `X` to get rid of it in there. 

So if you need a component in two different places it's best not to write the component twice, because then we're duplicating code, and if you have to change that code you have to change it in two places.

So kind of a nice way around that is we can store the actual  element or component in a variable and then just pull that variable in where we want it.

So right here we can say `var removeButton = `, just a regular old `<button>`, and inside of that I'm just going to do `&times`, and that's just an `X` that will show us that we can delete it. 

And then in that `<button>` we're going to give it our event listener, `onClick`, `= this.props.removeFromOrder`, and we have to do the same thing we did before which is the `bind` to `null` and pass it the actual key, which is being  pulled in from here and giving it to us.

So that gives us a button, and now we can go ahead and take that remove button and put it right here where we need it, right after it's no longer available.  Then we also need the remove button inside of when they do have it available, we can go ahead and say `removeButton` there.

So that's kind of neat where you can just put it into a little variable and pull it in wherever you want, just make sure to use the curly brackets.
If we try and run that now, you'll notice that we get an error and it says:

> Cannot read property `bind` of undefined.

Why is that?  Because we actually haven't made this thing yet called `removeFromOrder`.  So we're calling it right here, yet again we haven't yet made it.

So let's go back up to `App` and find where we do `addToOrder`, and we'll just do sort of the opposite.  So `removeFromOrder`, and that's going to be a function where it's going to pass us a `key`, and then inside of that we can go ahead and and `delete this.state.order[key]`, so that will remove it entirely from our `State`, and then we can go ahead and call `this.setState`, and inside of that what are we updating?  Last time called `fishes`, but this time we can say `order` is going to be set to `this.state.order`, and that will update it with the new order after we have just deleted it.

You can also put that on the same line if you'd like, it really just depends on your coding preferences there.

Last thing we need to do is actually make this `removeFromOrder` method available to our `Inventory` component, because remember we need to find this method in our `App` component.  we need to pass it down to the child which is our `order` component.

So go to our `order` component and we simply just say `removeFromOrder={this.removeFromOrder}`, and that will pass the method down from the parent to the child.

So let's give that a save and see if that works.  There we go: "Fish no longer available!".  If I click an `X` it's totally gone: `X`, `X`, I can click them on and then they're gone.  I can start adding new things to my order and then removing them altogether.

One little thing that might be bugging you right now is when you hover it over it kind of shifts the layout and puts this `X` on the end.  Not to worry about that right now.  When we hit our animation video what we're going to be doing is change the markup just a little but, and when you add in elements they're going to slide in, when you remove and element it's going to slide out, when you add multiple elements they're going to kind of 'ching, ching, ching' up there with it.  So grab the animation video if you're wondering exactly why that's doing it.