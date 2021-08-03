---
id: 20210803110115
uuid: cbe77241-a9e4-4c81-b796-64433017dfa7
title: Typesense Cheatsheet
date: 2021-08-03 11:01:15
modified: 
types: tech-note
categories: typesense
pinned: false
tags: [search, typesense, cheatsheet]
private: false
draft: false
---

```sh
# Run container:
docker run -p 8108:8108 -v/tmp:/data typesense/typesense:0.21.0 --data-dir /data --api-key=xyz

# Create collection:
curl --insecure --request POST 'http://localhost:8108/collections' --header 'content-type: application/json' --header "X-TYPESENSE-API-KEY: xyz" --data-binary '{
    "name": "log",
    "fields": [
        { "name": "id", "type": "string" },
        { "name": "uuid", "type": "string" },
        { "name": "title", "type": "string" },
        { "name": "date", "type": "string" },
        { "name": "tags", "type": "string[]" },
        { "name": "url", "type": "string" },
        { "name": "content", "type": "string" }
    ]
}' | jq '.'

# List collection:
curl --header "X-TYPESENSE-API-KEY: xyz" "http://localhost:8108/collections" | jq '.'

# Get Index:
wget --no-check-certificate https://log.ark.lan/index.json --output-document=log.json

# Convert to JSONL:
cat log.json | jq --compact-output '.[]' > log.jsonl

# Import document:
curl --insecure --header "X-TYPESENSE-API-KEY: xyz" --header 'content-type: application/json' --request POST "http://localhost:8108/collections/log/documents/import?action=create" --data-binary @log.jsonl | jq '.'

# Drop collection:
curl --header "X-TYPESENSE-API-KEY: xyz" --request DELETE "http://localhost:8108/collections/log" | jq '.'

# Search:
curl --header "X-TYPESENSE-API-KEY: xyz" "http://localhost:8108/collections/log/documents/search?q=<query>&query_by=title" | jq '.'
```
