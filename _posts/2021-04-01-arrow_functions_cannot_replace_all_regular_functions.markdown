---
layout: post
title:      "Arrow functions cannot replace all regular functions."
date:       2021-04-01 19:01:42 +0000
permalink:  arrow_functions_cannot_replace_all_regular_functions
---





Like any tool in one’s tool box, the wielder decides how and when to use it. 
Using the right tool for the job can make quick work of the task at hand.
Using the tool incorrectly may still complete the task but, possibly, with unintended results. 

When I tried to add arrow functions to my toolbox, I found that they saved me from typing “function” and “return” by placing a fat arrow immediately before the expression. 

let students = ["harry", "ron", "hermione", "ginevra"];
let rollCall = students.map(function(student) {
  return student + " the wizard";
});
//=> rollCall = ["harry the wizard", "ron the wizard", "hermione the wizard", "ginevra the wizard"];
 
vs

let students = ["harry", "ron", "hermione", "ginevra"];
let rollCall = students.map( student => student + " the wizard" )
//=> rollCall = ["harry the wizard", "ron the wizard", "hermione the wizard", "ginevra the wizard"];


Looking closely at arrow function syntax, “param => expression.”
Because student is the only argument, parenthesis are optional. 
If multiple arguments were written into this function, the arguments list would however, need to be wrapped in parenthesis and comma separated. 

(param1, param2) => expression

Because the expression is simple, the return is implied. 
If the body were to include more lines, brackets and an explicit return statement would be needed; 

param => { let a = 1; return a + param }


I thought, “Great!, let us avoid typing out ‘function’ and bask in concise readable code!”
I found that using arrow functions can trim down keystrokes significantly, mostly by not have to backspace to fix a spelling error(funciton and retrun.) 

I found that arrow functions can not only trim down the function body when array processing, but also make promise chaining more concise. 


	promise.then(a => {
  	// ...
	}).then(b => {
  	// ...
	});

A newly instantiated developer might think Arrow functions can do anything!
That lasts until one tries to use an arrow function to instantiate a new developer object. ;)


var Dev = () => {};
var dev = new Dev(); // TypeError: Dev is not a constructor

In fact, MDN docs explain “Arrow functions cannot be used as constructors and will throw an error when used with new.” Here we have our first reason not to use an arrow function, it’s forbidden with the 'new' keyword. Surely there must be other reasons not to use them. 

In real life, using a tool improperly can do all sorts of damage, void warranties, cause injury or other unintended consequences. This holds true with regards to functions as well. It took an eye opening blog post from Jason Orendorff, to help expand my understanding and more importantly, help me decide when to reach for the arrow function.

https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/

Before reading the article, I did not recognize the authors name, but I did notice how the date was from several years ago. I kept reading, thinking along the way, “wow, the author seems to know what he’s talking about.” Towards the end of the post, all becomes clear with this paragraph, 

“ES6 arrow functions were implemented in Firefox by me, back in 2013. Jan de Mooij made them fast. Thanks to Tooru Fujisawa and ziyunfei for patches.” 

If the those behind implementing arrow functions suggests to follow rules when using them, please follow them. Orendorff suggests using non-arrow functions for methods that will be called using object.method() syntax; functions that will receive a meaningful ‘this’ value from their caller. 


var obj = { // does not create a new scope
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log(this.i, this);
  }
}
obj.b(); // prints undefined, Window {...} (or the global object)
obj.c(); // prints 10, Object {...}


Inside the arrow function body (b), ‘this.i’ returns undefined and ‘this’ is referencing the window and not the object as one might expect. Why is this? First of all, we are trying to use arrow functions with object.method() syntax, breaking the rules. Second, from the MDN docs, “Arrow functions establish “this” based on the scope the Arrow function is defined within.” Arrow functions do not define their own execution context, ‘this’ is lexically bound, using ‘this’ from the caller outer function.
In classical functions, ‘this’ is bound to values based on the context in which it is called, dynamically. 
Arrow functions may still be used within objects, just not on the object method directly.  

var obj = {
    count : 10,
    doSomethingLater : function(){ // of course, arrow functions are not suited for methods
        setTimeout( () => { // since the arrow function was created within the "obj", it assumes the object's "this"
            this.count++;
            console.log(this.count);
        }, 300);
    }
}

obj.doSomethingLater();

The arrow function inside setTimeout(), created inside doSomethingLater, inherits the ‘this’ value of the outer function because it was created within the object. 

I am proud to say that I now understand why regular function expressions should be used when dealing with object.method() syntax. And that arrow functions can be an effective tool when used correctly. They can save key strokes by sweetly handling simple tasks with readable concise code. In the end, the reason for choosing any type of function for a task will be driven by specific needs and a desired return. 
