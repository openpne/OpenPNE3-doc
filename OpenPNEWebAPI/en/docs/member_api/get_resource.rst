.. _member_api_get_resource:

=================
Member::Get Entry
=================

Request
=======

The Way to Request
------------------

Do GET to Member URI.

::

  GET /api.php/feeds/member/{member_id} HTTP/1.1
  Host: example.com

Request Parameter
-----------------

None

Response
========

Response Header
---------------

Returns HTTP Status Code 200.

::

  HTTP/1.1 200 Ok
  Content-Type: application/atom+xml; charset=utf-8

Response Body
-------------

Returns XML Document.

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <id>http://example.com/member/{member_id}</id>
    <published>2009-01-23T16:21:12+09:00</published>
    <updated>2009-01-23T16:21:12+09:00</updated>
    <title type="text">Nickname</title>
    <content type="xhtml">
      <div xmlns="http://www.w3.org/1999/xhtml">
        <div id="sex">Sex</div>
        <div id="birthday">Birthday</div>
        <div id="self_intro">Self Introduction</div>
      </div>
    </content>
    <author>
      <email>example@example.com</email>
    </author>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/member/{member_id}"/>
    <link rel="alternate" type="text/html" href="http://example.com/member/{member_id}"/>
    <link rel="alternate" href="http://example.com/mobile_frontend.php/member/{member_id}"/>
    <link rel="enclosure" href="URL for Profile Image" />
  </entry>

