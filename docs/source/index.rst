Welcome to Quividi VidiCenter's API documentation
=================================================

Quividi's customers can access their data on VidiCenter via a Web service:

* Data is requested by accessing https://vidicenter.quividi.com/api/
* Data is returned in the JSON format
* Security is provided by token-based authentication

Access to VidiCenter's REST API is an optional service that you must request explicitly to Quividi; only
authorized customers will be able to generate the necessary authentication tokens.

Please be aware that any abuse of service (excessive number of requests per minute, repeated invalidation of the cache, etc.) will affect the quality of service experienced by all VidiCenter users; in this regard, Quividi reserves the right to disable access to the REST interface upon violation of the terms of service.

If we feel the volume of your requests may impact the performance of our service, the API may return responses with the 429 HTTP status code.

Contents:

.. toctree::
   :maxdepth: 2

   general_usage
   topology
   status
   tags
   data
   clip_metadata


.. Indices and tables
.. ==================

.. * :ref:`genindex`
.. * :ref:`modindex`
.. * :ref:`search`
