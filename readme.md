# re-tree
## JavaScript Extended regular expression engine - client side, server side and 'angular side' ready.

[![Build Status](https://travis-ci.org/srfrnk/re-tree.svg?branch=master)](https://travis-ci.org/srfrnk/re-tree)
[![GitHub issues](https://img.shields.io/github/issues/srfrnk/re-tree.svg)](https://github.com/srfrnk/re-tree/issues)
[![Known Vulnerabilities](https://snyk.io/package/npm/re-tree/badge.svg)](https://snyk.io/package/npm/re-tree)
[![Open Source Helpers](https://www.codetriage.com/srfrnk/re-tree/badges/users.svg)](https://www.codetriage.com/srfrnk/re-tree)

[![GitHub license](https://img.shields.io/github/license/srfrnk/re-tree.svg)](https://github.com/srfrnk/re-tree/blob/master/license.txt)
[![npm](https://img.shields.io/npm/dm/re-tree.svg)](https://www.npmjs.com/package/re-tree) 
[![npm](https://img.shields.io/npm/dt/re-tree.svg)](https://www.npmjs.com/package/re-tree)
[![npm](https://img.shields.io/npm/v/re-tree.svg)](https://www.npmjs.com/package/re-tree)

[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![Semver](http://img.shields.io/SemVer/2.0.0.png)](http://semver.org/spec/v2.0.0.html)

<!-- [![Dependency Status](https://david-dm.org/srfrnk/re-tree.svg)](https://david-dm.org/srfrnk/re-tree) -->
<!-- [![devDependency Status](https://david-dm.org/srfrnk/re-tree/dev-status.svg)](https://david-dm.org/srfrnk/re-tree#info=devDependencies) -->
<!-- [![Coverage Status](https://img.shields.io/codecov/c/github/srfrnk/re-tree.svg)](http://codecov.io/github/srfrnk/re-tree?branch=master) -->


Sometimes you get to a point where your regular expressions get so complex it's a nightmare to maintain.
Sometimes it's just easier to split these expressions into smaller - simpler - expression and use flow logic to handle the complex cases.
This is where re-tree comes and helps.

Consider the user-agent use case - you need to identify when you're in chrome... using the user-agent string.
* User agent string for chrome might look like that: `Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2049.0 Safari/537.36`
* But a Safari UA might look light that: `Mozilla/5.0 (iPad; CPU OS 6_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/6.0 Mobile/10A5355d Safari/8536.25`
* And an Opera UA might look light that: `Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.90 Safari/537.36 OPR/29.0.1795.47`

So you need a very complex RE expression to detect chrome and only chrome.
Or you might choose to create a simple RE to detect chrome ('CH'), then a simple RE to detect Safari ('SF') and a simple RE to detect Opera ('OP') and use the following:
```
var inChrome=CH.test(UA) && !(SF.test(UA) || OP.test(UA));
```
which is simpler to read and maintain.

This would be the equivalent of using re-tree like that:
```
var reTree-require('re-tree');
var inChrome=reTree.test(UA,{and:[CH,{not:{or:[SF,OP]}}]});
```

Even simpler - right?!

### Install
$ bower i re-tree -S

### Server-Side Usage
```
var reTree=require('re-tree');
```

### Client-Side Usage
Add script load to HTML:`<script type="text/javascript" src=".../re-tree.js"></script>`

To use from angular:
Add module to your app dependencies: `...angular.module("...", [..."reTree"...])...`

### API

reTree has two functions:

test(string,exp) : Boolean : return true iff exp matches the string.
exec(string,re | array_of_re) : Array : return the results of executing the first matching RE on the string.

The exec function will handle a single RE or an array of RE items.
It will find the first matching RE and return the value of execution of the RE on the string.

The test function can test for a full re-tree expression. An re-expression can take one of the following forms:
1) A string containing a correct RE string
2) A RegExp instance
3) An object { not: EXP } where EXP is an re-tree expression
3) An object { or: [EXP1,EXP2,...] } where EXP1,EXP2... are re-tree expressions
3) An object { and: [EXP1,EXP2,...] } where EXP1,EXP2... are re-tree expressions

Example:

```
// This matches Chrome but not Safari and Opera
var re = {
    and: [
        {
            or: [
                /\bChrome\b/,
                /\bCriOS\b/
            ]
        },
        {not: /\bOPR\b/}
    ]
};
```

### Support

Pull-requests with new stuff will be highly appreciated :)

### License

[MIT License](//github.com/srfrnk/re-tree/blob/master/license.txt)
