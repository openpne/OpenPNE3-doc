===============================
API for Community Topic Comment
===============================

Available Features
==================

.. toctree::
   :maxdepth: 1

   community_topic_comment_api/get_feed
   community_topic_comment_api/get_resource
   community_topic_comment_api/post_resource
   community_topic_comment_api/delete_resource

Entry Point
===========

Collection URI
    http://example.com/api.php/feeds/communityTopicComment/communityTopic/{ID of the community}

Member URI
    http://example.com/api.php/feeds/communityTopicComment/{ID of the community event}

Format of Entry Document
========================

An entry for a community topic comment is described like the following Atom Entry Document:

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <id>http://example.com/communityTopicComment/{community_topic_comment_id}</id>
    <published>2009-01-25T19:40:22+09:00</published>
    <updated>2009-01-25T19:40:22+09:00</updated>
    <content type="text">Comment Body</content>
    <author>
      <name>Nickname of Author</name>
      <uri>http://example.com/member/{member_id}</uri>
    </author>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/communityTopicComment/{community_topic_comment_id}"/>
  </entry>


The following is explaining for items in this entry.

 * /entry/id : URN as ID for identifying the entry (optional)
 * /entry/published : Created date of the entry (optonal)
 * /entry/updated : Updated date of the entry (optional)
 * /entry/content : Body of the entry
 * /entry/author : Informations about author of this community
    * /entry/author/name : Nickname
    * /entry/auhtor/uri : URN
 * /entry/link[@rel="self"]/@href : URI for accessing via OpenPNE Web API (optional)
