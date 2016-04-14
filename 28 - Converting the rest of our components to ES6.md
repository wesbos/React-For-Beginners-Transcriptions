# 28 - Converting the rest of our components to ES6

By now you probably have a pretty good handle on making your components with ES6 classes, and what I'd like you to do now is try to convert the rest of them.

So we have `fishes`, we've got `AddFishForm`, we've got `inventory`.  Try to convert all of these `React.createClass` on to our new `extends React.Component`.

So I'm going to do the rest on the video here, but if you can do them yourself feel free to skip the rest of this video.

### Inventory

So I'm going to - let's start with the `inventory` one here.  So instead of this we'll say `class Inventory extends React.Component` and open up our block there.

We'll close that and go down and delete the closing parenthesis.

Cool, and then we'll need to go through each of our functions here - just two of 'em in this case - open them on up, that looks good.  Take away the comma in between, so we've got our `renderInventory` method, we have our `render` one right here, and then we have these proptypes.

How do we handle proptypes?  After you've created your class you can go ahead and say `Inventory.propTypes = ` that.  That will close itself on up, so give that a save. 

What else do we need to do?  we need to autobind it, so we'll say `import autobind from 'autobind-decorator'` and we'll take that autobind decorator and we will autobind that.

So give it a save here, let's see if we have any errors.  Looks like everything is working properly.  Great.

So that's the `inventory` one, we moved over our proptypes to that, everything else is good to go so we'll close that one down.

### Fish

Let's go through `fish.js` and convert that one.

`class Fish extends React.Component`.  Open and close your block.  Delete that one.  Go to the end here, delete that.

Find our functions.

And that's looking good, don't have any proptypes here so we just need to - I'll go to the `inventory` one and steal those two lines for the autobind, put them up here, and that should be good.

There's our fishes, we can go ahead and add our fishes to Order, we can go ahead and edit them, we can change the price, and we're good to go.

So that's `fish`, good.  We've got our `App`, `AddFishForm`, let's do that one.  

### AddFishForm

So `class AddFishForm extends React.Component`.

Delete that, that.

go ahead and grab all of the functions in here which is just two.

Get rid of that, and what else do we have to do here?  We don't have any proptypes so it looks liek we have to go ahead and steal the autobind in as well so we can autobind that. give it a save.

Looks like I'm getting an error in my terminal, so I'm just going to bring that up.

It's telling us `unexpected token on line 11`, so if I look here, `createFish`, it's telling me that something is wrong with this.

Oh, there we go, it's because I have my `React.Component` opening with a parenthesis and it needs to be opened with curly brackets.  So give that a save and that looks to be loading just fine.  Still works.  `<AddFishForm>` still running.  'Add New Fish'.  'testing', 'testing.jpg', and that seems to be working just great.  I'll still be able to remove it.  Great.

So we've got `<AddFishForm>`, we've got our `App`, we've got our `fish`, we've got our `header`, we've got our `inventory`, we've got our `NotFound`.

### order.js

`order.js`, let's go ahead and do that one.  So `class Order extends React.Component`.

Open up our block.

Close our block.

We've got some proptypes here so we can say `Order.propTypes = ` that.

Make sure indentation is good, and then we'll go through and change out all of these functions.  Delete those and get rid of this.

That one's done, `render` function is done.

Looks to be right.  Let's check our order, see if it loads.

We've got an error here: 

> cannot read 'props' of undefined.

Oh, I forgot the autobind on this one, so it's trying to bind it to the wrong thing, so we should be able to import that autobind and this will append it to the class.

Give that a save.  Awesome. That seems to be working, and `StorePicker`.

So that looks like to be all of our classes have been converted to ES6.  

---

### Quick Review

- you can't use mixins, so we use `react-mixin`.
- the autobinding stuff doesn't happen automatically for you so we used the ES6 decorator, which we had to enable in our Gulp file to compile
- we just need to make sure that we're properly passing our state, our mixins and all of our proptypes.
