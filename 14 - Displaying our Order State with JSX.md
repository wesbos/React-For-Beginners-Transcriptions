# 14 - Displaying our Order State with JSX

Now that we have our state displaying and now that we have `addToOrder` adding to our `order` state, it's time to go ahead and display our current order in our `order` component.

Now, this may seem pretty simple, it's actually one of the more complicated components of the app.

So let's go on over.  We are just rendering out this `<order>` tag which - down here we have our `order` component, but before we go down to the `order` component we need to pass a couple of things along to this `order` component.

So the first one is going to be - we need to give it all of the `fishes`, which is `this.state.fishes`, and we also need to give it the `order`, so let's say `order={this.state.order}` because again the `state` is only accessible in the `App` component because that's where the `State` lives, however we need to make it accessible inside of our `order` component.

So we've passed those two things along, and we can go ahead on down to our `order` component.  Right now we're just rendering out a `<p>` tag but we want to do a little more than that.

Let's give ourselves a `<div>` and give that a `className` of `"order-wrap"` and inside of that we'll give ourselves a `<h2>` with the `className` of `"order-title"` and we can just put `Your Order` in there.

And then here this unordered list right here - let's just go ahead and give ourselves an unordered list with the `className` of `"order"` .

Inside this unordered list is where all of the list items are going to be rendered out, but before we can do any of that we need to do a little bit of calculation as well as some data normalisation.

So if we look at one of the examples we have here, we've got a list of all of our orders, we've got how many pounds they have ordered, we've got information about it like the name, we're pulling in "Lobster", we're not just saying "2lbs of Fish 6", we're showing "2lbs Lobster", and then we're  also calculating how much they have spent.  That's not the price, that's how much they have ordered times the price, and then finally we're calculating the actual total.

So let's go here to your `render` function, and right above before we return it let's first store our `orderIds`.

So `var orderIds = Object.keys`.  We'll use that again, and we'll pass it `this.props.order`, because what that will do is give us an array of all the fishes that we've ordered.  and then we'll also go ahead and give ourselves a `total` function.

So `var total = orderIds.reduce()` - now if you've never used `reduce()`, what that will do is it'll run the function against everything in the array, and we can do a bunch of calculations.  In our case we're going to sum up how much they have spent on a particular fish and return the total there.

So I'm going to use a little ES6 here.  we're going to pass the `prevTotal` and the `key` with a fat arrow, and we want our curly brackets there.  The only reason I'm doing this is because there's some scoping issues that happen with the keyword of `this`, that I don't really want to get into actually fixing because they're a bit of a pain.  So we're going to use some ES6 here to fix it and then in the ES6 video I'm going to show you exactly why we use that and why it's so helpful.

So `total` there - we need to do a couple of things inside of here.  First we need to grab the actual fish, so `var fish = this.props.fish[key];`.  We also need a `count`, so how many of the fish have they ordered, so it's `this.props.order[key]`, and again that's going to give us things like `6`, `2`, `2` and `1` returned to us.

And then finally we need that `isAvailable` variable again to us as well, because we don't want to be calculating the total here if it's not available to them, right? 

So if we have something in our cart and then all of a sudden it's made unavailable we don't want to be calculating that to total, because again this is a real timeline.

So `var isAvailable = fish && fish.status === 'available';`.

So what we're doing here is we're first making sure that `fish` exists, because it's possible that we have it in our order and then it's deleted, and then we're checking that the `fish.status` is available.

Cool.  So, we'll say if there's a `fish` *and* `isAvailable`, then we want to return the previous `total` plus `count` (how many they have purchased) `* parseInt(fish.price) || 0);`.

So what this will do is - we're wrapping that in `parseInt` so it'll convert it to a true number.  It's taking the `fish.price`, multiplying it by the number that they've orderered, and it's going to return that plus what they previously did.

So it's going to loop through every single one, and finally `total` will equal actual total value to us. 

Finally we can say `return prevTotal;`, and then right here we have to say `0`.  This little `, 0` - that is the starting value. So the first time around, `prevTotal` is going to equal zero.

Let's go ahead and display our total.  So give yourself a list item, and that should have a class name of `total`, and inside of that we'll give ourselves a `<strong>` tag that says `Total`.  And we want to pop in the total number there, and `total`.

<edit>

If we're going to load in some sample fishes into our `State`, there we go - and now I'm going to add one to `order`.

And it doesn't seem to be working, but we've got an error:

> Uncaught TypeError: cannot read property 'fish1' of undefined

So what does that mean?  Let's click it and see where it's telling us - it's telling us that inside of our `total` calculation we're trying to say `this.props.fish[key]`, so it's trying to find `fish1` of `fish`, and `fish` doesn't exist.

So if we go back here it's because it's not `fish`, it's `fishes`.  I mis-typed that.  So let's see if we fixed it there.  Load in the sample fishes, "Add to Order"... alright!  There we go.  

Now, every time that I add things to my order you can see that this "Total" number here is growing. Why does that look so weird?  That's because we're storing the amount of our `fish` in cents.

So how can we display that in a human-friendly way?

Well, you've probably guessed it - we can use our helpers.  `h.formatPrice()` and wrap it around.  Because this is just JavaScript at the end of the day, which makes it super nice.  You don't have to do any special templating, you can just use regular JavaScript.

So let's try that once more.  Load in the Sample Fishes, Add to Order... alright.  $17.24.  Keep adding.  As I add more and more items to it the total keeps on growing.

Cool, so we've got the order total working, but we actually need to display what the user has been buying, which is right here.  we have the number that they have as well as the name as well as the price displayed.

So the way we can do that is give yourself some curly brackets, and we need a list of the fishes that the person has ordered.  And where is that?  Well we've got this nice array called `orderIds`.  We can call `map()` on it, and on that `map` we can pass it a function that will run.  
Instead of writing the function inside of here like we did last time we can go ahead and say `this.renderOrder` and pass that off to a separate function, just to keep our code as clean as we possibly can.

So right above this `render` we'll call `renderOrder`.  that will be a function and that will take in the `key`, because again the `orderIds`, that's just a list of `fish1`, `fish2`, `fish3` so that each one will pass in a `key`, and let's just do again - let's return a list item with the key inside of it.

Just to make sure it's working and then we'll dive a little deeper into it to get all the different parts displaying.  So Load Sample Fishes in, add Pacific Halibut... there we go.  It's showing me `fish1`, `addToOrder`, `fish2`... if I add more of them it doesn't show me how many I've ordered, but it does not add it multiple times which is great.  If I add in "Oysters" it shows me `fish7`.

So that's great, I'm happy with that.  Now let's go back to this list item right here, and instead of just displaying the `key` we want to go ahead and display a little bit more information.

So put that in parentheses so we can use multiple lines in JSX, and we first need a couple of variables before we can even go to `return`, so right above `return` say `var fish = this.props.fishes[key]`.

Just like we did in the other one we need to get all of the details about the fish, not just the `key`, we need the `image` and the `title`.

We want the `count`, how many have they bought, and the count is stored in our `order` state, so if I open up my `<Router>`, go to my `<RoutingContext>`, `<App>`, `order` - we want this number right here, 4, 9 or 8.

So `this.props.order[key]`, and again `key` is going to be like `fish1`, `fish2` - it's just dynamic.  That's why we use square brackets there.

We're going to have a Remove button, but that's in a video coming right up.  Now we can go ahead and render everything on out.  

First we need to say if there's no fish, for example if they've deleted the fish, we can return a list item.  A `key` of `{key}`.  That is just there for when we do animation, so I'm just going to make sure that we preset that.

And we can say `Sorry, fish no longer available`.

If there is no fish just return a list item that says the fish is no longer available, and we'll let them delete that when we get into the management one.

Otherwise we are going to return this list item here, and inside of that list item I'm going to have a `<span>`.  Inside of that we'll have a `count` which will show them how many - and we can tack on an `lbs` to that, we can say "four pounds of `fish.name`", and we also want to give it a price, so give ourselves a `<span>` with a `className` of `price`, and inside of that - we don't just want to say `fish.price`, because that will give us the price for a single fish, but we want to do `count * fish.price`, because that will actually go ahead and calculate the cost of - if we buy 4lbs of oysters it's got to be four times $25.49, and that is going to give us the number of cents, so we can wrap it in our little helper tag called `h.formatPrice`, and that should output it properly for us.

So give that a save and we'll check where we are.  Load in some Sample Fishes, add in some of the order... oh, beautiful.  We've got one pound of Pacific Halubut, one pound of lobster.  If I click that again, two, three four.

The spacing is a little bit weird, and I'm going to show you exactly why that's weird, is if we inspect it in - notin the React devtools but just in the regular devtools.

You'll see that React doesn't like when you just have plain old text inside of it, because if this is a variable right here, chances are that variable might just be updated and it doesn't have any sort of element around that.

So if we look at it, React actually just puts a `<span>` around every single little thing right here.  So if any of these thihngs over here change - most likely this one is going to change - that it knows exactly what `<span>` to update, and I'm using some CSS where it's trying to evenly centre them all, but when we get to the animation video we will fix all of that, so not to worry about that.

So let's just try it again. Update, Update, Update as we go.  Same if we go into our React devtools, open up our `App`, click on our `order` and if you were manually change our order from one to ten, immediately the number that we ordered, the price of that order as well as the total is immediately updated.