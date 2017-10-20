#HSLIDE

## Docker container for <span style="color:#e49436">MySQL</span> Master/Slave
#### Without the Frustration 
(Thanks to <span style="color:#e49436">Ansible</span>)

#VSLIDE?image=assets/images/cali.jpg

### <span style="color:#37342e; background-color:#0e2a3500">Daniel Guzman Burgos</span> 
##### <span style="color:#37342e">electronic engineer</span> 
##### <span style="color:#37342e">Tech Lead MySQL at Percona in Cali, Colombia</span> 
##### <span style="color:#37342e">Managed services POD Team</span> 

#VSLIDE

## What is Percona?

- The only company that delivers enterprise-class support, consulting, managed services and software <!-- .element: class="fragment" data-fragment-index="1" -->
- MySQL® <!-- .element: class="fragment" data-fragment-index="2" -->
- MariaDB® <!-- .element: class="fragment" data-fragment-index="2" -->
- MongoDB® <!-- .element: class="fragment" data-fragment-index="2" -->
- And other open source databases across on-premise and cloud-based platforms. <!-- .element: class="fragment" -->
- 3000 clients worldwide / Employs a global network of experts with a staff of over 140 people <!-- .element: class="fragment" -->

#HSLIDE

# Percona Server and MySQL

#VSLIDE

## What is Percona Server?

Percona Server for MySQL® is a free, fully compatible, enhanced, open source drop-in replacement for MySQL 

<p style="max-width: 70%; margin: 0 auto;">
![Percona Server](assets/images/percona-server-web.png)
</p>

#VSLIDE

## What is MySQL?

- Is an open-source relational database management system (RDBMS) <!-- .element: class="fragment" data-fragment-index="1" -->
- Is THE RDBMS <!-- .element: class="fragment" data-fragment-index="2" -->
- Used by far more companies that you expect <!-- .element: class="fragment" data-fragment-index="2" -->
- With more features than you can use (or not :) ) <!-- .element: class="fragment" -->
- Being the Replication, the feature that we need to put focus in here. <!-- .element: class="fragment" -->

#VSLIDE

```bash
mysql> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: mysql1
```

![Replication](assets/images/replication.png)

<!--
MySQL’s built-in replication is the foundation for building large, high-performance applications on top of MySQL, using the so-called “scale-out” architecture. Replication lets you configure one or more servers as replicas1 of another server, keeping their data synchronized with the master copy. This is not just useful for high-performance applications—it is also the cornerstone of many strategies for high availability, scala- bility, disaster recovery, backups, analysis, data warehousing, and many other tasks. In fact, scalability and high availability are related topics.
-->

#HSLIDE

# Docker

<!--
As with any project, the first thing to do is gather information. Figure out what you want to do with your new API version. Chances are, there are some things about the current version you aren’t too happy with, so start by finding better ways and identifying problems you didn’t even realize existed.
-->

#VSLIDE?image=assets/images/turtles.jpg

## Stand on the Shoulders of Giants

<!--
APIs are not new; people have been working on this stuff for decades. Especially when the goal of your technology is to interoperate, it pays to leverage the best practices happening in the space.
-->

#VSLIDE?video=assets/videos/amber-feng.mp4

## Conferences and Talks

<figcaption>https://www.heavybit.com/library/video/move-fast-dont-break-api/</figcaption>
<figcaption>http://amberonrails.com/move-fast-dont-break-your-api/</figcaption>

<!--
Talks are a great way to get ideas and get out of your day-to-day headspace.
You’re here, so you’re off to a good start!
This talk by Amber Feng at Stripe, entitled "Move Fast, Don’t Break the API", set me up with some great ideas about how to manage multiple versions of an API on a technical level.
-->

#VSLIDE?image=assets/images/dropbox-blog.png

## Developer Blogs

<figcaption><a href="https://blogs.dropbox.com/developers/2015/04/a-preview-of-the-new-dropbox-api-v2/" style="color: white">https://blogs.dropbox.com/developers/2015/04/a-preview-of-the-new-dropbox-api-v2/</a></figcaption>

<!--
A large part of your change is going to involve publicity. So read up, and find good work by others in the space.
This post by Leah Culver at Dropbox was a great example of how to be concise and helpful when talking about a new API version.
-->

#VSLIDE

### Protocols and Hypermedia Types

- HAL: http://stateless.co/hal_specification.html
- OData: http://www.odata.org
- Collection+JSON: https://github.com/collection-json/spec
- JSON-API: http://jsonapi.org
- JSON-LD: http://json-ld.org
- GData: https://developers.google.com/gdata/
- GraphQL: http://graphql.org
- Siren: https://github.com/kevinswiber/siren

<figcaption>https://sookocheff.com/post/api/on-choosing-a-hypermedia-format/</figcaption>

<!--
Protocols/media types like OData, JSON API, GData, or GraphQL, Siren have done a lot of the standardization work for you so there are fewer decisions left to be made, but be aware of the tradeoffs. Remember that REST is an architectural style, not a protocol — you’ll end up developing your own. I said "fewer" because you’ll still need to make application-specific choices, but reducing the scope of these can help maintain sanity.
-->

#HSLIDE

# Ansible

#VSLIDE?image=assets/images/bike-shed.jpg

## Make decisions.

<!--
Design requires decision-making. Especially when it comes to things that on the surface appear purely aesthetic (like an API), everyone will have input.
In some technical areas, it can be quite valuable to have a decision maker whose head is in the game and can make a final call and move on.
Even better, outsource your decisions if possible.
-->

#VSLIDE?image=assets/images/pygerduty.png

## Go to the Source

<figcaption><a href="https://github.com/dropbox/pygerduty" style="color: white">https://github.com/dropbox/pygerduty</a></figcaption>

<!--
Go to the source — literally. We’re fortunate enough to have a large community of developers who write and publish code for talking to the PagerDuty APIs in a variety of languages. Looking at this code can give you a fresh perspective on how developers are interpreting your API’s documentation and patterns, helping you look for where developers need to jump through hoops and you could perhaps provide a better experience.
-->

#VSLIDE?image=assets/images/pour-over.jpg

## Pour over logs

- which endpoints aren’t being used?
- which endpoints are being used in ways they shouldn’t?
- which customers are using the APIs?

<!--
A big advantage you have over developing a brand new API is tons of data about your existing API and how people use it. So leverage that data — go through your logs and build reports on which endpoints are popular, which are neglected, what the common patterns are of requests being made, what developers are trying to do with your API that appears painful.
Talking to your developers directly is valuable, don’t neglect that; but make sure you take a look at the big overall picture so you don’t end up narrowing in on the specifics of certain developers.
-->

#VSLIDE?gist=2f939ec894dfadc749bd1ac4b74c3d13

<!--
Here's one example of a pattern established and supported by the usage data.
-->

#VSLIDE?gist=e33e5d401e5f8fa9e260ba1b7a20bdd2

<!--
The result was this: an "API v2 vision" that describes the goals of the new API version and the specifics of the patterns we were to formalize.
-->

#VSLIDE

## Pick your Battles

- Don’t change what works <!-- .element: class="fragment" -->
- Prioritize <!-- .element: class="fragment" -->
- Keep bullets away from feet <!-- .element: class="fragment" -->

<!--
When you’re working on a new version of your API, you’re fighting a war on two fronts: your developers will need to work to adopt, and your company will need to provide the time and resources to make it happen. So focus only on what’s most important and will make a meaningful difference for yourselves and your developers.
At PagerDuty, our API is a key competitive differentiator. When we can say to customers, "sure, you can do that with the API", it has real results. Building an ecosystem makes those conversations even easier; enabling developer productivity is the #1 goal.
But there’s a secondary motivation. Designing APIs is tough. Designing an API to last 10 years is nearly impossible without change. You need to address the decisions that were fine when they were made but are now causing performance, scaling, or quality issues.
-->

#VSLIDE

### header- and key-based versioning

```
Accept: application/vnd.pagerduty+json;version=2
```

![API Key Creation](assets/images/api-key-creation.png)

<figcaption>http://www.iana.org/assignments/media-types/application/vnd.pagerduty+json</figcaption>

#VSLIDE

### canonical API subdomain

##### ~~initech.pagerduty.com~~
##### ~~acmemonitoring.pagerduty.com~~
##### ~~evilcorp.pagerduty.com~~
### api.pagerduty.com

#VSLIDE

### use standards

- respect HTTP semantics
- ISO 8601
- IANA tzinfo

#VSLIDE

### deprecate basic auth

![Basic Auth Use](assets/images/basic-auth-use.png)

#VSLIDE

### performance tweak

```json
{
  "incidents": [{
    "id": "P1R8NC6",
    "type": "incident",
    "summary": "[#7893] There are 2 Charizard nearby!",
    "self": "https://api.pagerduty.com/incidents/P1R8NC6",
  }],
  "limit": 1,
  "offset": 0,
  "total": null, //don't care about this
  "more": true //more resources available
}
```

#VSLIDE

### consistency

![Resource References](assets/images/resource-references.png)

#VSLIDE

## Change the Plan

![Changed Includes](assets/images/change-proposal.png)

<!--
As you develop, things will change. You’ll find where your patterns break down. You’ll see where you need something different. Other smart people will question your choices.
Don’t ignore these inputs; adapt to them. Have a process that encourages change instead of blocking it.
-->

#VSLIDE?image=assets/images/api-concerns-proposal.png

## Process is Paramount

<!--
The most difficult part of maintaining an API is adapting to change. We need to strike a balance between building new and shiny things and preventing thrash for our developers. As consistency is one of the main tenants of our API, a big challenge was, and remains, keeping new development in line with the API goals without standing in the way of progress.
Originally, we had an email list that we’d use to discuss new API changes. It was common knowledge among the engineers that if you wanted to make an API change, it would need to be proposed to the email group where our API Czar sat. But this was slow, cumbersome, and difficult to make sure the right discussion happened and translated into the proper course of action.
-->

#VSLIDE?gist=2109c70ecff5b71d8bc0a072760b03fe

<!--
So we used a tool that our developers were already familiar with: GitHub. Contributors can now write in Markdown, comment on specific lines within the proposal, and have the back-and-forth type of review discussion that GitHub facilities. Familiar GitHub semantics like merging a pull request to indicate finality and version tracking to see how a proposal evolved gives much more context of the past and current state of a proposal. Plus, nice things like the abilitiy to link directly to something being discussed.
-->

#VSLIDE

![Flagger in Action](assets/images/flagger.png)
<figcaption>https://github.com/PagerDuty/flagger</figcaption>

<!--
In practice, this still needs refinement. One of the big challenges is making sure that people are doing this diligence when they need to be. It’s difficult enough to get dozens of engineers to buy into a new process; even moreso when they’re often unaware they need to participate.

Here’s one thing we experimented with: using knowledge of our Rails codebase to flag pull requests making changes to API code for further review.
-->

#HSLIDE

# Hands on!

#VSLIDE?image=assets/images/maintainable.jpg

## Keep it maintainable

#HSLIDE

# Appendix

#VSLIDE

## Image Credits

- "Steve Rice" By Steve Rice © 2016 (Own work), all rights reserved
- "Stand on the Shoulders of Giants" By Samir Eberlin [CC0], via Wikimedia Commons — https://commons.wikimedia.org/wiki/File:Turtles.on.a.stone.in.brazil.jpg
- "Make Decisions." By SeppVei (Own work) [CC0], via Wikimedia Commons — https://commons.wikimedia.org/wiki/File:Bicycle_shed.JPG
- "Be Pragmatic" By Liveon001 © Travis K. Witt (Own work) [CC BY-SA 3.0 (http://creativecommons.org/licenses/by-sa/3.0) or GFDL (http://www.gnu.org/copyleft/fdl.html)], via Wikimedia Commons — https://commons.wikimedia.org/wiki/File:Farfalle_Pasta.JPG
- "Dogfood. Everything." Original source not found. Used without permission.
- "Keep it maintainable" By David Jones from Isle of Wight, United Kingdom [[CC BY 2.0](http://creativecommons.org/licenses/by/2.0)], via Wikimedia Commons — https://commons.wikimedia.org/wiki/File:Mike_O%27Callaghan-Pat_Tillman_Bridge_construction.jpg
- "Pour over logs" by Timothy Marsee [[CC BY 2.0](http://creativecommons.org/licenses/by/2.0)], via Flickr — https://www.flickr.com/photos/tmarsee530/9899684046
- "BFF" By Guillaume Paumier (Own work) [CC BY-SA 3.0 (http://creativecommons.org/licenses/by-sa/3.0)], via Wikimedia Commons — https://commons.wikimedia.org/wiki/File:Best_Frends_Forever_-_Golden_Gate_bridge_guard_rail_166.jpg
- "Fin?" Copyright 1984 Warner Bros. Pictures, via Cinema Autopsy — https://blog.cinemaautopsy.com/2013/04/02/film-review-the-neverending-story-1984/
