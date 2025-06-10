To start things off, JS is the programming language of the modern web.
This learning style and note is targeted for bug bounty hunting and web app pentesting purposes of learning JS.

**Setup**
For ease of learning, you can open Chrome browser.
Right click and 'inspect', navigate to sources tab, 'new snippet', name your file and save it.
Now we can run JS and interact with the console.

```javascript
alert("hi mom");
alert(123);

console.log("xD"); // kind of like print

// in console
typeof("hello"); // string
typeof(123); // number
typeof(true); // boolean
// ---

var num = 5+5;
console.log(num);

var psw = prompt("what is your password ;)");
console.log(psw);
```

```javascript
var i = 1;
while(i <= 50) {
	console.log(i);
	i++
}

for(let i = 1; i <= 50; i++) {
	console.log(i);
}

```