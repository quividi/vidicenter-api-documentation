.. _status:


Status calls
============


The following API endpoints allow you to see the status of your networks


Alerts list
###########

Returns a list of the 1000 most recent alerts in your networks

URL
---

``https://vidicenter.quividi.com/api/v1/alerts/``

Notable data keys
-----------------

* ``alert_type``: numeric identifier representing the type of alert:

    * 1: Upload Period Alert
    * 2: Watchers Rate Alert
    * 3: Input or License Errors Alert
    * 4: Daily Watchers Alert
    * 5: Time Drift Alert
    * 6: Fingerprint Change Alert
    * 7: Upload Error Alert
    * 8: Refused Events Alert
    * 9: Session ID Alert
    * 10: Timezone Change Alert
    * 11: Camera Not Found Alert
    * 12: Unsupported Option Alert

* ``label``: string representing the type of the alert. May be subject to changes.
* ``status``: status of the alert ("raised" or "terminated").
* ``extra_data``: in case of Input or License Errors Alerts, contains more info on the alert. This can be a combination of the following keywords:

    * Unknown
    * Idle
    * Processing
    * Waiting Input
    * Input Lost
    * On Error
    * Terminating
    * Restricted
    * Disabled
    * VidiProof
    * License Error
    * Black Input

* ``linked_alerthistory``: unique identifier of the alert the current alert is terminating.

Optional arguments
------------------

* ``active``: if specified as True, only the currently active alerts will be returned.

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/alerts/
    [
        {
            "alert_type": 9,
            "box": 181,
            "extra_data": "",
            "host_timestamp": "2015-10-27T05:02:31",
            "id": 199756,
            "label": "Session ID Alert",
            "location": 88,
            "network": 12,
            "site": 37,
            "status": "terminated",
            "timestamp": "2015-10-26T22:20:23",
            "linked_alerthistory": 192751
        },
        {
            "alert_type": 3,
            "box": 1689,
            "extra_data": "Processing,Black Input",
            "host_timestamp": "2015-10-26T22:18:43",
            "id": 192751,
            "label": "Input or License Errors Alert",
            "location": 1768,
            "network": 47,
            "site": 219,
            "status": "raised",
            "timestamp": "2015-10-26T22:18:45",
            "linked_alerthistory": 192755
        },
        {
            "alert_type": 8,
            "box": 1512,
            "extra_data": "",
            "host_timestamp": "2015-10-27T08:18:53",
            "id": 199250,
            "label": "Refused Events Alert",
            "location": 158,
            "network": 84,
            "site": 208,
            "status": "raised",
            "timestamp": "2015-10-26T22:18:31",
            "linked_alerthistory": null
        }
      ]

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/alerts/?active=True
    [
        {
            "alert_type": 3,
            "box": 1689,
            "extra_data": "Processing,Black Input",
            "host_timestamp": "2015-10-26T22:18:43",
            "id": 192751,
            "label": "Input or License Errors Alert",
            "location": 1768,
            "network": 47,
            "site": 219,
            "status": "raised",
            "timestamp": "2015-10-26T22:18:45",
            "linked_alerthistory": 192755
        },
        {
            "alert_type": 8,
            "box": 1512,
            "extra_data": "",
            "host_timestamp": "2015-10-27T08:18:53",
            "id": 199250,
            "label": "Refused Events Alert",
            "location": 158,
            "network": 84,
            "site": 208,
            "status": "raised",
            "timestamp": "2015-10-26T22:18:31",
            "linked_alerthistory": null
        }
      ]


Network's alerts list
#####################

Returns a list of a network's 1000 most recent alerts

URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/alerts/``

Optional arguments
------------------

* ``active``: if specified as True, only the currently active alerts will be returned.


Site's alerts list
##################

Returns a list of a site's 1000 most recent alerts

URL
---

``https://vidicenter.quividi.com/api/v1/site/{site_id}/alerts/``

Optional arguments
------------------

* ``active``: if specified as True, only the currently active alerts will be returned.


Site tag's alerts list
######################

Returns a list of a site tag's 1000 most recent alerts

URL
---

``https://vidicenter.quividi.com/api/v1/site_tag/{tag}/alerts/``

Optional arguments
------------------

* ``active``: if specified as True, only the currently active alerts will be returned.


Location's alerts list
######################

Returns a list of a locations's 1000 most recent alerts

URL
---

``https://vidicenter.quividi.com/api/v1/location/{location_id}/alerts/``

Optional arguments
------------------

* ``active``: If specified as True, only the currently active alerts will be returned.


Location tag's alerts list
##########################

Returns a list of a location tag's 1000 most recent alerts

URL
---

``https://vidicenter.quividi.com/api/v1/location_tag/{tag}/alerts/``

Optional arguments
------------------

* ``active``: if specified as True, only the currently active alerts will be returned.


Box's alerts list
#################

Returns a list of a box's 1000 most recent alerts

URL
---

``https://vidicenter.quividi.com/api/v1/box/{box_id}/alerts/``

Optional arguments
------------------

* ``active``: if specified as True, only the currently active alerts will be returned.


Monitoring messages list
########################

Returns a list of your 1000 most recent monitoring messages

URL
---

``https://vidicenter.quividi.com/api/v1/monitoring_msgs/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/monitoring_msgs/
    [
        {
            "avg_luma": "43.1%",
            "box": 1742,
            "cpu_load": 27,
            "fps": "27.8",
            "location": 1982,
            "nb_inputlost": 0,
            "status": "running",
            "host_timestamp": "2015-10-27T11:00:00",
            "timestamp": "2015-10-27T12:02:25",
            "vr_status": "Processing"
        },
        {
            "avg_luma": "100.0%",
            "box": 1582,
            "cpu_load": 22,
            "fps": "-",
            "location": 1692,
            "nb_inputlost": 0,
            "status": "running",
            "host_timestamp": "2015-10-27T12:00:00",
            "timestamp": "2015-10-27T12:29:25",
            "vr_status": "Input Lost"
        }
    ]


Network's monitoring messages list
##################################

Returns a list of a network's 1000 most recent monitoring messages

URL
---

``https://vidicenter.quividi.com/api/v1/network/{network_id}/monitoring_msgs/``


Site's monitoring messages list
###############################

Returns a list of a site's 1000 most recent monitoring messages

URL
---

``https://vidicenter.quividi.com/api/v1/site/{site_id}/monitoring_msgs/``


Site tag's monitoring messages list
###################################

Returns a list of a site tag's 1000 most recent monitoring messages

URL
---

``https://vidicenter.quividi.com/api/v1/site_tag/{tag}/monitoring_msgs/``


Location's monitoring messages list
###################################

Returns a list of a location's 1000 most recent monitoring messages

URL
---

``https://vidicenter.quividi.com/api/v1/location/{location_id}/monitoring_msgs/``


Location tag's monitoring messages list
#######################################

Returns a list of a location tag's 1000 most recent monitoring messages

URL
---

``https://vidicenter.quividi.com/api/v1/location_tag/{tag}/monitoring_msgs/``


Box's monitoring messages list
##############################

Returns a list of a box's 1000 most recent monitoring messages

URL
---

``https://vidicenter.quividi.com/api/v1/box/{box_id}/monitoring_msgs/``


Continue to :ref:`tags`
