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
    * ``viewers``: Viewers data

* ``time_resolution``: The time resolution used in the aggregation. Allowed values:

    * ``finest``: Do not aggregate, display all raw objects (unavailable for OTS exports).
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

Optional arguments
------------------

* ``group_by_demographics``: If specified, will group the aggregate by demographics for each gender and each age group. Only works with viewers export.
* ``force_rebuild``: Force the reconstruction of the export. See `Asynchronous usage`_.

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
        "creation_date": "2016-08-25T15:22:35"
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
        "creation_date": "2016-08-25T15:22:35"
    }
