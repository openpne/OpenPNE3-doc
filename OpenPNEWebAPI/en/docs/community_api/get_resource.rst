.. _community_api_get_resource:

====================
Community::Get Entry
====================

Request
=======

The Way to Request
------------------

Do GET to Member URI.

::

  GET /api.php/feeds/community/{community_id} HTTP/1.1
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
    <id>http://example.com/diary/{diary_id}</id>
    <published>2009-01-25T19:40:22+09:00</published>
    <updated>2009-01-25T19:40:22+09:00</updated>
    <title type="text">Title</title>
    <content type="text">Body</content>
    <author>
      <name>Nickname of Author</name>
      <uri>http://example.com/member/{member_id}</uri>
    </author>
    <link rel="edit" type="application/atom+xml" href="http://example.com/api.php/feeds/diary/{diary_id}"/>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/diary/{diary_id}"/>
    <link rel="alternate" type="text/html" href="http://example.com/diary/{diary_id}"/>
    <link rel="alternate" href="http://example.com/mobile_frontend.php/diary/{diary_id}"/>
  </entry>
