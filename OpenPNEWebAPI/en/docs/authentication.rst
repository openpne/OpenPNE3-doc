==============
Authentication
==============

OpenPNE Web API has support for multiple authentication ways. In default, OAuth is available. Also, OAuth is recommended way, please use that except that you have some reason.

You can change the authentication way, in configuration page of opWebAPIPlugin that is in "プラグイン設定" of the pc_backend application.

Authentication by OAuth
=======================

OAuth is an open authorization protocol for handling data of web application via API. If you want to access OpenPNE Web API with using OAuth, you need to register Consumer. Please read document for OAuth of OpenPNE, and register Consumer.

In this way, Access Token is sent with accessing OpenPNE Web API. In OAuth of OpenPNE suggests the two ways; containing Access Token in request parameter, and containing that in Authorization header. OpenPNE Web API needs to use request body for Atom Entry Document, so containing in Authorization header is too recommended.

Describing OAuth is beyond the scope of this document. Please read the document of OAuth of OpenPNE to know that, and try authenticating.

If you authenticate by OAuth, your accessable resources are restricted by Access Token type. If the Access Token is authorized by administorator, you can do anything of using OpenPNE Web API. If the Access Token is authorized by normal member, you get restriction that islike that member has.

If you want to get information about using OAuth, visit : http://sandbox.ebihara.dazai.pne.jp/oauth.en.html

Authentication by IP Address (not-recommended)
==============================================

In this way, OpenPNE Web API is usable from client that comes from allowed IP address.

If an IP address are allowed, client can do anything of using OpenPNE Web API.

Allowing IP Address
-------------------

You can allow IP addresses in configuration page of opWebAPIPlugin that is in "プラグイン設定" of the pc_backend application.

Pre-Allowed IP Address
----------------------

In default, 127.0.0.1 is permitted.
