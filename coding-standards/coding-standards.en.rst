================================
OpenPNE3 Coding Standard for PHP
================================

Overview
========

This document provides guidelines for code formatting to individuals and teams contributing to OpenPNE3.

Codes to include OpenPNE3 platform must follow these coding standard. Codes to include OpenPNE3 plugin doesn't always have to follow that, however it is recommended that all plugins follow that for other developers.

Thanks
------

This document based on `Zend Framework Coding Standard for PHP`_ and `Coding Standards of The Doctrine ORM Framework`_. And `Coding Standards of symfony`_ is incorporated into this document.

Thanks for these projects that publish useful document.

.. _`Zend Framework Coding Standard for PHP`: http://zendframework.com/manual/en/coding-standard.html
.. _`Coding Standards of The Doctrine ORM Framework`: http://www.doctrine-project.org/documentation/manual/1_1/en/coding-standards
.. _`Coding Standards of symfony`: http://trac.symfony-project.org/wiki/HowToContributeToSymfony#CodingStandards

PHP File Formatting
===================

General
-------

For files that contain only PHP code, the closing tag ("?>") is never permitted. It is not required by PHP. Not including it prevents trailing whitespace from being accidentally injected into the output.

Indentation
-----------

Use an indent of 2 spaces, with no tabs::

  <?php
   
  class opFoo
  {
    public function bar()
    {
      return true;
    }
  }

Maximum Line Length
-------------------

The target line length is 80 characters, i.e. developers should aim keep code as close to the 80-column boundary as is practical. However, longer lines are acceptable. The maximum length of any line of PHP code is 120 characters. 

Line Termination
----------------

Line termination is the standard way for Unix text files to represent the end of a line. Lines must end only with a linefeed (LF). Linefeeds are represented as ordinal 10, or hexadecimal 0x0A.

You should not use carriage returns (CR) like Macintosh computers (0x0D) and do not use the carriage return/linefeed combination (CRLF) as Windows computers (0x0D, 0x0A).

Naming Conventions
==================

Classes
-------

Class name must always be prefixed with "op".

Class names must only contain alphanumeric characters and underscores are not permitted. When class name consists of more than one word, the first letter of each new word must be capitalized (This is called "camelCase").

Interfaces
----------

Interface classes must follow the same conventions as other classes (see above).

They must also end with the word "Interface".

Functions and Methods
---------------------

Method names must be camelCase.

Defining functions is NOT permitted expecting helper functions. Function names must use underscores in accordance with core PHP functions (See `PHP Coding Standards`_).

.. _`PHP Coding Standards`: http://cvs.php.net/viewvc.cgi/php-src/CODING_STANDARDS?view=co

Variables
---------

Variable names may only contain alphanumeric characters. Underscores are not permitted. Numbers are permitted in variable names but are discouraged. They must always follow the "camelCaps" capitalization convention.

Verbosity is encouraged. Variables should always be as verbose as practical. Terse variable names such as "$i" and "$n" are discouraged for anything other than the smallest loop contexts. If a loop contains more than 20 lines of code, the variables for the indices need to have more descriptive names.

Constants
---------

Constants may contain both alphanumeric characters and the underscore. They must always have all letters capitalized. For readablity reasons, words in constant names must be separated by underscore characters.

Constants must be defined as class members by using the "const" construct. As a rule, defining constants in the global scope with "define" is NOT permitted.

Record Columns
--------------

All record columns must be in lowercase and usage of underscores(_) are encouraged for columns that consist of more than one word.

Foreign key fields must be in format [table_name]_[column].

Options and Parameters
----------------------

Options and Parameters that often appear as key of an array, must use lowercase and underscore.

Filenames
---------

For all other files, only alphanumeric characters, underscores, and the dash character ("-") are permitted. Spaces are prohibited.

Any file that contains any PHP code must end with the extension ".php". And if the file contains some classes and interfaces, the file name end with ".class.php", with the notable exception of model scripts.

Plugins
-------

All OpenPNE plugin names are in camelCase, start with "op" and end with "Plugin".

If a plugin is for authentication, its name should contain "Auth".

Coding Style
============

PHP Code Demarcation
--------------------

PHP code must always be delimited by the full-form, standard PHP tags and short tags ("<? ?>" and "<?= ?>") are never allowed. For files containing only PHP code, the closing tag must always be omitted.

Strings
-------

Literal String
++++++++++++++

When a string is literal (contains no variable substitutions), the "single quote" must always used to demarcate the string.

String Containing Single Quote
++++++++++++++++++++++++++++++

When a literal string itself contains single quote, it is permitted to demarcate the string with quotation marks "double quotes".

Variable Substitution
+++++++++++++++++++++

Variable substitution in strings is not permitted.

Use string-concatenation or the sprintf() function instead::

  $newString = $string.' is good.';
  $newString = sprintf('%s is good.', $string);

String Concatenation
++++++++++++++++++++

Strings may be concatenated using the "." operator. No space must be added before and after the "." operator::

  $openpne = 'OpenPNE'.' is '.' a '.' SNS '.' Engine.';

When concatenating strings with the "." operator, it is permitted to break the statement into multiple lines to improve readability. In these cases, each successive line should be padded with whitespace such that the "."; operator is aligned under the "=" operator::

  $sql = "SELECT id, name FROM user "
       . "WHERE name = ? "
       . "ORDER BY name ASC";

Arrays
------

Numerically Indexed Arrays
++++++++++++++++++++++++++

Negative numbers are not permitted as indices and a indexed array may be started with any non-negative number, however this is discouraged and it is recommended that all arrays have a base index of 0.

When declaring indexed arrays with the array construct, a trailing space must be added after each comma delimiter to improve readability::

  $sampleArray = array('OpenPNE', 'SNS', 1, 2, 3);

It is permitted to declare multi-line indexed arrays using the "array" construct. In this case, the initial array item must begin on the following line, be padded at one indentation level greater than the line containing the array declaration, and all successive lines should have the same indentation; the closing paren should be on a line by itself at the same indentation level as the line containing the array declaration::

  $sampleArray = array(
    1, 2, 3,
    $a, $b, $c,
    56.44, $d, 500,
  );

When using this latter declaration, we encourage using a trailing comma for the last item in the array; this minimizes the impact of adding new items on successive lines, and helps to ensure no parse errors occur due to a missing comma. 

Associative Arrays
++++++++++++++++++

When declaring associative arrays with the array construct, breaking the statement into multiple lines is encouraged. The initial array item must begin on the following line, be padded at one indentation level greater than the line containing the array declaration, and all successive lines should have the same indentation; the closing paren should be on a line by itself at the same indentation level as the line containing the array declaration. For readability, the various "=>" assignment operators should be padded such that they align::

  $sampleArray = array(
    'first'  => 'firstValue',
    'second' => 'secondValue',
  );

When using this latter declaration, we encourage using a trailing comma for the last item in the array; this minimizes the impact of adding new items on successive lines, and helps to ensure no parse errors occur due to a missing comma. 

Classes
-------

The brace is always written next line after the class name.

Every class must have a documentation block that conforms to the PHPDocumentor standard.

This is an example of an acceptable class declaration::

 /**
  * Documentation here
  */
  class opSampleClass 
  {
    // entire content of class
    // must be indented 2 spaces
  }

Functions and Methods
---------------------

Defining
++++++++

Methods must always declare their visibility by using one of the private, protected, or public constructs.

Like classes, the brace is always written next line after the method name. There is no space between the function name and the opening parenthesis for the arguments.

This is an example of an acceptable function declaration in a class::

  /**
   * Documentation Block Here
   */
   class Foo 
   {
    /**
     * Documentation Block Here
     */
     public function bar() 
     {
       // entire content of function
       // must be indented 2 spaces
     }
   }

Passing by-reference is permitted in the function declaration only. 

Return statements should have a blank line prior to it to increase readability::

  public function isBar()
  {
    $flag = true;
     
    if ($flag)
    {
      $this->someThingToDo();
       
      return $flag;
    }
     
    return false;
  }

Using
+++++

Function arguments are separated by a single trailing space after the comma delimiter.

For functions whose arguments permitted arrays, the function call may include the array construct and can be split into multiple lines to improve readability.

Property
--------

Properties must always declare their visibility by using one of the private, protected, or public constructs.

Control Statements
------------------

Control statements must have a single space before the opening parenthesis of the conditional.

The opening brace is always written next line after the conditional statement. The closing brace is always written on its own line. Any content within the braces must be indented 2 spaces.

Don't put spaces after an opening parenthesis and before a closing one::

  if ($foo == 1)
  {
    // body
  }
  elseif ($foo == 2)
  {
    // body
  }
  else
  {
    // body
  }

PHP allows statements to be written without braces in some circumstances. This coding standard makes no differentiation- all "if", "elseif" or "else" statements must use braces. 

All content within the switch statement must be indented 2 spaces. Content under each case statement must be indented an additional 2 spaces but the breaks must be at the same indentation level as the case statements::

  switch ($case)
  {
    case 1:
      break;
    default:
      break;
  }

The construct default may never be omitted from a switch statement.

Comment
-------

C style comments (`/* */`) and standard C++ comments (//) are both fine. Use of Perl/shell style comments (#) is discouraged.

All standard C++ comments should start with a space.

Checking a Variable
-------------------

To check if a variable is null or not, use the is_null() native PHP function::

  if (is_null($var))
  {
    echo '$var is null.';
  }

When comparing a variable to a value, put the value first and use type testing when applicable::

  if (1 === $var)

Inline Documentation
--------------------

Documentation Format
++++++++++++++++++++

All documentation blocks ("docblocks") must be compatible with the phpDocumentor format. Describing the phpDocumentor format is beyond the scope of this document. For more information, visit: http://phpdoc.org/

All class files must contain a "class-level" docblock immediately above each class. Examples of such docblocks can be found below. 

Class-level docblock
++++++++++++++++++++

Every class must have a docblock that contains these phpDocumentor tags at a minimum.

 * A one-line description of the class
 * The @author annotation
 * The @package annotation. It must have a value of "OpenPNE" or plugin name

Here is an example of minimum class-level docblock::

    /**
     * Short description for class
     *
     * @package OpenPNE
     * @author  John Smith <jsmith@example.com>
     */

Copyright and License
=====================

::

  Copyright (c) 2005-2009, Zend Technologies USA, Inc.
  All rights reserved.

  Redistribution and use in source and binary forms, with or without modification,
  are permitted provided that the following conditions are met:

      * Redistributions of source code must retain the above copyright notice,
        this list of conditions and the following disclaimer.

      * Redistributions in binary form must reproduce the above copyright notice,
        this list of conditions and the following disclaimer in the documentation
        and/or other materials provided with the distribution.

      * Neither the name of Zend Technologies USA, Inc. nor the names of its
        contributors may be used to endorse or promote products derived from this
        software without specific prior written permission.

  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
  ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
  WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
  DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
  ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
  (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
  LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
  ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
  (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
  SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
