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
            "active":true
        },
        {
            "network_id":15678,
            "id":54422,
            "label":"Another site",
            "active":true
        },
        {
            "network_id":25444,
            "id":66531,
            "label":"VidiReports",
            "active":false
        }
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

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/locations/
    [
        {
            "last_upload_timestamp":"2015-10-12T10:29:43",
            "box_id":193538,
            "site_id":8654,
            "id":204452,
            "creation_date":"2014-09-11T09:18:32",
            "label":"location-204452",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-27T11:29:57",
            "box_id":192366,
            "site_id":54422,
            "id":27046,
            "creation_date":"2012-07-09T07:05:19",
            "label":"Entrance",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-13T21:40:57",
            "box_id":219354,
            "site_id":66531,
            "id":320453,
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
            "box_id":193538,
            "site_id":8654,
            "id":204452,
            "creation_date":"2014-09-11T09:18:32",
            "label":"location-204452",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-27T11:29:57",
            "box_id":192366,
            "site_id":54422,
            "id":27046,
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
            "box_id":193538,
            "site_id":8654,
            "id":204452,
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
            "box_id":114242,
            "site_id":54892,
            "id":1330,
            "creation_date":"2014-09-11T09:18:32",
            "label":"A location's name",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-29T12:13:02",
            "box_id":114246,
            "site_id":54892,
            "id":1334,
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
            "box_id":114242,
            "site_id":54892,
            "id":1330,
            "creation_date":"2014-09-11T09:18:32",
            "label":"A location's name",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-29T12:13:02",
            "box_id":114246,
            "site_id":54892,
            "id":1334,
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
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-27T11:19:32",
            "location_id":27046,
            "site_id":54422,
            "id":192366,
            "label":"box-192366 (C001246) (box-192366)",
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-27T11:26:47",
            "location_id":320453,
            "site_id":66531,
            "id":219354,
            "label":"My third box",
            "active":false
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
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-27T11:19:32",
            "location_id":27046,
            "site_id":54422,
            "id":192366,
            "label":"box-192366 (C001246) (box-192366)",
            "active":true
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
            "active":true
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
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-29T12:13:02",
            "location_id":1334,
            "site_id":54892,
            "id":114246,
            "label":"Another box",
            "active":true
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
            "active":true
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
            "active":true
        },
        {
            "last_upload_timestamp":"2015-10-29T12:13:02",
            "location_id":1334,
            "site_id":54892,
            "id":114246,
            "label":"Another box",
            "active":true
        }
    ]


Continue to :ref:`status`
