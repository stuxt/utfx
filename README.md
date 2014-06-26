![utfx - A compact library to encode, decode and convert UTF8 / UTF16 in JavaScript.](https://raw.github.com/dcodeIO/utfx/master/utfx.png)
====
**utfx** is a compact library to encode, decode and convert UTF8 / UTF16 in JavaScript using arbitrary sources and
destinations through the use of successively called functions, basically eliminating [memory overhead](https://github.com/dcodeIO/utfx/wiki#faq).

It is also capable of using binary strings and arrays (with overhead) and provides polyfills for `String.fromCodePoint`
and `String#codePointAt`.

API
---

### Class TruncatedError : Error

An error indicating a truncated source. Contains the remaining bytes as an array in its `bytes` property.

### encodeUTF8(src, dst, noAssert=)

Encodes UTF8 code points to an arbitrary output destination of UTF8 bytes.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; number* | Code points source, either as a function returning the next code point respectively `null` if there are no more code points left or a single numeric code point. 
| dst             | *function(number)* | Bytes destination as a function successively called with the next byte 
| noAssert        | *boolean*       | Set to `true` to skip argument and range assertions, defaults to `false` 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If a code point is invalid in UTF8 

### decodeUTF8(src, dst, noAssert=)

Decodes an arbitrary input source of UTF8 bytes to UTF8 code points.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null)* | Bytes source as a function returning the next byte respectively `null` if there are no more bytes left. 
| dst             | *function(number)* | Code points destination as a function successively called with each decoded code point. 
| noAssert        | *boolean*       | Set to `true` to skip argument assertions, defaults to `false` 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If a starting byte is invalid in UTF8 
| **@throws**     | *utfx.TruncatedError* | If the last sequence is truncated. Has an array property `bytes` holding the remaining bytes. 

### UTF16toUTF8(src, dst, noAssert=)

Converts an arbitrary input source of UTF16 characters to an arbitrary output destination of UTF8 code points.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null)* | Characters source as a function returning the next char code respectively `null` if there are no more characters left. 
| dst             | *function(number)* | Code points destination as a function successively called with each converted code point. 
| noAssert        | *boolean*       | Set to `true` to skip argument and range assertions, defaults to `false` 
| **@throws**     | *TypeError*     | If arguments are invalid or a char code is invalid 
| **@throws**     | *RangeError*    | If a char code is out of range 

### UTF8toUTF16(src, dst, noAssert=)

Converts an arbitrary input source of UTF8 code points to an arbitrary output destination of UTF16 characters.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; number* | Code points source, either as a function returning the next code point respectively `null` if there are no more code points left or a single numeric code point. 
| dst             | *function(number)* | Characters destination as a function successively called with each converted char code. 
| noAssert        | *boolean*       | Set to `true` to skip argument and range assertions, defaults to `false` 
| **@throws**     | *TypeError*     | If arguments are invalid or a code point is invalid 
| **@throws**     | *RangeError*    | If a code point is out of range 

### encodeUTF16toUTF8(src, dst, noAssert=)

Converts and encodes an arbitrary input source of UTF16 characters to an arbitrary output destination of UTF8
bytes.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null)* | Characters source as a function returning the next char code respectively `null` if there are no more characters left. 
| dst             | *function(number)* | Bytes destination as a function successively called with the next byte. 
| noAssert        | *boolean*       | Set to `true` to skip argument and range assertions, defaults to `false` 
| **@throws**     | *TypeError*     | If arguments are invalid or a char code is invalid 
| **@throws**     | *RangeError*    | If a char code is out of range 

### decodeUTF8toUTF16(src, dst, noAssert=)

Decodes and converts an arbitrary input source of UTF8 bytes to an arbitrary output destination of UTF16
characters.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null)* | Bytes source as a function returning the next byte respectively `null` if there are no more bytes left. 
| dst             | *function(number)* | Characters destination as a function successively called with each converted char code. 
| noAssert        | *boolean*       | Set to `true` to skip argument and range assertions, defaults to `false` 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If a starting byte is invalid in UTF8 
| **@throws**     | *utfx.TruncatedError* | If the last sequence is truncated. Has an array property `bytes` holding the remaining bytes. 

### calculateUTF8(src, noAssert=)

Calculates the number of UTF8 bytes required to store an arbitrary input source of UTF8 code points.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; number* | Code points source, either as a function returning the next code point respectively `null` if there are no more code points left or a single numeric code point. 
| noAssert        | *boolean*       | Set to `true` to skip argument assertions, defaults to `false` 
| **@returns**    | *number*        | Number of UTF8 bytes required 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If a code point is out of range 

### calculateUTF16asUTF8(src, noAssert=)

Calculates the number of UTF8 bytes required to store an arbitrary input source of UTF16 characters when
converted to UTF8 code points.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; !Array.&lt;number&gt; &#124; string* | Characters source, either as a function returning the next char code respectively `null` if there are no more characters left, an array of char codes or a standard JavaScript string. 
| noAssert        | *boolean*       | Set to `true` to skip argument and range assertions, defaults to `false` 
| **@returns**    | *number*        | Number of UTF8 bytes required 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If an intermediate code point is out of range 

### arraySource(a, noAssert=)

Creates a source function for an array.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| a               | *!Array.&lt;number&gt;* | Array to read from 
| noAssert        | *boolean*       | Set to `true` to skip argument assertions, defaults to `false` 
| **@returns**    | *function():(number &#124; null)* | Source function returning the next value respectively `null` if there are no more values left. 
| **@throws**     | *TypeError*     | If the argument is invalid 

### arrayDestination(a, noAssert=)

Creates a destination function for an array.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| a               | *!Array.&lt;number&gt;* | Array to write to 
| noAssert        | *boolean*       | Set to `true` to skip argument assertions, defaults to `false` 
| **@returns**    | *function(number)* | Destination function successively called with the next value. 
| **@throws**     | *TypeError*     | If the argument is invalid 

### stringSource(s, noAssert=)

Creates a source function for a string.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| s               | *string*        | String to read from 
| noAssert        | *boolean*       | Set to `true` to skip argument assertions, defaults to `false` 
| **@returns**    | *function():(number &#124; null)* | Source function returning the next char code respectively `null` if there are no more characters left. 
| **@throws**     | *TypeError*     | If the argument is invalid 

### stringDestination()

Creates a destination function for a string.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| **@returns**    | *function(number=):(undefined &#124; string)* | Destination function successively called with the next char code. Returns the final string when called without arguments. 

### fromCodePoint(var_args)

A polyfill for `String.fromCodePoint`.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| var_args        | *...number*     | Code points 
| **@returns**    | *string*        | JavaScript string 
| **@throws**     | *TypeError*     | If arguments are invalid or a code point is invalid 
| **@throws**     | *RangeError*    | If a code point is out of range 

### codePointAt(s, i)

A polyfill for `String#codePointAt`.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| s               | *string*        | JavaScript string 
| i               | *number*        | Index 
| **@returns**    | *number &#124; undefined* | Code point or `undefined` if `i` is out of range 
| **@throws**     | *TypeError*     | If arguments are invalid 

### polyfill(override=)

Installs utfx as a polyfill for `String.fromCodePoint` and `String#codePointAt` if not implemented.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| override        | *boolean*       | Overrides an existing implementation if `true`, defaults to `false` 
| **@returns**    | *!Object.&lt;string,*&gt;* | utfx namespace 

Usage
-----
* **node.js**: `npm install utfx`   
   
   ```js
   var utfx = require("utfx");
   ...
   ```

* **Browser**: `<script src="/path/to/utfx.min.js"></script>`   
   
   ```js
   var utfx = dcodeIO.utfx;
   ...
   ```
   
* **Require.js/AMD**   
   
   ```js
   require.config({
       "paths": {
           "utfx": "/path/to/utfx.min.js"
       }
   });
   require(["utfx"], function(utfx) {
       ...
   }
   ```

Downloads
---------
* [Distributions](https://github.com/dcodeIO/utfx/tree/master/dist)

FAQ and examples
----------------
* [Wiki](https://github.com/dcodeIO/utfx/wiki)

License
-------
Apache License, Version 2.0
