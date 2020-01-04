---
id: 20200104152016
uuid: 52c9bf0b-9ff2-4da8-86df-9229a412fcaf
title: Convert Python Dictionary to JSON
date: 2020-01-04 15:20:16
modified: 
types: tech-note
categories: python
pinned: false
tags: [python, dict, json]
private: false
draft: false
---

``` python
import json

example_dictionary = {
    'query': 'Genesis 1:1',
    'canonical': 'Genesis 1:1',
    'parsed': [[1001001, 1001001]],
    'passage_meta': [{
        'canonical': 'Genesis 1:1',
        'chapter_start': [1001001, 1001031],
        'chapter_end': [1001001, 1001031],
        'prev_verse': None,
        'next_verse': 1001002,
        'prev_chapter': None,
        'next_chapter': [1002001, 1002025]
        }],
    'passages': ['The Creation of the World\n\n  In the beginning, God created the heavens and the earth. (ESV)']
    }

example_json = json.dumps(example_dictionary, sort_keys=True, indent=4)
print(example_json)
```
