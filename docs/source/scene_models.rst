.. _scene_models:


Scene models
============


Scene models are snapshots of the scene captured by a location's camera. They let you
verify a camera's framing and placement over time.

These endpoints are only available on networks that have scene models enabled, and to
users granted the corresponding permission. On a network where scene models are not
enabled, the endpoints return an HTTP 403 response.


Request a scene model
#####################

Asks the box attached to a location to capture and upload a new scene model. The capture
is performed asynchronously by the box; poll the list endpoint to retrieve it once it is
available.

URL
---

``https://vidicenter.quividi.com/api/v1/location/{location_id}/request_scene_model/``

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/location/1982/request_scene_model/
    {
        "message": "Scene model request made successfully"
    }

A request returns an HTTP 400 response when the location has no box attached, or when a
scene model request is already in progress for that box.


List scene models
#################

Returns the 10 most recent scene models available for a location, most recent first. Use
the returned ``id`` to download a specific scene model.

URL
---

``https://vidicenter.quividi.com/api/v1/location/{location_id}/list_scene_models/``

Notable data keys
-----------------

* ``id``: unique identifier of the scene model, to be passed to the download endpoint.
* ``timestamp``: capture time of the scene model.

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/location/1982/list_scene_models/
    [
        {
            "id": 84321,
            "timestamp": "2026-08-19T09:14:07"
        },
        {
            "id": 84102,
            "timestamp": "2026-08-12T09:12:55"
        }
    ]


Download a scene model
######################

Returns the scene model image as a JPEG file. Without arguments, the most recent scene
model is returned.

URL
---

``https://vidicenter.quividi.com/api/v1/location/{location_id}/download_scene_model/``

Optional arguments
------------------

* ``scene_model_id``: identifier of a specific scene model to download (as returned by
  the list endpoint). If omitted, the most recent scene model is returned.

Example
-------

 ::

    curl -u USERNAME:AUTH_TOKEN -OJ https://vidicenter.quividi.com/api/v1/location/1982/download_scene_model/?scene_model_id=84321

A request returns an HTTP 400 response when no matching scene model exists.


Continue to :ref:`tags`
