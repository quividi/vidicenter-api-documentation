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

    * ``gate``: Gate data.
    * ``ots``: OTS data.
    * ``viewers``: Viewers data.
    * ``viewers_apc``: Viewers with content data (**BETA**). Will only contain viewers who have content data.
    * ``proof_of_play_by_location``: Proof of play data grouped by location (**BETA**). This requires APC data.
    * ``proof_of_play_by_site``: Proof of play data grouped by site (**BETA**).  This requires APC data.
    * ``extrapolated_watchers``: Extrapolated Watchers (**BETA**)
    * ``extrapolated_ots``: Extrapolated OTS (**BETA**)
    * ``vehicles``: Vehicles
    * ``footfall``: Footfall
    * ``im``: Impression multiplier (only available with time_resolution=1h)

* ``time_resolution``: The time resolution used in the aggregation. Allowed values:

    * ``finest``: Do not aggregate, display all raw objects (unavailable for OTS, Gate, Proof of play, Footfall and Vehicle exports).
    * ``5m``: 5 minutes aggregates.
    * ``10m``: 10 minutes aggregates.
    * ``15m``: 15 minutes aggregates.
    * ``30m``: 30 minutes aggregates.
    * ``1h``: 1 hour aggregates.
    * ``1d``: 1 day aggregates.
    * ``7d``: 7 days aggregates.
    * ``30d``: 30 days aggregates.
    * ``1M``: 1 month aggregates.

Note that the Viewers with content data export only support ``finest`` exports.

* ``start``: The start time for the export. Expected format is ``YYYY-MM-DDTHH:mm:SS``. For example, 2016 August 25 at 8:05PM would be ``2016-08-25T20:05:00``.
* ``end``: The end time for the export. Expected format is ``YYYY-MM-DDTHH:mm:SS``.

Sources
-------

The api endpoint expects you to specify at least one of the following values:

* ``locations``: A comma separated list of location ids.
* ``sites``: A comma separated list of site ids.
* ``location_tags``: A comma separated list of location tag ids.
* ``site_tags``: A comma separated list of site tag ids.

Optional arguments
------------------

* ``group_by_demographics``: If specified, will group the aggregate by demographics for each gender and each age group. Only works with aggregated viewers export.
* ``force_rebuild``: Force the reconstruction of the export. See `Asynchronous usage`_. Note that this parameter is active as soon as it is given in the query. Hence both ``force_rebuild=1`` and ``force_rebuild=true`` would force a rebuild, but ``force_rebuild=false`` also does.
* ``content_ids``: If specified, will filter the results to show only the relevant content id's. Optional argument, only works with viewers_apc and the proof of play exports. Specify one or more content id's separated by a comma.

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

Asynchronous usage
------------------

The export data endpoint is asynchronous. The first time you make a specific request, VidiCenter will start working on your export. When you make the same request again, the response will let you know if the export is done or not in the ``status`` field.

Export are cached for around 24 hours. If you want to ignore the cache and force VidiCenter to build a new export, you can use the ``force_rebuild`` parameter.

Rate limiting
-------------

Exports can be intensive on our servers so we limit the number of exports one user can start in parallel. **You cannot start more than 3 exports in parallel**.

Ordering
--------

At this time it is not possible to ask for a specific ordering of the data. The data will be served as is, with no pre-defined ordering.

curl examples
-------------

Here are some examples on how to make calls against the data export API.

First call starts the export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1056&data_type=viewers&start=2016-04-29T10:00:00&end=2016-04-29T11:00:00&time_resolution=1h'
    {
        "state": "started",
    }

We immediately make the same call
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1056&data_type=viewers&start=2016-04-29T10:00:00&end=2016-04-29T11:00:00&time_resolution=1h'
    {
        "state": "in_progress",
        "creation_date": "2016-08-25 15:22:35"
    }

Some time later, the same call returns the data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1056&data_type=viewers&start=2016-04-29T10:00:00&end=2016-04-29T11:00:00&time_resolution=1h'
    {
        "state": "finished",
        "data": [...],
        "creation_date": "2016-08-25 15:22:35"
    }

We may ask for VidiCenter to rebuild the exports, to take into accounts recent uploads for example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1056&data_type=viewers&start=2016-04-29T10:00:00&end=2016-04-29T11:00:00&time_resolution=1h&force_rebuild=1'
    {
        "state": "started",
    }


Data formats
------------

Finest viewers export
^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""

* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for the current viewer event.

And the following metrics, which apply to the current viewer event:

* ``gender``: numeric identifier for gender. Possible values:

    * ``0``: unknown
    * ``1``: male
    * ``2``: female

* ``age``: numeric identifier for age group. Possible values:

    * ``0``: unknown
    * ``1``: child
    * ``2``: young adult
    * ``3``: adult
    * ``4``: senior

* ``glasses``: glasses information (expert only):

    * ``0``: unknown
    * ``1``: no glasses
    * ``2``: glasses
    * ``3``: sunglasses

* ``mustache``: mustache information (expert only):

    * ``0``: unknown
    * ``1``: no mustache
    * ``2``: mustache

* ``beard``: beard information (expert only):

    * ``0``: unknown
    * ``1``: no beard
    * ``2``: beard

* ``age_value``: numeric age in years (expert only).
* ``dwell_time_in_tenths_of_sec``: dwell time in **tenths of seconds**.
* ``attention_time_in_tenths_of_sec``: attention time in **tenths of seconds**.
* Mood values (expert only) are given in percentage, they represent the distribution of a viewer's mood over time. The sum of the five moods totals 100. Each mood is a key:

    * ``very_happy``
    * ``happy``
    * ``neutral``
    * ``unhappy``
    * ``very_unhappy``

PRO example
"""""""""""

Expert keys are present, but are filled with ``null`` values.

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=8264&start=2018-01-29T00:00:00&end=2018-01-29T02:00:00&data_type=viewers&time_resolution=finest'
    {
        "state":"finished",
        "data":[
            {
                "happy":null,
                "dwell_time_in_tenths_of_sec":41,
                "gender":1,
                "age":3,
                "age_value":null,
                "neutral":null,
                "unhappy":null,
                "very_unhappy":null,
                "attention_time_in_tenths_of_sec":16,
                "period_start":"2018-01-29T00:00:27",
                "glasses":null,
                "location_id":8264,
                "very_happy":null,
                "mustache":null,
                "beard":null
            },
            {
                "happy":null,
                "dwell_time_in_tenths_of_sec":54,
                "gender":1,
                "age":2,
                "age_value":null,
                "neutral":null,
                "unhappy":null,
                "very_unhappy":null,
                "attention_time_in_tenths_of_sec":39,
                "period_start":"2018-01-29T00:03:57",
                "glasses":null,
                "location_id":8264,
                "very_happy":null,
                "mustache":null,
                "beard":null
            }
        ],
        "creation_date":"2018-01-29 09:24:18"
    }

Expert example
""""""""""""""

Expert values are present.

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=8866&start=2018-01-29T00:00:00&end=2018-01-29T02:00:00&data_type=viewers&time_resolution=finest'
    {
        "state":"finished",
        "data":[
            {
                "happy":0.0,
                "dwell_time_in_tenths_of_sec":24,
                "gender":2,
                "age":2,
                "age_value":19,
                "neutral":66.66666666666666,
                "unhappy":0.0,
                "very_unhappy":0.0,
                "attention_time_in_tenths_of_sec":8,
                "period_start":"2018-01-29T01:28:52",
                "glasses":1,
                "location_id":8866,
                "very_happy":33.333333333333336,
                "mustache":1,
                "beard":1
            },
            {
                "happy":49.80392156862745,
                "dwell_time_in_tenths_of_sec":37,
                "gender":1,
                "age":3,
                "age_value":57,
                "neutral":0.39215686274509665,
                "unhappy":49.80392156862745,
                "very_unhappy":0.0,
                "attention_time_in_tenths_of_sec":3,
                "period_start":"2018-01-29T00:25:18",
                "glasses":3,
                "location_id":8866,
                "very_happy":0.0,
                "mustache":2,
                "beard":2
            }
        ],
        "creation_date":"2018-01-29 09:18:53"
    }



Finest viewers APC export
^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""

Viewers APC exports contain the same keys than `Finest viewers export`_, and a few more:

* ``contents``: contains the list of contents played while the viewer was in front of the camera. Each content has the following keys:

    * ``content_id``: identifier of the content played.
    * ``app_id``: app_id of the content.
    * ``campaign_id``: campaign_id of the content.

    And the following metrics, which apply to the current viewer event for this content:

    * ``dwell_time_in_milliseconds``: cumulated dwell time, in **milliseconds**.
    * ``attention_time_in_milliseconds``: cumulated attention time, in **milliseconds**.
    * Mood time values (expert only), in **milliseconds**:
        * ``very_happy_time``
        * ``happy_time``
        * ``neutral_time``
        * ``unhappy_time``
        * ``very_unhappy_time``

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=38918&start=2018-01-14T00:00:00&end=2018-01-14T10:00:00&data_type=viewers_apc&time_resolution=finest'
    {
        "state":"finished",
        "data":[
            {
                "beard":1,
                "dwell_time_in_tenths_of_sec":29,
                "start_time":"2018-01-14T09:29:10",
                "gender":2,
                "age":1,
                "age_value":8,
                "neutral":70.19607843137254,
                "mustache":1,
                "unhappy":0.0,
                "attention_time_in_tenths_of_sec":12,
                "location_id":38918,
                "glasses":1,
                "very_unhappy":0.0,
                "very_happy":9.803921568627452,
                "contents":[
                    {
                        "campaign_id":null,
                        "dwell_time_in_milliseconds":928,
                        "unhappy_time":0,
                        "happy_time":0,
                        "very_happy_time":0,
                        "app_id":"my_app_id",
                        "very_unhappy_time":0,
                        "attention_time_in_milliseconds":192,
                        "content_id":"my_very_own_content_id",
                        "neutral_time":192
                    },
                    {
                        "campaign_id":"A campaign id",
                        "dwell_time_in_milliseconds":925,
                        "unhappy_time":0,
                        "happy_time":0,
                        "very_happy_time":0,
                        "app_id":"my_app_id",
                        "very_unhappy_time":0,
                        "attention_time_in_milliseconds":925,
                        "content_id":"another_content_id",
                        "neutral_time":925
                    }
                ],
                "happy":20.0
            },
            {
                "beard":1,
                "dwell_time_in_tenths_of_sec":10,
                "start_time":"2018-01-14T09:21:54",
                "gender":2,
                "age":3,
                "age_value":40,
                "neutral":33.33333333333333,
                "mustache":1,
                "unhappy":0.0,
                "attention_time_in_tenths_of_sec":5,
                "location_id":38918,
                "glasses":1,
                "very_unhappy":0.0,
                "very_happy":0.0,
                "contents":[
                    {
                        "campaign_id":null,
                        "dwell_time_in_milliseconds":15,
                        "unhappy_time":0,
                        "happy_time":542,
                        "very_happy_time":0,
                        "app_id":"my_app_id",
                        "very_unhappy_time":0,
                        "attention_time_in_milliseconds":542,
                        "content_id":"my_very_own_content_id",
                        "neutral_time":0
                    }
                ],
                "happy":66.66666666666667
            }
        ],
        "creation_date":"2018-01-29 09:56:11"
    }


Aggregated viewers export
^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for data aggregation.

And the following metrics, which apply to the current aggregate:

* ``watcher_count``: number of watchers.
* ``dwell_time_in_tenths_of_sec``: cumulated dwell time, in **tenths of seconds**.
* ``attention_time_in_tenths_of_sec``: cumulated attention time, in **tenths of seconds**.
* ``conversion_ratio``: number of watcher divided by the number of OTS. Not present if grouping by demographics.
* ``gender``: numeric identifier for gender, if grouping by demographics. Possible values:

    * ``0``: unknown
    * ``1``: male
    * ``2``: female

* ``age``: numeric identifier for age, if grouping by demographics. Possible values:

    * ``0``: unknown
    * ``1``: child
    * ``2``: young adult
    * ``3``: adult
    * ``4``: senior

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=4636&start=2018-01-29T02:00:00&end=2018-01-29T04:59:59&data_type=viewers&time_resolution=1h'
    {
        "state":"finished",
        "data":[
            {
                "dwell_time_in_tenths_of_sec":12,
                "conversion_ratio":11.11111111111111,
                "watcher_count":1,
                "attention_time_in_tenths_of_sec":3,
                "period_start":"2018-01-29 02:00:00",
                "location_id":4636
            },
            {
                "dwell_time_in_tenths_of_sec":0,
                "conversion_ratio":0.0,
                "watcher_count":0,
                "attention_time_in_tenths_of_sec":0,
                "period_start":"2018-01-29 03:00:00",
                "location_id":4636
            },
            {
                "dwell_time_in_tenths_of_sec":83,
                "conversion_ratio":27.272727272727273,
                "watcher_count":3,
                "attention_time_in_tenths_of_sec":27,
                "period_start":"2018-01-29 04:00:00",
                "location_id":4636
            },
        ],
        "creation_date":"2018-01-29 10:06:09"
    }


Group by demographics example
"""""""""""""""""""""""""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=9876&start=2018-01-29T04:00:00&end=2018-01-29T04:59:59&data_type=viewers&time_resolution=1h&group_by_demographics=1'
    {
        "state":"finished",
        "data":[
            {
                "dwell_time_in_tenths_of_sec":83,
                "gender":1,
                "age":3,
                "watcher_count":3,
                "attention_time_in_tenths_of_sec":27,
                "period_start":"2018-01-29 04:00:00",
                "location_id":9876
            },
            {
                "dwell_time_in_tenths_of_sec":null,
                "gender":0,
                "age":0,
                "watcher_count":0,
                "attention_time_in_tenths_of_sec":null,
                "period_start":"2018-01-29 04:00:00",
                "location_id":9876
            },
            ...
        ],
        "creation_date":"2018-01-29 10:12:28"
    }


Aggregated OTS export
^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for data aggregation.

And the following metrics, which apply to the current aggregate:

* ``ots_count``: cumulated number of OTS.
* ``duration``: cumulated duration of the OTS events, in seconds.
* ``watcher_count``: cumulated number of watchers.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1467&start=2018-01-29T00:00:00&end=2018-01-29T04:59:59&data_type=ots&time_resolution=1h'
    {
        "state":"finished",
        "data":[
            {
                "duration":3600,
                "watcher_count":3,
                "period_start":"2018-01-29 00:00:00",
                "location_id":1467,
                "ots_count":4
            },
            {
                "duration":3600,
                "watcher_count":0,
                "period_start":"2018-01-29 01:00:00",
                "location_id":1467,
                "ots_count":0
            },
            {
                "duration":3600,
                "watcher_count":1,
                "period_start":"2018-01-29 02:00:00",
                "location_id":1467,
                "ots_count":9
            },
            {
                "duration":3600,
                "watcher_count":0,
                "period_start":"2018-01-29 03:00:00",
                "location_id":1467,
                "ots_count":0
            },
            {
                "duration":3600,
                "watcher_count":3,
                "period_start":"2018-01-29 04:00:00",
                "location_id":1467,
                "ots_count":11
            }
        ],
        "creation_date":"2018-01-29 10:15:49"
    }


Aggregated gate export
^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for data aggregation.

And the following metrics, which apply to the current aggregate:

* ``gate_id``: unique numeric identifier of the gate.
* ``in_count``: cumulated number of people who entered the gate.
* ``out_count``: cumulated number of people who exited the gate.
* ``duration``: cumulated duration of the gate events, in seconds.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=26549&start=2018-01-19T10:00:00&end=2018-01-19T12:59:59&data_type=gate&time_resolution=1h'
    {
        "state":"finished",
        "data":[
            {
                "in_count":8,
                "gate_id":1,
                "out_count":18,
                "duration":3600,
                "period_start":"2018-01-19 10:00:00",
                "location_id":26549
            },
            {
                "in_count":14,
                "gate_id":1,
                "out_count":36,
                "duration":3600,
                "period_start":"2018-01-19 11:00:00",
                "location_id":26549
            },
            {
                "in_count":16,
                "gate_id":1,
                "out_count":32,
                "duration":3600,
                "period_start":"2018-01-19 12:00:00",
                "location_id":26549
            }
        ],
        "creation_date":"2018-01-29 10:23:23"
    }


Proof of play by location export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for data aggregation.
* ``content_id``: identifier of the content played.

And the following metrics, which apply to the current aggregate:

* ``content_duration``: cumulated play duration of the content, in seconds.
* ``duration``: total observation time, in seconds.
* ``impressions``: estimated amount of impressions calculated using the conversion ratio.
* ``play_count``: how many times the content was played.
* ``impressions_per_play``: number of impressions divided by number of plays.
* ``watchers``: number of watchers.
* ``watchers_2sec``: number of watchers with an attention time > 2 seconds.
* ``total_dwell_time_in_tenths_of_sec``: total dwell time, in **tenths of seconds**.
* ``total_attention_time_in_tenths_of_sec``: total attention time, in **tenths of seconds**.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=4636&start=2018-01-29T02:00:00&end=2018-01-29T04:59:59&data_type=proof_of_play_by_location&time_resolution=1h'
    {
        "state":"finished",
        "data":[
            {
                "content_duration":60,
                "content_id":"content one",
                "duration":3600,
                "impressions":32,
                "location_id":4636,
                "period_start":"2018-01-29 02:00:00",
                "play_count":12,
                "impressions_per_play":2.67,
                "watchers":8,
                "watchers_2sec":6,
                "total_dwell_time_in_tenths_of_sec": 54.1,
                "total_attention_time_in_tenths_of_sec": 12.9,
            },
            {
                "content_duration":110,
                "content_id":"content one",
                "duration":3600,
                "impressions":96,
                "location_id":4636,
                "period_start":"2018-01-29 03:00:00",
                "play_count":22,
                "impressions_per_play":4.36,
                "watchers":64,
                "watchers_2sec":20,
                "total_dwell_time_in_tenths_of_sec": 105.1,
                "total_attention_time_in_tenths_of_sec": 38.5,
            },
            {
                "content_duration":165,
                "content_id":"content one",
                "duration":3600,
                "impressions":8,
                "location_id":4636,
                "period_start":"2018-01-29 04:00:00",
                "play_count":33,
                "impressions_per_play":0.24,
                "watchers":4,
                "watchers_2sec":1,
                "total_dwell_time_in_tenths_of_sec": 6.3,
                "total_attention_time_in_tenths_of_sec": 3.4,
            },
        ],
        "creation_date":"2018-01-29 10:06:09"
    }


Proof of play by site export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``site_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for data aggregation.
* ``content_id``: identifier of the content played.

And the following metrics, which apply to the current aggregate:

* ``content_duration``: cumulated play duration of the content, in seconds.
* ``duration``: total observation time, in seconds.
* ``impressions``: estimated amount of impressions calculated using the conversion ratio.
* ``play_count``: how many times the content was played.
* ``impressions_per_play``: number of impressions divided by number of plays.
* ``watchers``: number of watchers.
* ``watchers_2sec``: number of watchers with an attention time > 2 seconds.
* ``total_dwell_time_in_tenths_of_sec``: total dwell time, in **tenths of seconds**.
* ``total_attention_time_in_tenths_of_sec``: total attention time, in **tenths of seconds**.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?sites=178&start=2018-01-29T02:00:00&end=2018-01-29T04:59:59&data_type=proof_of_play_by_site&time_resolution=1h'
    {
        "state":"finished",
        "data":[
            {
                "content_duration":50,
                "content_id":"content one",
                "duration":3600,
                "impressions":31,
                "period_start":"2018-01-29 02:00:00",
                "play_count":10,
                "impressions_per_play":3.10,
                "site_id":178,
                "watchers":7,
                "watchers_2sec":5,
                "total_dwell_time_in_tenths_of_sec": 9.3,
                "total_attention_time_in_tenths_of_sec": 5.4,
            },
            {
                "content_duration":110,
                "content_id":"content one",
                "duration":3600,
                "impressions":28,
                "period_start":"2018-01-29 03:00:00",
                "play_count":22,
                "impressions_per_play":1.27,
                "site_id":178,
                "watchers":14,
                "watchers_2sec":14,
                "total_dwell_time_in_tenths_of_sec": 36.3,
                "total_attention_time_in_tenths_of_sec": 19.5,
            },
            {
                "content_duration":20,
                "content_id":"content one",
                "duration":3600,
                "impressions":87,
                "period_start":"2018-01-29 04:00:00",
                "play_count":4,
                "impressions_per_play":21.75,
                "site_id":178,
                "watchers":42,
                "watchers_2sec":12,
                "total_dwell_time_in_tenths_of_sec": 95.1,
                "total_attention_time_in_tenths_of_sec": 42.2,
            },
        ],
        "creation_date":"2018-01-29 10:08:12"
    }

Extrapolated watchers export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``period_start``: starting time for data aggregation.

And the following metrics, which apply to the current aggregate:

* ``watcher_count``: number of watchers.
* ``dwell_time_in_tenths_of_sec``: cumulated dwell time, in **tenths of seconds**.
* ``attention_time_in_tenths_of_sec``: cumulated attention time, in **tenths of seconds**.

Mandatory arguments
"""""""""""""""""""

* ``extrapolation_amount``: integer value that defines to how many locations we should extrapolate. Leave this empty to get the average of the sampled locations.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=4636&start=2018-01-29T02:00:00&end=2018-01-29T04:59:59&data_type=extrapolated_watchers&time_resolution=1h&extrapolation_amount=12'
    {
        "state":"finished",
        "data":[
            {
                "dwell_time_in_tenths_of_sec":12,
                "watcher_count":1,
                "attention_time_in_tenths_of_sec":3,
                "period_start":"2018-01-29 02:00:00",
            },
            {
                "dwell_time_in_tenths_of_sec":0,
                "watcher_count":0,
                "attention_time_in_tenths_of_sec":0,
                "period_start":"2018-01-29 03:00:00",
            },
            {
                "dwell_time_in_tenths_of_sec":83,
                "watcher_count":3,
                "attention_time_in_tenths_of_sec":27,
                "period_start":"2018-01-29 04:00:00",
            },
        ],
        "creation_date":"2018-01-29 10:06:09"
    }


Extrapolated OTS export
^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``period_start``: starting time for data aggregation.

And the following metrics, which apply to the current aggregate:

* ``ots_count``: cumulated number of OTS.
* ``duration``: cumulated duration of the OTS events, in seconds.
* ``watcher_count``: cumulated number of watchers.

Mandatory arguments
"""""""""""""""""""

* ``extrapolation_amount``: integer value that defines to how many locations we should extrapolate. Leave this empty to get the average of the sampled locations.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=1467&start=2018-01-29T00:00:00&end=2018-01-29T04:59:59&data_type=extrapolated_ots&time_resolution=1h&extrapolation_amount=12'
    {
        "state":"finished",
        "data":[
            {
                "duration":3600,
                "watcher_count":3,
                "period_start":"2018-01-29 00:00:00",
                "ots_count":4
            },
            {
                "duration":3600,
                "watcher_count":0,
                "period_start":"2018-01-29 01:00:00",
                "ots_count":0
            },
            {
                "duration":3600,
                "watcher_count":1,
                "period_start":"2018-01-29 02:00:00",
                "ots_count":9
            },
            {
                "duration":3600,
                "watcher_count":0,
                "period_start":"2018-01-29 03:00:00",
                "ots_count":0
            },
            {
                "duration":3600,
                "watcher_count":3,
                "period_start":"2018-01-29 04:00:00",
                "ots_count":11
            }
        ],
        "creation_date":"2018-01-29 10:15:49"
    }

Finest vehicles export
^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""

* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for data aggregation.

And the following metrics, which apply to the current vehicle event:

* ``type``: vehicle type. Possible values:
    * ``0``: unknown
    * ``1``: car
    * ``2``: bus
    * ``3``: truck and SUV
    * ``4``: van
* ``color``: vehicle color. Possible values:
    * ``0``: unknown
    * ``1``: white
    * ``2``: gray
    * ``3``: yellow
    * ``4``: red
    * ``5``: green
    * ``6``: blue
    * ``7``: black
* ``vehicle_presence_time``: vehicle presence time, in **tenths of seconds**.
* ``vehicle_impressions``: number of impressions (= the number of impressions per vehicle).
* ``impressions_per_vehicle``: number of impressions per vehicle.

Example
"""""""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=8264&start=2018-01-29T00:00:00&end=2018-01-29T02:00:00&data_type=vehicles&time_resolution=finest'
    {
        "state":"finished",
        "data":[
            {
                "vehicle_presence_time":41,
                "period_start":"2018-01-29T00:00:27",
                "type":4,
                "location_id":8264,
                "color":null,
                "vehicle_impressions":1.83,
                "impressions_per_vehicle":1.83,
            },
            {
                "vehicle_presence_time":54,
                "period_start":"2018-01-29T00:03:57",
                "type":3,
                "location_id":8264,
                "color":12356,
                "vehicle_impressions":1.72,
                "impressions_per_vehicle":1.72,
            }
        ],
        "creation_date":"2018-01-29 09:24:18"
    }

Aggregated vehicles export
^^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for data aggregation.

And the following metrics, which apply to the current aggregate:

* ``vehicle_count``: number of vehicles.
* ``vehicle_presence_time``: cumulated presence time, in **tenths of seconds**.
* ``vehicle_impressions``: number of impressions.
* ``impressions_per_vehicle``: number of impressions per vehicle.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=4636&start=2018-01-29T02:00:00&end=2018-01-29T04:59:59&data_type=vehicles&time_resolution=1h'
    {
        "state":"finished",
        "data":[
            {
                "vehicle_presence_time":12,
                "vehicle_count":1,
                "period_start":"2018-01-29 02:00:00",
                "location_id":4636
                "vehicle_impressions":1.72,
                "impressions_per_vehicle":1.72,
            },
            {
                "vehicle_presence_time":0,
                "vehicle_count":0,
                "period_start":"2018-01-29 03:00:00",
                "location_id":4636
                "vehicle_impressions":0,
                "impressions_per_vehicle":0,
            },
            {
                "vehicle_presence_time":83,
                "vehicle_count":3,
                "period_start":"2018-01-29 04:00:00",
                "location_id":4636
                "vehicle_impressions":6.04,
                "impressions_per_vehicle":2.01,
            },
        ],
        "creation_date":"2018-01-29 10:06:09"
    }

Finest footfall export
^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""

* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for the current footfall event.
* ``footfall_presence_time``: presence time of the current person, in **tenths of seconds**.

Example
"""""""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=8264&start=2018-01-29T00:00:00&end=2018-01-29T02:00:00&data_type=persons&time_resolution=finest'
    {
        "state":"finished",
        "data":[
            {
                "footfall_presence_time":41,
                "period_start":"2018-01-29T00:00:27",
                "location_id":8264,
            },
            {
                "footfall_presence_time":54,
                "period_start":"2018-01-29T00:03:57",
                "location_id":8264,
            }
        ],
        "creation_date":"2018-01-29 09:24:18"
    }

Aggregated footfall export
^^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for data aggregation.

And the following metrics, which apply to the current aggregate:

* ``footfall_impressions``: number of footfall impressions.
* ``footfall_presence_time``: cumulated presence time, in **tenths of seconds**.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=4636&start=2018-01-29T02:00:00&end=2018-01-29T04:59:59&data_type=persons&time_resolution=1h'
    {
        "state":"finished",
        "data":[
            {
                "footfall_presence_time":12,
                "footfall_impressions":1,
                "period_start":"2018-01-29 02:00:00",
                "location_id":4636
            },
            {
                "footfall_presence_time":0,
                "footfall_impressions":0,
                "period_start":"2018-01-29 03:00:00",
                "location_id":4636
            },
            {
                "footfall_presence_time":83,
                "footfall_impressions":3,
                "period_start":"2018-01-29 04:00:00",
                "location_id":4636
            },
        ],
        "creation_date":"2018-01-29 10:06:09"
    }

Finest footfall + vehicles export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Note
"""""""""""""
This api endpoint returns a combination of vehicles and persons. Each record being either a vehicle or a person, some keys will consequently be void.

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for the current vehicle or footfall event.
* ``type``: vehicle type (see "Finest vehicles export" for possible values).
* ``color``: vehicle color (see "Finest vehicles export" for possible values).
* ``vehicle_impressions``: number of impressions (= number of impressions per vehicle).
* ``vehicle_presence_time``: presence time of the current vehicle, in **tenths of seconds**.* `
* ``impressions_per_vehicle``: number of impressions per vehicle.
* ``footfall_presence_time``:  presence time of the current person, in **tenths of seconds**.

Example
"""""""
In this example, in a 3 min timeframe, we registered one vehicle (first record) and one person (second record).

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=4636&start=2021-11-01T09:03:00&end=2021-11-01T09:06:00&data_type=vehicles_persons&time_resolution=finest'
    {
        "state":"finished",
        "data":[
            {
                "type": 3,
                "color": 6,
                "location_id": 4636,
                "period_start": "2021-11-01T09:03:25",
                "vehicle_presence_time": 29,
                "vehicle_impressions": 1.85,
                "impressions_per_vehicle": 1.85,
                "footfall_presence_time": 0
            },
            {
                "type": 0,
                "color": 0,
                "location_id": 4636,
                "period_start": "2021-11-01T09:05:21",
                "vehicle_presence_time": 0,
                "vehicle_impressions": 0.0,
                "impressions_per_vehicle": 0.0,
                "footfall_presence_time": 85
            },
        ],
        "creation_date": "2021-12-07 17:30:28",
    }

Aggregated footfall + vehicles export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for the current aggregate.

And the following metrics, which apply to the current aggregate:

* ``vehicle_count``: number of vehicles.
* ``vehicle_impressions``: number of vehicle impressions.
* ``vehicle_presence_time``: cumulated vehicle presence time, in **tenths of seconds**.
* ``impressions_per_vehicle``: average number of impressions per vehicle.
* ``footfall_impressions``: number of footfall impressions.
* ``footfall_presence_time``: cumulated footfall presence time, in **tenths of seconds**.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=4636&start=2021-11-01T09:00:00&end=2021-11-01T10:00:00&data_type=vehicles_persons&time_resolution=30m'
    {
        "state":"finished",
        "data":[
            {
                "location_id": 4636,
                "vehicle_impressions": 155,
                "impressions_per_vehicle": 1.85,
                "period_start": "2021-11-01 09:00:00",
                "vehicle_count": 84,
                "vehicle_presence_time": 23569,
                "footfall_impressions": 2,
                "footfall_presence_time": 133
            },
            {
                "location_id": 4636,
                "vehicle_impressions": 187,
                "impressions_per_vehicle": 1.85,
                "period_start": "2021-11-01 09:30:00",
                "vehicle_count": 101,
                "vehicle_presence_time": 7634,
                "footfall_impressions": 1,
                "footfall_presence_time": 91
            },
            {
                "location_id": 4636,
                "vehicle_impressions": 0,
                "impressions_per_vehicle": 0.0,
                "period_start": "2021-11-01 10:00:00",
                "vehicle_count": 0,
                "vehicle_presence_time": 0,
                "footfall_impressions": 0,
                "footfall_presence_time": 0
            }
        ],
        "creation_date": "2021-12-07 17:06:28",
    }

Impression multiplier export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Expected keys
"""""""""""""
* ``location_id``: unique numeric identifier of the data source.
* ``period_start``: starting time for the current aggregate.

And the following metrics, which apply to the current aggregate:

* ``vehicle_count``: number of vehicles.
* ``vehicle_total_presence_time``: cumulated presence time, in **tenths of seconds**.
* ``vehicle_impressions``: number of vehicle impressions.
* ``impressions_per_vehicle``: number of impressions per vehicle.
* ``footfall_impressions``: number of footfall impressions.
* ``footfall_total_dwell_time``: cumulated footfall presence time, in **tenths of seconds**.
* ``im_footfall``: impression multiplier for footfall.
* ``im_vehicle``: impression multiplier for vehicles.
* ``im``: combined impression multiplier.
* ``backup_value``: if this contains "yes" it means a backup im value was calculated based on equivalent data of the previous week.
* ``analysis_window``: time window during which the analysis took place, in **tenths of seconds**.

Example
"""""""

 ::

    curl -u USERNAME:AUTH_TOKEN 'https://vidicenter.quividi.com/api/v1/data/?locations=4636&start=2018-01-29T02:00:00&end=2018-01-29T04:59:59&data_type=im&time_resolution=1h'
    {
        "state":"finished",
        "data":[
            {
              "location_id": 60628,
              "vehicle_impressions": 50,
              "impressions_per_vehicle": 1.85,
              "footfall_impressions": 76,
              "footfall_total_dwell_time": 23482,
              "im": 0.79,
              "im_footfall": 0.65,
              "im_vehicle": 0.14,
              "analysis_window": 36000,
              "backup_value": "",
              "period_start": "2021-11-14 21:00:00",
              "vehicle_count": 27,
              "vehicle_total_presence_time": 2696
            },
            {
              "location_id": 60628,
              "vehicle_impressions": 70,
              "impressions_per_vehicle": 1.94,
              "footfall_impressions": 49,
              "footfall_total_dwell_time": 6662,
              "im": 0.4,
              "im_footfall": 0.19,
              "im_vehicle": 0.21,
              "analysis_window": 36000,
              "backup_value": "yes",
              "period_start": "2021-11-14 22:00:00",
              "vehicle_count": 36,
              "vehicle_total_presence_time": 3989
            },
        ],
        "creation_date":"2018-01-29 10:06:09"
    }

Placeholder data and null values
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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


Continue to :ref:`clip_metadata`
