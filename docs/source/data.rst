.. _data:


Data export
===========

The data api endpoint provides you with raw data from your networks using the JSON format. This is the same data you can export in CSV format using the VidiCenter website.


URL
---

``https://vidicenter.quividi.com/api/v1/data/``

Mandatory arguments
-------------------

* ``data_type``: The type of data you want to extract. Allowed values:

    * ``ots``: OTS data
    * ``gate``: Gate data
    * ``viewers``: Viewers data

* ``time_resolution``: The time resolution used in the aggregation. Allowed values:

    * ``finest``: Do not aggregate, display all raw objects (unavailable for OTS and Gate exports).
    * ``5m``: 5 minutes aggregates
    * ``10m``: 10 minutes aggregates
    * ``15m``: 15 minutes aggregates
    * ``30m``: 30 minutes aggregates
    * ``1h``: 1 hour aggregates
    * ``1d``: 1 day aggregates
    * ``7d``: 7 days aggregates
    * ``30d``: 30 days aggregates

* ``start``: The start time for the export. Expected format is ``YYYY-MM-DDTHH:mm:SS``. For example, 2016 August 25 at 8:05PM would be ``2016-08-25T20:05:00``.
* ``end``: The end time for the export. Expected format is ``YYYY-MM-DDTHH:mm:SS``.

Sources
-------

The api endpoint expects you to specify at least one of the following values:

* ``locations``: A comma separated list of location ids.
* ``sites``: A comma separated list of site ids.
* ``locationgroups``: A comma separated list of location group ids.
* ``sitegroups``: A comma separated list of site group ids.
* ``location_tags``: A comma separated list of location tag ids.
* ``site_tags``: A comma separated list of site tag ids.

Optional arguments
------------------

* ``group_by_demographics``: If specified, will group the aggregate by demographics for each gender and each age group. Only works with viewers export.
* ``force_rebuild``: Force the reconstruction of the export. See `Asynchronous usage`_. Note that this parameter is active as soon as it is given in the query. Hence both ``force_rebuild=1`` and ``force_rebuild=true`` would force a rebuild, but ``force_rebuild=false`` also does.

Response's form
---------------

This api endpoint returns a JSON dictionary. It will always contain the ``state`` key:

* ``started``: your export just started.
* ``in_progress``: we are processing your export and the data is not ready just yet.
* ``finished``: the export is done, your data is available.
* ``failed``: an error occurred with your export.

The dictionary may also contain the following keys:

* ``fail_reason``: in case of failure, may help you understand what happened.
* ``creation_date``: the creation date of your export.
* ``data``: the actual export data.

The data can contain the following keys:

* ``location_id``: the ID of the location the data comes from.
* ``gate_id``: for Gate exports, the ID of the gate the data comes from.
* ``period_start``: the start of the aggregate or the event (in finest mode).
* ``ots_count``: for OTS exports, the number of OTS in the current aggregate.
* ``in_count``: for Gate exports, the number of people who entered the gate.
* ``out_count``: for Gate exports, the number of people who exited the gate.
* ``duration``: for OTS and Gate exports, the duration of the OTS or Gate event in seconds.
* ``watcher_count``: the number of watcher in the current aggregate.
* ``conversion_ratio``: the number of watcher divided by the number of OTS in the current aggregate.
* ``gender``: the gender for the current aggregate. Possible values:

    * ``0``: unknown
    * ``1``: male
    * ``2``: female

* ``age``: the age for the current aggregate. Possible values:

    * ``0``: unknown
    * ``1``: child
    * ``2``: young adult
    * ``3``: adult
    * ``4``: senior

* ``glasses``: viewer's glasses information (expert only):

    * ``0``: unknown
    * ``1``: no glasses
    * ``2``: glasses
    * ``3``: sunglasses

* ``mustache``: viewer's mustache information (expert only):

    * ``0``: unknown
    * ``1``: no mustache
    * ``2``: mustache

* ``beard``: viewer's beard information (expert only):

    * ``0``: unknown
    * ``1``: no beard
    * ``2``: beard

* ``age_value``: the viewer's numeric age in years (expert only).

* ``dwell_time``: the dwell time for the current aggregate in **tenth of seconds**.
* ``attention_time``: the attention time for the current aggregate in **tenth of seconds**.
* Mood values (expert only) are given in percentage, they represent the distribution of a viewer's mood over time. The sum of the five moods totals 100. Each mood is a key:

    * ``very_happy``
    * ``happy``
    * ``neutral``
    * ``unhappy``
    * ``very_unhappy``

Asynchronous usage
------------------

The export data endpoint is asynchronous. The first time you make a specific request, VidiCenter will start working on your export. When you make the same request again, the response will let you know if the export is done or not in the ``status`` field.

Export are cached for around 24 hours. If you want to ignore the cache and force VidiCenter to build a new export, you can use the ``force_rebuild`` parameter.

Rate limiting
-------------

Exports can be intensive on our servers so we limit the number of exports one user can start in parallel. **You cannot start more than 3 exports in parallel**.

Placeholder data and null values
--------------------------------

The API will try to fill "missing" lines with placeholder values. Let's say you ask for the OTS data day by day for a location, on a two-day period. The data returned may look like this::

    [
        {
            "duration": 86400.0,
            "location_id": 1234,
            "ots_count": 504,
            "watcher_count": 156,
            "period_start": '2016-04-29 00:00:00'
        },
        {
            "duration": null,
            "location_id": 1234,
            "ots_count": null,
            "watcher_count": null,
            "period_start": '2016-04-30 00:00:00'
        }
    ]

The first line looks normal. The second line has ``null`` values for the three metrics `duration`, `ots_count` and `watcher_count`. This means that we don't have any data for the concerned period. Rather than omitting the line from the results, we add a placeholder line with ``null`` values.

Example
-------

First call starts the export
****************************

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1056&data_type=viewers&start=2016-04-29T10:00:00&end=2016-04-29T11:00:00&time_resolution=1h'
    {
        "state": "started",
    }

We immediately make the same call
*********************************

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1056&data_type=viewers&start=2016-04-29T10:00:00&end=2016-04-29T11:00:00&time_resolution=1h'
    {
        "state": "in_progress",
        "creation_date": "2016-08-25 15:22:35"
    }

Some time later, the same call returns the data
***********************************************

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1056&data_type=viewers&start=2016-04-29T10:00:00&end=2016-04-29T11:00:00&time_resolution=1h'
    {
        "state": "finished",
        "data": [
            {
                "attention_time": 556,
                "watcher_count": 27,
                "dwell_time": 2419,
                "location_id": 1056,
                "period_start": "2016-04-29 10:00:00"
            },
            {
                "attention_time": 0,
                "watcher_count": 0,
                "dwell_time": 0,
                "location_id": 1056,
                "period_start": "2016-04-29 11:00:00"
            }
        ],
        "creation_date": "2016-08-25 15:22:35"
    }

We may ask for VidiCenter to rebuild the exports, to take into accounts recent uploads for example
**************************************************************************************************

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1056&data_type=viewers&start=2016-04-29T10:00:00&end=2016-04-29T11:00:00&time_resolution=1h&force_rebuild=1'
    {
        "state": "started",
    }


Example of expert return values
*******************************

 ::

    {
        "state": "finished",
        "data": [
            {
                "happy": 20,
                "dwell_time": 11,
                "gender": 1,
                "location_id": 22383,
                "unhappy": 0,
                "age": 4,
                "neutral": 80,
                "age_value": 86,
                "attention_time": 5,
                "period_start": "2016-07-25 00:11:26",
                "glasses": 1,
                "very_unhappy": 0,
                "very_happy": 0,
                "mustache": 1,
                "beard": 1
            },
            {
                "happy": 19.215686274509803,
                "dwell_time": 139,
                "gender": 1,
                "location_id": 22383,
                "unhappy": 8.235294117647058,
                "age": 3,
                "neutral": 69.80392156862746,
                "age_value": 39,
                "attention_time": 55,
                "period_start": "2016-07-25 00:46:52",
                "glasses": 1,
                "very_unhappy": 0,
                "very_happy": 2.7450980392156863,
                "mustache": 1,
                "beard": 1
            }
        ],
        "creation_date": "2016-08-25 17:10:32"
    }


Continue to :ref:`clip_metadata`
