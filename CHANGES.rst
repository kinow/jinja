.. currentmodule:: jinja2

Jinja Changelog
===============


Version 2.11
------------

unreleased

- Async support is only loaded the first time an
  :class:`~environment.Environment` enables it, in order to avoid a
  slow initial import. (`#765`_)
- Python 2.6 and 3.3 are not supported anymore.
- The ``map`` filter in async mode now automatically awaits
- Added a new ``ChainableUndefined`` class to support getitem
  and getattr on an undefined object. (`#977`_)
- Allow `'{%+'` syntax (with NOP behavior) when
  `lstrip_blocks == False` (`#748`_)

.. _#765: https://github.com/pallets/jinja/issues/765
.. _#748: https://github.com/pallets/jinja/issues/748
.. _#977: https://github.com/pallets/jinja/issues/977


Version 2.10.2
--------------

_Unreleased_

-  Fix Python 3.7 deprecation warnings.


Version 2.10.1
--------------

Released 2019-04-06

-   ``SandboxedEnvironment`` securely handles ``str.format_map`` in
    order to prevent code execution through untrusted format strings.
    The sandbox already handled ``str.format``.


Version 2.10
------------

released on November 8th 2017

- Added a new extension node called ``OverlayScope`` which can be used to
  create an unoptimized scope that will look up all variables from a
  derived context.
- Added an ``in`` test that works like the in operator.  This can be used
  in combination with ``reject`` and ``select``.
- Added ``previtem`` and ``nextitem`` to loop contexts, providing access to the
  previous/next item in the loop. If such an item does not exist, the value is
  undefined.
- Added ``changed(*values)`` to loop contexts, providing an easy way of
  checking whether a value has changed since the last iteration (or rather
  since the last call of the method)
- Added a ``namespace`` function that creates a special object which allows
  attribute assignment using the ``set`` tag.  This can be used to carry data
  across scopes, e.g. from a loop body to code that comes after the loop.
- Added a ``trimmed`` modifier to ``{% trans %}`` to strip linebreaks and
  surrounding whitespace. Also added a new policy to enable this for all
  ``trans`` blocks.
- The ``random`` filter is no longer incorrectly constant folded and will
  produce a new random choice each time the template is rendered. (`#478`_)
- Added a ``unique`` filter. (`#469`_)
- Added ``min`` and ``max`` filters. (`#475`_)
- Added tests for all comparison operators: ``eq``, ``ne``, ``lt``, ``le``,
  ``gt``, ``ge``. (`#665`_)
- ``import`` statement cannot end with a trailing comma. (`#617`_, `#618`_)
- ``indent`` filter will not indent blank lines by default. (`#685`_)
- Add ``reverse`` argument for ``dictsort`` filter. (`#692`_)
- Add a ``NativeEnvironment`` that renders templates to native Python types
  instead of strings. (`#708`_)
- Added filter support to the block ``set`` tag. (`#489`_)
- ``tojson`` filter marks output as safe to match documented behavior.
  (`#718`_)
- Resolved a bug where getting debug locals for tracebacks could
  modify template context.
- Fixed a bug where having many ``{% elif ... %}`` blocks resulted in a
  "too many levels of indentation" error.  These blocks now compile to
  native ``elif ..:`` instead of ``else: if ..:`` (`#759`_)

.. _#469: https://github.com/pallets/jinja/pull/469
.. _#475: https://github.com/pallets/jinja/pull/475
.. _#478: https://github.com/pallets/jinja/pull/478
.. _#489: https://github.com/pallets/jinja/pull/489
.. _#617: https://github.com/pallets/jinja/pull/617
.. _#618: https://github.com/pallets/jinja/pull/618
.. _#665: https://github.com/pallets/jinja/pull/665
.. _#685: https://github.com/pallets/jinja/pull/685
.. _#692: https://github.com/pallets/jinja/pull/692
.. _#708: https://github.com/pallets/jinja/pull/708
.. _#718: https://github.com/pallets/jinja/pull/718
.. _#759: https://github.com/pallets/jinja/pull/759


Version 2.9.6
-------------

(bugfix release, released on April 3rd 2017)

- Fixed custom context behavior in fast resolve mode (#675)


Version 2.9.5
-------------

(bugfix release, released on January 28th 2017)

- Restored the original repr of the internal ``_GroupTuple`` because this
  caused issues with ansible and it was an unintended change.  (#654)
- Added back support for custom contexts that override the old ``resolve``
  method since it was hard for people to spot that this could cause a
  regression.
- Correctly use the buffer for the else block of for loops.  This caused
  invalid syntax errors to be caused on 2.x and completely wrong behavior
  on Python 3 (#669)
- Resolve an issue where the ``{% extends %}`` tag could not be used with
  async environments. (#668)
- Reduce memory footprint slightly by reducing our unicode database dump
  we use for identifier matching on Python 3 (#666)
- Fixed autoescaping not working for macros in async compilation mode. (#671)


Version 2.9.4
-------------

(bugfix release, released on January 10th 2017)

- Solved some warnings for string literals.  (#646)
- Increment the bytecode cache version which was not done due to an
  oversight before.
- Corrected bad code generation and scoping for filtered loops.  (#649)
- Resolved an issue where top-level output silencing after known extend
  blocks could generate invalid code when blocks where contained in if
  statements.  (#651)
- Made the ``truncate.leeway`` default configurable to improve compatibility
  with older templates.


Version 2.9.3
-------------

(bugfix release, released on January 8th 2017)

- Restored the use of blocks in macros to the extend that was possible
  before.  On Python 3 it would render a generator repr instead of
  the block contents. (#645)
- Set a consistent behavior for assigning of variables in inner scopes
  when the variable is also read from an outer scope.  This now sets the
  intended behavior in all situations however it does not restore the
  old behavior where limited assignments to outer scopes was possible.
  For more information and a discussion see #641
- Resolved an issue where ``block scoped`` would not take advantage of the
  new scoping rules.  In some more exotic cases a variable overridden in a
  local scope would not make it into a block.
- Change the code generation of the ``with`` statement to be in line with the
  new scoping rules.  This resolves some unlikely bugs in edge cases.  This
  also introduces a new internal ``With`` node that can be used by extensions.


Version 2.9.2
-------------

(bugfix release, released on January 8th 2017)

- Fixed a regression that caused for loops to not be able to use the same
  variable for the target as well as source iterator.  (#640)
- Add support for a previously unknown behavior of macros.  It used to be
  possible in some circumstances to explicitly provide a caller argument
  to macros.  While badly buggy and unintended it turns out that this is a
  common case that gets copy pasted around.  To not completely break backwards
  compatibility with the most common cases it's now possible to provide an
  explicit keyword argument for caller if it's given an explicit default.
  (#642)


Version 2.9.1
-------------

(bugfix release, released on January 7th 2017)

- Resolved a regression with call block scoping for macros.  Nested caller
  blocks that used the same identifiers as outer macros could refer to the
  wrong variable incorrectly.


Version 2.9
-----------
(codename Derivation, released on January 7th 2017)

- Change cache key definition in environment. This fixes a performance
  regression introduced in 2.8.
- Added support for ``generator_stop`` on supported Python versions
  (Python 3.5 and later)
- Corrected a long standing issue with operator precedence of math operations
  not being what was expected.
- Added support for Python 3.6 async iterators through a new async mode.
- Added policies for filter defaults and similar things.
- urlize now sets "rel noopener" by default.
- Support attribute fallback for old-style classes in 2.x.
- Support toplevel set statements in extend situations.
- Restored behavior of Cycler for Python 3 users.
- Subtraction now follows the same behavior as other operators on undefined
  values.
- ``map`` and friends will now give better error messages if you forgot to
  quote the parameter.
- Depend on MarkupSafe 0.23 or higher.
- Improved the ``truncate`` filter to support better truncation in case
  the string is barely truncated at all.
- Change the logic for macro autoescaping to be based on the runtime
  autoescaping information at call time instead of macro define time.
- Ported a modified version of the ``tojson`` filter from Flask to Jinja2
  and hooked it up with the new policy framework.
- Block sets are now marked ``safe`` by default.
- On Python 2 the asciification of ASCII strings can now be disabled with
  the ``compiler.ascii_str`` policy.
- Tests now no longer accept an arbitrary expression as first argument but
  a restricted one.  This means that you can now properly use multiple
  tests in one expression without extra parentheses.  In particular you can
  now write ``foo is divisibleby 2 or foo is divisibleby 3``
  as you would expect.
- Greatly changed the scoping system to be more consistent with what template
  designers and developers expect.  There is now no more magic difference
  between the different include and import constructs.  Context is now always
  propagated the same way.  The only remaining differences is the defaults
  for ``with context`` and ``without context``.
- The ``with`` and ``autoescape`` tags are now built-in.
- Added the new ``select_autoescape`` function which helps configuring better
  autoescaping easier.
- Fixed a runtime error in the sandbox when attributes of async generators
  were accessed.


Version 2.8.1
-------------

(bugfix release, released on December 29th 2016)

- Fixed the ``for_qs`` flag for ``urlencode``.
- Fixed regression when applying ``int`` to non-string values.
- SECURITY: if the sandbox mode is used format expressions are now sandboxed
  with the same rules as in Jinja.  This solves various information leakage
  problems that can occur with format strings.


Version 2.8
-----------

(codename Replacement, released on July 26th 2015)

- Added ``target`` parameter to urlize function.
- Added support for ``followsymlinks`` to the file system loader.
- The truncate filter now counts the length.
- Added equalto filter that helps with select filters.
- Changed cache keys to use absolute file names if available
  instead of load names.
- Fixed loop length calculation for some iterators.
- Changed how Jinja2 enforces strings to be native strings in
  Python 2 to work when people break their default encoding.
- Added :func:`make_logging_undefined` which returns an undefined
  object that logs failures into a logger.
- If unmarshalling of cached data fails the template will be
  reloaded now.
- Implemented a block ``set`` tag.
- Default cache size was increased to 400 from a low 50.
- Fixed ``is number`` test to accept long integers in all Python versions.
- Changed ``is number`` to accept Decimal as a number.
- Added a check for default arguments followed by non-default arguments. This
  change makes ``{% macro m(x, y=1, z) %}...{% endmacro %}`` a syntax error.
  The previous behavior for this code was broken anyway (resulting in the
  default value being applied to ``y``).
- Add ability to use custom subclasses of ``jinja2.compiler.CodeGenerator`` and
  ``jinja2.runtime.Context`` by adding two new attributes to the environment
  (``code_generator_class`` and ``context_class``) (pull request ``#404``).
- added support for context/environment/evalctx decorator functions on
  the finalize callback of the environment.
- escape query strings for urlencode properly.  Previously slashes were not
  escaped in that place.
- Add 'base' parameter to 'int' filter.


Version 2.7.3
-------------

(bugfix release, released on June 6th 2014)

- Security issue: Corrected the security fix for the cache folder.  This
  fix was provided by RedHat.


Version 2.7.2
-------------

(bugfix release, released on January 10th 2014)

- Prefix loader was not forwarding the locals properly to
  inner loaders.  This is now fixed.
- Security issue: Changed the default folder for the filesystem cache to be
  user specific and read and write protected on UNIX systems.  See
  `Debian bug 734747`_ for more information.

.. _Debian bug 734747: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=734747


Version 2.7.1
-------------

(bugfix release, released on August 7th 2013)

- Fixed a bug with ``call_filter`` not working properly on environment
  and context filters.
- Fixed lack of Python 3 support for bytecode caches.
- Reverted support for defining blocks in included templates as this
  broke existing templates for users.
- Fixed some warnings with hashing of undefineds and nodes if Python
  is run with warnings for Python 3.
- Added support for properly hashing undefined objects.
- Fixed a bug with the title filter not working on already uppercase
  strings.


Version 2.7
-----------

(codename Translation, released on May 20th 2013)

- Choice and prefix loaders now dispatch source and template lookup
  separately in order to work in combination with module loaders as
  advertised.
- Fixed filesizeformat.
- Added a non-silent option for babel extraction.
- Added ``urlencode`` filter that automatically quotes values for
  URL safe usage with utf-8 as only supported encoding.  If applications
  want to change this encoding they can override the filter.
- Added ``keep-trailing-newline`` configuration to environments and
  templates to optionally preserve the final trailing newline.
- Accessing ``last`` on the loop context no longer causes the iterator
  to be consumed into a list.
- Python requirement changed: 2.6, 2.7 or >= 3.3 are required now,
  supported by same source code, using the "six" compatibility library.
- Allow ``contextfunction`` and other decorators to be applied to ``__call__``.
- Added support for changing from newline to different signs in the ``wordwrap``
  filter.
- Added support for ignoring memcache errors silently.
- Added support for keeping the trailing newline in templates.
- Added finer grained support for stripping whitespace on the left side
  of blocks.
- Added ``map``, ``select``, ``reject``, ``selectattr`` and ``rejectattr``
  filters.
- Added support for ``loop.depth`` to figure out how deep inside a recursive
  loop the code is.
- Disabled py_compile for pypy and python 3.


Version 2.6
-----------

(codename Convolution, released on July 24th 2011)

- internal attributes now raise an internal attribute error now instead
  of returning an undefined.  This fixes problems when passing undefined
  objects to Python semantics expecting APIs.
- traceback support now works properly for PyPy.  (Tested with 1.4)
- implemented operator intercepting for sandboxed environments.  This
  allows application developers to disable builtin operators for better
  security.  (For instance limit the mathematical operators to actual
  integers instead of longs)
- groupby filter now supports dotted notation for grouping by attributes
  of attributes.
- scoped blocks now properly treat toplevel assignments and imports.
  Previously an import suddenly "disappeared" in a scoped block.
- automatically detect newer Python interpreter versions before loading code
  from bytecode caches to prevent segfaults on invalid opcodes.  The segfault
  in earlier Jinja2 versions here was not a Jinja2 bug but a limitation in
  the underlying Python interpreter.  If you notice Jinja2 segfaulting in
  earlier versions after an upgrade of the Python interpreter you don't have
  to upgrade, it's enough to flush the bytecode cache.  This just no longer
  makes this necessary, Jinja2 will automatically detect these cases now.
- the sum filter can now sum up values by attribute.  This is a backwards
  incompatible change.  The argument to the filter previously was the
  optional starting index which defaults to zero.  This now became the
  second argument to the function because it's rarely used.
- like sum, sort now also makes it possible to order items by attribute.
- like sum and sort, join now also is able to join attributes of objects
  as string.
- the internal eval context now has a reference to the environment.
- added a mapping test to see if an object is a dict or an object with
  a similar interface.


Version 2.5.5
-------------

(re-release of 2.5.4 with built documentation removed for filesize.
 Released on October 18th 2010)

- built documentation is no longer part of release.


Version 2.5.4
-------------

(bugfix release, released on October 17th 2010)

- Fixed extensions not loading properly with overlays.
- Work around a bug in cpython for the debugger that causes segfaults
  on 64bit big-endian architectures.


Version 2.5.3
-------------

(bugfix release, released on October 17th 2010)

- fixed an operator precedence error introduced in 2.5.2.  Statements
  like "-foo.bar" had their implicit parentheses applied around the
  first part of the expression ("(-foo).bar") instead of the more
  correct "-(foo.bar)".


Version 2.5.2
-------------
(bugfix release, released on August 18th 2010)

- improved setup.py script to better work with assumptions people
  might still have from it (``--with-speedups``).
- fixed a packaging error that excluded the new debug support.


Version 2.5.1
-------------

(bugfix release, released on August 17th 2010)

- StopIteration exceptions raised by functions called from templates
  are now intercepted and converted to undefineds.  This solves a
  lot of debugging grief.  (StopIteration is used internally to
  abort template execution)
- improved performance of macro calls slightly.
- babel extraction can now properly extract newstyle gettext calls.
- using the variable ``num`` in newstyle gettext for something else
  than the pluralize count will no longer raise a :exc:`KeyError`.
- removed builtin markup class and switched to markupsafe.  For backwards
  compatibility the pure Python implementation still exists but is
  pulled from markupsafe by the Jinja2 developers.  The debug support
  went into a separate feature called "debugsupport" and is disabled
  by default because it is only relevant for Python 2.4
- fixed an issue with unary operators having the wrong precedence.


Version 2.5
-----------

(codename Incoherence, released on May 29th 2010)

- improved the sort filter (should have worked like this for a
  long time) by adding support for case insensitive searches.
- fixed a bug for getattribute constant folding.
- support for newstyle gettext translations which result in a
  nicer in-template user interface and more consistent
  catalogs. (:ref:`newstyle-gettext`)
- it's now possible to register extensions after an environment
  was created.


Version 2.4.1
-------------

(bugfix release, released on April 20th 2010)

- fixed an error reporting bug for undefineds.


Version 2.4
-----------

(codename Correlation, released on April 13th 2010)

- the environment template loading functions now transparently
  pass through a template object if it was passed to it.  This
  makes it possible to import or extend from a template object
  that was passed to the template.
- added a :class:`ModuleLoader` that can load templates from
  precompiled sources.  The environment now features a method
  to compile the templates from a configured loader into a zip
  file or folder.
- the _speedups C extension now supports Python 3.
- added support for autoescaping toggling sections and support
  for evaluation contexts (:ref:`eval-context`).
- extensions have a priority now.


Version 2.3.1
-------------

(bugfix release, released on February 19th 2010)

- fixed an error reporting bug on all python versions
- fixed an error reporting bug on Python 2.4


Version 2.3
-----------

(codename 3000 Pythons, released on February 10th 2010)

- fixes issue with code generator that causes unbound variables
  to be generated if set was used in if-blocks and other small
  identifier problems.
- include tags are now able to select between multiple templates
  and take the first that exists, if a list of templates is
  given.
- fixed a problem with having call blocks in outer scopes that
  have an argument that is also used as local variable in an
  inner frame (#360).
- greatly improved error message reporting (#339)
- implicit tuple expressions can no longer be totally empty.
  This change makes ``{% if %}...{% endif %}`` a syntax error
  now. (#364)
- added support for translator comments if extracted via babel.
- added with-statement extension.
- experimental Python 3 support.


Version 2.2.1
-------------

(bugfix release, released on September 14th 2009)

- fixes some smaller problems for Jinja2 on Jython.


Version 2.2
-----------

(codename Kong, released on September 13th 2009)

- Include statements can now be marked with ``ignore missing`` to skip
  non existing templates.
- Priority of ``not`` raised.  It's now possible to write `not foo in bar`
  as an alias to `foo not in bar` like in python.  Previously the grammar
  required parentheses (`not (foo in bar)`) which was odd.
- Fixed a bug that caused syntax errors when defining macros or using the
  `{% call %}` tag inside loops.
- Fixed a bug in the parser that made ``{{ foo[1, 2] }}`` impossible.
- Made it possible to refer to names from outer scopes in included templates
  that were unused in the callers frame (#327)
- Fixed a bug that caused internal errors if names where used as iteration
  variable and regular variable *after* the loop if that variable was unused
  *before* the loop.  (#331)
- Added support for optional ``scoped`` modifier to blocks.
- Added support for line-comments.
- Added the ``meta`` module.
- Renamed (undocumented) attribute "overlay" to "overlayed" on the
  environment because it was clashing with a method of the same name.
- speedup extension is now disabled by default.


Version 2.1.1
-------------

(bugfix release, released on December 25th 2008)

- Fixed a translation error caused by looping over empty recursive loops.


Version 2.1
-----------

(codename Yasuzō, released on November 23rd 2008)

- fixed a bug with nested loops and the special loop variable.  Before the
  change an inner loop overwrote the loop variable from the outer one after
  iteration.
- fixed a bug with the i18n extension that caused the explicit pluralization
  block to look up the wrong variable.
- fixed a limitation in the lexer that made ``{{ foo.0.0 }}`` impossible.
- index based subscribing of variables with a constant value returns an
  undefined object now instead of raising an index error.  This was a bug
  caused by eager optimizing.
- the i18n extension looks up ``foo.ugettext`` now followed by ``foo.gettext``
  if an translations object is installed.  This makes dealing with custom
  translations classes easier.
- fixed a confusing behavior with conditional extending.  loops were partially
  executed under some conditions even though they were not part of a visible
  area.
- added ``sort`` filter that works like ``dictsort`` but for arbitrary sequences.
- fixed a bug with empty statements in macros.
- implemented a bytecode cache system.  (:ref:`bytecode-cache`)
- the template context is now weakref-able
- inclusions and imports "with context" forward all variables now, not only
  the initial context.
- added a cycle helper called ``cycler``.
- added a joining helper called ``joiner``.
- added a ``compile_expression`` method to the environment that allows compiling
  of Jinja expressions into callable Python objects.
- fixed an escaping bug in urlize


Version 2.0
-----------

(codename jinjavitus, released on July 17th 2008)

- the subscribing of objects (looking up attributes and items) changed from
  slightly.  It's now possible to give attributes or items a higher priority
  by either using dot-notation lookup or the bracket syntax.  This also
  changed the AST slightly.  ``Subscript`` is gone and was replaced with
  :class:`~jinja2.nodes.Getitem` and :class:`~jinja2.nodes.Getattr`.

  For more information see :ref:`the implementation details <notes-on-subscriptions>`.
- added support for preprocessing and token stream filtering for extensions.
  This would allow extensions to allow simplified gettext calls in template
  data and something similar.
- added :meth:`jinja2.environment.TemplateStream.dump`.
- added missing support for implicit string literal concatenation.
  ``{{ "foo" "bar" }}`` is equivalent to ``{{ "foobar" }}``
- ``else`` is optional for conditional expressions.  If not given it evaluates
  to ``false``.
- improved error reporting for undefined values by providing a position.
- ``filesizeformat`` filter uses decimal prefixes now per default and can be
  set to binary mode with the second parameter.
- fixed bug in finalizer


Version 2.0rc1
--------------

(no codename, released on June 9th 2008)

- first release of Jinja2
