# Add item to object if the object exists or create new object if it does not exists

I need to add an item to an object if the object exists, otherwise I need to create new object with the item.
I know the existence of Object.assign function, but I am not sure how to use it.

Right now this is what I tried:

`var myvar = obj ? Object.assign(obj, { "myvar" : "myvalue" }) : { "myvar" : "myvalue" };`

Giacomo M asked Oct 29 '19 at 10:10

[https://stackoverflow.com/questions/58605281/add-item-to-object-if-the-object-exists-or-create-new-object-if-it-does-not-exis]

# After reading this, I decided not to submit my answer: 

Thanks for contributing an answer to Stack Overflow!

- Please be sure to answer the question. Provide details and share your research!

But avoid …

- Asking for help, clarification, or responding to other answers.
- Making statements based on opinion; back them up with references or personal experience.

To learn more, see our tips on writing great answers.

# My prepared but not submitted answer

It would be helpful for us to better understand how to answer if you can provide additional details or more specifics, perhaps with some test cases and desired results, of what you're looking to do. I'm actually curious what would be a great solution to your question as well. If you already found your answer, would you mind sharing it? Otherwise, hope this helps! 

Just want to bring up a few points that could use some clarification:

 1. (I'm camel casing all your variables.) Assuming `obj` is an object that you want to check to see if it has the "item" (a property complete with its key & value pair), your ternary code is actually checking if `obj` is **truthy** or **falsy**. By the way, an empty obj would be truthy. If `obj` is not defined, you'll get a Reference Error, so I doubt "if the object exists" is what you're intending to check for. Your code also would not run if `obj` is `undefined`, `null`, or a non-object datatype.
 2. Your use of `Object.assign()` is more accurately checking if `obj` has the key of `myVar` and it will assign/reassign it the value of `myValue`, aka override its old value, at the same time reassign `obj`'s property. So, if your target `obj` has `myVar` and some useful `"oldMyValue"`, it would be replaced with "myValue".

Regarding the syntax: Object.assign(target, ...sources) accepts (1) target object — what to apply the sources’ properties to, which is returned after it is modified, and (2) the source object(s) — objects containing the properties you want to apply. Of course, you can always refer to [its MDN][1].

If my understanding is correct: 

    // check if the object exists
    // if yes, add item (property) to the object
    	// if item matches an existing key, update the value
    	// if item is not an existing key, add item (key-value pair)
    // if not, 
    	// modify the object that was checked
    	// assign the evaluated result to a new variable 

I think your code is great and actually does exactly what you're asking, but, among other scenarios, expect: 

    // scenario 1:
    const obj = undefined;
    
    var myVar = obj ? Object.assign(obj, { "myVar" : "myValue" }) : { "myVar" : "myValue" };
    // obj evaluates to false, myVar evaluates to { "myVar" : "myValue" }
    // obj remains undefined

    // scenario 2: 
    const obj = { "hisVar" : "hisValue" }
    
    var myVar = obj ? Object.assign(obj, { "myVar" : "myValue" }) : { "myVar" : "myValue" };
    // obj evaluates to true; 
    // both obj && myVar = { hisVar: 'hisValue', myvar: 'myvalue' }


 3. AZ_'s code makes it shorter for sure, but changes the goal slightly... In AZ_'s code, he's assigning the contents of obj and adding "myVar" : "myValue" key-value pair into an empty object every time. At the same time changing the original obj.

His suggestion was:

    var myVar = Object.assign({}, obj, { "myVar" : "myValue" })

But then again both myVar and obj will now have the same properties, where as thatVar will be a separate variable. Using Object.assign() definitely is more sophisticated, yet, without some context or more specific goals, I'm wondering if Object.assign() is the right tool.

It's actually very similar to a declaring an object with an object literal, but this does not change the original obj. 

    var thatVar = { ...obj, "myVar": 'newValue'} // => updates 'myVar' key with 'newValue' value, replacing 'myValue' 

My guess is using a function can better achieve what you're looking to do, but I'm not confident if I understand what you're asking for. I'm also not too good at one-line cryptic codes if that's what you're looking for... 

  [1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
