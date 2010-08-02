Array.sortOn
===========

![icon](http://github.com/gonchuki/mootools-Array.sortOn/raw/master/icon.png)

Adds Array.sortOn function and related constants that works like in ActionScript for sorting arrays of objects (plus adding support for nested keys).
The implementation strictly respects all the rules set by the [ActionScript API](http://www.adobe.com/livedocs/flash/9.0/ActionScriptLangRefV3/Array.html#sortOn(\))

How to use
----------

Given an array of objects:

    #JS
    var data = [
      {name:"john", city:"omaha", zip:68144},
      {name:"john", city:"kansas city", zip:72345},
      {name:"bob", city:"omaha", zip:94010},
      {name:"Frank", city:"omaha", zip:9210}
    ]
  
It can be sorted by any key in the object:

    #JS
    data.sortOn("name");

    [
      {"name":"Frank","city":"omaha","zip":9210},
      {"name":"bob","city":"omaha","zip":94010},
      {"name":"john","city":"omaha","zip":68144},
      {"name":"john","city":"kansas city","zip":72345}
    ]

Several constants are defined to control the sorting rules (you can use more than one option using a bitwise OR(|) operator):

 - Array.CASEINSENSITIVE  
  do not perform case-sensitive comparisons (a precedes Z)
 - Array.DESCENDING  
  invert the sorting order
 - Array.UNIQUESORT  
  do not sort the array if there are 2 or more items with the same value in a sort key. Must be in the first options entry if sorting by multiple keys.
 - Array.RETURNINDEXEDARRAY  
  do not modify the array, return a copy with the sorted data. Must be in the first options entry if sorting by multiple keys.
 - Array.NUMERIC  
  comparisons are done alphabetically by default (11 precedes 2), use this flag to perform numerical comparisons on the selected keys.

Sorting case-insensitive and reversing order:

    #JS
    data.sortOn("name", Array.CASEINSENSITIVE | Array.DESCENDING)
    
    [
      {"name":"john","city":"omaha","zip":68144},
      {"name":"john","city":"kansas city","zip":72345},
      {"name":"Frank","city":"omaha","zip":9210},
      {"name":"bob","city":"omaha","zip":94010}
    ]

You can also sort by multiple keys by providing an array as the keys parameter.
When using this approach, the options array must have the same length as the keys array or options are ignored altogether. You can use _null_ to avoid passing options for some keys.

Sorting is done from "left to right": the first key is the primary sort key, and comparisons that determine two items are equal are defined by using the next sort key.

    #JS
    data.sortOn(["name", "city"], [Array.CASEINSENSITIVE, null]);
    
    [
      {"name":"bob","city":"omaha","zip":94010},
      {"name":"Frank","city":"omaha","zip":9210},
      {"name":"john","city":"kansas city","zip":72345},
      {"name":"john","city":"omaha","zip":68144}
    ]

Sort keys can also be part of a nested object, so with input data like:

    #JS
    var data = [
      { name: { first: 'Josh', last: 'Jones' }, age: 30 },
      { name: { first: 'Carlos', last: 'Jacques' }, age: 19 },
      { name: { first: 'Carlos', last: 'Dante' }, age: 23 },
      { name: { first: 'Tim', last: 'Marley' }, age: 9 },
      { name: { first: 'Courtney', last: 'Smith' }, age: 27 },
      { name: { first: 'Bob', last: 'Smith' }, age: 30 }
    ]
    
you can do:

    #JS
    data.sortOn(['name.first', 'name.last']);

    [
      {"name":  {"first":"Bob","last":"Smith"}, "age":30},
      {"name":  {"first":"Carlos","last":"Dante"}, "age":23},
      {"name":  {"first":"Carlos","last":"Jacques"}, "age":19},
      {"name":  {"first":"Courtney","last":"Smith"}, "age":27},
      {"name":  {"first":"Josh","last":"Jones"}, "age":30},
      {"name":  {"first":"Tim","last":"Marley"}, "age":9}
    ]

Note: this last feature is outside of the ActionScript API but it's been included for its potential usefulness.

Misc
----------

Credits to [Eneko Alonso](http://github.com/eneko) as I based part of this documentation from his version of [Array.sortBy](http://github.com/eneko/Array.sortBy) and took his idea of implementing sorting by nested keys.
For the curious, this plugin works much like Array.sortBy but with a different (and more extense) API.

Also, the code is not pretty nor I ever intended it to be. All efforts have gone in the road of robustness and strictly abiding to the ActionScript API.
Optimizations are at a minimum and tail recursion on nested lambdas was used instead of breaking out of iterations as a personal coding preference. Feel free to contribute partial (or full) rewritings on any section of the code.