.. _PageReleaseNotes:

Release notes
=============

.. contents:: :local:
    :depth: 1

This page contains release notes of notable changes in Pine Script.

December 2021
-------------

Linefills
^^^^^^^^^
The space between lines drawn in Pine can now be filled! We’ve added a new ``linefill`` drawing type, along with a number of functions dedicated to manipulating it. 
Linefills are created by passing two lines and a color to the ``linefill.new()`` function, and their behavior is based on the lines they're tied to: 
they extend in the same direction as the lines, move when their lines move, and are deleted when one of the two lines is deleted.

New linefill-related functions:

* `array.new_linefill() <https://www.tradingview.com/pine-script-reference/v5/#fun_array{dot}new_linefill>`__
* `linefill() <https://www.tradingview.com/pine-script-reference/v5/#fun_linefill>`__
* `linefill.delete() <https://www.tradingview.com/pine-script-reference/v5/#fun_linefill{dot}delete>`__
* `linefill.get_line1() <https://www.tradingview.com/pine-script-reference/v5/#fun_linefill{dot}get_line1>`__
* `linefill.get_line2() <https://www.tradingview.com/pine-script-reference/v5/#fun_linefill{dot}get_line2>`__
* `linefill.new() <https://www.tradingview.com/pine-script-reference/v5/#fun_linefill{dot}new>`__
* `linefill.set_color() <https://www.tradingview.com/pine-script-reference/v5/#fun_linefill{dot}set_color>`__
* `linefill.all() <https://www.tradingview.com/pine-script-reference/v5/#var_linefill{dot}all>`__


New functions for string manipulation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Added a number of new functions that provide more ways to process strings, and introduce regular expressions to Pine:

* `str.contains(source, str) <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}contains>`__ - Determines if the ``source`` string contains the ``str`` substring.
* `str.pos(source, str) <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}pos>`__ - Returns the position of the ``str`` string in the ``source`` string.
* `str.substring(source, begin_pos, end_pos) <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}substring>`__ - Extracts a substring from the ``source`` string.
* `str.replace(source, target, replacement, occurrence) <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}replace>`__ - 
  Contrary to the existing `str.replace_all() <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}replace_all>`__ function, ``str.replace()`` allows the selective replacement of a matched substring with a replacement string.
* `str.lower(source) <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}lower>`__ and 
  `str.upper(source) <https://www.tradingview.com/pine-script-reference/v5/#fun_str%7Bdot%7Dupper>`__ - Convert all letters of the ``source`` string to lower or upper case:
* `str.startswith(source, str) <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}startswith>`__ and 
  `str.endswith(source, str) <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}endswith>`__ - Determine if the ``source`` string starts or ends with the ``str`` substring.
* `str.match(source, regex) <https://www.tradingview.com/pine-script-reference/v5/#fun_str{dot}match>`__ - Extracts the substring matching the specified `regular expression <https://en.wikipedia.org/wiki/Regular_expression#Perl_and_PCRE>`__.


Textboxes
^^^^^^^^^

Box drawings now supports text. The `box.new() <https://www.tradingview.com/pine-script-reference/v5/#fun_box{dot}new>`__ function has five new parameters for text manipulation: 
``text``, ``text_size``, ``text_color``, ``text_valign``, and ``text_halign``. Additionally, five new functions to set the text properties of existing boxes were added:

* `box.set_text() <https://www.tradingview.com/pine-script-reference/v5/#fun_box{dot}set_text>`__
* `box.set_text_color() <https://www.tradingview.com/pine-script-reference/v5/#fun_box{dot}set_text_color>`__
* `box.set_text_size() <https://www.tradingview.com/pine-script-reference/v5/#fun_box{dot}set_text_size>`__
* `box.set_text_valign() <https://www.tradingview.com/pine-script-reference/v5/#fun_box{dot}set_text_valign>`__
* `box.set_text_halign() <https://www.tradingview.com/pine-script-reference/v5/#fun_box{dot}set_text_halign>`__

New built-in variables
^^^^^^^^^^^^^^^^^^^^^^

Added new built-in variables that return the ``bar_index`` and ``time`` values of the last bar in the dataset. Their values are known at the beginning of the script's calculation:

* `last_bar_index <https://www.tradingview.com/pine-script-reference/v5/#var_last_bar_index>`__ - Bar index of the last chart bar.
* `last_bar_time <https://www.tradingview.com/pine-script-reference/v5/#var_last_bar_time>`__ - UNIX time of the last chart bar.

New built-in ``source`` variable:

* `hlcc4 <https://www.tradingview.com/pine-script-reference/v5/#var_hlcc4>`__ - A shortcut for ``(high + low + close + close)/4``. It averages the high and low values with the double-weighted close.

November 2021
-------------

for...in
^^^^^^^^

Added a new `for...in <https://www.tradingview.com/pine-script-reference/v5/#op_for{dot}{dot}{dot}in>`__ operator to iterate over all elements of an array::

	//@version=5
	indicator("My Script")
	int[] a1 = array.from(1, 3, 6, 3, 8, 0, -9, 5)

	highest(array) =>
	    var int highestNum = na
	    for item in array
                if na(highestNum) or item > highestNum
		    highestNum := item
	    highestNum

	plot(highest(a1))
	
Function overloads
^^^^^^^^^^^^^^^^^^
Added function overloads. Several functions in a script can now share the same name, as long one of the following conditions is true:

* Each overload has a different number of parameters::

	//@version=5
	indicator("Function overload")

	// Two parameters
	mult(x1, x2) =>
	    x1 * x2
	
	// Three parameters
	mult(x1, x2, x3) =>
	    x1 * x2 * x3

	plot(mult(7, 4))
	plot(mult(7, 4, 2))

* When overloads have the same number of parameters, all parameters in each overload must be explicitly typified, and their type combinations must be unique::

	//@version=5
	indicator("Function overload")

	// Accepts both 'int' and 'float' values - any 'int' can be automatically cast to 'float'
	mult(float x1, float x2) =>
	    x1 * x2

	// Returns a 'bool' value instead of a number
	mult(bool x1, bool x2) =>
	    x1 and x2 ? true : false

	mult(string x1, string x2) =>
	    str.tonumber(x1) * str.tonumber(x2)

	// Has three parameters, so explicit types are not required
	mult(x1, x2, x3) =>
	    x1 * x2 * x3

	plot(mult(7, 4))
	plot(mult(7.5, 4.2))
	plot(mult(true, false) ? 1 : 0)
	plot(mult("5", "6"))
	plot(mult(7, 4, 2))

Currency conversion
^^^^^^^^^^^^^^^^^^^
Added a new `currency` argument to most ``request.*()`` functions. If specified, price values returned by the function will be converted from the source currency to the target currency. 
The following functions are affected:

* `request.dividends() <https://www.tradingview.com/pine-script-reference/v5/#fun_request{dot}dividends>`__
* `request.earnings() <https://www.tradingview.com/pine-script-reference/v5/#fun_request{dot}earnings>`__
* `request.financial() <https://www.tradingview.com/pine-script-reference/v5/#fun_request{dot}financial>`__
* `request.security() <https://www.tradingview.com/pine-script-reference/v5/#fun_request{dot}security>`__

.. _PageReleaseNotes_October2021:

October 2021
--------------
Pine v5 is here! 
This is a list of the **new** features added to the language, and a few of the **changes** made.
See the :ref:`Migration guide to Pine v5 <PageToPineVersion5>` for a complete list of the **changes** in v5.


New features
^^^^^^^^^^^^^
Libraries are a new type of publication. They allow you to create custom functions for reuse in other scripts. See this manual's page on :ref:`Libraries <PageLibraries>`.

Pine now supports `switch <https://www.tradingview.com/pine-script-reference/v5/#op_switch>`__ structures!
They provide a more convenient and readable alternative to long ternary operators and `if <https://www.tradingview.com/pine-script-reference/v5/#op_if>`__ statements.

`while <https://www.tradingview.com/pine-script-reference/v5/#op_while>`__ loops are here! 
They allow you to create a loop that will only stop when its controlling condition is false, or a ``break`` command is used in the loop.

New built-in array variables are maintained by the Pine runtime to hold the IDs of all the active objects of the same type drawn by your script. They are 
`label.all <https://www.tradingview.com/pine-script-reference/v5/#var_label{dot}all>`__,
`line.all <https://www.tradingview.com/pine-script-reference/v5/#var_line{dot}all>`__,
`box.all <https://www.tradingview.com/pine-script-reference/v5/#var_box{dot}all>`__ and
`table.all <https://www.tradingview.com/pine-script-reference/v5/#var_table{dot}all>`__.

The `runtime.error() <https://www.tradingview.com/pine-script-reference/v5/#fun_runtime{dot}error>`__
function makes it possible to halt the execution of a script and display a runtime error with a custom message. 
You can use any condition in your script to trigger the call.

Parameter definitions in user-defined functions can now include a default value: 
a function defined as ``f(x = 1) => x`` will return 1 when called as ``f()``, i.e., without providing an argument for its ``x`` parameter.

New variables and functions provide better script visibility on strategy information:

- `strategy.closedtrades.entry_price() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}entry_price>`__ and 
  `strategy.opentrades.entry_price() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}opentrades{dot}entry_price>`__
- `strategy.closedtrades.entry_bar_index() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}entry_bar_index>`__ and 
  `strategy.opentrades.entry_bar_index() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}opentrades{dot}entry_bar_index>`__
- `strategy.closedtrades.entry_time() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}entry_time>`__ and 
  `strategy.opentrades.entry_time() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}opentrades{dot}entry_time>`__
- `strategy.closedtrades.size() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}size>`__ and 
  `strategy.opentrades.size() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}opentrades{dot}size>`__
- `strategy.closedtrades.profit() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}profit>`__ and 
  `strategy.opentrades.profit() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}opentrades{dot}profit>`__
- `strategy.closedtrades.commission() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}commission>`__ and 
  `strategy.opentrades.commission() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}opentrades{dot}commission>`__
- `strategy.closedtrades.max_runup() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}max_runup>`__ and 
  `strategy.opentrades.max_runup() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}opentrades{dot}max_runup>`__
- `strategy.closedtrades.max_drawdown() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}max_drawdown>`__ and 
  `strategy.opentrades.max_drawdown() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}opentrades{dot}max_drawdown>`__
- `strategy.closedtrades.exit_price() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}exit_price>`__
- `strategy.closedtrades.exit_bar_index() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}exit_bar_index>`__
- `strategy.closedtrades.exit_time() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}closedtrades{dot}exit_time>`__
- `strategy.convert_to_account() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}convert_to_account>`__
- `strategy.convert_to_symbol() <https://www.tradingview.com/pine-script-reference/v5/#fun_strategy{dot}convert_to_symbol>`__
- `strategy.account_currency <https://www.tradingview.com/pine-script-reference/v5/#var_strategy{dot}account_currency>`__

A new `earnings.standardized <https://www.tradingview.com/pine-script-reference/v5/#var_earnings{dot}standardized>`__ constant for the 
`request.earnings() <https://www.tradingview.com/pine-script-reference/v5/#fun_request{dot}earnings>`__ function allows requesting standardized earnings data.

A v4 to v5 converter is now included in the Pine Editor. 
See the :ref:`Migration guide to Pine v5 <PageToPineVersion5>` for more information on converting your scripts to v5.

The `Reference Manual <https://www.tradingview.com/pine-script-reference/v5/>`__ 
now includes the systematic mention of the form and type (e.g., "simple int") required for each function parameter.

The :ref:`User Manual <IndexUserManual>` was reorganized and new content was added.

Changes
^^^^^^^
Many built-in variables, functions and function arguments were renamed or moved to new namespaces in v5. 
The venerable ``study()``, for example, is now `indicator() <https://www.tradingview.com/pine-script-reference/v5/#fun_indicator>`__,
and ``security()`` is now `request.security() <https://www.tradingview.com/pine-script-reference/v5/#fun_request{dot}security>`__.
New namespaces now group related functions and variables together.
This consolidation implements a more rational nomenclature and provides an orderly space to accommodate the many additions planned for Pine. 

See the :ref:`Migration guide to Pine v5 <PageToPineVersion5>` for a complete list of the **changes** made in v5.

.. _PageReleaseNotes_September2021:

September 2021
---------
New parameter has been added for the ``dividends()``, ``earnings()``, ``financial()``, ``quandl()``, ``security()``, and ``splits()`` functions:

* ``ignore_invalid_symbol`` - determines the behavior of the function if the specified symbol is not found: if ``false``, the script will halt and return a runtime error; if ``true``, the function will return ``na`` and execution will continue.


July 2021
---------
``tostring`` now accepts "bool" and "string" types.

New argument for ``time`` and ``time_close`` functions was added:

* ``timezone`` - timezone of the ``session`` argument, can only be used when a session is specified. Can be written out in GMT notation (e.g. "GMT-5") or as an `IANA time zone database name <https://en.wikipedia.org/wiki/List_of_tz_database_time_zones>`__ (e.g. "America/New_York").

It is now possible to place a drawing object in the future with ``xloc = xloc.bar_index``.

New argument for ``study`` and ``strategy`` functions was added:

* ``explicit_plot_zorder`` - specifies the order in which the indicator's plots, fills, and hlines are rendered. If true, the plots will be drawn based on the order in which they appear in the indicator's code, each newer plot being drawn above the previous ones.


June 2021
---------
New variable was added:

* ``barstate.islastconfirmedhistory`` - returns ``true`` if script is executing on the dataset's last bar when market is closed, or script is executing on the bar immediately preceding the real-time bar, if market is open. Returns ``false`` otherwise.

New function was added:

* ``round_to_mintick(x)`` - returns the value rounded to the symbol's mintick, i.e. the nearest value that can be divided by ``syminfo.mintick``, without the remainder, with ties rounding up.

Expanded ``tostring()`` functionality. The function now accepts three new formatting arguments:

* ``format.mintick`` to format to tick precision.
* ``format.volume`` to abbreviate large values.
* ``format.percent`` to format percentages.


May 2021
--------
Improved backtesting functionality by adding the Leverage mechanism.

Added support for table drawings and functions for working with them. 
Tables are unique objects that are not anchored to specific bars; they float in a script’s space, independently of the chart bars being viewed or the zoom factor used. 
For more information, see the :ref:`Tables <PageTables>` User Manual page.

New functions were added:

* ``color.rgb(red, green, blue, transp)`` - creates a new color with transparency using the RGB color model.
* ``color.from_gradient(value, bottom_value, top_value, bottom_color, top_color)`` - returns color calculated from the linear gradient between bottom_color to top_color.
* ``color.r(color)``, ``color.g(color)``, ``color.b(color)``, ``color.t(color)`` - retrieves the value of one of the color components.
* ``array.from()`` - takes a variable number of arguments with one of the types: ``int``, ``float``, ``bool``, ``string``, ``label``, ``line``, ``color``, ``box``, ``table`` and returns an array of the corresponding type. 

A new ``box`` drawing has been added to Pine, making it possible to draw rectangles on charts using the Pine syntax. 
For more details see the `Pine Script reference <https://www.tradingview.com/pine-script-reference/v5/#fun_box{dot}new>`_ and the :ref:`Lines and boxes <PageLinesAndBoxes>` User Manual page.

The ``color.new`` function can now accept series and input arguments, in which case, the colors will be calculated at runtime. For more information about this, see our :ref:`Colors <PageColors>` User Manual page.



April 2021
----------
New math constants were added: 

* ``math.pi`` - is a named constant for Archimedes' constant. It is equal to 3.1415926535897932.
* ``math.phi`` - is a named constant for the golden ratio. It is equal to  1.6180339887498948.
* ``math.rphi`` - is a named constant for the golden ratio conjugate. It is equal to 0.6180339887498948.
* ``math.e`` - is a named constant for Euler's number. It is equal to 2.7182818284590452.

New math functions were added: 

* ``round(x, precision)`` - returns the value of ``x`` rounded to the nearest integer, with ties rounding up. If the precision parameter is used, returns a float value rounded to that number of decimal places.
* ``median(source, length)`` - returns the median of the series.
* ``mode(source, length)`` - returns the mode of the series. If there are several values with the same frequency, it returns the smallest value.
* ``range(source, length)`` - returns the difference between the ``min`` and ``max`` values in a series.
* ``todegrees(radians)`` - returns an approximately equivalent angle in degrees from an angle measured in radians.
* ``toradians(degrees)`` - returns an approximately equivalent angle in radians from an angle measured in degrees.
* ``random(min, max, seed)`` - returns a pseudo-random value. The function will generate a different sequence of values for each script execution. Using the same value for the optional seed argument will produce a repeatable sequence.

New functions were added:

* ``session.ismarket`` - returns ``true`` if the current bar is a part of the regular trading hours (i.e. market hours), ``false`` otherwise.
* ``session.ispremarket`` - returns ``true`` if the current bar is a part of the pre-market, ``false`` otherwise.
* ``session.ispostmarket`` - returns ``true`` if the current bar is a part of the post-market, ``false`` otherwise.
* ``str.format``  - converts the values to strings based on the specified formats. Accepts certain ``number`` modifiers: ``integer``, ``currency``, ``percent``.



March 2021
----------
New assignment operators were added:

* ``+=``  - addition assignment
* ``-=``  - subtraction assignment
* ``*=``  - multiplication assignment
* ``/=``  - division assignment
* ``%=``  - modulus assignment

New parameters for inputs customization were added:

* ``inline`` - combines all the input calls with the same inline value in one line.
* ``group`` - creates a header above all inputs that use the same group string value. The string is also used as the header text.
* ``tooltip`` - adds a tooltip icon to the ``Inputs`` menu. The tooltip string is shown when hovering over the tooltip icon.

New argument for ``fill`` function was added:

* ``fillgaps`` - controls whether fills continue on gaps when one of the ``plot`` calls returns an ``na`` value. 

A new keyword was added:

* ``varip`` - is similar to the ``var`` keyword, but variables declared with ``varip`` retain their values between the updates of a real-time bar.

New functions were added:

* ``tonumber()`` - converts a string value into a float.
* ``time_close()`` - returns the UNIX timestamp of the close of the current bar, based on the resolution and session that is passed to the function.
* ``dividends()`` - requests dividends data for the specified symbol.
* ``earnings()`` - requests earnings data for the specified symbol.
* ``splits()`` - requests splits data for the specified symbol.

New arguments for the study() function were added:

* ``resolution_gaps`` - fills the gaps between values fetched from higher timeframes when using ``resolution``.
* ``format.percent`` - formats the script output values as a percentage.



February 2021
-------------
New variable was added:

* ``time_tradingday`` - the beginning time of the trading day the current bar belongs to.



January 2021
------------
The following functions now accept a series length parameter:

* `bb() <https://www.tradingview.com/pine-script-reference/v4/#fun_bb>`__
* `bbw() <https://www.tradingview.com/pine-script-reference/v4/#fun_bbw>`__
* `cci() <https://www.tradingview.com/pine-script-reference/v4/#fun_cci>`__
* `cmo() <https://www.tradingview.com/pine-script-reference/v4/#fun_cmo>`__
* `cog() <https://www.tradingview.com/pine-script-reference/v4/#fun_cog>`__
* `correlation() <https://www.tradingview.com/pine-script-reference/v4/#fun_correlation>`__
* `dev() <https://www.tradingview.com/pine-script-reference/v4/#fun_dev>`__
* `falling() <https://www.tradingview.com/pine-script-reference/v4/#fun_falling>`__
* `mfi() <https://www.tradingview.com/pine-script-reference/v4/#fun_mfi>`__
* `percentile_linear_interpolation() <https://www.tradingview.com/pine-script-reference/v4/#fun_percentile_linear_interpolation>`__
* `percentile_nearest_rank() <https://www.tradingview.com/pine-script-reference/v4/#fun_percentile_nearest_rank>`__
* `percentrank() <https://www.tradingview.com/pine-script-reference/v4/#fun_percentrank>`__
* `rising() <https://www.tradingview.com/pine-script-reference/v4/#fun_rising>`__
* `roc() <https://www.tradingview.com/pine-script-reference/v4/#fun_roc>`__
* `stdev() <https://www.tradingview.com/pine-script-reference/v4/#fun_stdev>`__
* `stoch() <https://www.tradingview.com/pine-script-reference/v4/#fun_stoch>`__
* `variance() <https://www.tradingview.com/pine-script-reference/v4/#fun_variance>`__
* `wpr() <https://www.tradingview.com/pine-script-reference/v4/#fun_wpr>`__

A new type of alerts was added - script alerts. More information can be found in our `Help Center <https://www.tradingview.com/chart/?solution=43000597494/>`__.



2020
----

December 2020
^^^^^^^^^^^^^

New array types were added:

* ``array.new_line()``
* ``array.new_label()``
* ``array.new_string()``

New functions were added:

* ``str.length()`` - returns number of chars in source string.
* ``array.join()`` - concatenates all of the elements in the array into a string and separates these elements with the specified separator.
* ``str.split()`` - splits a string at a given substring separator.

November 2020
^^^^^^^^^^^^^

* New ``max_labels_count`` and ``max_lines_count`` parameters were added to the study and strategy functions. Now you can manage the number of lines and labels by setting values for these parameters from 1 to 500.

New function was added:

* ``array.range()`` - return the difference between the min and max values in the array.

October 2020
^^^^^^^^^^^^

The behavior of ``rising()`` and ``falling()`` functions have changed. For example, ``rising(close,3)`` is now calculated as following::

    close[0] > close[1] and close[1] > close[2] and close[2] > close[3]
    
September 2020
^^^^^^^^^^^^^^

Added support for ``input.color`` to the ``input()`` function. Now you can provide script users with color selection through the script’s "Settings/Inputs" tab with the same color widget used throughout the TradingView user interface. Learn more about this feature in our `blog <https://www.tradingview.com/blog/en/create-color-inputs-in-pine-20751/>`__::

    //@version=4
    study("My Script", overlay = true)
    color c_labelColor = input(color.green, "Main Color", input.color)
    var l = label.new(bar_index, close, yloc = yloc.abovebar, text = "Colored label")
    label.set_x(l, bar_index)
    label.set_color(l, c_labelColor)
    
.. image:: images/ReleaseNotes-input_color.png

Added support for arrays and functions for working with them. You can now use the powerful new array feature to build custom datasets. See our `User Manual page on arrays <https://www.tradingview.com/pine-script-docs/en/v4/essential/Arrays.html>`__ and our `blog <https://www.tradingview.com/blog/en/arrays-are-now-available-in-pine-script-20052/>`__::

    //@version=4
    study("My Script")
    a = array.new_float(0)
    for i = 0 to 5
        array.push(a, close[i] - open[i])
    plot(array.get(a, 4))

The following functions now accept a series length parameter. Learn more about this feature in our `blog <https://www.tradingview.com/blog/en/pine-functions-support-dynamic-length-arguments-20554/>`__:

* `alma() <https://www.tradingview.com/pine-script-reference/v4/#fun_alma>`__
* `change() <https://www.tradingview.com/pine-script-reference/v4/#fun_change>`__
* `highest() <https://www.tradingview.com/pine-script-reference/v4/#fun_highest>`__
* `highestbars() <https://www.tradingview.com/pine-script-reference/v4/#fun_highestbars>`__
* `linreg() <https://www.tradingview.com/pine-script-reference/v4/#fun_linreg>`__
* `lowest() <https://www.tradingview.com/pine-script-reference/v4/#fun_lowest>`__
* `lowestbars() <https://www.tradingview.com/pine-script-reference/v4/#fun_lowestbars>`__
* `mom() <https://www.tradingview.com/pine-script-reference/v4/#fun_mom>`__
* `sma() <https://www.tradingview.com/pine-script-reference/v4/#fun_sma>`__
* `sum() <https://www.tradingview.com/pine-script-reference/v4/#fun_sum>`__
* `vwma() <https://www.tradingview.com/pine-script-reference/v4/#fun_vwma>`__
* `wma() <https://www.tradingview.com/pine-script-reference/v4/#fun_wma>`__

::

    //@version=4
    study("My Script", overlay = true)
    length = input(10, "Length", input.integer, minval = 1, maxval = 100)
    avgBar = avg(highestbars(length), lowestbars(length))
    float dynLen = nz(abs(avgBar) + 1, length)
    dynSma = sma(close, int(dynLen))
    plot(dynSma)

August 2020
^^^^^^^^^^^

* Optimized script compilation time. Scripts now compile 1.5 to 2 times faster.

July 2020
^^^^^^^^^

* Minor bug fixes and improvements.

June 2020
^^^^^^^^^

* New ``resolution`` parameter was added to the ``study`` function. Now you can add MTF functionality to scripts and decide the timeframe you want the indicator to run on. 

.. image:: images/ReleaseNotes-Mtf.png

Please note that you need to reapply the indicator in order for the `resolution` parameter to appear.

* The ``tooltip`` argument was added to the ``label.new`` function along with the ``label.set_tooltip`` function::

    //@version=4
    study("My Script", overlay=true)
    var l=label.new(bar_index, close, yloc=yloc.abovebar, text="Label")
    label.set_x(l,bar_index)
    label.set_tooltip(l, "Label Tooltip")
    
.. image:: images/ReleaseNotes-Tooltip.png

* Added an ability to create `alerts on strategies <https://www.tradingview.com/chart/?solution=43000481368>`__.

* A new function `line.get_price() <https://www.tradingview.com/pine-script-reference/v4/#fun_line{dot}get_price>`__ can be used to determine the price level at which the line is located on a certain bar.

* New `label styles <https://www.tradingview.com/pine-script-reference/v4/#fun_label{dot}new>`__ allow you to position the label pointer in any direction.

.. image:: images/ReleaseNotes-new_label_styles.png


* Find and Replace was added to Pine Editor. To use this, press CTRL+F (find) or CTRL+H (find and replace).

.. image:: images/ReleaseNotes-FindReplace.jpg

* ``timezone`` argument was added for time functions. Now you can specify timezone for ``second``, ``minute``, ``hour``, ``year``, ``month``, ``dayofmonth``, ``dayofweek`` functions::

    //@version=4
    study("My Script")
    plot(hour(1591012800000, "GMT+1"))

* ``syminfo.basecurrency`` variable was added. Returns the base currency code of the current symbol. For EURUSD symbol returns EUR.

May 2020
^^^^^^^^

* ``else if`` statement was added

* The behavior of ``security()`` function has changed: the ``expression`` parameter can be series or tuple.

April 2020
^^^^^^^^^^
New function was added:

* ``quandl()`` - request quandl data for a symbol


March 2020
^^^^^^^^^^

New function was added:

* ``financial()`` - request financial data for a symbol


New functions for common indicators were added:

* ``cmo()`` - Chande Momentum Oscillator
* ``mfi()`` - Money Flow Index
* ``bb()`` - Bollinger Bands
* ``bbw()`` - Bollinger Bands Width
* ``kc()`` - Keltner Channels
* ``kcw()`` - Keltner Channels Width 
* ``dmi()`` - DMI/ADX
* ``wpr()`` - Williams % R 
* ``hma()`` - Hull Moving Average
* ``supertrend()`` - SuperTrend


Added a detailed description of all the fields in the `Strategy Tester Report <https://www.tradingview.com/chart/?solution=43000561856/>`__


February 2020
^^^^^^^^^^^^^

* New Pine indicator VWAP Anchored was added. Now you can specify the time period: Session, Month, Week, Year.

* Fixed a problem with calculating ``percentrank`` function. Now it can return a zero value, which did not happen before due to an incorrect calculation.

* The default ``transparency`` parameter for the ``plot()``, ``plotshape()``, and ``plotchar()`` functions is now 0%.

* For the functions ``plot()``, ``plotshape()``, ``plotchar()``, ``plotbar()``, ``plotcandle()``, ``plotarrow()``, you can set the ``display`` parameter, which controls the display of the plot. The following values can be assigned to it:

  * ``display.none`` - the plot is not displayed
  * ``display.all`` - the plot is displayed (Default)

* The ``textalign`` argument was added to the ``label.new`` function along with the ``label.set_textalign`` function. Using those, you can control the alignment of the label's text::

    //@version=4
    study("My Script", overlay = true)
    var l = label.new(bar_index, high, text="Right\n aligned\n text", textalign=text.align_right)
    label.set_xy(l, bar_index, high)

  .. image:: images/ReleaseNotes-Label_text_align.png


January 2020
^^^^^^^^^^^^
  
New built-in variables were added:


* ``iii`` - Intraday Intensity Index
* ``wvad`` - Williams Variable Accumulation/Distribution
* ``wad`` - Williams Accumulation/Distribution
* ``obv`` - On Balance Volume
* ``pvt`` - Price-Volume Trend
* ``nvi`` - Negative Volume Index 
* ``pvi`` - Positive Volume Index
   
New parameters were added for ``strategy.close()``:


* ``qty`` -  the number of contracts/shares/lots/units to exit a trade with
* ``qty_percent`` - defines the percentage of entered contracts/shares/lots/units to exit a trade with
* ``comment`` - addtional notes on the order
    
New parameter was added for ``strategy.close_all``:


* ``comment`` - additional notes on the order



2019
----

December 2019
^^^^^^^^^^^^^
* Warning messages were added.

  For example, if you don't specify exit parameters for ``strategy.exit`` - ``profit``, ``limit``, ``loss``, ``stop`` or one of the following pairs: ``trail_offset`` and ``trail_price`` / ``trail_points`` - you will see a warning message in the console in the Pine editor.
* Increased the maximum number of arguments in ``max``, ``min``, ``avg`` functions. Now you can use up to ten arguments in these functions.  

October 2019
^^^^^^^^^^^^
* ``plotchar()`` function now supports most of the Unicode symbols::

    //@version=4
    study("My Script", overlay=true)
    plotchar(open > close, char="🐻")


  .. image:: images/ReleaseNotes-Bears_in_plotchar.png

* New ``bordercolor`` argument of the ``plotcandle()`` function allows you to change the color of candles' borders::

    //@version=4
    study("My Script")
    plotcandle(open, high, low, close, title='Title', color = open < close ? color.green : color.red, wickcolor=color.black, bordercolor=color.orange)

* New variables added:
  
  * ``syminfo.description`` - returns a description of the current symbol
  * ``syminfo.currency`` - returns the currency code of the current symbol (EUR, USD, etc.)
  * ``syminfo.type`` - returns the type of the current symbol (stock, futures, index, etc.)

September 2019
^^^^^^^^^^^^^^


New parameters to the ``strategy`` function were added:

* ``process_orders_on_close`` allows the broker emulator to try to execute orders after calculating the strategy at the bar's close

* ``close_entries_rule`` allows to define the sequence used for closing positions

Some fixes were made:

* ``fill()`` function now works correctly with ``na`` as the ``color`` parameter value

* ``sign()`` function now calculates correctly for literals and constants

``str.replace_all(source, target, replacement)`` function was added. It replaces each occurrence of a ``target`` string in the ``source`` string with a ``replacement`` string

July-August 2019
^^^^^^^^^^^^^^^^


New variables added: 


* ``timeframe.isseconds`` returns true when current resolution is in seconds
    
* ``timeframe.isminutes`` returns true when current resolution is in minutes
    
* ``time_close`` returns the current bar's close time 

The behavior of some functions, variables and operators has changed:

* The ``time`` variable returns the correct open time of the bar for more special cases than before

* An optional *seconds* parameter of the ``timestamp()`` function allows you to set the time to within seconds 

* ``security()`` function:
  
  * Added the possibility of requesting resolutions in seconds:

    1, 5, 15, 30 seconds (chart resolution should be less than or equal to the requested resolution)
    
  * Reduced the maximum value that can be requested in some of the other resolutions:
    
    from 1 to 1440 minutes
    
    from 1 to 365 days  
    
    from 1 to 52 weeks
    
    from 1 to 12 months



* Changes to the evaluation of ternary operator branches:

  In Pine v3, during the execution of a ternary operator, both its branches are calculated, so when this script is added to the chart, a long position is opened, even if the long() function is not called::

    //@version=3
    strategy(title = "My Strategy")
    long() =>
        strategy.entry("long", true, 1, when = open > high[1])
        1
    c = 0
    c := true ? 1 : long()
    plot(c)
    
  Pine v4 contains built-in functions with side effects ( ``line.new`` and ``label.new`` ). If calls to these functions are present in both branches of a ternary operator, both function calls would be executed following v3 conventions. Thus, in Pine v4, only the branch corresponding to the evaluated condition is calculated. While this provides a viable solution in some cases, it will modify the behavior of scripts which depended on the fact that both branches of a ternary were evaluated. The solution is to pre-evaluate expressions prior to the ternary operator. The conversion utility takes this requirement into account when converting scripts from v3 to v4, so that script behavior will be identical in v3 and v4.




June 2019
^^^^^^^^^

* Support for drawing objects. Added *label* and *line* drawings
* ``var`` keyword for one time variable initialization
* Type system improvements:

  * *series string* data type
  * functions for explicit type casting
  * syntax for explicit variable type declaration
  * new *input* type forms

* Renaming of built-ins and a version 3 to 4 converter utility
* ``max_bars_back`` function to control series variables internal history buffer sizes
* Pine Script documentation versioning



2018
----

October 2018
^^^^^^^^^^^^
* To increase the number of indicators available to the whole community, Invite-Only scripts can now be published by Premium users only.

April 2018
^^^^^^^^^^
* Improved the Strategy Tester by reworking the Maximum Drawdown calculation formula.


2017
----

August 2017
^^^^^^^^^^^
* With the new argument ``show_last`` in the plot-type functions, you can restrict the number of bars that the plot is displayed on.

June 2017
^^^^^^^^^
* A major script publishing improvement: it is now possible to update your script without publishing a new one via the Update button in the publishing dialog.

May 2017
^^^^^^^^
* Expanded the type system by adding a new type of constants that can be calculated during compilation.

April 2017
^^^^^^^^^^
* Expanded the keyword argument functionality: it is now possible to use keyword arguments in all built-in functions.
* A new ``barstate.isconfirmed`` variable has been added to the list of variables that return bar status. It lets you create indicators that are calculated based on the closed bars only.
* The ``options`` argument for the ``input()`` function creates an input with a set of options defined by the script's author.

March 2017
^^^^^^^^^^
* Pine Script v3 is here! Some important changes:
  
  * Changes to the default behavior of the ``security()`` function: it can no longer access the future data by default. This can be changes with the ``lookahead`` parameter.
  * An implicit conversion of boolean values to numeric values was replaced with an implicit conversion of numeric values (integer and float) to boolean values.
  * Self-referenced and forward-referenced variables were removed. Any PineScript code that used those language constructions can be equivalently rewritten using mutable variables.


February 2017
^^^^^^^^^^^^^
* Several improvements to the strategy tester and the strategy report:

  * New Buy & Hold equity graph – a new graph that lets you compare performance of your strategy versus a "buy and hold", i.e if you just bought a security and held onto it without trading.
  * Added percentage values to the absolute currency values.
  * Added Buy & Hold Return to display the final value of Buy & Hold Equity based on last price.
  * Added Sharpe Ratio – it shows the relative effectiveness of the investment portfolio (security), a measure that indicates the average return minus the risk-free return divided by the standard deviation of return on an investment.
  * Slippage lets you simulate a situation when orders are filled at a worse price than expected. It can be set through the Properties dialog or through the slippage argument in the ``strategy()`` function.
  * Commission allows yot to add commission for placed orders in percent of order value, fixed price or per contract. The amount of commission paid is shown in the Commission Paid field. The commission size and its type can be set through the Properties dialog or through the commission_type and commission_value arguments in the ``strategy()`` function.



2016
----

December 2016
^^^^^^^^^^^^^
* Added invite-only scripts. The invite-only indicators are visible in the Community Scripts, but nobody can use them without explicit permission from the author, and only the author can see the source code.

October 2016
^^^^^^^^^^^^
* Introduded indicator revisions. Each time an indicator is saved, it gets a new revision, and it is possible to easily switch to any past revision from the Pine Editor.

September 2016
^^^^^^^^^^^^^^
* It is now possible to publish indicators with protected source code. These indicators are available in the public Script Library, and any user can use them, but only the author can see the source code.

July 2016
^^^^^^^^^
* Improved the behavior of the ``fill()`` function: one call can now support several different colors.

March 2016
^^^^^^^^^^
* Color type variables now have an additional parameter to set default transparency. The transparency can be set with the ``color.new()`` function, or by adding an alpha-channel value to a hex color code.

February 2016
^^^^^^^^^^^^^
* Added ``for`` loops and keywords ``break`` and ``continue``.
* Pine now supports mutable variables! Use the ``:=`` operator to assign a new value to a variable that has already been defined.
* Multiple improvements and bug fixes for strategies.

January 2016
^^^^^^^^^^^^
* A new ``alertcondition()`` function allows for creating custom alert conditions in Pine-based indicators.



2015
----

October 2015
^^^^^^^^^^^^
* Pine has graduated to v2! The new version of Pine Script added support for ``if`` statements, making it easier to write more readable and concise code.

September 2015
^^^^^^^^^^^^^^
* Added backtesting functionality to Pine. It is now possible to create trading strategies, i.e. scripts that can send, modify and cancel orders to buy or sell. Strategies allow you to perform backtesting (emulation of strategy trading on historical data) and forward testing (emulation of strategy trading on real-time data) according to your algorithms. Detailed information about the strategy's calculations and the order fills can be seen in the newly added Strategy Tester tab.

July 2015
^^^^^^^^^
* A new ``editable`` parameter allows hiding the plot from the Style menu in the indicator settings so that it is not possible to edit its style. The parameter has been added to all the following functions: all plot-type functions, ``barcolor()``, ``bgcolor()``, ``hline()``, and ``fill()``.

June 2015
^^^^^^^^^
* Added two new functions to display custom barsets using PineScipt: ``plotbar()`` and ``plotcandle()``.

April 2015
^^^^^^^^^^
* Added two new shapes to the ``plotshape()`` function: shape.labelup and shape.labeldown.
* PineScipt Editor has been improved and moved to a new panel at the bottom of the page.
* Added a new ``step`` argument for the ``input()`` function, allowing to specify the step size for the indicator's inputs.

March 2015
^^^^^^^^^^
* Added support for inputs with the ``source`` type to the ``input()`` function, allowing to select the data source for the indicator's calculations from its settings.

February 2015
^^^^^^^^^^^^^
* Added a new ``text`` argument to ``plotshape()`` and ``plotchar()`` functions.
* Added four new shapes to the ``plotshape()`` function: shape.arrowup, shape.arrowdown, shape.square, shape.diamond.



2014
----

August 2014
^^^^^^^^^^^
* Improved the script sharing capabilities, changed the layout of the Indicators menu and separated published scripts from ideas.

July 2014
^^^^^^^^^
* Added three new plotting functions, ``plotshape()``, ``plotchar()``, and ``plotarrow()`` for situations when you need to highlight specific bars on a chart without drawing a line.
* Integrated QUANDL data into Pine Script. The data can be accessed by passing the QUANDL ticker to the ``security`` function.

June 2014
^^^^^^^^^

* Added Pine Script sharing, enabling coders and traders to share their scripts with the rest of the TradingView community.

April 2014
^^^^^^^^^^

* Added line wrapping.

February 2014
^^^^^^^^^^^^^

* Added support for inputs, allowing users to edit the indicator inputs through the properties window, without needing to edit the Pine script.
* Added self-referencing variables.
* Added support for multiline functions.
* Implemented the type-casting mechanism, automatically casting constant and simple float and int values to series when it is required.
* Added several new functions and improved the existing ones: 

  * ``barssince()`` and ``valuewhen()`` allow you to check conditions on historical data easier.
  * The new ``barcolor()`` function lets you specify a color for a bar based on filling of a certain condition.
  * Similar to the ``barcolor()`` function, the ``bgcolor()`` function changes the color of the background.
  * Reworked the ``security()`` function, further expanding its functionality.
  * Improved the ``fill()`` function, enabling it to be used more than once in one script.
  * Added the ``round()`` function to round and convert float values to integers.



2013
----

* The first version of Pine is introduced to all TradingView users, initially as an open beta, on December 13th.
