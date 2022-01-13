.. _chart:


Charts
======

The chart api endpoint provides you with a small set of predefined charts.


URL
---

``https://vidicenter.quividi.com/api/v1/chart/``

Mandatory arguments
-------------------

* ``chart_type``: The type of chart you want to generate. Allowed values:

    * ``0``: Test chart.
    * ``1``: Total Impressions and viewers for the period.

* ``chart_period``: The period of the chart you want to generate. Allowed values:

    * ``day``: The previous full day.
    * ``week``: The past week.
    * ``month``: The past month.

* ``network_id``: The id of the network for which the chart should be generated.


Response's form
---------------

This api endpoint returns a binary image file in PNG format.


curl examples
-------------

Here is an examples on how to make calls against the chart API.

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/chart/?chart_type=1&chart_period=day&network_id=123'
