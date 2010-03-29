.. _member_api_get_feed:

===================
Member::Get Entries
===================

Request
=======

The Way to Request
------------------

Do GET to Collection URI.

::

  GET /api.php/feeds/member HTTP/1.1
  Host: example.com

Request Parameter
-----------------

* :ref:`q <search-query>` (optional)
* :ref:`updated-min <updated-query>` (optional)
* :ref:`updated-max <updated-query>` (optional)
* :ref:`published-min <published-query>` (optional)
* :ref:`published-max <published-query>` (optional)
* :ref:`start <start-query>` (optional)
* :ref:`max-results <max-requests-query>` (optional)
* :ref:`orderby <orderby-query>` (optional)
* :ref:`sortorder <sortorder-query>` (optional)
* uid (optional)
    - Filtering by Mobile UID
* email (optional)
    - Filtering by Mobile E-mail Address

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

Returns XML Document. It has entries.

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <feed xmlns="http://www.w3.org/2005/Atom"
          xmlns:openSearch="http://a9.com/-/spec/opensearchrss/1.0/">
    <title>Members</title>
    <id>http://example.com/member/{member_id}</id>
    <updated>2009-01-28T11:23:55+09:00</updated>
    <generator uri="http://www.openpne.jp/">OpenPNE</generator>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/member"/>
    <link rel="http://schemas.google.com/g/2005#feed" type="application/atom+xml" href="http://example.com/api.php/feeds/member"/>
    <openSearch:totalResults>Maximum Number of Results</openSearch:totalResults>
    <openSearch:startIndex>Starting Index of Entry</openSearch:startIndex>
    <openSearch:itemsPerPage>Number of per a Page</openSearch:itemsPerPage>
    <entry>
      <id>http://example.com/member/{member_id}</id>
      <published>2009-01-23T16:21:12+09:00</published>
      <updated>2009-01-23T16:21:12+09:00</updated>
      <title type="text">Nickname</title>
      <content type="xhtml">
        <div xmlns="http://www.w3.org/1999/xhtml">
          <div id="sex">Sex</div>
          <div id="birthday">Profile</div>
          <div id="self_intro">Self Intro</div>
        </div>
      </content>
      <author>
        <email>example@example.com</email>
      </author>
      <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/member/{member_id}"/>
      <link rel="alternate" type="text/html" href="http://example.com/member/{member_id}"/>
      <link rel="alternate" href="http://example.com/mobile_frontend.php/member/{member_id}"/>
      <link rel="enclosure" href="Member Image URL" />
    </entry>
    <entry>
      <id>http://example.com/member/{member_id}</id>
      <published>2009-01-23T16:21:12+09:00</published>
      <updated>2009-01-23T16:21:12+09:00</updated>
      <title type="text">Nickname</title>
      <content type="xhtml">
        <div xmlns="http://www.w3.org/1999/xhtml">
          <div id="sex">Sex</div>
          <div id="birthday">Profile</div>
          <div id="self_intro">Self Intro</div>
        </div>
      </content>
      <author>
        <email>example@example.com</email>
      </author>
      <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/member/{member_id}"/>
      <link rel="alternate" type="text/html" href="http://example.com/member/{member_id}"/>
      <link rel="alternate" href="http://example.com/mobile_frontend.php/member/{member_id}"/>
      <link rel="enclosure" href="Member Image URL" />
    </entry>
  </feed>
