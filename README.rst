language-check
==============

Python wrapper for LanguageTool.

.. image:: https://travis-ci.org/myint/language-check.png?branch=master
    :target: https://travis-ci.org/myint/language-check
    :alt: Build status

This is a fork of
https://bitbucket.org/spirit/language_tool that produces more easily parsable
results from the command-line.

Example usage
-------------

From the interpreter:

>>> import language_check
>>> tool = language_check.LanguageTool('en-US')
>>> text = 'A sentence with a error in the Hitchhiker’s Guide tot he Galaxy'
>>> matches = tool.check(text)
>>> len(matches)
2

Check out some ``Match`` object attributes:

>>> matches[0].fromy, matches[0].fromx
(0, 16)
>>> matches[0].ruleId, matches[0].replacements
('EN_A_VS_AN', ['an'])
>>> matches[1].fromy, matches[1].fromx
(0, 50)
>>> matches[1].ruleId, matches[1].replacements
('TOT_HE', ['to the'])

Print a ``Match`` object:

>>> print(matches[1])
Line 1, column 51, Rule ID: TOT_HE[1]
Message: Did you mean 'to the'?
Suggestion: to the
...

Automatically apply suggestions to the text:

>>> language_check.correct(text, matches)
'A sentence with an error in the Hitchhiker’s Guide to the Galaxy'

From the command line::

    $ echo 'This are bad.' > example.txt

    $ language-check example.txt
    example.txt:1:1: THIS_NNS[3]: Did you mean 'these'?


Installation
------------

To install via pip::

    $ pip install --user --upgrade language-check


Prerequisites
-------------

- `Python 3.2+ <http://www.python.org>`_ (or 2.7)
- `LanguageTool <http://www.languagetool.org>`_
- `lib3to2 <https://bitbucket.org/amentajo/lib3to2>`_
  (if installing for Python 2)


The installation process should take care of downloading LanguageTool
(it may take a few minutes).
Otherwise, you can manually download `LanguageTool-stable.zip
<http://www.languagetool.org/download/LanguageTool-stable.zip>`_
and unzip it into where the ``language_check`` package resides.

LanguageTool requires Java 6 or later.

Vim plugin
----------

To use language-check in Vim, install Syntastic_ and use the following
settings:

.. code-block:: vim

    let g:syntastic_text_checkers = ['language_check']
    let g:syntastic_text_language_check_args = '--language=en-US'

Customize your language as appropriate.

.. _Syntastic: https://github.com/scrooloose/syntastic
