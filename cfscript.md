
Table of Contents
=================

  * [CFScript documentation](#cfscript-documentation)
      * [Comments](#comments)
      * [Statements](#statements)
      * [Variables](#variables)
      * [Operators](#operators)
        * [Decision](#decision)
        * [Arithemetic](#arithemetic)
          * [Increment/decrement](#incrementdecrement)
          * [Inline assignment](#inline-assignment)
        * [Boolean](#boolean)
        * [Decision](#decision-1)
          * [Ternary operator](#ternary-operator)
          * [Null-coalescing variation](#null-coalescing-variation)
      * [Conditions](#conditions)
        * [if/elseif/else](#ifelseifelse)
        * [switch](#switch)
        * [try/catch/finally, throw/rethrow](#trycatchfinally-throwrethrow)
      * [Iteration](#iteration)
        * [General-purpose for loop](#general-purpose-for-loop)
        * [Pre-condition loop](#pre-condition-loop)
        * [Post-condition loop](#post-condition-loop)
        * [Array loop](#array-loop)
          * [For statement](#for-statement)
          * [arrayEach()](#arrayeach)
          * [Array.each()](#arrayeach-1)
        * [Struct loop](#struct-loop)
          * [For statement](#for-statement-1)
          * [structEach()](#structeach)
          * [Struct.each()](#structeach-1)
        * [Query loop](#query-loop)
        * [List loop](#list-loop)
        * [File loop](#file-loop)
        * [Date/time range loop](#datetime-range-loop)
      * [Other flow control statements](#other-flow-control-statements)
        * [Abort processing](#abort-processing)
        * [Exit from current file](#exit-from-current-file)
      * [Code reuse](#code-reuse)
        * [Include](#include)
        * [Module](#module)
      * [Components / interfaces](#components--interfaces)
        * [Attributes](#attributes)
        * [Interface](#interface)
        * [Properties](#properties)
        * [Functions](#functions)
        * [Arguments](#arguments)
        * [Function/argument annotations](#functionargument-annotations)
        * [Function expressions](#function-expressions)
        * [Calling functions dynamically](#calling-functions-dynamically)
        * [Import](#import)
        * [Object creation](#object-creation)
      * [File system operations](#file-system-operations)
        * [Directories](#directories)
        * [Files](#files)
      * [Database](#database)
        * [Query](#query)
        * [Stored Procedure](#stored-procedure)
        * [Insert](#insert)
        * [Update](#update)
        * [DB Info](#db-info)
        * [Transactions](#transactions)
      * [Debugging](#debugging)
        * [Dump](#dump)
        * [Log](#log)
        * [Trace](#trace)
        * [Timer](#timer)
      * [General / Miscellaneous](#general--miscellaneous)
        * [Output](#output)
        * [File Encoding](#file-encoding)
        * [Save content](#save-content)
        * [Threading](#threading)
        * [Locking](#locking)
        * [Image / XLS manipulation](#image--xls-manipulation)
        * [PDF Manipulation](#pdf-manipulation)
      * [Elements of tag-based CFML with no <em>specific</em> CFScript implementation](#elements-of-tag-based-cfml-with-no-specific-cfscript-implementation)
        * [CFC-based solutions](#cfc-based-solutions)
        * [http.cfc](#httpcfc)
        * [mail.cfc](#mailcfc)
        * [The rest](#the-rest)

# CFScript documentation

This attempts to document all of CFScript, as a resource for people migrating from old-school tag-based code to script-based code. The reason I am doing this is because neither ColdFusion nor Railo/Lucee provide much (or in the case of Railo/Lucee: _any_) useful [documentation of CFScript](https://wikidocs.adobe.com/wiki/display/coldfusionen/The+CFScript+language).

This is not a document for converting tags to script. It is not written from a point of view of "if you use ``<cfsometag>`` then you need to instead use [some script construct]". It simply documents CFScript.

There are some syntax-neutral solutions to some CFML constructs which are listed at the bottom of the document. These are the CFCs found in [customtag]/com/adobe/coldfusion. These are not CFScript constructs per se, but related enough to be relevant here.

It is also not an exercise in teaching CFML (or at least the script part). It assumes you know what you're doing, and is purely a reference.

I assume Railo 4.2/Lucee 4.5 or ColdFusion 11, except where stated.


### Comments
```cfc
// single line comment
```
```cfc
a=1; // single line comment at end of line
```
```cfc
/*
    multiple
    line
    comment
*/
```
```cfc
/*
    multiple line
    /* comments */
    cannot be nested
*/
```

In this case, the commented bit is `/* multiple line /* comments */`, making the next bit a syntax error.

### Statements

Statements end in semi-colons:
```cfc
a = 1;
```
Semi-colons are _generally_ optional on Railo/Lucee:
```cfc
a = 1
```

Where "generally" means "if the end of the statement is unambiguous without a semi-colon". It is better to just always use semi-colons.

Block statements (with curly braces) do not have semi-colons:
```cfc
while (condition) {
    // statements
}
```

### Variables

Assigning a variable:
```cfc
varName = "foo";
```

Assigning a function-local variable:
```cfc
var varName = "foo"; // analogous to local.varName =  "foo";
```

Note that the var keyword can appear inline in most statements where a variable is first initialised, eg:
```cfc
for (var i=1; i <= 10; i++);
```

Assigning a dynamically-named variable:
```cfc
varName = "foo";
"#varName#" = "bar";
writeOutput(foo); // bar
```
<a href="http://trycf.com/editor/gist/aafe518f91bce312349e/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>

This is the same as with a `<cfset>` tag, but confuses some people due to it being slightly odd-looking. Obviously one can also use associative array syntax too (eg: `variables[varName] = "bar";`. This is preferable as it's more clear what's going on).

Defaulting a variable:
```cfc
param numeric variableName=defaultValue; // where "numeric" could be any type
```
For more complex situations:
```cfc
param name="variableName" type="regex" pattern="."; // any cfparam attribute is supported
```

### Operators

All operators available to tag-based code still work in CFScript. In addition, CFScript has these ones:

#### Decision
```cfc
a == 1; // equality
a < 1;  // less than
a <= 1; // less-than-or-equal
a >= 1; // greater-than-or-equal
a > 1;  // greater than
a != 1; // inequality
a <> 1; // inequality (Railo/Lucee only)
```
<a href="http://trycf.com/editor/gist/5b6ecbe3d5ebbdc3b8d8/lucee?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### Arithemetic

##### Increment/decrement
```cfc
// increment
a = 1;
b = a++; // b=1, a=2    // postfix operators returns value, then peforms action (in this case: increments a)
c = ++a; // c=3, a=3    // prefix operator peforms action then returns result
```
```cfc
// decrement
a--; // a=2
--a; // a=1
```
<a href="http://trycf.com/editor/gist/eefca5ca3fe8c3ef800a/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
##### Inline assignment
```cfc
a += 2; // equivalent to a=a+2
a -= 3; // equivalent to a=a-3
a *= 4; // equivalent to a=a*4
a /= 5; // equivalent to a=a/5
a %= 6; // equivalent to a=a%6
s &= "a"; // equivalent to s = s & "a"
```
<a href="http://trycf.com/editor/gist/717ea2e12e318d6ae18e/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### Boolean
```cfc
!a;     // NOT a
a && b; // a AND b
a || b; // a OR b
```
<a href="http://trycf.com/editor/gist/85479549681ea703256c/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### Decision

##### Ternary operator
```cfc
result = condition ? trueExpression : falseExpression;

//eg:
coinTossResult = randRange(0,1) ? "heads" : "tails";
```
<a href="http://trycf.com/editor/gist/007072ccaf71629f3a48/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
```cfc
// NB: only one of trueExpression or falseExpression is evaluated:
a = 1;
b = 1;
c = false ? ++a : ++b; // a=1, b=2, c=2
```
<a href="http://trycf.com/editor/gist/da4ec1cfa62aa134ca4e/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
##### Null-coalescing variation
```cfc
result = left ?: right; // left-hand expression is used unless it is null, in which case the right one is used

//eg:
a = d ?: "default"; // a = default
```
<a href="http://trycf.com/editor/gist/e1e440bab2b856474e88/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
```cfc
d = 1;
a = d ?: "default"; // a = 1
```
<a href="http://trycf.com/editor/gist/7824257754c541adbacb/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>

Note that whilst this appears to be the same as other language's "Elvis" operator, it is not the same. The "Elvis" operator checks for `false` (and in some languages `null` is falsey); this operator checks specifically for `null`. This is likely due to a misunderstanding one the part of Adobe (and perpetuated by the Lucee devs when copying it).

### Conditions

#### if/elseif/else
```cfc
if (booleanExpression)
    // single statement executed if booleanExpression is true
else if (anotherBooleanExpression)
    // single statement executed if anotherBooleanExpression is true
else
    // single statement executed if condition(s) are false
```
```cfc
if (booleanExpression) {
    // multiple statements executed if booleanExpression is true
} else if (anotherBooleanExpression) {
    // multiple statements executed if anotherBooleanExpression is true
} else {
    // multiple statements executed if condition(s) are false
}
```
<a href="http://trycf.com/editor/gist/79d1a9adeea8c2b3d0b6/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### switch
```cfc
switch (expression) {
    case "some constant value": // value can be dynamic on Railo/Lucee
         // statements executed if expression = "some constant value"
    break; // exit switch statement
    case "a different constant value":
        // statements executed if expression = "a different constant value"
        // if there is no break, then processing continues to execute statements until a break is encountered...
        // ... but subsequent case evaluations are not made. A switch is basically a GOTO mechanism, which does a...
        // single GOTO the first matching case. It is NOT a series of if/elseif/else statements
    case "third constant value":
        // statements executed if expression = "a different constant value" or "third constant value"
    break;
    case "4th value":
    case "5th value":
            // statements executed if expression is one of "4th value" or "5th value"
    break;
    default:
        // statements executed if no case was fulfilled (or if the last fulfilled case did not have a break)
    break;
}
```
 <a href="http://trycf.com/editor/gist/9d104178a7630d198a16/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### try/catch/finally, throw/rethrow
```cfc
try {
    // statements

    throw "message"; // throws an Application exception, with the given message

    // or
    throw (type="ExceptionType", message="message", detail="detail", errorCode="errorCode", extendedInfo="extendedInfo"); // despite appearances, this is NOT a function

    // or
    throw (object=JavaExceptionObject);
}
catch (SomeExceptionType variableContainingExceptionObject) {
    // statements executed if code in try block errors with a SomeExceptionType exception

    rethrow; // rethrows the caught exception
}
catch (SomeOtherExceptionType variableCOntainingExceptionObject) {
    // statements executed if code in try block errors with a SomeOtherExceptionType exception
}
catch (any variableCOntainingExceptionObject) {
    // statements executed if code in try block errors with any not-yet-caught exception type
}
finally {
    // statements executed in any case, INCLUDING unhandled exceptions. This code ALWAYS runs
}
```

### Iteration

#### General-purpose for loop
```cfc
for (initialisation; condition; repetition) statement;
```

or:
```cfc
for (initialisation; condition; repetition) {
    // statements
}
```

EG:
```cfc
for (i=1; i <=5; i++) writeOutput(i); // just the following single statement is looped over
```

or:
```cfc
for (i=1; i <=5; i++) {
    // all statements within the block are looped over
    result = i * 2;
    writeOutput(result);
}
```

The general perception is that this is the only form of a general-purpose for() loop: initialising a counter variable, testing it and adjusting it (increment, decrement). This is not the case. Each of the statements can be _anything_ (the condition needs to evaluate to a boolean), and indeed are optional. This is an endless loop, equivalent to while (true):
```cfc
for (;;)
```

A very contrived example to demonstrate the freedom one has with the parameters of the <span id="contrivedFor">for()</span>:
```cfc
i=0;
for (; true; writeOutput(i)) {
    if (++i > 5) break;
}
```

In general, all looping constructs have either the single-statement or block-of-statements syntax. I'll only offer the more common (and recommended, for code-clarity) block syntax henceforth.

#### Pre-condition loop

This form of loop evaluates a single condition at the beginning of each iteration, and continues to loop whilst the condition is true:
```cfc
while (condition) {
    // statements
}
```

This form of loop will execute zero or more times.

#### Post-condition loop

This form of loop evaluates a single condition at the beginning of each iteration, and continues to loop whilst the condition is true:
```cfc
do {
    // statements
} while (condition);
```

This form of loop will execute _one_ or more times. It's important to consider that the body of the loop will always run the first time, because no condition is evaluated until the _end_ of the loop.

#### Array loop

##### For statement
```cfc
for (element in [1,2,3,4,5]) {
    writeOutput(element); // 12345
}
```

##### arrayEach()
```cfc
arrayEach(["a","b","c"], function(element,index,array) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
});
```

##### Array.each()
```cfc
a = ["a","b","c"];
a.each(function(element,index,array) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
});
```

Note that Railo/Lucee can call methods directly on a literal, so this works:
```cfc
["a","b","c"].each(function(element,index,array) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
});
```

#### Struct loop

##### For statement
```cfc
struct = {a=1,b=2,c=3};
for (key in struct) {
    writeOutput("#key#:#struct[key]#;"); // a:1;b:2;c:3; (order of keys not guaranteed, obviously)
}
```

##### structEach()
```cfc
structEach(struct, function(key,value,struct) {
    writeOutput("#key#:#value#;"); // a:1;b:2;c:3;
});
```

##### Struct.each()
```cfc
struct.each(function(key,value,struct) {
    writeOutput("#key#:#value#;"); // a:1;b:2;c:3;
});
```

#### Query loop
```cfc
q = queryNew("id,data", "integer,varchar",[
    [11, "aa"],
    [22, "bb"],
    [33, "cc"]
]);
for (row in q){
    writeOutput("#q.currentRow#:#row.id#:#row.data#;"); // 1:11:aa;2:22:bb;3:33:cc;
}
```

Using grouping:
```cfc
q = queryNew("pk,fk,data", "integer,integer,varchar",[
    [1, 10, "aa"],
    [2, 20, "bb"],
    [3, 20, "cc"],
    [4, 30, "dd"],
    [5, 30, "ee"],
    [6, 30, "ff"]
]);
cfloop(query=q, group="fk") {
    writeOutput("<strong>#fk#</strong>");
    cfloop() {
        writeOutput("#pk#:#data#<br>");
    }
    writeOutput("<hr>");
}
```

Railo/Lucee only:
```cfc
loop query=q group="fk" {
    writeOutput("<strong>#fk#</strong>");
    loop {
        writeOutput("#pk#:#data#<br>");
    }
    writeOutput("<hr>");
}
```

#### List loop
```cfc
list = "a;b;c";
listEach(list, function(element,index,list) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
}, ";");

// or

list.each(function(element,index,list) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
}, ";");

// or (ColdFusion only, see [RAILO-3207](https://issues.jboss.org/browse/RAILO-3207))

for (element in "a,b,c,d,e") {
    writeOutput(element); // abcde
}
```

I am not sure how one would specify a delimiter for the last example: it does not seem supported.

Railo/Lucee only:
```cfc
cfloop(list="a;b;c", index="element", delimiters=";") {
    writeOutput(element); // abc
}

// or
loop list="a;b;c" index="element" delimiters=";" {
    writeOutput(element); // abc
}
```

#### File loop
```cfc
filePath = getCurrentTemplatePath();
cfloop(file=filePath, index="chars", characters=16, charset="UTF-8"){
    writeOutput(chars); // outputs the contents of this file
}
```

Railo/Lucee only:
```cfc
loop file=filePath index="chars" characters=16 charset="UTF-8" {
    writeOutput(chars);
}
```

#### Date/time range loop

ColdFusion has no specific CFScript-specific construct for this as of ColdFusion 11

Work around:
```cfc
from = now();
to   = dateAdd("d", 7, from);

for (date=from; dateCompare(date, to, "d") <= 0; date = dateAdd("d", 1, date)) {
    writeOutput(dateTimeFormat(date, "yyyy-mm-dd HH:nn:sstt") & "<br>");
}
```

Railo/Lucee only:
```cfc
cfloop(from=from, to=to, index="date", step=createTimespan(1,0,0,0)) {
    writeOutput(dateTimeFormat(date, "yyyy-mm-dd HH:nn:sstt") & "<br>");
}

// or

loop from=from to=to index="date" step=createTimespan(1,0,0,0) {
    writeOutput(dateTimeFormat(date, "yyyy-mm-dd HH:nn:sstt") & "<br>");
}
```

#### Iteration flow control

cfcontinue:
```cfc
for (i=1; i <= 5; i++) {
    writeOutput("#i# is ");
    if (i mod 2) {
        writeOutput("ODD<br>");
        continue;
    }
    writeOutput("EVEN<br>");
}
```

### Other flow control statements

#### Abort processing
```cfc
abort;

// or

abort "error message";
```

#### Exit from current file

```cfc
exit;

//or

exit "method";
```

### Code reuse

#### Include

```cfc
include "pathToFile";

// or

include "pathToFile" runonce=true;
```

#### Module

```cfc
// Railo/Lucee
module template="inc.cfm" attr1="val1" attr2="val2";

// ColdFusion
cfmodule(template="inc.cfm", attr1="val1", attr2="val2");
```

### Components / interfaces

```cfc
component {

}
```

#### Attributes
```cfc
component extends="Parent" {

}
```

Or:

```cfc
/**
 * @extends Parent
 */
component {

}
```

Note that the comment for annotations is `/**` not simply `/*`.

Also note that the latter syntax does not currently work on Railo (see [RAILO-3169](https://issues.jboss.org/browse/RAILO-3169)).

#### Interface

```cfc
interface {
    public void function f(required numeric x); // note no braces, and ends with semi-colon
}
```

#### Properties

Basic:

```cfc
property string myProperty;
```

With additional parameters:

```cfc
property type="string" name="myProperty" default="default value"; // and all the same attributes as `<cfproprty>`
```

#### Functions

Basic:

```cfc
function f() { // assumes public function, returntype any

}
```

With access and return type modifiers:

```cfc
private void function f() {
    // statements
}
```

#### Arguments

Basic:
```cfc
function f(x) { // optional argument of type "any"
    //statements
}
```

Type:
```cfc
function f(numeric x) { // optional argument of type "numeric"
    //statements
}
```
Required:
```cfc
function f(required numeric x) { // required argument of type "numeric"
    // statements
}
```
Default value:
```cfc
function f(numeric x = 0) { // optional argument of type "numeric" with default value of 0
    // statements
}
```

#### Function/argument annotations
```cfc
/**
 * @x.hint hint for argument x
 * @x.type numeric
 * @x.required true
 */
function f(x) {
    // statements
}
```

Note these annotations do not current correctly work on Railo (see [RAILO-3170](https://issues.jboss.org/browse/RAILO-3170))

Also note that this does not currently work on ColdFusion (see [3808960](https://bugbase.adobe.com/index.cfm?event=bug&id=3808960))
```cfc
/**
 * @x.type numeric
 * @x.default 0 // this causes a compile error
 */
function f(x) {
    // statements
}
```

#### Function expressions
```cfc
f = function(x) {
    // statements
};
```

Functions defined by function expressions use closure, functions defined by a function statement do not

Annotations for function expressions are not supported on ColdFusion ([3808978](https://bugbase.adobe.com/index.cfm?event=bug&id=3808978)); are supported on Railo, but have same shortcomings as noted above.

#### Calling functions dynamically
```cfc
test = new Test();
methodToCall = "f";
argumentsToPass = {x=1};
result = invoke(test, methodToCall, argumentsToPass);
```

Railo/Lucee-only:
```cfc
result = test[methodToCall](argumentCollection=argumentsToPass);
```

#### Import
```cfc
import com.domain.app.package.*;
```

#### Object creation
```cfc
myObj = createObject(type, "path.to.class"); // along with other type/situation-specific arguments

// or

myObj = new path.to.some.cfc.file(); // NB: will call the CFC's init() (by default), or method identified by the initmethod attribute of the component (bug in Railo: [RAILO-2294](https://issues.jboss.org/browse/RAILO-2294))
```

### File system operations

#### Directories
```cfc
// simple directory creation
directoryCreate("path/to/directory");
```
```cfc
// using other optional attributes
cfdirectory(action="create", directory="path/to/directory", mode="777");
```
```cfc
// Railo/Lucee only
directory action="create" directory="path/to/directory" mode="777";
```
```cfc
// delete
directoryDelete("path/to/directory");
```
```cfc
// list
listing = directoryList("path/to/directory", true, "query", "*.cfm", "size desc"); // CF11 added an additional "type" attribute. Not currently supported on Railo/Lucee
```
```cfc
// rename
directoryRename("path/to/directory", "path/to/new/directory");
```

#### Files
```cfc
// read
// text
if (fileExists("path/to/file")) {
   result = fileRead("path/to/file");
}

// or
fileHandle = fileOpen("path/to/file", "read");
result = fileRead(fileHandle, bytesToRead);
fileClose(fileHandle);

// or
fileHandle = fileOpen("path/to/file", "read");
while (!fileIsEOF(fileHandle)) {
    result = fileReadLine(fileHandle);
}
fileClose(fileHandle);

```
```cfc
// binary
result = fileReadBinary("path/to/file");

//or
fileHandle = fileOpen("path/to/file", "readbinary");
result = fileRead(fileHandle, bytesToRead);
```
```cfc
// append
fileHandle = fileOpen("path/to/file", "append");
fileWrite(fileHandle, textToAppend);
fileClose(fileHandle);
```
```cfc
// copy
fileCopy("path/to/file", "path/to/copyOfFile");
```
```cfc
// delete
fileDelete("path/to/file");
```
```cfc
// move / rename
fileMove("path/to/file", "new/path/to/file");
```
```cfc
// upload
fileUpload("path/to/upload/file/to");
fileUpload(destination [, fileField [, accept [, nameConflict]]]);

fileUploadAll("path/to/upload/files/to");
fileUploadAll(destination [, fileField [, accept [, nameConflict]]]);
```
```cfc
// write
fileWrite("path/to/file", data);

// or

fileWrite(fileHandle, data);
```

```cfc
/* cflocation */
// ColdFusion
location(url="http://example.com", statuscode="301 OR 302", addtoken=false);

// Railo/Lucee
location url="http://example.com", statuscode="301 OR 302", addtoken=false;
```

### Database

#### Query
```cfc
// general form
recordset = queryExecute(sqlString, params, options);
```
```cfc
// with params array
numbers = queryExecute("
    SELECT    columns
    FROM    table
    WHERE    id BETWEEN ? AND ?
",
[1,4],
{
    datasource    ="myDsn",
    result        = "result"    // this is analogous to the result attribute of `<cfquery>`
});
```
```cfc
// with params struct
numbers = queryExecute("
    SELECT    columns
    FROM    table
    WHERE    id BETWEEN :low AND :high
",{low=2,high=3});
```

To qualify parameters with SQL types you can specify the equivalent of the cfqueryparam options with the parameters in the following way:

```cfc
// with sql types in params struct
numbers = queryExecute("
    SELECT    columns
    FROM    table
    WHERE    id BETWEEN :low AND :high
",
{
    low = { value = 2
            cfsqltype = "cf_sql_integer"
        },
    high = { value = 3,
             cfsqltype = "cf_sql_integer"
        }
});
```

For versions prior to ColdFusion 11 (in which queryExecute() was implemented), there is a CFC-based solution: [Query.cfc](https://wikidocs.adobe.com/wiki/display/coldfusionen/query). An example is as follows:
```cfc
numbers = new Query(
    sql            = "
        SELECT    columns
        FROM    table
        WHERE    id BETWEEN :low AND :high
    ",
    parameters    =[
        {name="low", value=2},
        {name="high", value=3}
    ]
).execute().getResult();
```

#### Stored Procedure
```cfc
cfstoredproc(procedure="procName") {
    cfprocparam(type="in", cfsqltype="cf_sql_varchar", value="someValue");
    cfprocresult(name="result",resultSet=1);
    cfprocresult(name="result2",resultSet=2);
}
```

Railo/Lucee only
```cfc
storedproc procedure="procName" 
    dataSource = "myDataSource" 
    result="response" 
    returncode="true" {
    procparam type="in" cfsqltype="cf_sql_varchar" value="someValue";
    procparam type="out" cfsqltype="cf_sql_integer" variable="myVariable";
    procresult resultSet=1 name="result";
    procresult resultSet=2 name="result2";
}
```

There is a change request you should vote for, to implement this syntax:
```cfc
options = {
    datasource = "scratch_mssql",
    fetchclientinfo = true,
    returncode = true
};
params = [
    {value=URL.low, type="INTEGER"},
    {value=URL.high, type="INTEGER"},
    {type="out", variable="inclusiveCount", type="INTEGER"},
    {type="out", variable="exclusiveCount", type="INTEGER"}
];

result = executeProcedure("uspGetColours", params, options);
```

See ColdFusion ticket: [3791737](https://bugbase.adobe.com/index.cfm?event=bug&id=3791737); Railo ticket: [RAILO-3184](https://issues.jboss.org/browse/RAILO-3184), and earlier blog article: '[ColdFusion 11: calling a stored procedure from script. And can we please stop wearing out our "c" and "f" keys?](http://blog.adamcameron.me/2014/03/coldfusion-11-calling-stored-procedure.html#executeProcedure)'.

#### Insert

Railo/Lucee only:
```cfc
insert datasource="myDataSource" tableName="myTable" formFields="list,of,form,fields"; // arguments the same as `<cfinsert>`. datasource is optional
```

Note: there is a bug with this: [RAILO-3180](https://issues.jboss.org/browse/RAILO-3180).

ColdFusion only:
```cfc
cfinsert(datasource="myDataSource", tableName="myTable", formFields="list,of,form,fields"); // arguments the same as `<cfinsert>`
```

Note that datasource is currently required, which is a bug: [3814079](https://bugbase.adobe.com/index.cfm?event=bug&id=3814079).

#### Update

Railo/Lucee only:
```cfc
update datasource="myDataSource" table="myTable" formFields="list,of,form,fields"; // arguments the same as `<cfupdate>`. datasource is optional
```

Note the same bug applies here as does with insert.

ColdFusion only:
```cfc
cfupdate(datasource="myDataSource", table="myTable", formFields="list,of,form,fields"); // arguments the same as `<cfupdate>`
```

#### DB Info
```cfc
cfdbinfo(type="tables", name="info"); // arguments the same as `<cfdbinfo>`
```

Railo/Lucee only:
```cfc
dbinfo type="tables" name="info"; // arguments the same as `<cfdbinfo>`
```

#### Transactions
```cfc
transaction {
    try {
        // stuff to do
        transaction action="commit";
    }
    catch (any e) {
        transaction action="rollback";
    }
}
```

Note that all attributes of `<cftransaction>` are supported as space-separated name/value pairs.

### Debugging

#### Dump
```cfc
writeDump(myVar); // can use either ordered or named arguments.
```
With named arguments:
```cfc
writeDump(var=myVar, output=ExpandPath('/debug/log.txt'));
```

Railo/Lucee only:
```cfc
dump(myVar)
```

#### Log
```cfc
writeLog("text to log"); // can use either ordered or named arguments.
```

#### Trace
```cfc
// Railo/Lucee only
trace category="test" text="trace text" { // plus all same params as `<cftrace>`
    // stuff to trace
}
```
```cfc
// COLDFUSION only
trace(category="test", text="trace text") { // plus all same params as `<cftrace>`
    // stuff to trace
}
// note that CF11 incorrectly records timing information (see [3811003](https://bugbase.adobe.com/index.cfm?event=bug&id=3811003))
```

#### Timer
```cfc
cftimer(label="timer label" type="outline") { // plus all same params as `<cftimer>`
    // stuff to time
}
```
```cfc
// Railo/Lucee only
timer label="timer label" type="outline" { // plus all same params as `<cftimer>`
    // stuff to time
}
```

### General / Miscellaneous

#### Output
```cfc
writeOutput(expression); // expression must resolve to a string
```

Railo/Lucee only:
```cfc
echo(expression); // expression must resolve to a string
```

#### File Encoding
```cfc
pageencoding "UTF-8";
```

Note that this only works in CFC files on ColdFusion ([3712167](https://bugbase.adobe.com/index.cfm?event=bug&id=3712167)). It works correctly on Railo/Lucee.

#### Save content
```cfc
savecontent variable="saved" {
    writeOutput("stuff to save");
}
```

#### Threading
```cfc
thread action="run" name="threadName" {
    // code to run in separate thread here
}
thread action="join" name="threadName,anotherThreadName";
```

#### Locking
```cfc
lock name="lockName" timeout=1 throwontimeout=true {
    // code to lock
}
```

#### Image / XLS manipulation

The function equivalents of [`<cfimage>`](https://wikidocs.adobe.com/wiki/display/coldfusionen/Image+functions) and [`<cfspreadsheet>`](https://wikidocs.adobe.com/wiki/display/coldfusionen/Spreadsheet+functions) are all well documented, and are not specifically CFScript constructs.

#### PDF Manipulation

I have to concede I have never ever done any work with PDFs, so cannot make an informed comment on the CFScript equivalents. However in lieu of particular CFScript-specific constructs that I am aware of, the generic syntax ought to work, eg:

ColdFusion:
```cfc
cfdocument(format="PDF") {
    // mark-up here
}
```
Railo/Lucee:
```cfc
document format="PDF" {
    // mark-up here
}
```

The same should work on other PDF-oriented tags. For versions of ColdFusion prior to CF11, there is a PDF.cfc (similar to Query.cfc, and also in cfusion/CustomTags/com/adobe/coldfusion). I have never used it, do not know how it works, and have no interest in finding out. If someone would like to donate some example code, I will integrate it here.


### Elements of tag-based CFML with no _specific_ CFScript implementation

#### CFC-based solutions

As far as I can tell, there is no CFScript-specific implementations for the following pieces of functionality:

*   `<cfftp>`
*   `<cfpop>`
*   `<cfimap>`
*   `<cffeed>`
*   `<cfldap>`
*   `<cfcollection>`
*   `<cfindex>`
*   `<cfsearch>`

There are CFC wrappers for these in cfusion/CustomTags/com/adobe/coldfusion. These are not CFScript-specific solutions per se - one can just as easily use them in tag-based code - but can be included here for the sake of completeness. I personally do not use these constructs, and as they are not part of CFScript, did not attempt to include them here. Feel free to document and include them if you so choose.

#### http.cfc

ColdFusion (CF9+):

__The pattern seems to be `set{ATTRIBUTENAME}( value );` for setting attributes that are normally set via the `<cfhttp>` tag__

```cfc
// there's a built-in CFC that handles this in CF9+
httpService = new http();
httpService.setMethod( "post" )
httpService.setCharset( "utf-8" );

// the API provider may require you to export this cert
// so you’ll need to save it in a secure place…
// and reference it here
httpService.setClientCert( "#ExpandPath(‘.’)#/my_cert.p12" );
httpService.setClientCertPassword( "mypassword!" );
httpService.setUrl( "https://api.sample.com/" );

// these params are form fields to POST
httpService.addParam(type="formfield", name="field1", value="1111");
httpService.addParam(type="formfield", name="field2", value="some text here");

// this is the cfscript way to grab the response
httpResponse = httpService.send().getPrefix();

writeDump(httpResponse);
```

#### mail.cfc

ColdFusion (CF9+):

__The pattern seems to be `set{ATTRIBUTENAME}( value );` for setting attributes that are normally set via the `<cfmail>` tag__

```cfc
mailerService = new mail();
/* set mail attributes using implicit setters provided */
mailerService.setType("html");
mailerService.setCharset( "utf-8" );
mailerService.setTo( "adam@sample.com" );
mailerService.setFrom( "test@sample.com;joe@sample.com" );
mailerService.setSubject( "Super interesting subject" );

/* add mailparams */
mailerService.addParam( file=expandpath(form.attachment), type="text/plain", remove=false );
// create the body
savecontent variable="mailBody"{
    WriteDump( CGI );
    WriteDump( SESSION );
}
// send the email
mailerService.send( body=mailBody );
```


#### The rest

To use any other functionality not listed here within CFScript, one needs to use the generalised syntax.

On Railo/Lucee this is a matter of removing the "`<cf`" and the "`>`", and using normal block syntax (curly braces) where the tag-version is a block-oriented tag.

On ColdFusion, replace the "`<cftagname`" with "`cftagname(`", and the "`>`" with "`)`", and comma-separate the attributes. Note that this will make the construct _look_ like a function, but it actually is not, and cannot be used like a function, eg this is invalid syntax:

```cfc
result = cfhttp(method="post", url="http://example.com");
```

* * *

[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/)
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).
[trycf-logo]:http://trycf.com/img/trycf-logo-tiny.png "TryCF.com"
