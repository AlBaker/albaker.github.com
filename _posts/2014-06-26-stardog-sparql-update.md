---
layout: post
title: "Stardog SPARQL Update"
description: "Quick how to for SPARQL Update with Stardog"
category:
tags: []
---
{% include JB/setup %}

Many folks have been getting started with [Stardog](http://www.stardog.com), and looking to take advantage of SPARQL 1.1 features, including SPARQL Update.  Stardog has supported SPARQL Update since the 2.0 release.  In addition to the standard HTTP support, the SNARL API supports executing update queries.  If you've been following Stardog and using SNARL with good ole select queries, then you'll be right at home with SPARQL Update.  From a SNARL Connection, you can get access to an UpdateQuery. You then pass in parameters you want to bind to variables, and execute it like you normally would.

{% gist AlBaker/956b641b9954e2031a2e %}


Enjoy!

