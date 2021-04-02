---
title: Ruby
category: Ruby
tags: [Featured, WIP]
updated: 2020-12-30
weight: -4
---

Intro
-------------------------------------

### Getting a timestamp like Rails migration

```ruby
Time.now.utc.to_formatted_s(:number)  # using Rails

Time.now.utc.strftime("%Y%m%d%H%M%S") # using Ruby
```

Ransack
-------------------------------------

- [Ransack basic searching](https://github.com/activerecord-hackery/ransack/wiki/Basic-Searching)
