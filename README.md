# RegReplacer
A library to facilitate **regex** matches and **replacing** matches.

```
const regRep     = new RegReplacer(/Word/g);
const str        = "Word Word Word";
const regMatches = regRep.match(str);
console.log(regMatches.replace(["This", "is", "RegReplacer."], "matches"));
This is RegReplacer
```
*This is RegReplacer.*

## Installing
`npm install regreplacer`

## Main Example
Setup.
```
// Generate a RegReplacer object tied to a specific regex
const regRep = new RegReplacer(/\$(\S*)/g);

// Create a string to match on
let string = "$v1 $v2 $v3";

// Calling match function on string returns a RegRepMatches object
let regMatches = regRep.match(string);
```

Retrieve match data.
```
// From the RegRepMatches object we can do several things
// First we can get the matches
let matches = regMatches.getMatches();
console.log(matches); // ['$v1', '$v2', '$v3']

// Next we can get the captures
let captures = regMatches.getCaptures();
console.log(captures); // ['v1', 'v2', 'v3']

// Finally we can get the indices of matches
let indices = regMatches.getIndices();
console.log(indices); // [0, 4, 8]
```

Replace on matches.
```
// After matching, we can replace the matches with an array of values
let newString1 = regMatches.replace(["This", "is", "cool."], "matches");
console.log(newString1); // This is cool

newString2 = regMatches.replace([1, 2, 3], "matches");
console.log(newString2); // 1 2 3
```

Replace on captures.
```
// Or we can replace the captures
let newString3 = regMatches.replace(["This", "is", "cool."], "captures");
console.log(newString3); // $This $is $cool

newString4 = regMatches.replace([1, 2, 3], "captures");
console.log(newString4); // $1 $2 $3
```

Replacements can be done programmtically.
```
let capMap = {'v1':'This', 'v2':'is', 'v3':'cool'};
let reaAr = [];

for(const match of captures) {
   reaAr.push(capMap[match]);
}

let newString5 = regMatches.replace(reaAr, "matches");
console.log(newString5); // This is cool
```

## API

### RegReplacer
Main class, constructor takes a valid regex and returns a RegReplacer object.
Invalid regexs will throw an error on construction.

#### Construction
```
const RegReplacer = require("../regreplacer.js");

const regRep  = new RegReplacer(/\S+/g);
const regReg2 = new RegReplacer(new RegExp("/s/g"));
```
Returns RegReplacer object.
#### Methods

##### Match
Match function takes a word and returns a RegRepMatches Object.
```
const regRep  = new RegReplacer(/Word/g);
const match   = regRep.match("Word");
```
Will throw and error if passed anything other than a string.
Returns RepRapMatches Object.


##### isClass
Used to determine class type equality. Implemented internally using symbols.
```
const regRep  = new RegReplacer(/\S+/g);
const regReg2 = new RegReplacer(new RegExp("/s/g"));
console.log(regRep.isClass(regReg2));
true
```
Returns boolean.

### RegRepMatches

#### Methods

##### getMatches
Returns the matches that would have been found after initializing RegReplacer and passing a string to match.
```
const regRep  = new RegReplacer(/Word/g);
const match   = regRep.match("Word Word Word");
console.log(match.getMatches());
[Word, Word, Word]
```
Returns empty array if no matches found.

##### getCaptures
Returns the captures that would have been found after initializing RegReplacer and passing a string to match.
```
const regRep  = new RegReplacer(/W(ord)/g);
const match   = regRep.match("Word Word Word");
console.log(match.getCaptures());
[ord, ord, ord]
```
Returns empty array if no matches/captures found.

##### getIndices
Returns the indices of the mathces that would have been found after initializing RegReplacer and passing a string to match.
```
const regRep  = new RegReplacer(/W(ord)/g);
const match   = regRep.match("Word Word Word");
console.log(match.getIndices());
[0, 5, 10]
```
Returns empty array if no matches found.

##### isClass
Used to determine class type equality. Implemented internally using symbols.
```
const regRep  = new RegReplacer(/Word/g);
const match   = regRep.match("Word");
const match2  = regRep.match("Word");
console.log(match.isClass(match2));
true
```
Returns boolean.

##### replace
Used to replace matches or captures with an array of values.
Extra values are ignored, and insufficient values will be replaced with undefined.
```
// Simple replacement on matches
const regRep  = new RegReplacer(/Word/g);
const match   = regRep.match("Word Word Word");
console.log(match.replace(["This", "is", "cool"], "matches"));
"This is cool"

// Too few arguments, undefined in output
const regRep  = new RegReplacer(/Word/g);
const match   = regRep.match("Word Word Word");
console.log(match.replace([1,2], "matches"));
"1 2 undefined"

// Too many arguments, last argument ignored
const regRep  = new RegReplacer(/Word/g);
const match   = regRep.match("Word Word Word");
console.log(match.replace([1,2,3,4], "matches"));
"1 2 3"

// Add a capture group to regex and replace on captures instead
const regRep  = new RegReplacer(/W(ord)/g);
const match   = regRep.match("Word Word Word");
console.log(match.replace(["This", "is", "cool"], "captures"));
"WThis Wis Wcool"
```

## Scripts

#### Testing
To run mocha/chai tests.
`npm run test`

#### Examples
To run the main example.
`npm run ex`

## License
RegReplace.js is released under the MIT license.