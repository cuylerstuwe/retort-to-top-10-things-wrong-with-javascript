# Retort to "The Top 10 Things Wrong with Javascript"

Today, while skimming my email, I came across a piece of writing on Medium written by one of Quora's most notorious Javascript-haters.

The title of the article is *"The Top 10 Things Wrong with JavaScript"*, and you can view the piece in archive format [here](http://web.archive.org/web/20190609142005/https://medium.com/javascript-non-grata/the-top-10-things-wrong-with-javascript-58f440d6b3d8) in case it is deleted or edited.

I'll respond to each issue, piece-by-piece, here:

## #1: "There is no integer type."

Yes, this can be an issue at times. That's why [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) is being pushed through the proposal process.

On the other hand, as long as you're aware that you're always working with floating-point numbers, there's a set of consistent rules you can expect to be able to play by. You don't have to debug situations where integers and floating-point numbers are being converted implicitly in a large equation, influencing the outcome of a calculation ways that might be unintuitive.

## #2: "JavaScript’s loose typing and aggressive coercions exhibit odd behaviour."

Here's the first few lines, verbatim, of the example given to demonstrate this "problem":

    [] + [] → "" // Empty string? These are arrays!
    [] + {} → [object object]
    {} + [] → 0 // Why isn't the operation commutative???
    {} + {} → NaN // ???
    
My question is: *"What result were you **expecting** as the outcome of adding two arrays? An array to an object? An object to another object?"*

**There is no reason you should be trying to use an infix addition operator to "add" an object to an array.**

The ridiculousness of the example aside, this problem is manufactured simply by not understanding how the language works. Any time you don't understand how a language works, regardless of the language, you will be confused. Period. The solution is to actively learn how it works rather than to passively complain.

**Javascript can sanely add only *"add"* strings *(as concatenation)* or numbers *(as floating-point addition)*.**

Here's a quick breakdown of why each of these examples work the way they do:

### 1. `[] + []` returns `""`

These are two empty arrays being added together. To produce a result, Javascript casts them to something it knows how to perform *"addition"* on: **strings**. Arrays are converted to strings by calling `.toString()` on each of of their elements and concatenating the results with a comma between them.

So, for example, the array:
`[1, 2, "surprise"]`
would convert to the string:
`"1,2,surprise"`.

In the example above, the emptiness in each `[]` leads each array to become the string `""`, and result of concatenating two `""` is `""`.

### 2. `[] + {}` returns `"[object Object]"`

Again, by the rules in step 1, the empty array is cast to the string `""`. The `toString()` method of `{}` *(a plain-old-object)*, by default, returns `"[object Object]"`. `""` concatenated with `"[object Object]"` is `"[object Object]"`.

### 3. `{} + []` returns `0`

This one seems like a head-scratcher, doesn't it?

**Here's the trick:** `{}` is **not** used *solely* for object literals.

Instead, `{}` is interpreted here as an **empty code block**. This empty code block is essentially ignored for the purpose of returning a value, and it is juxtaposed next to a unary `+` which, according to the rules of the language, casts the string result of `[]` to a number. We get the same `""` string result from the empty array that we've been getting, and then `""` is converted to the number `0`, as it will always be, by-spec. It might not seem obvious that an empty string converts to the number zero; Maybe `NaN` would make more sense. Possibly. But if this conversion seems nonsensical to you, don't put the language in a position where it has to compare disparate types.

**NOTE:** If you want to get this to evaluate the way you expect it to, surround the `{}` in parenthesis. This `({})` structure makes it clear to the parser that `{}` exists in the context of evaluating an expression, and so it will interpret `{}` as an object literal rather than as an empty code block. You'll get the `"[object Object]"` you might have been expecting.

### 4. `{} + {}` returns `NaN`

This one isn't necesarily true, and the ambiguity in whether it's true or not depends on how the code is resolved.

In **Firefox**:

![Firefox evaluating {} + {} as NaN](https://i.imgur.com/nS52LCP.png)

In **Chrome**:

![Chrome evaluating {} + {} as "[object Object][object Object]"](https://i.imgur.com/CzAFBH5.png)

Though all modern browsers agree that `{}` should be treated as a code block when they come across `{} + []` in the REPL, browsers disagree on whether the first operand is a code block or not when they come across `{} + {}` *(they do agree that the second `{}` is an object literal, though)*. It's a corner case in parsing that's not really specified *(probably because it's a stupid thing to intentionally do in the first place)*.

If the first `{}` is treated as a code block, as is the case with Firefox, then the expression being evaluated is effectively a unary `+{}`. This results in `NaN`, as `{}` is turned into `"[object Object]"`, which has no sensical means for being cast to a number by the `+` operator.

However, if the first `{}` is treated as an object literal, as Chrome currently does, then each of these is converted to the string `"[object Object]"`, and these are concatenated to generate the string `"[object Object][object Object]"`.

**Again: There's an important distinction between simply not understanding a thing vs. that thing making no sense.**

## #3: "Automatic semicolon insertion."

It's definitely a little strange. However, it only takes maybe a day to grasp.

In the meantime, while you're trying to wrap you head around it, use the standard Javascript style of danging your brackets, and use an editor, IDE, or linter which catches missing semicolons.

If you want to learn how ASI does its job, start by learning the difference between a statement and an expression, and go from there.

In **any** language which terminates statements with semicolons, you're prone to unintended behavior when you leave them off. In a compiled language, this happens at compile-time. In an interpreted language, this happens at runtime. There are only a handful of *"gotcha"* cases where not understanding it will mess you up, and memorizing these few scenarios is very simple.

## #4: "JavaScript is seriously abused. Much of the code in the wild, especially those in commonly used libraries, are [sic] very badly written."

This is grasping for straws; It's not even really a complaint about Javascript at all, and it's impossible to confirm or falsify.

Somewhere out there, someone is writing *"bad C"*, *"bad Java"*, and so on. A language can't force you to write code that's highly-performant and/or well-organized.

## #5: "JavaScript is highly dependent on global variables."

It's not. No examples were given to support this claim, because it's not true. Even before modules existed, you could use closures to avoid polluting the global scope with variables. It's standard professional practice to write highly-modular Javscript with either ES6 or CommonJS modules.

## #6: "JavaScript code can fail silently due to syntactical slip-ups."

Again, no examples were given, because this is grasping at straws. Any language can fail when you leave off a semicolon, when you leave a typo in a variable name, when you forget to leave a comma in an array literal, when you leave off the closing quote on a string value, and so on.

## #7: "Object prototypes do not scale well to large applications; it’s a rather primitive and sloppy way to do object-oriented programming (but it’s flexible!)."

Yet again, grasping for straws.

No examples were given to support the assertion that *"Object prototypes do not scale well to large applications."*

A dynamic and weakly-typed language like Javascript might not be ideal for enforcing interfaces in huge applications *(TypeScript to the rescue here!)*, but that has nothing to do with the difference between prototypal and classical inherence.

The author is conflating unrelated issues here.

## #8: "Asynchronous programming in JavaScript is very messy."

What year is this? 2006?

We're coming up on 2020, and today's Javascript has Promises, async/await, generator functions, and RxJS.

There's no reason that asynchronous code in Javascript needs to be messy; It's likely that the author of this article simply ditched Javascript in the past and hasn't kept up to date on recent trends.

## #9: "Douglas Crockford says that JavaScript is “Lisp in C’s Clothing.” Lisp is a wonderful language and by extension, so is JavaScript."

...

OK.

The author really wanted to write a *"Top 10"* title, but started running out of straws in the jar to grasp for.

Javscript's latest sin is apparently that *"it's not Lisp"*.

## #10: "The main draw of JavaScript is actually in frameworks like Node.js and AngularJS. If not for them, why would you use JavaScript??? Prior to 2009, when Node.js was released, people generally avoided using JavaScript; its use was limited to some jQuery and webpage enhancements."

This is a **misunderstanding** of the history of the web relevant to Javascript's growth.

*People didn't start using Javascript in droves the very moment that Node.js was released!*

The reason that people didn't use Javascript so much *(and why Javascript's spec floundered in the period between 1999 and 2009)* is that Flash, Java applets, and various ActiveX extensions were popular.

In the 10 years between 1999 and 2009, we'd seen utter dominance of the browser market shift between three browsers; First, IE, then Firefox, then Chrome. These browsers varied wildly in how they implemented things, and cross-browser compatibility was a major issue.

In a world of instability, Flash was the cross-platform tool of choice to guarantee consistency. As long as a browser had a Flash extension, you could write a Flash applet that would embed consistently in a page across all desktop browsers. Browser compatibility was such a hell that it was much simpler to simply frame something seperate from the browser within the browser.

Toward the end of that 10-year stretch, Apple released the iPhone with no support for Flash. Later, Steve Jobs would write his [Thoughts on Flash](http://archive.is/LKxtg), cementing his support for HTML5 and Javascript to replace Flash. Around this time, HTML5 granted Javascript some magical animation features that had previously been available only to Flash or other applets. With the growing expectation that most web browsing would occur on smartphones, and with one of the most popular smartphones not supporting Flash *(or other browser plugins)*, what language remained? Which language worked on iPhones and Android phones? Which language worked on the iPad and the Android tablets that copied it? **Javascript**.

Concurrently around the time the iPhone was released, Crockford was writing his seminal book *"Javscript: The Good Parts"*. It was published in 2008, and provided developers with a reasonable stopgap for writing well-organized Javascript in the period before ES6 *(2008-2016)*. This powerful book would undoubtedly have a serious influence on Node.

What else happened around 2008? Google [developed V8](https://en.wikipedia.org/wiki/Chrome_V8) for Chrome. Javascript started getting significantly more performant, as one of today's most powerful companies in the world took a vested interest in making it so. It needed to, after all; Gmail was a single-page app with a ton of client-side code, and it would become publicly available a year later, in 2009.

Java applets died out around 2014 or so, as [public perception and national agencies considered applets to be a security risk](https://krebsonsecurity.com/2016/02/good-riddance-to-oracles-java-plugin/), further cementing Javascript's position as *"the client-side language of the web"*.

Flash was on its deathbed around this same time that Java applets were dying out, heading toward [the looming 2020 phase-out](http://archive.is/5Teyn). Which language were Flash applets written in, by the way? **ActionScript 3**, which was... **[basically Javascript](http://archive.is/LWbQH)** *(along with some features which would, later on, inform ES6)*. Moving from the dying Flash to the growing Javascript was both painless and a no-brainer.

Speaking of 2014... Despite its release a few years earlier, Node actually hadn't actually reached a tipping point to start becoming popular until around this time. It took those few years for its features, ecosystem, and community to reach a critical mass. And it couldn't have happened at a more opportune time -- Ruby *(on Rails)* had been popular for a while, but people had started becoming fatigued of it. Node's promise of being able to write a single language *(Javascript)* on both the client and server was a very unique proposition, and the allure was enough to draw some people away from the hipster web framework they'd become so invested in to that point.

**These are some of the combined circumstances which triggered the Javascript renaissance.** It **wasn't** any **one** of them in isolation; It was all of them influencing one another, along with any other critical things I may have missed. Between 2008 to 2019, Javascript started to run faster, people learned to write better Javascript, and Javascript became more ubiquitous.

It's OK for a language to be valued for its ubiquity. English is that way -- Full of weird mannerisms that make it hard to learn, but one of the world's most valuable languages as a consequence of who is using it *(and for what)*. People write terrible English all of the time, but rarely blame the language itself for this. And because of its global draw, English constantly mutates according to the needs of global culture. Javascript is similar. If a student learned C# or Java in school, only to write Javascript in his first job, he will take those influences along for the ride *(and perhaps conceive Typescript as a result, or ES6's `class` semantics, etc.)*.
