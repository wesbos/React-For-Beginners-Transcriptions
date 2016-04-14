# 26 - Handling React PropTypes with ES6

Next up we need to know how do you actually handle  PropTypes, because inside of a class these are all the methods and this is just a property on it.  So how do we actually handle PropTypes in it?

So let's go ahead and convert this `Header` component over to ES6 and then we'll figure out how that works.

So we can say `class Header extends React.Component`, and that will be a block, and I'm going to take that `render` function from there and put it inside mine, and we all know that we need to - don't need the word `function` there.  That looks good.

Now, what we've got left over here is this, don't need that - we've got this `propTypes` here, and the way that we handle that is we say `Header.propTypes = ` and that would actually just set the property on the actual class itself.  So it has to happen a little bit afterwards.

And make sure that works, give that a re-save, refresh and it works great.