
ConsoleAway
===========
**version:** 0.9b

Bash script to remove ```console.log```, ```console.warn```, ```console...``` from javascript and coffeescript files.

####WARNING
First the classic " i am not responsible for any damage, loss of files, rage towards the machine or causing world disasters... " 
This is my first batch script ever and i have being using it in a controlled environment, hence...**EXPECT PROBLEMS**, recommendations are **very appreciated**.

---

###USAGE
```
user@machine$ ./consoleaway.sh
Usage: consoleaway.sh options (-f -r) [FILE || FOLDER]

consoleaway -h
  Prints this message

consoleaway -f [FILE.js || FILE.coffee]
  Process single file

consoleaway -r [FOLDER]
  Process folder recursively

WARNING: this will modify your files make sure you back them up
```
---

###WHY
This came out of the necessity to automatically remove the darn ```console``` upon deployment to a production enviroment.

I never felt comfortable neither with overriding the ```console``` object nor any of the options suggested [here](http://www.elijahmanor.com/grunt-away-those-pesky-console-log-statements/") for that matter, but take a look they might work for you.

If you use node or can afford to spawn a node instance for such tasks i totally recommend [groundskeeper](https://github.com/Couto/groundskeeper) as a waaaayyyyy more robust solution.

---

###HOW
The script goes trough the file(s) and comments out the beggining of a ```console``` instruction using [sed](http://en.wikipedia.org/wiki/Sed) ( available in both Linux and OSX ).

Use it before the linting and/or minification process, that way you can check if the ```console``` removal process caused undesired effects with lint, and you can remove them out with your minification tool hence not weighting on your file.

---

###DOS AND DONT'TS
You can expect the script to work on basic ```console``` declarations, a quick reference can be seen below

this code:
```javascript
// ALL GOOD
console.log('hello');
var a = 1; console.log(a);
console.log({ ping: "pong" });
console.log(function(){return true});
console.log('one'); console.log('two');

// TROUBLE AHEAD
function heyThere() {
  DEBUG && console.log( "Hey There!" ); 
}

console.log(function(){
    console.log('woot');
    return true
});
```

will come out as:
```javascript
// ALL GOOD
//console.log('hello');
var a = 1; //console.log(a);
//console.log({ ping: "pong" });
//console.log(function(){return true});
//console.log('one'); console.log('two');

// TROUBLE AHEAD
function heyThere() {
  DEBUG && //console.log( "Hey There!" ); 
}

//console.log(function(){
    //console.log('woot');
    return true
});
```
The need to have some discipline while writting ```console``` instructions is obvious. The problem is, as it encouters the instruction it comments it out, on the spot, ignoring anything behind, ahead, before or after the instruction. You can probably guess by now that running the script on a **already minified file is a big no no**.