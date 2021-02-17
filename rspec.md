---
title: Rspec
category: Ruby
tags: [WIP]
updated: 2020-12-30
weight: -3
---

Intro
-------------------------------------

Tags
-------------------------------------

```ruby
RSpec.describe RequestsController, type: [:controller, :feature] do
    it "is a feature" do |example|
        expect(example.metadata[:type]).to include(:ccb)
    end
end
```

```shell
# Run all tests with the tag type:feature
rspec -t type:feature
# Run all tests EXCEPT the ones with tag type:feature
rspec -t -type:feature
```
