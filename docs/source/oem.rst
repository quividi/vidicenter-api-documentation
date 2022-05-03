.. _oem:


OEM
===

OEM API: provisioning integration
---------------------------------

The OEM API is a way for player vendors to integrate their software provisioning system with VidiReports, so that pre-deployed VidiReports installations can be tied to player installations without additional user intervention. It actually consists of two APIs, one on the edge (VidiReports) and one on the cloud (VidiCenter). The VidiCenter part is documented here. The full documentation is available at https://vidicenter.quividi.com/vrmanual/oemapi.html

The cloud API point will typically be called from the vendor’s backend, upon request from the user to enable Quividi audience measurement on a player. It is an **HTTP POST** call at the following URL:


URL
---

``https://vidicenter.quividi.com/api/v1/oem/register/``

Mandatory arguments
-------------------

* ``vendor_id``: the vendor id

* ``vendor_player_id``: the player id

* ``vendor_network_id``: An arbitrary identifier for the “network”, or any such topological notion available on the vendor’s backend, to which this player belongs.

The vendor_network_id identifier may be used in subsequent calls to the Charts API.


Response's form
---------------

This api endpoint returns a JSON dictionary containing the result of the call (success or failure).
