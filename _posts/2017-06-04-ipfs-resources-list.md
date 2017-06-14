---
layout: post
title: IPFS(InterPlanetary File System) resources list
subtitle: 星际文件系统资源列表
image: /ico/icon-ipfs.png
tags:
  - decentration
comments:
  - author:
      type: full
      displayName: uraplutonium
      url: 'https://github.com/uraplutonium'
      picture: 'https://avatars2.githubusercontent.com/u/5404452?v=3&s=73'
    content: tessssst
    date: 2017-06-14T01:47:08.373Z
  - author:
      type: full
      displayName: uraplutonium
      url: 'https://github.com/uraplutonium'
      picture: 'https://avatars2.githubusercontent.com/u/5404452?v=3&s=73'
    content: this is a comment
    date: 2017-06-14T01:50:10.858Z
  - author:
      type: full
      displayName: uraplutonium
      url: 'https://github.com/uraplutonium'
      picture: 'https://avatars2.githubusercontent.com/u/5404452?v=3&s=73'
    content: ipfs ipfs
    date: 2017-06-14T01:57:25.623Z
  - author:
      type: github
      displayName: Veviz
      url: 'https://github.com/Veviz'
      picture: 'https://avatars0.githubusercontent.com/u/6380826?v=3&s=73'
    content: 'Test,test,test...'
    date: 2017-06-14T02:02:07.327Z

---

The InterPlanetary File System (IPFS) is a new hypermedia distribution protocol, addressed by content and identities.
IPFS enables the creation of completely distributed applications.
It aims to make the web faster, safer, and more open.
IPFS enables completely decentralized and distributed apps.
And it now supports fully dynamic apps, like real-time chat!

## Videos

### 1. [IPFS Alpha Demo 2015](https://gateway.ipfs.io/ipfs/QmeK22pqtVT4yNawXcHbZh2fDrncdYHQbepjrxFmD8tGYZ)

An overview and demo of the go-ipfs alpha.

### 2. [TED: The next Internet Revolution - Juan Benet](https://gateway.ipfs.io/ipfs/QmUgTn3nvJL1X5a1c7qgbPQw6Y6gCCTr9QTaUj5d8bkMBk)

Juan explains why the web is in danger and how there's an open source movement focused on creating a new way of sharing knowledge is building the Interplanetary Files System (IPFS).

### 3. [Distributed Apps with IPFS 2016](https://gateway.ipfs.io/ipfs/QmQM8y88GPFKjjYWZNJ8oznBYXjuH88JdmCeXgCTNprvgE)

This talk breaks down how to build a dynamic app on top of IPFS with CRDTs, pub/sub, and slick UIs. It also delves into new models for distributed computation, and the ethical importance of distributing the web.

### 4. [IPFS Hands on Introduction 2015](https://gateway.ipfs.io/ipfs/QmSntc8JNM2c9QUKsC1Jf98HAbWgWiPCCq45US9nqCMAfC)

This is a Hands on Introduction to IPFS.

### 5. [The Decentralized Web, IPFS and Filecoin 2016](https://gateway.ipfs.io/ipfs/QmZZupEgyT5iRsA13wToqYWVEzpujB5TeBJnUNkCdgDvfe)

Recorded at the Silicon Valley Ethereum Meetup - October 22nd, 2016


{% assign sorted_comments = (page.comments | sort: 'date') %}

{% for c in sorted_comments %}
  <div class="media">
    <div class="media-left">
      <img src="{{ c.author.picture }}" alt="{{ c.author.displayName}}" height="73" width="73">
    </div>
    <div class="media-body">
      <p class="text-muted">
        <a href="{{ c.author.url }}">{{ c.author.displayName }}</a>
        on {{ c.date | date_to_string }}
      </p>
      <p>{{ c.content | newline_to_br }}</p>
    </div>
  </div>
{% else %}
  There are no comments on this post.
{% endfor %}
