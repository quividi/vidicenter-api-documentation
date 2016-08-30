General usage
=============


Authentication
##############

VidiCenter's REST protocol is stateless and therefore **you have to provide an authentication token with each request**. Authentication relies on the HTTP Basic Auth method, with your VidiCenter username as username and a unique and user-dependent token as the password. So, for example, using curl you would use the following syntax for each request::

    $> curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/.../


You can obtain your user token from your `VidiCenter profile page <http://vidicenter.quividi.com/next/profile>`_. The page will show your current active token and possibly a list of revoked tokens. Keep your token secure; you can revoke an active token if you think that it has been leaked or compromised. Once revoked, the token is forever inactive and you can generate a new one.


Do note that making an insecure call (using http instead of https for example) will **revoke your token**. You will need to go into VidiCenter and generate a new one.


You can test your token with the following query::

    $> curl -u USERNAME:AUTH_TOKEN https://vidicenter.quividi.com/api/v1/whoami
    {"username": "USERNAME"}


HTTP responses
##############

All successful requests will return a JSON object. If the request fails, the HTTP response code should be used to monitor the cause of error. A response code of 200 will indicate a correctly executed exchange, other possible codes follow the HTTP RFC:

* Unauthorized access will return a 401
* Forbidden access will return a 403
* etc.

In case of error, the response can contain a JSON object explaining the issue::

    $> curl -u USERNAME:INVALID_TOKEN https://vidicenter.quividi.com/api/v1/whoami
    {"message": "Invalid API Token", "error": 401}


Test page
#########

`This page on VidiCenter <http://vidicenter.quividi.com/api/v1/test/>`_ allows you to generate curl commands and query the export API. You can use it to famliarize yourself with the API.

