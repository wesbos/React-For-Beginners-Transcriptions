

## 4 - Writing HTML with JSX

### NB
Hey there, quick note about the code in this upcoming video as well as a lot of the code you're going to find online.

Up until version 0.14 React looked a lot like this where we `require`d React and we called `React.render`, we pass it our component and we pass it where we'd like it to render to, and this is what we talked about in the last video, except in the last video I showed you that as of 0.14 we need to `require` a separate package called `ReactDom`, so we had `var ReactDom = require('react-dom');

And then go down here, and instead of calling `React.render` we call `ReactDOM.render`, and that's just a small thing that React has done in order to take all of the stuff that's related to actually touching the DOM, which is very little, is it has put it in its own package.

So rather than just switch it out in the videos I thought I'd show that to you, just because you're gonna be coming across a lot of examples online that just show you plain old `React`. 

As we're developing modern React right now make sure you switch that to `ReactDOM.render`.  Alright, enjoy the video!

***

You may be looking at this saying howcome I can just write HTML? I'm right inside my javascript and we're not getting any errors in our console. 

If you just tried to do this in regular JavaScript you'd get "`unexpected token <`".  Howcome I can just write `<StorePicker/>`  and it's not surrounded in quotes?

Also if you've ever tried to write HTML in JavaScript you know that it's sort of like:

```javascript
return (
	"<p>" + hello + "</p>"
	)
```

And you're concatenating your variables, and it's just a pain in the ass to actually maintain because you don't have any syntax highlighting, and if you mistake an open or closed quote everything breaks.

That's because we are writing JSX and part of our Gulp build task will transpile it into regular js.  So you don't actually need to use JSX when you're building React, you can just use a simple

```javascript
React.createElement('p', {className: 'testing'}, 'content'
}
```

`React.createElement`, and first you pass it what element you want (`'p'`), second you pass it what attributes you want, so you could say `{Classname: 'testing'}`, and then third you pass it the content. 

And what I've done there is I've created a `<p>` with a class of `testing` and the content inside of it, and if I went here and inspected that you'll see that here's a `<p>`, a class of `testing`, and the content is inside of there.

However, who wants to do that?!  That seems like a lot of work, and I'd rather just write my plain old HTML.  So that's why we use JSX syntax.

So there's a couple of things we need to know about writing JSX, and we actually want to go ahead and build the form, so I'm going to give myself a form tag.

If you use Emmet, you can still use your Emmet inside of it.  You simply just type the tag that you want and hit `Ctrl+e`, and that will expand it, and inside of that `<form>` tag we are going to put a bunch of stuff.

Before I do that I want to say it's really important that you're always returning a single element.  You can have many, many children inside of there, as long as you're always returning a single element.

So if I went back here and just returned `<p>Hello<p>` and then another paragraph that said `<p>World<p>` and gave it a Save, you'll notice that your console gives you an Error right here, where it says:

> Adjacent JSX elements must be wrapped in an enclosing tag (13:6) while parsing file: ...

So it doesn't like for you to return to sibling elements.  You must always wrap them in some sort of parent element like a `<div>`, and then you can return those two as children.

So give that a Save, it recompiles just fine here, and you would then see "Hello World."

But that's not what we want!  We actually want a `<form>` tag, so I'm going to go ahead and do that.  I don't need an `<action>` there, we're going to talk about that later.

Then inside of that we'll just quickly fill out everything else we want.  We want an `<h2>`, that's going to say 'Please Enter A Store', and we want an `<input>` with the type of `"text"`.  

Where we need a `ref="storeId"` - and we're going to talk about what the `ref` is in just a second - and then finally another `<input>` with the type of  `"Submit"`.

Now I'm going to go ahead and give that a Save, and you'll notice that our Gulp task throws a little error to us.  If we go to our console, it's telling us:

> Expected corresponding JSX closing tag for <input>.

So some of our elements are self-closing elements, which means - a `<form>` has an open and close tag, `<h2>` has an open and close, but things like `<input>`, things like an `<image>` tag, all these things, they are self-closing, and React doesn't like it when you don't self-close it.

So you actually have to go back - I know this is something that a lot of developers are starting to stop doing - but you have to put the closing angle bracked right inside of the `<input>` tag for it to actually go ahead and work.  

So give that a Save.  Looks like our Build task is working correctly again.  If we head back to the browser it looks like we've started to get some of it, but the styles aren't working for us, and that's because this `<form>` needs a `class` on it.

So normally I would just do something like this:

```javascript
<form class="store-selector">
```

and that's going to line up with some of the css that I have.  However if we go here (browser) you'll notice that our console has a warning in it.  It says:

> Warning: Unknown DOM property class.  Did you mean className?

This is a little bit of a gotcha in JSX, essentially because `class` is a reserved word, which means you're not allowed to use the word `class` in JavaScript unless you're actually creating a class.

So we can't just say `class=`, we have to say `className="store-selector"`, and watch this refresh.  There we go - JSX will change it to say `class` - see it says `class` right here - but if we look in React if we open that up it says `className` there.

So that's kind of a gotcha.  That always gets me, but it's something that you'll learn as you go along.

Other things: we want someone to actually type in something, so we can say:

```
required />
```

That's no problem.  That's just the `required` attribute.  

Another thing we want to know is that when you have a variable and you want to put that variable inside of there - we're going to learn more about this in the future - let's say we have `var name = "wes"` and we want to put that `name` variable somewhere inside here.

So we say:
```javascript
<h2>Please Enter A Store {name}</h2>
```

and React uses single curly brackets in order to interpolate that variable there, and we can go back to our browser and you can see right away that variable is interpolated. 

Which is pretty cool, you don't have to do any sort of concatenation.  You simply just say `{name}`.

Similarly, if you wanted to make a comment, say `// creating the store` - give it a Save - you'll notice that what actually happens is that your comments show up in there, and that's because there are JavaScript comments, however we are inside of JSX at this point, so it's technically not JavaScript - we're writing JSX.  

Comments, for whatever reason - I'm not too fond of this - but comments are going to show up.  Even if you use block comments, they're going to show up right in your HTML, which is not what we want.

So the way that you can write comments in JSX is with curly brackets and *then* do a block comment, which is a `/*` and a `*/` and then your comment goes in the middle.

So it's a bit of a pain to write comments inside of JSX.  If you're up here you can write normal comments, because right here we're not inside of JSX.  It's only when we're returning JSX that you have to do this, but that's not going to show up for us. 

Similarly if you wanted to comment out some HTML you'd simply just open it up and then close it up when you don't want it, and that's going to comment out these two items there.

Finally, in a coming video what we're going to do is what happens when you actually submit this.  When someone submits it or clicks it we're gonna learn about inline event listeners called `onClick` and things like that, but that's for a future video.

We'll see you in the next one.

