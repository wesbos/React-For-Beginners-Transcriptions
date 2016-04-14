# 22 - ES6 Components Extended

By now you probably have a pretty good handle on making your components with ES6 classes, and what I'd like you to do now is try to convert the rest of them so we have `fish`, we've got `addFishForm`, we've got `inventory`... try to convert all of these `React.createClass` on to our new `extends React.Component`.

So I'm going to go ahead and do the rest on the video here, but if you can do them yourself feel free to skip the rest of this video.

### Inventory

So I'm going to - let's start with the `inventory` one here.  So instead of this we'll say `class Inventory extends React.Component` and open up our block there, we can close that and go down and delete this parenthesis.

Cool.  And then we'll need to go through each of our functions here, so just two of them in this case.  Open them on up, that looks good.  Take away the comma in between so we've got our `renderInventory` method, we have a `render` one right here, and then we have these PropTypes.  How do we handle PropTypes?

Well after you've created your class you can go ahead and say `Inventory.propTypes` equals that.

So we'll close this on up.  So give that a save. 

What else do we need to do?  we need to autobind it, so we'll say `import autobind from 'autobind-decorator';` and we'll take that autobind decorator and we will autobind that.

So give it a save here, let's see if we have any errors.  Looks like everything is working properly.  Great.

So that's the `inventory` one, we moved over our PropTypes to that, everything else is good to go, so let's close that one down.

### Fish

Let's go through `fish.js` and convert that one.

`class Fish extends React.Component`.  Open and close your block.  Delete that one.  And here delete that.  Find our functions, there we go.  And that's looking good, don't have any PropTypes here, so we just need to - I'll just go to the `inventory` one and steal those two lines for the autobind up here, and that should be good.

"Add Fishes" we can add fishes to order, we can go ahead and delete them, we can go ahead and edit them, we can change the price, and we're good to go.

So that's `fish`, good.  We've got our `App`, `<AddFishForm>` - let's do that one.

So `class AddFishForm extends React.Component`.  Delete that, that.  we're going to go ahead and grab all the functions in here which is just two.  Get rid of that, and what else do we have to do here?  We don't have any PropTypes so it looks like we have to go ahead and steal the `autobind` in as well so we can autobind that.  give it a save.

Looks like I'm getting an error in my terminal so I'm just going to bring that up.

It's telling us `Unexpected token on line 11`, so look here, `createFish` - something is wrong with this - there we go.  It's because I have my `React.Component` opening with a parenthesis and it needs to be open with a curly bracket.

So give that a save and that looks to be loading just fine.  Still works.  `<AddFishForm>` is still running.  'add new fish', '100', 'testing', 'testing.jpg'.  That seems to be working just great, I'll still be able to remove it, great.

So we've got `<AddFishForm>`, we've got our `App`, we've got our `fish` we've got our `header`, we've got our `inventory`, we've got our `NotFound`.

### Order

`order.js`, let's go ahead and do that one.  So `class Order extends React.Component`, set up our block, close our block.  We've got some PropTypes here, so we can say `Order.propTypes = ` that.  Make sure our indentation is good, and then we'll go through and change out all of these functions.  Delete those and get rid of this.

So that one's done, `render` function is done, looks to be right.  Let's check our `order`, see if it loads.

We've got an error here: `Cannot read property 'props' of undefined`.  Oh, I forgot the autobind on this one, so it's trying to bind it on to the wrong thing.  So we should be able to import that autobind and this will append it to the class.

Give that a save, awesome.  That seems to be working.  And `StorePicker`.

---

So that looks like all of our classes have been converted to ES6.  Again a quick review: you can't use mixins so we use `reactMixin`.  The autobinding stuff doesn't happen automatically for you so we used the ES6 decorator which we had enabled in our Gulp file to compiles, and we just need to make sure that we're properly passing our state, our mixins and all of our PropTypes.