.. _tags:


Tags calls
==========


The following API endpoints allow you to retrieve informations about your location and site tags.


Site tags list
##############

Returns a list of your site tags

URL
---

``https://vidicenter.quividi.com/api/v1/site_tags/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/site_tags/
    [
        {
            "network_id":4834,
            "id":87,
            "label":"a tag"
        },
        {
            "network_id":4834,
            "id":514,
            "label":"another tag"
        },
        {
            "network_id":9842,
            "id":90244,
            "label":"my special tag"
        }
    ]


Network's site tags list
########################

Returns a list of a network's site tags

URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/site_tags/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/network/4834/site_tags/
    [
        {
            "network_id":4834,
            "sites":[
                {
                    "id":20019,
                    "label":"My site"
                },
                {
                    "id":14006,
                    "label":"Another site of mine"
                }
            ],
            "id":2,
            "label":"another tag"
        }
    ]

.. note::
    Each entry in the ``sites`` array contains the full set of site fields
    returned by the ``Sites list`` endpoint (``active``, ``description``,
    ``address``, ``venue_type``, ``venue_subtype``, ``max_active_locations``,
    ``exclude_from_network_vrc``, ``upload_period_alert``,
    ``daily_watchers_alert_threshold``, ``black_input_is_error``,
    ``ignore_low_fps``, ...); the example above is abbreviated for brevity.


Location tags list
##################

Returns a list of your location tags

URL
---

``https://vidicenter.quividi.com/api/v1/location_tags/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/location_tags/
    [
        {
            "network_id":4834,
            "id":2231,
            "label":"Test tag"
        },
        {
            "network_id":4834,
            "id":4562,
            "label":"Another tag"
        },
        {
            "network_id":9842,
            "id":903,
            "label":"Tag Heuer"
        }
    ]


Network's location tags list
############################

Returns a list of a network's location tags

URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/location_tags/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/network/4834/location_tags/
    [
        {
            "network_id":4834,
            "locations":[
                {
                    "last_upload_timestamp":"2015-10-27T10:08:21",
                    "id":2608,
                    "label":"location-2608"
                }
            ],
            "id":2231,
            "label":"Test tag"
        },
        {
            "network_id":4834,
            "locations":[
                {
                    "last_upload_timestamp":"2015-10-27T11:19:32",
                    "id":31789,
                    "label":"location-31789"
                },
                {
                    "last_upload_timestamp":"2015-10-27T10:08:21",
                    "id":2608,
                    "label":"location-2608"
                }
            ],
            "id":4562,
            "label":"Another tag"
        }
    ]

.. note::
    Each entry in the ``locations`` array contains the full set of location
    fields returned by the ``Locations list`` endpoint (``id_broadsign_uuid``,
    ``id_broadsign_playerid``, ``id_broadsign_screenid_v1``,
    ``id_broadsign_displayunitid``, ``id_pointer``, ``description``,
    ``last_timezone``, ``last_seen``, ``last_ots_uploaded``, ``creation_date``,
    ``active``, ``location_type``, ``venue_type``, ``venue_subtype``,
    ``bm_ignore``, ...); the example above is abbreviated for brevity.


Continue to :ref:`data`
