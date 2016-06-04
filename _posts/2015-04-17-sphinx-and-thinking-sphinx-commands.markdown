---
layout: post
title:  Sphinx and Thinking Sphinx commands
date:   2015-04-17 19:35:03 -0500
comments: true
keywords: sphinx thinking sphinx
description: Sphinx and Thinking Sphinx commands
---

![Sphinx]({{ site_url }}/assets/sphinx.png){: .center-image }

For installing Sphinx and setting up Thinking Sphinx for a Ruby on Rails project, go to this link: [http://pat.github.io/thinking-sphinx/](http://pat.github.io/thinking-sphinx/)

Basic commands to start, stop and index sphinx

```
rake ts:stop
rake ts:index
rake ts:start
rake ts:rebuild (stops, re-index and start)
```

Other options

```
# rake ts:index generates the configuration file every time
# if you do not want to create the configuration file
# run the below
INDEX_ONLY=true rake ts:index

# to generate the configuration file without indexing
rake ts:configure
```

Querying options

```
# To query a specific model
query = “Ruby on Rails”
articles = Article.search(query)
posts = Post.search(query)
books = Book.search(query)

# To query all the models
tss = ThinkingSphinx.search(query)

# Sphinx paginates search results by default.
# And returns 20 results by default
articles = Article.search(query)
articles.size          (returns 20 by default)
articles.total_entries (returns total entries in the search)

# You can request more results by passing the limit to search
articles = Article.search(query, per_page: 200)
```

There might be a situation where we want to re-index just one index. There is not a way to do it via ThinkingSphinx. rake ts:index will reindex all the indexes in the app/indices directory. This can be done via Sphinx’s indexer tool.

```
indexer article_core --rotate
```

Other helpful command line tools

```
# dump index header by index name
indextool --dumpheader article_core
```

The sphinx commands like `indexer`, `indextool` look for a config file in `/etc/sphinxsearch/sphinx.conf` or `./sphinx.conf`. It would be easy to create a sym link in the root path

```
ln -s PATH-TO/production.sphinx.conf ./sphinx.conf
```
