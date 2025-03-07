.. _PageNextSteps:

Next steps
==========

.. contents:: :local:
    :depth: 3

After your :ref:`first steps <PageFirstSteps>` and your :ref:`first indicator <PageFirstIndicator>`, 
let us explore a bit more of the Pine landscape by sharing some pointers to guide you in your journey to learn Pine.



"indicators" vs "strategies"
----------------------------

Pine :ref:`strategies <PageStrategies>` are used to backtest on historical data and forward test on open markets. 
In addition to indicator calculations, they contain ``strategy.*()`` calls to send trade orders to Pine's broker emulator, which can then simulate their execution.
Strategies display backtest results in the "Strategy Tester" tab at the bottom of the chart, next to the "Pine Editor" tab.

Pine indicators also contain calculations, but cannot be used in backtesting. 
Because they do not require the broker emulator, they use less resources and will run faster.
It is thus advantageous to use indicators whenever you can.

Both indicators and strategies can run in either overlay mode (over the chart's bars) or pane mode 
(in a separate section below or above the chart). Both can also plot information in their respective space, 
and both can generate :ref:`alert events <PageAlerts>`.



How scripts are executed
------------------------

A Pine script is **not** like programs in many programming languages that execute once and then stop. 
In the Pine *runtime* environment, a script runs in the equivalent of an invisible loop 
where it is executed once on each bar of whatever chart you are on, from left to right. 
Chart bars that have already closed when the script executes on them are called *historical bars*. 
When execution reaches the chart's last bar and the market is open, it is on the *realtime bar*. 
The script then executes once every time a price or volume change is detected, and one last time for that realtime bar when it closes. 
That realtime bar then becomes an *elapsed realtime bar*. Note that when the script executes in realtime, 
it does not recalculate on all the chart's historical bars on every price/volume update. 
It has already calculated once on those bars, so it does not need to recalculate them on every chart tick. See the :ref:`Execution model <PageExecutionModel>` page for more information.

When a script executes on a historical bar, the `close <https://www.tradingview.com/pine-script-reference/v5/#var_close>`__ 
built-in variable holds the value of that bar's close.
When a script executes on the realtime bar, `close <https://www.tradingview.com/pine-script-reference/v5/#var_close>`__
returns the **current** price of the symbol until the bar closes.

Contrary to indicators, Pine strategies normally execute only once on realtime bars, when they close.
They can also be configured to execute on each price/volume update if that is what you need. 
See the page on :ref:`Strategies <PageStrategies>` for more information,
and to understand how strategies calculate differently than indicators.



Time series
-----------

The main data structure used in Pine scripts is called a :ref:`time series <PageTimeSeries>`. Time series contain one value for each bar the script executes on, 
so they continuously expand as the script executes on more bars. Past values of the time series can be referenced using Pine's history-referencing operator: 
`[] <https://www.tradingview.com/pine-script-reference/v5/#op_[]>`__. ``close[1]``, for example, 
refers to the value of `close <https://www.tradingview.com/pine-script-reference/v5/#var_close>`__ on the bar preceding the one where the script is executing.

While this indexing mechanism may remind many programmers of arrays, 
a time series is different and thinking in terms of arrays will be detrimental to understanding this key Pine concept. 
A good comprehension of both the :ref:`execution model <PageExecutionModel>` 
and :ref:`time series <PageTimeSeries>` is essential in understanding how Pine scripts work. 
If you have never worked with data organized in time series before, you will need practice to put them to work for you. 
Once you familiarize yourself with these key concepts, 
you will discover that by combining the use of time series with our built-in functions specifically designed to handle them efficiently, 
much can be accomplished in very few lines of Pine code.



Publishing scripts
------------------

TradingView is home to a large community of Pine coders and millions of traders from all around the world. Once you become proficient enough in Pine, 
you can choose to share your scripts with other traders. Before doing so, please take the time to learn Pine well-enough to supply traders with an original and reliable tool.
All publicly published scripts are analyzed by our team of moderators and must comply with our `Script Publishing Rules <https://www.tradingview.com/house-rules/?solution=43000590599>`__, 
which require them to be original and well-documented.

If want to use Pine scripts for your own use, simply write them in the Pine Editor and add them to your chart from there; 
you don't have to publish them to use them. If you want to share your scripts with just a few friends, 
you can publish them privately and send your friends the browser's link to your private publication. 
See the page on :ref:`Publishing <PagePublishing>` for more information.



Getting around the Pine documentation
-------------------------------------

While reading code from published scripts is no doubt useful, spending time in our documentation will be necessary to attain any degree of proficiency in Pine.
Our two main sources of documentation on Pine are:

- This `Pine User Manual <https://www.tradingview.com/pine-script-docs/en/v5/index.html>`__
- Our `Pine Reference Manual <https://www.tradingview.com/pine-script-reference/v5/>`__

The `Pine User Manual <https://www.tradingview.com/pine-script-docs/en/v5/index.html>`__ is in HTML format and in English only.

The `Pine Reference Manual <https://www.tradingview.com/pine-script-reference/v5/>`__ documents what each variable, function or Pine keyword does.
It is an essential tool for all Pine programmers; your life will be miserable if you try to write scripts of any reasonable complexity without consulting it.
It exists in two formats: the HTML format we just linked to, 
and the popup version, which can be accessed from the Pine Editor, by either :kbd:`ctrl` + :kbd:`clicking` on a keyword, 
or by using the Editor's "More/Pine Script reference (pop-up)" menu. The Reference Manual is translated in other languages.


There are five different versions of Pine. Ensure the documentation you use corresponds to the Pine version you are coding with.



Where to go from here?
----------------------

This `Pine User Manual <https://www.tradingview.com/pine-script-docs/en/v5/index.html>`__ contains numerous examples of code used to illustrate the concepts we discuss.
By going through it, you will be able to both learn the foundations of Pine and study the example scripts. 
Reading about key concepts and trying them out right away with real code is a productive way to learn any programming language.
As you hopefully have already done in the :ref:`First indicator <PageFirstIndicator>` page, copy this documentation’s examples in the Editor and play with them. Explore! You won’t break anything.

This is how the `Pine User Manual <https://www.tradingview.com/pine-script-docs/en/v5/index.html>`__ you are reading is organized:

- The :ref:`Language <IndexLanguage>` section explains the main components of the Pine language and how scripts execute.
- The :ref:`Concepts <IndexConcepts>` section is more task-oriented. It explains how to do things in Pine.
- The :ref:`Writing <IndexWriting>` section explores tools and tricks that will help you write and publish scripts.
- The :ref:`FAQ <PageFaq>` section answers common questions from Pine coders.
- The :ref:`Error messages <PageErrorMessages>` page documents causes and fixes for the most common runtime and compiler errors.
- The :ref:`Release Notes <PageReleaseNotes>` page is where you can follow the frequent updates to the Pine.
- The :ref:`Migration guides <IndexMigrationGuides>` section explains how to port between different versions of Pine.
- The :ref:`Where can I get more information <PageWhereCanIGetMoreInformation>` page lists other useful Pine-related content, including where to ask questions when you are stuck on code.

We wish you a successful journey with Pine... and trading!
