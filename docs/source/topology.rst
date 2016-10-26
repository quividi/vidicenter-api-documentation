.. _topology:


Topology calls
==============


The following API endpoints allow you to retrieve informations pertaining to your networks.


Networks list
#############

Returns a list of your networks.

URL
---

``https://vidicenter.quividi.com/api/v1/networks/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/networks/
    [
        {
            "id": 15678,
            "label": "My first network"
        },
        {
            "id": 25444,
            "label": "My other network"
        }
    ]


Sites list
##########

Returns a list of your sites.

URL
---

``https://vidicenter.quividi.com/api/v1/sites/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/sites/
    [
        {
            "network_id": 15678,
            "id": 8654,
            "label": "VidiReports"
        }, {
            "network_id": 15678,
            "id": 54422,
            "label": "Another site"
        }, {
            "network_id": 25444,
            "id": 66531,
            "label": "VidiReports"
        }
    ]


Network's sites list
####################

Returns a list of a network's sites.

URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/sites/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/network/15678/sites/
    [
        {
            "network_id": 15678,
            "id": 8654,
            "label": "VidiReports"
        }, {
            "network_id": 15678,
            "id": 54422,
            "label": "Another site"
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
            "label":"location-204452"
        },
        {
            "last_upload_timestamp":"2015-10-27T11:29:57",
            "box_id":192366,
            "site_id":54422,
            "id":27046,
            "label":"Entrance"
        },
        {
            "last_upload_timestamp":"2015-10-13T21:40:57",
            "box_id":219354,
            "site_id":66531,
            "id":320453,
            "label":"Exit screen"
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
            "label":"location-204452"
        },
        {
            "last_upload_timestamp":"2015-10-27T11:29:57",
            "box_id":192366,
            "site_id":54422,
            "id":27046,
            "label":"Entrance"
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
            "label":"location-204452"
        }
    ]


Boxes list
##########

Returns a list of your boxes


Url
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
            "label":"box-193538 (C001680) (box-193538)"
        },
        {
            "last_upload_timestamp":"2015-10-27T11:19:32",
            "location_id":27046,
            "site_id":54422,
            "id":192366,
            "label":"box-192366 (C001246) (box-192366)"
        },
        {
            "last_upload_timestamp":"2015-10-27T11:26:47",
            "location_id":320453,
            "site_id":66531,
            "id":219354,
            "label":"My third box"
        },
    ]


Network's boxes list
####################

Returns a list of a network's boxes


Url
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
            "label":"box-193538 (C001680) (box-193538)"
        },
        {
            "last_upload_timestamp":"2015-10-27T11:19:32",
            "location_id":27046,
            "site_id":54422,
            "id":192366,
            "label":"box-192366 (C001246) (box-192366)"
        }
    ]


Site's boxes list
#################

Returns a list of a site's boxes


Url
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
            "label":"box-192366 (C001246) (box-192366)"
        }
    ]


Location's boxes list
#####################

Returns a list of a location's boxes

Url
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
            "label":"box-192366 (C001246) (box-192366)"
        }
    ]


Continue to :ref:`status`
