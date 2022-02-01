.. _chart:


Charts
======

The chart api endpoint provides you with a small set of predefined charts.


URL
---

``https://vidicenter.quividi.com/api/v1/chart/``

Mandatory arguments
-------------------

* ``chart_type``: the type of chart you want to generate. Allowed values:

    * ``0``: test chart.
    * ``1``: total impressions and watchers for the period.
    * ``2``: average attention time and presence time.
    * ``3``: breakdown of watchers per demographics.
    * ``4``: impressions and watchers per day.
    * ``5``: average attention time and presence time per day.

* ``chart_period``: period of the chart you want to generate. Allowed values:

    * ``day``: previous full day.
    * ``week``: past week.
    * ``month``: past month.

* ``chart_format``: type of response expected. Allowed values:

    * ``image``: a binary image file in PNG format.
    * ``json``: raw numbers representing the chart in JSON format, useful to generate your own charts.

* ``network_id``: ID of the network for which the chart should be generated.


Response's form
---------------

This api endpoint returns a binary image file in PNG format.


curl examples
-------------

Here is an examples on how to make calls against the chart API.

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/chart/?chart_type=1&chart_period=day&chart_format=image&network_id=123'
