#Ragen's Thoughts on "Master the JavaScript Interview: What’s the Difference Between Class & Prototypal Inheritance?" by Eric Elliot

###Tasks
- [x] Read the article
- [x] Talk about it in a README.md file
- [x] Use Markdown to code your readme.
- [x] try the code thingies in codepen
- [x] copy the URL from codepen into the readme.

*NOTE: this is day 1 on these topics. I am allowed to not know stuff yet.*

*NOTE 2: This author uses OO a lot without explanation. I googled it. That stands for **Object Oriented Design **.

##Class Inheritance
Elliot (2016) defines class inheritance this way:
>Class Inheritance: A class is like a blueprint — a description of the object to be created. 
>Classes inherit from classes and create subclass relationships: hierarchical class taxonomies.

This makes sense to me. This thing has qualities. It also has a kid. That kid has his own qualities and his own kids. But no one would make it without the original parent thing. Okay.

But then Elliot says a bunch of words in the second and third paragraph, and I don't know what any of those words put together mean. I even tried googling parts of it. No dice. He says:
>Instances are typically instantiated via constructor functions with the `new` keyword. Class inheritance may or 
>may not use the `class` keyword from ES6. Classes as you may know them from languages like Java don’t 
>technically exist in JavaScript. Constructor functions are used, instead. The ES6 `class` keyword desugars to a 
>constructor function:
>class Foo {}
>typeof Foo // 'function'
>In JavaScript, class inheritance is implemented on top of prototypal inheritance, but that does not mean that it does the same thing:
>JavaScript’s class inheritance uses the prototype chain to wire the child `Constructor.prototype` to the parent `Constructor.prototype` for delegation. Usually, the `super()` constructor is also called. Those steps form single-ancestor parent/child hierarchies and create the tightest coupling available in OO design. 

Yes. I'm sorry to say, I don't understand a lick of what he's saying there. So explaining it to you, dear reader, is impossible. Perhaps you can tell me. 

##Prototype Inheritance
Elliot defines prototype inheritance (though we all learned it as "Prototypical Inheritance") as:
>Prototypal Inheritance: A prototype is a working object instance. Objects inherit directly from other objects.

This is a bit murky for me, as we haven't worked with it yet. So I don't know how to translate this definition into something a normal person (like me) would be able to read or work with. It's just jibberish as far as my brain is concerned. Using my earlier translation I guess you could say that you have a Thing. And you have another Thing.  And *somehow* one of the things lends stuff to the other one. How this happens or why it works, I don't know.

There were more words that were used. I'll include them here so that you, dear reader, can reference them:
>Instances may be composed from many different source objects, allowing for easy selective inheritance and a 
>flat [[Prototype]] delegation hierarchy. In other words, class taxonomies are not an automatic side-effect of 
>prototypal OO: a critical distinction.
>Instances are typically instantiated via factory functions, object literals, or `Object.create()`.
>“A prototype is a working object instance. Objects inherit directly from other objects.”

##Why This Matters
Well, it's hard to talk about why something matters when you don't understand the basic definitions in the first place. So I'm going to give up on that. Just go read the article yourself, if you're so interested. It is here: https://medium.com/javascript-scene/master-the-javascript-interview-what-s-the-difference-between-class-prototypal-inheritance-e4cd0a7562e9#.jt4bt8gdb 

Here is another quote from the article about things that can happen if you don't use prototype whatever:
*The tight coupling problem (class inheritance is the tightest coupling available in oo design), which leads to the next one…
*The fragile base class problem
*Inflexible hierarchy problem (eventually, all evolving hierarchies are wrong for new uses)
*The duplication by necessity problem (due to inflexible hierarchies, new use cases are often shoe-horned in by duplicating, rather than adapting existing code)
*The Gorilla/banana problem (What you wanted was a banana, but what you got was a gorilla holding the banana, and the entire jungle)

###What *Not* To Do
I saved this thing into codepen, but wasn't sure what I was supposed to do with it, since it's just an example of what *not* to do. So I forked it and saved it here: http://codepen.io/RagenS/pen/OpLowQ?editors=001

###What *To* Do
http://codepen.io/RagenS/pen/gmYdZo?editors=001

##Three Types
He said:
*Concatenative inheritance: The process of inheriting features directly from one object to another by copying the source objects properties. In JavaScript, source prototypes are commonly referred to as mixins. Since ES6, this feature has a convenience utility in JavaScript called `Object.assign()`. Prior to ES6, this was commonly done with Underscore/Lodash’s `.extend()` jQuery’s `$.extend()`, and so on… The composition example above uses concatenative inheritance.
*Prototype delegation: In JavaScript, an object may have a link to a prototype for delegation. If a property is not found on the object, the lookup is delegated to the delegate prototype, which may have a link to its own delegate prototype, and so on up the chain until you arrive at `Object.prototype`, which is the root delegate. This is the prototype that gets hooked up when you attach to a `Constructor.prototype` and instantiate with `new`. You can also use `Object.create()` for this purpose, and even mix this technique with concatenation in order to flatten multiple prototypes to a single delegate, or extend the object instance after creation.
*Functional inheritance: In JavaScript, any function can create an object. When that function is not a constructor (or `class`), it’s called a factory function. Functional inheritance works by producing an object from a factory, and extending the produced object by assigning properties to it directly (using concatenative inheritance). Douglas Crockford coined the term, but functional inheritance has been in common use in JavaScript for a long time.

I really liked this part about the secret sauce:

>As you’re probably starting to realize, concatenative inheritance is the secret sauce that enables object 
>composition in JavaScript, which makes both prototype delegation and functional inheritance a lot more 
>interesting.

##Fragile Base Class Problem
Dude. If you use the old way that he says not to do, and you change something at the top, you break all of the kids. Poor, little broken children, lying around without their limbs. So sad.


##With Composition
So if you use the better way, he says: 
>Imagine you have features instead of classes:
>feat1, feat2, feat3, feat4
>`C` needs `feat1` and `feat3`, `D` needs `feat1`, `feat2`, `feat4`:
>const C = compose(feat1, feat3);
>const D = compose(feat1, feat2, feat4);
>Now, imagine you discover that `D` needs slightly different behavior from `feat1`. It doesn’t actually need to 
>change `feat1`, instead, you can make a customized version of `feat1` and use that, instead. You can still 
>inherit the existing behaviors from `feat2` and `feat4` with no changes:
>const D = compose(custom1, feat2, feat4);
>And `C` remains unaffected.


#You wanted us to put code in here.
It's not my own example, because I don't really know what to do for that. but here is what he did with the guitar.
```javascript
// Composition Example

// http://codepen.io/ericelliott/pen/XXzadQ?editors=001
// https://gist.github.com/ericelliott/fed0fd7a0d3388b06402

const distortion = { distortion: 1 };
const volume = { volume: 1 };
const cabinet = { cabinet: 'maple' };
const lowCut = { lowCut: 1 };
const inputLevel = { inputLevel: 1 };

const GuitarAmp = (options) => {
  return Object.assign({}, distortion, volume, cabinet, options);
};

const BassAmp = (options) => {
  return Object.assign({}, lowCut, volume, cabinet, options);
};

const ChannelStrip = (options) => {
  return Object.assign({}, inputLevel, lowCut, volume, options);
};


test('GuitarAmp', assert => {
  const msg = 'should have distortion, volume, and cabinet';
  const level = 2;
  const cabinet = 'vintage';

  const actual = GuitarAmp({
    distortion: level,
    volume: level,
    cabinet
  });
  const expected = {
    distortion: level,
    volume: level,
    cabinet
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});

test('BassAmp', assert => {
  const msg = 'should have volume, lowCut, and cabinet';
  const level = 2;
  const cabinet = 'vintage';

  const actual = BassAmp({
    lowCut: level,
    volume: level,
    cabinet
  });
  const expected = {
    lowCut: level,
    volume: level,
    cabinet
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});

test('ChannelStrip', assert => {
  const msg = 'should have inputLevel, lowCut, and volume';
  const level = 2;

  const actual = ChannelStrip({
    inputLevel: level,
    lowCut: level,
    volume: level
  });
  const expected = {
    inputLevel: level,
    lowCut: level,
    volume: level
  };

  assert.deepEqual(actual, expected, msg);
  assert.end();
});
```

##Those URLs Collin wanted (again)
Even though I had them above, here are the links you wanted at the bottom.
http://codepen.io/RagenS/pen/OpLowQ?editors=001
http://codepen.io/RagenS/pen/gmYdZo?editors=001

I think I'm done!