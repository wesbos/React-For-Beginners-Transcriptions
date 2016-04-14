# 6 - Passing dyanmic data with Props

So far all the components we've made have been absolutely static, which means that the content that goes into making the component - in this case just the text and the tag - has been all static and I've just typed it in there.

However, if we take a look at our app here, you'll notice that all the parts of the fish, including the title, the image, the description and price, all of those are dynamic and we should be able to pass in data to the component before it gets built.

So in React the way that you pass data to a component before it's built is with something called "props".  So you may be used to HTML where we have these things - these are called HTML "attributes".  And the way that they work is that you can give the tag, which is `<img>`, `<p>` or `<input>`, a little bit more information about the tag when the browser goes to create it.

So for example an `<img>` you might tell it please use `"dog.jpg"` or you might give an alternate description of `"A cool Dog"`.

So that's just more information about the `<tag>`, and similarly in React we give props, which are the exact same thing as attributes, as a way of supplying more information to the tag.

So what we're going to do here is work through building the header, and we're going to make the tagline of the header dynamic by passing it props.  So that "Fresh Seafood Market" there is going to be totally dynamic.

So I'm going to work a little bit backwards here and go up to where we put in our `<Header>` tag, and notice that this is a capital 'H', Header because I just made that tag up.  That's not to be confused with the HTML5 header tag.

We're just going to go ahead and give it props called `tagline`.  And what's important to note here is that I just made this property called `tagline` up.  It's not something that's predefined in React.  You can just essentially make anything up, and you can pass it along in there.  Then when we go down to creating our header we can access that property, and we'll show you that in just a second.

So go ahead and delete the paragraph there, and we actually want a `header` tag, and that `header` tag is going to have a `className` of `"top"`.  Now inside of that we want an `h1` tag that says `Catch of the Day`, and we want `h3` tag which will have a `className` of `"tagline"` and inside of that I'm just going to say `"Fill me in"`, just because I want to show that we can make this dynamic.

So give that a save, and go back to our app here, and it should refresh.  There we go.  We've to this `Catch of the Day` as our `h1` and the `Fill Me In` is our `h3`.  Now don't worry that they don't look exactly like this.  We'll figure out how to do that with some `span`s in a second, but what I want to do is show you that if we go to the React dev tools, open up our app, click on our header - because again this is our `header` component, it's not a `header` tag. 

And if you go to the right here you'll see that this `header` component has props, and the one prop that it has is the tagline that we passed it.  If I went ahead and passed it something else like `tagline` and `num="5000"` for whatever reason, and we refresh it - and open it up again, you'll see that that information is then provided to the `header` component, and we can access it inside of our component with curly brackets.

So I showed you earlier that you can insert variables.  You simply just say `this.props.` and then the name of the property that you want, so `tagline`.  `this.props.tagline`.

Give it a save, refresh and you see that it's automatically pulling itself in.

So that makes sense.  I want to show you a couple of things.  If we had right above the return, inside of our `render` function.  If you want `console.log(this)`.  I want to show you that if we go to our console here, `this` refers to the constructor which is the header function.  If we say `console.log(this.props)`, let it refresh a second, you'll see that `this.props` is an object, which has all the information about what properties were sent to it.

So in our case it's just the tag line, and that's how we can easily access it in there.  In a future video what I'm going to be doing is showing you a couple of things.  First one is called `spread`, which allows us to pass all of our properties from a parent component to a child component, and then the second thing is going to be something called `propTypes`, which allows you to validate and make sure that all of your properties are of the right type.

But before we do that let's just make this `h1` and `h3` match. Kind of what we're looking for - just because this is a bit of a funny layout we need to add a few extra HTML elements in there. 

So, "Catch of the Day": let's just bring these `of the` on to their own lines.  And we need to have a `span` with the className of `"ofThe"` and we'll put `of the` inside of there, and then each of these ones also need a `span`, `className="of"`, and `className="the"`. 

Now all of this is nothing to do with React, it's just so I can apply the CSS around the "OF THE" as well as the anchor there.  I find that sometimes it's worth doing a little bit of extra work so the app looks cool and you're really really invested in it.

Last thing we need to do is wrap this `this.props.tagline`.  We just need to wrap that in a `span` so I can get a white background over the top of these little lines.  Cool. Again, nothing to do with React.

So that is our first component.  We are using props to pass information to the element.  If I change this to include "FRESH SEAFOOD GOOD" it should automatically change that out.  We should be able to open up our React here.  Go to header, see the props in here.

One cool thing is, if you click on a component it will say in the corner `$r` and the console.  So if you head over to the console and type `$r`, you can  actually see the entire component that we have there, and you can see that we've got props, and inside of there is taglines.

So sometimes if you're trying to debug it and you want to know what's going on, you can type `$r.props`.  You can see everything that's in there.  We're going to also have `$r.state`.  We don't have any `state` yet but we're going to learn about that in a coming video.