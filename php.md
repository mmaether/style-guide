# PHP Style Guide
This section outlines the style guide for PHP code. Most of the coding style will follow [Drupal's standards](https://www.drupal.org/docs/develop/standards), although there will be some slight alterations. The overall rule of thumb is this: keep your code clean, readable, and ensure that there’s proper documentation.

## Indenting and Whitespace ##
Use an indent of 2 spaces, not tabs.

Lines should have no trailing whitespace at the end.

All text files should end in a single newline `\n`. This avoids the verbose `"\ No newline at end of file"` patch warning and makes patches easier to read since it's clearer what is being changed when lines are added to the end of a file.

```
#!php
<?php
if (condition1 || condition2) {
  echo "Hello";
}
?>
```

## Operators ##
All binary operators (operators that come between two values), such as `+, -, =, !=, ==, >,` etc. should have a space before and after the operator for readability. For example, an assignment should be formatted as `$foo = $bar;` rather than `$foo=$bar;`. Unary operators (operators that operate on only one value), such as `++`, should not have a space between the operator and the variable or number they are operating on.

```
#!php

<?php
// Correct
$foo == $bar;
$foo += 4;
$foo = $bar++;

// Incorrect
$foo==$bar;
$foo = $bar ++;
?>
```

## Casting ##
Don’t put a space between the `(type)` and the `$variable` in a cast.

```
#!php

<?php
// Correct
$cast = (int)$mynumber;

// Incorrect
$cast = (int) $mynumber;
?>
```

## Control Structures ##
Control structures includes `if, for, while, switch,` etc. Here is a sample `if` statement, since it covers most styles needed:

```
#!php

<?php
// Both of these examples have acceptable spacing, although Drupal recommends the first method.
if (condition1 || condition2) {
  action1;
}
elseif (condition3 && condition4) {
  action2;
}
else {
  defaultaction;
}

if (condition1 || condition2) {
  action1;
} elseif (condition3 && condition4) {
  action2;
} else {
  defaultaction;
}
?>
```

Note: Don't use `"else if"` -- always use `elseif`.

Control statements should have one space between the control keyword and opening parenthesis, to distinguish them from function calls.

Always use curly braces even in situations where they are technically optional. Having them increases readability and decreases the likelihood of logic errors being introduced when new lines are added. The opening curly should be on the same line as the opening statement, preceded by one space. The closing curly should be on a line by itself and indented to the same level as the opening statement.

For `switch` statements:

```
#!php

<?php
switch (condition) {
  case 1:
    action1;
    break;

  case 2:
    action2;
    break;

  default:
    defaultaction;
}
?>
```

For `do-while` statements:


```
#!php

<?php
do {
  actions;
} while ($condition);
?>
```

## Line Length and Wrapping ##
In general, Drupal recommends that all lines of code should not be longer than 80 characters. This is good practice, but it isn’t required for CypherWorx employees. Do as much possible to keep the code cleanly even if it exceeds 80 characters.

## Function Calls ##
Functions should be called with no spaces between the function name, the opening parenthesis, and the first parameter; spaces between commas and each parameter, and no space between the last parameter, the closing parenthesis, and the semicolon. Here's an example:


```
#!php

<?php
$var = foo($bar, $baz, $quux);
?>
```

As displayed above, there should be one space on either side of an equals sign used to assign the return value of a function to a variable. In the case of a block of related assignments, more space may be inserted to promote readability:


```
#!php

<?php
$short         = foo($bar);
$long_variable = foo($baz);
?>
```

## Function Declarations ##
Arguments with default values go at the end of the argument list. Always attempt to return a meaningful value from a function if one is appropriate.


```
#!php

<?php
function funstuff_system($field) {
  $system["description"] = t("This module inserts funny text into posts randomly.");
  return $system[$field];
}
?>
```

## Arrays ##
Arrays should be formatted with a space separating each element (after the comma), and spaces around the `=>` key association operator, if applicable:


```
#!php

<?php
$some_array = array('hello', 'world', 'foo' => 'bar');
?>
```

Note that if the line declaring an array spans longer than 80 characters (often the case with form and menu declarations), each element should be broken into its own line, and indented one level:

```
#!php

<?php
$form['title'] = array(
  '#type' => 'textfield',
  '#title' => t('Title'),
  '#size' => 60,
  '#maxlength' => 128,
  '#description' => t('The title of your node.'),
);
?>
```

Note the comma at the end of the last array element. This is not a typo! It helps prevent parsing errors if another element is placed at the end of the list later.

## Quotes ##
Drupal does not have a hard standard for the use of single quotes vs. double quotes. Where possible, keep consistency within each module, and respect the personal style of other developers.

With that caveat in mind, single quote strings is recommended to be used by default. Their use is recommended except in two cases:

1. Deliberate in-line variable interpolation, e.g. `<h2>$header</h2>`.
2. Translated strings where one can avoid escaping single quotes by enclosing the string in double quotes. One such string would be `"He's a good person."` It would be `'He\'s a good person.'` with single quotes. Such escaping may not be handled properly by .pot file generators for text translation, and it's also somewhat awkward to read.

## String Concatenations ##
Always use a space between the dot and the concatenated parts to improve readability.

```
#!php

<?php
$string = 'Foo' . $bar;
$string = $bar . 'foo';
$string = bar() . 'foo';
$string = 'foo' . 'bar';
?>
```

When you concatenate simple variables, you can use double quotes and add the variable inside; otherwise, use single quotes.

```
#!php

<?php
$string .= 'Foo';
$string .= $bar;
$string .= baz();
?>
```

When using the concatenating assignment operator (` .= `), use a space on each side as with the assignment operator:

## Including Code ##
Anywhere you are unconditionally including a class file, use `require_once()`. Anywhere you are conditionally including a class file (for example, factory methods), use `include_once()`. Either of these will ensure that class files are included only once. They share the same file list, so you don't need to worry about mixing them -- a file included with `require_once()` will not be included again by `require_once()`.

Note: `include_once()` and `require_once()` are statements, not functions. You don't need parentheses around the file name to be included.

When including code from the same directory or a sub-directory, start the file path with `.`:

```
#!php

<?php
include_once ./includes/mymodule_formatting.inc;
?>
```

In Drupal 7.x and later versions, use `DRUPAL_ROOT`:

```
#!php

<?php
require_once DRUPAL_ROOT . '/' . variable_get('cache_inc', 'includes/cache.inc');
?>
```

To include code in a module:


```
#!php

<?php
module_load_include('inc', 'node', 'node.admin');
?>
```

## PHP Code Tags ##
Always use `<?php ?>` to delimit PHP code, not the shorthand, `<? ?>`. This is required for Drupal compliance and is also the most portable way to include PHP code on differing operating systems and set-ups.

Note that as of Drupal 4.7, the `?>` at the end of code files is purposely omitted. This includes for module and include files. The reasons for this can be summarized as:

- Removing it eliminates the possibility for unwanted whitespace at the end of files which can cause "header already sent" errors, XHTML/XML validation issues, and other problems.
- The [closing delimiter at the end of a file is optional](http://www.php.net/basic-syntax.instruction-separation).
- PHP.net itself removes the closing delimiter from the end of its files (example: [prepend.inc](https://github.com/php/web-php/blob/master/include/prepend.inc)), so this can be seen as a "best practice."

## Semicolons ##
The PHP language requires semicolons at the end of most lines, but allows them to be omitted at the end of code blocks. Drupal coding standards require them, even at the end of code blocks. In particular, for one-line PHP blocks:


```
#!php

<?php
// Correct
<?php print $tax; ?>

// Incorrect
<?php print $tax ?>
?>
```

## Example URLs ##
Use `example.com` for all example URLs, per [RFC 2606](http://www.rfc-editor.org/rfc/rfc2606.txt).

## Naming Conventions ##
We strongly recommend using the following naming conventions to ensure code is consistent site-wide.

### Functions and Variables ###
Functions and variables should be named using lowercase, and words should be separated with an underscore. Functions should in addition have the grouping/module name as a prefix, to avoid name collisions between modules.


```
#!php

<?php
// Correct
function nationlms_this_is_correct() {
  // Code
}

// Incorrect
function thisIsJust-PlainWrong() {
  // Code
}
?>
```

### Persistent Variables ###
Persistent variables (variables/settings defined using Drupal's `variable_get()` / `variable_set()` functions) should be named using all lowercase letters, and words should be separated with an underscore. They should use the grouping/module name as a prefix, to avoid name collisions between modules.

### Constants ###
- Constants should always be all-uppercase, with underscores to separate words. (This includes pre-defined PHP constants like `TRUE`, `FALSE`, and `NULL`.)
- Module-defined constant names should also be prefixed by an uppercase spelling of the module that defines them.
- In Drupal 8 and later, constants should be defined using the `const` PHP language keyword (instead of `define()`), because it is better for performance.

Note that `const` does not work with PHP expressions. `define()` should be used when defining a constant conditionally or with a non-literal value:

```
#!php

<?php
define("ADMIN", "admin");
?>
```

### Global Variables ###
If you need to define global variables, their name should start with a single underscore followed by the module/theme name and another underscore.


```
#!php

// TODO: create example
```

### File Names ###
All documentation files should have the file name extension `.txt` to make viewing them on Windows systems easier. Also, the file names for such files should be all-caps (e.g. `README.txt` instead of `readme.txt`) while the extension itself is all-lowercase (i.e. `txt` instead of `TXT`). No spaces.
Examples: `README.txt`, `INSTALL.txt`, `TODO.txt`, `CHANGELOG.txt`
