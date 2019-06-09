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

This is a manufactured problem caused simply by not understanding how the language works. Any time you don't understand how a language works, regardless of the language, you will be confused. Period. The solution is to actively learn how it works rather than to passively complain.

## #3: "Automatic semicolon insertion."

Use an editor, IDE, or linter which catches missing semicolons. In any language that uses semicolons, you're prone to unintended behavior when you leave them off. In a compiled language, this happens at compile-time. In an interpreted language, this happens at runtime.

## #4: "JavaScript is seriously abused. Much of the code in the wild, especially those in commonly used libraries, are [sic] very badly written."

This is grasping for straws; It's not even really a complaint about Javascript at all, and it's impossible to confirm or falsify.
