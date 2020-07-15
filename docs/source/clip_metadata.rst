.. _clip_metadata:


Clip Metadata
=============

The Clip Metadata api endpoint is useful for clients using the CMS content integration with VidiReports. With this feature, you provide VidiReports with clip ids, and you are able to see analytics based on these clip ids. Clip ids may not be explicit, or easy to use, that's why you can provide clip metadata to VidiCenter.


Important note about unicode and encoding
-----------------------------------------

If you want to upload unicode data, use an utf-8 encoding. If you are using curl, make sure your ``LANG`` environment variable is utf-8 compliant. For example:

 ::

    echo $LANG
    en_US.UTF-8


Create
------

Creates a Clip Metadata and returns its api url.

URL
***

``https://vidicenter.quividi.com/api/v1/clip/metadata/``

HTTP Method
***********

``POST``

Mandatory arguments
*******************

* ``clip_id``: The clip id linked to the metadata you are trying to create.
* ``network``: The network's id in which the clip is used.
* ``name``: The name of the clip, will be used in VidiCenter. Max length is 64 characters.
* ``clip_duration``: The duration of the clip, in seconds, between 0 and 999 with up to 2 decimal places.
* ``advertiser_name``: The advertiser name for the clip. Max length is 255 characters.

Optional arguments
******************

* ``description``: Description of the clip. No length limit.
* ``screenshot``: An image for the clip.
* ``video``: A video file for the clip.
* ``creative_agency_name``: The creative agency name for the clip. Max length is 255 characters.

Examples
********

Successful creation
^^^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN --form "screenshot=@path/to/local/image.jpg;filename=desired-filename-in-vidicenter.jpg" --form clip_id="clip_123" --form network=9876 --form name="My clip name" --form description="The description of my clip" --form clip_duration=10 --form advertiser_name "My favorite advertiser" https://vidicenter.quividi.com/api/v1/clip/metadata/
    {
        "url": "https://vidicenter.quividi.com/api/v1/network/9876/clip/metadata/clip_123/"
    }



Clip id is unique on a network
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN --form clip_id="clip_123" --form network=9876 --form name="Another clip name" https://vidicenter.quividi.com/api/v1/clip/metadata/
    > Response has a 400 status code with message: "Clip metadata with this Clip id and Network already exists"


Name is mandatory
^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN --form clip_id="clip_456" --form network=9876 https://vidicenter.quividi.com/api/v1/clip/metadata/
    > Response has a 400 status code with message: "name: This field is required"


List
----

Returns a list of your Clip Metadata.

URL
***

``https://vidicenter.quividi.com/api/v1/clip/metadata/``

HTTP Method
***********

``GET``

Example
*******

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/clip/metadata/
    [
        {
            "description": "The description of my clip",
            "screenshot": "http://vidicenter.quividi.com/media/clip_screenshots/my-filename_Y2W21O8.jpg",
            "network_id": 9876,
            "clip_id": "clip_123",
            "id": 3,
            "name": "My clip name"
        }
    ]


Detail
------

Returns information about a specific Clip Metadata.

URL
***

``https://vidicenter.quividi.com/api/v1/network/{network_id}/clip/metadata/{clip_id}/``

HTTP Method
***********

``GET``

Example
*******

 ::

    curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/network/9876/clip/metadata/clip_123/
    {
        "description": "The description of my clip",
        "screenshot": "http://vidicenter.quividi.com/media/clip_screenshots/my-filename_Y2W21O8.jpg",
        "network_id": 9876,
        "clip_id": "clip_123",
        "id": 3,
        "name": "My clip name"
    }


Update
------

Updates a specific Clip Metadata.

URL
***

``https://vidicenter.quividi.com/api/v1/network/{network_id}/clip/metadata/{clip_id}/``

HTTP Method
***********

``PUT``

Mandatory arguments
*******************

* ``name``: The name of the clip, will be used in VidiCenter. Max length is 64 characters.

Optional arguments
******************

* ``description``: Description of the clip. No length limit.
* ``screenshot``: An image for the clip.


Examples
********

Successful update
^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN -X PUT --form "screenshot=@another-image.jpg;filename=a-new-filename.jpg" --form name="Another name" --form description="Something else entirely" https://vidicenter.quividi.com/api/v1/network/9876/clip/metadata/clip_123/
    {}


Name is mandatory
^^^^^^^^^^^^^^^^^

 ::

    curl -u USERNAME:AUTH_TOKEN -X PUT --form "screenshot=@another-image.jpg;filename=a-new-filename.jpg" --form description="Something else entirely" https://vidicenter.quividi.com/api/v1/network/9876/clip/metadata/clip_123/
    > Response has a 400 status code with message: "name: This field is required"


Delete
------

Deletes a specific Clip Metadata.

URL
***

``https://vidicenter.quividi.com/api/v1/network/{network_id}/clip/metadata/{clip_id}/``

HTTP Method
***********

``DELETE``

Example
*******

 ::

    curl -u USERNAME:AUTH_TOKEN -X DELETE https://vidicenter.quividi.com/api/v1/network/9876/clip/metadata/clip_123/
    {}
