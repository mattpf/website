---
layout: page
title: "auralsex"
comments: true
sharing: true
footer: true
---
{% img center /projects/auralsex/hero.png center %}
Auralsex was developed in 2010 as a multi-zone audio system for [Beast][].
It was written as the successor to the Aural Sex system developed four years prior, but *torn 
out of the ceiling* by cleaning ladies. If you happen to live on Beast, you can access it
[here](https://auralsex.mit.edu/).

Auralsex was a collaboration between **atw**, **cjfman**, **tshen2** and myself. I was primarily
involved in writing software; everyone else devised hardware for the software to actually
run on.

Auralsex is split into server and client systems, as follows:

The Server
----------

The server runs on machines hooked up to the output speakers. It is a very simple python wrapper
around [mplayer][], providing an HTTP interface to control music output, volume, etc. The webserver
is based on Python's [BaseHTTPServer][].

The source is available [on Github](https://github.com/mattpf/auralsex-server).

The Client
----------

The client is a web app based on [ExtJS][], using a [cherrypy][] backend. UI features include:

- Control of any auralsex zone
- A searchable listing of all available music
- Personal playlists for each user
- Full queue manipulation – addition, removal, reordering, etc. – by drag-and-drop
- Provides previews of music through your web browser using HTML5 `<audio>`.
- Integrated with [Athena][]:
  - Uses moira lists and MIT client certificates for authentication
  - Zephyrs queue change information to `-c auralsex`

As run in production, auralsex-client is run via fcgi behind [lighttpd][], which also handles
certificate authentication.

The source is available [on Github](https://github.com/mattpf/auralsex-client).

[Beast]: http://mit.edu/beast/www/
[mplayer]: http://www.mplayerhq.hu/
[BaseHTTPServer]: http://docs.python.org/library/basehttpserver
[ExtJS]: http://www.sencha.com/products/extjs/
[cherrypy]: http://cherrypy.org/
[Athena]: http://ist.mit.edu/services/athena
[lighttpd]: http://www.lighttpd.net/
