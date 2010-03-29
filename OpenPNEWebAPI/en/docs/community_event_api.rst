=======================
API for Community Event
=======================

Available Features
==================

.. toctree::
   :maxdepth: 1

   community_event_api/get_feed
   community_event_api/get_resource
   community_event_api/post_resource
   community_event_api/put_resource
   community_event_api/delete_resource

Entry Point
===========

Collection URI
    http://example.com/api.php/feeds/communityEvent/community/{ID of the community}

Member URI
    http://example.com/api.php/feeds/communityEvent/{ID of the community event}

Format of Entry Document
========================

An entry for a community event is described like the following Atom Entry Document:

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <id>http://example.com/communityEvent/{community_event_id}</id>
    <published>2009-01-25T19:40:22+09:00</published>
    <updated>2009-01-25T19:40:22+09:00</updated>
    <title type="text">Title</title>
    <content type="text">Body</content>
    <author>
      <name>Nickname of Author</name>
      <uri>http://example.com/member/{member_id}</uri>
    </author>
    <link rel="edit" type="application/atom+xml" href="http://example.com/api.php/feeds/communityEvent/{community_event_id}"/>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/communityEvent/1"/>
    <link rel="alternate" type="text/html" href="http://example.com/communityEvent/{community_event_id}"/>
    <link rel="alternate" href="http://example.com/mobile_frontend.php/communityEvent/{community_event_id}"/>
  </entry>

The following is explaining for items in this entry.

 * /entry/id : URN as ID for identifying the entry (optional)
 * /entry/published : Created date of the entry (optonal)
 * /entry/updated : Updated date of the entry (optional)
 * /entry/title : Title of the entry
 * /entry/content : Body of the entry
 * /entry/author : Informations about author of this community
    * /entry/author/name : Nickname
    * /entry/auhtor/uri : URN
 * /entry/link[@rel="edit"]/@href : URI for editing via OpenPNE Web API (optional)
 * /entry/link[@rel="self"]/@href : URI for accessing via OpenPNE Web API (optional)
 * /entry/link[@rel="alternate" and @type="text/html"]/@href : URI for accessing via PC (optional)
 * /entry/link[@rel="alternate" and not(@type)]/@href : URI for accessing via mobile (optional)
 * /entry/link[@rel="enclosure"]/@href : An image of the entry (optional)
