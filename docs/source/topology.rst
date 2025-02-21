.. _topology:


Topology calls
==============


The following API endpoints allow you to retrieve informations pertaining to your networks.


Networks list
#############

Returns a list of your networks

URL
---

``https://vidicenter.quividi.com/api/v1/networks/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/networks/
    [
        {
            "id":15678,
            "label":"My first network",
            "active":true
        },
        {
            "id":25444,
            "label":"My other network",
            "active":false
        }
    ]


Sites list
##########

Returns a list of your sites

URL
---

``https://vidicenter.quividi.com/api/v1/sites/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/sites/
    [
        {
            "network_id":15678,
            "id":8654,
            "label":"VidiReports",
            "active":true,
            "black_input_is_error": true,
            "daily_watchers_alert_threshold": 0,
            "description": "VEHICULOS",
            "exclude_from_network_vrc": false,
            "max_active_locations": 0,
            "upload_period_alert": 780,
            "venue_subtype": "Bowling Center",
            "venue_type": "Entertainment / hospitality"

        },
        {
            "network_id":15678,
            "id":54422,
            "label":"Another site",
            "active":true,
            "black_input_is_error": true,
            "daily_watchers_alert_threshold": 0,
            "description": "",
            "exclude_from_network_vrc": false,
            "max_active_locations": 0,
            "upload_period_alert": 780,
            "venue_subtype": "Bowling Center",
            "venue_type": "Entertainment / hospitality"
        },
        {
            "network_id":25444,
            "id":66531,
            "label":"VidiReports",
            "active":false
            "black_input_is_error": true,
            "daily_watchers_alert_threshold": 0,
            "description": "",
            "exclude_from_network_vrc": false,
            "max_active_locations": 0,
            "upload_period_alert": 780,
            "venue_subtype": "Bowling Center",
            "venue_type": "Entertainment / hospitality"
        }
    ]


Sites list with vehicles factors
################################

Returns a list of your sites with vehicles factors

URL
---

``https://vidicenter.quividi.com/api/v1/sites/vehicles/``

Example
-------

::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/sites/vehicles/
    [
        {
            "id": 4200,
            "label": "My only site",
            "active": true,
            "network_id": 567,
            "vehicles_factors": {
                "factor_car": 1.6,
                "factor_van": 3.5,
                "factor_bus": 9.5,
                "factor_truck": 1.2,
            }
        },
    ]



Network's sites list
####################

Returns a list of a network's sites

URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/sites/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/network/15678/sites/
    [
        {
            "network_id":15678,
            "id":8654,
            "label":"VidiReports",
            "active":true,
        },
        {
            "network_id":15678,
            "id":54422,
            "label":"Another site",
            "active":true,
        }
    ]


Network's sites list with vehicles factors
##########################################

Returns a list of a network's sites with vehicles factors

URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/sites/vehicles``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/network/567/sites/vehicles
    [
        {
            "id": 4200,
            "label": "My only site",
            "active": true,
            "network_id": 567,
            "vehicles_factors": {
                "factor_car": 1.6,
                "factor_van": 3.5,
                "factor_bus": 9.5,
                "factor_truck": 1.2,
            }
        },
    ]


Site tag's sites list
#####################

Returns a list of a site tag's sites

URL
---

``https://vidicenter.quividi.com/api/v1/site_tag/{tag}/sites/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/site_tag/my%20special%20tag/sites/
    [
        {
            "network_id":9842,
            "id":54892,
            "label":"A tagged site",
            "active":true
        }
    ]


Locations list
##############

Returns a list of your locations

URL
---

``https://vidicenter.quividi.com/api/v1/locations/``

Notable data keys
-----------------

* ``last_timezone``: timezone of the latest upload to this location. It is an integer representing the offset in minutes to the UTC timezone, more info can be found `here <https://en.wikipedia.org/wiki/UTC%C2%B100:00>`_.
* ``last_seen``: last timestamp when the location was in contact with vidicenter.
* ``last_upload_timestamp``: last timestamp when the location uploaded data to vidicenter.
* ``last_ots_uploaded``: last timestamp when the location uploaded OTS events to vidicenter.
* ``id_pointer``: optional pointer id to a different location
* ``id_broadsign_displayunitid``: optional extra ids to allow identifying link to BroadSign
* ``id_broadsign_playerid``: optional extra ids to allow identifying link to BroadSign
* ``id_broadsign_screenid_v1``: optional extra ids to allow identifying link to BroadSign
* ``id_broadsign_uuid``: optional extra ids to allow identifying link to BroadSign
* ``location_type``: type describing this location
* ``venue_type``: venue_type of the site this location belongs to
* ``venue_subtype``: venue_subtype of the site this location belongs to

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/locations/
    [
        {
            "last_upload_timestamp":"2015-10-12T10:29:43",
            "last_timezone": 0,
            "box_id":193538,
            "site_id":8654,
            "id":204452,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2014-09-11T09:18:32",
            "label":"location-204452",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-27T11:29:57",
            "last_timezone": 3600,
            "box_id":192366,
            "site_id":54422,
            "id":27046,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2012-07-09T07:05:19",
            "label":"Entrance",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-13T21:40:57",
            "last_timezone": -14400,
            "box_id":219354,
            "site_id":66531,
            "id":320453,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2014-01-04T19:55:41",
            "label":"Exit screen",
            "active":true
        }
    ]


Network's locations list
########################

Returns a list of a network's locations

URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/locations/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/network/15678/locations/
    [
        {
            "last_upload_timestamp":"2015-10-12T10:29:43",
            "last_timezone": 0,
            "box_id":193538,
            "site_id":8654,
            "id":204452,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2014-09-11T09:18:32",
            "label":"location-204452",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-27T11:29:57",
            "last_timezone": 3600,
            "box_id":192366,
            "site_id":54422,
            "id":27046,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2012-07-09T07:05:19",
            "label":"Entrance",
            "active":true
        }
    ]


Site's locations list
#####################

Returns a list of a site's locations

URL
---

``https://vidicenter.quividi.com/api/v1/site/{site_id}/locations/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/site/8654/locations/
    [
        {
            "last_upload_timestamp":"2015-10-12T10:29:43",
            "last_timezone": 0,
            "box_id":193538,
            "site_id":8654,
            "id":204452,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2014-09-11T09:18:32",
            "label":"location-204452",
            "active":true
        }
    ]


Site tag's locations list
#########################

Returns a list of a site tag's locations

URL
---

``https://vidicenter.quividi.com/api/v1/site_tag/{tag}/locations/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/site_tag/my%20special%20tag/locations/
    [
        {
            "last_upload_timestamp":"2015-09-15T11:00:16",
            "last_timezone": -28800,
            "box_id":114242,
            "site_id":54892,
            "id":1330,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2014-09-11T09:18:32",
            "label":"A location's name",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-29T12:13:02",
            "last_timezone": 14400,
            "box_id":114246,
            "site_id":54892,
            "id":1334,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2012-07-09T07:05:19",
            "label":"Another location",
            "active":true
        }
    ]


Location tag's locations list
#############################

Returns a list of a location tag's locations

URL
---

``https://vidicenter.quividi.com/api/v1/location_tag/{tag}/locations/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/location_tag/tag%20of%20mine/locations/
    [
        {
            "last_upload_timestamp":"2015-09-15T11:00:16",
            "last_timezone": -28800,
            "box_id":114242,
            "site_id":54892,
            "id":1330,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2014-09-11T09:18:32",
            "label":"A location's name",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-29T12:13:02",
            "last_timezone": 14400,
            "box_id":114246,
            "site_id":54892,
            "id":1334,
            "id_broadsign":"",
            "id_pointer":"",
            "creation_date":"2012-07-09T07:05:19",
            "label":"Another location",
            "active":true
        }
    ]


Boxes list
##########

Returns a list of your boxes


URL
---

``https://vidicenter.quividi.com/api/v1/boxes/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/boxes/
    [
        {
            "last_upload_timestamp":"2015-10-27T11:13:47",
            "location_id":204452,
            "site_id":8654,
            "id":193538,
            "label":"box-193538 (C001680) (box-193538)",
            "active":true,
            "box_mac":123456
        },
        {
            "last_upload_timestamp":"2015-10-27T11:19:32",
            "location_id":27046,
            "site_id":54422,
            "id":192366,
            "label":"box-192366 (C001246) (box-192366)",
            "active":true,
            "box_mac":123457
        },
        {
            "last_upload_timestamp":"2015-10-27T11:26:47",
            "location_id":320453,
            "site_id":66531,
            "id":219354,
            "label":"My third box",
            "active":false,
            "box_mac":123458
        },
    ]


Network's boxes list
####################

Returns a list of a network's boxes


URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/boxes/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/network/15678/boxes/
    [
        {
            "last_upload_timestamp":"2015-10-27T11:13:47",
            "location_id":204452,
            "site_id":8654,
            "id":193538,
            "label":"box-193538 (C001680) (box-193538)",
            "active":true,
            "box_mac":123456
        },
        {
            "last_upload_timestamp":"2015-10-27T11:19:32",
            "location_id":27046,
            "site_id":54422,
            "id":192366,
            "label":"box-192366 (C001246) (box-192366)",
            "active":true,
            "box_mac":123456
        }
    ]


Site's boxes list
#################

Returns a list of a site's boxes


URL
---

``https://vidicenter.quividi.com/api/v1/site/{site_id}/boxes/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/site/54422/boxes/
    [
        {
            "last_upload_timestamp":"2015-10-27T11:19:32",
            "location_id":27046,
            "site_id":54422,
            "id":192366,
            "label":"box-192366 (C001246) (box-192366)",
            "active":true,
            "box_mac":123456
        }
    ]


Site tag's boxes list
#####################

Returns a list of a site tag's boxes

URL
---

``https://vidicenter.quividi.com/api/v1/site_tag/{tag}/boxes/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/site_tag/my%20special%20tag/boxes/
    [
        {
            "last_upload_timestamp":"2015-09-15T11:00:16",
            "location_id":1330,
            "site_id":54892,
            "id":114242,
            "label":"A first box",
            "active":true,
            "box_mac":123456
        },
        {
            "last_upload_timestamp":"2015-10-29T12:13:02",
            "location_id":1334,
            "site_id":54892,
            "id":114246,
            "label":"Another box",
            "active":true,
            "box_mac":123456
        }
    ]


Location's boxes list
#####################

Returns a list of a location's boxes

URL
---

``https://vidicenter.quividi.com/api/v1/location/{location_id}/boxes/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/location/27046/boxes/
    [
        {
            "last_upload_timestamp":"2015-10-27T11:19:32",
            "location_id":27046,
            "site_id":54422,
            "id":192366,
            "label":"box-192366 (C001246) (box-192366)",
            "active":true,
            "box_mac":123456
        }
    ]


Location tag's boxes list
#########################

Returns a list of a location tag's boxes

URL
---

``https://vidicenter.quividi.com/api/v1/location_tag/{tag}/boxes/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/location_tag/tag%20of%20mine/boxes
    [
        {
            "last_upload_timestamp":"2015-09-15T11:00:16",
            "location_id":1330,
            "site_id":54892,
            "id":114242,
            "label":"A first box",
            "active":true,
            "box_mac":123456
        },
        {
            "last_upload_timestamp":"2015-10-29T12:13:02",
            "location_id":1334,
            "site_id":54892,
            "id":114246,
            "label":"Another box",
            "active":true,
            "box_mac":123456
        }
    ]


Continue to :ref:`status`
