---
title: Download Schema with makefile
layout: post
date: 2020-06-16 23:03:19
categories: GraphQL Network
---

## 🤔  How do I have been downloading this painful schema for graphql?

I have been doing makefiles for generate my `schema.json` files, by calling the `apollo` CLI.

A sample of `makefile` I have been using so far.

```makefile
default: help

TOKEN ?= ""
ENDPOINT ?= "https://app.pipefy.com/queries"

## bootstrap:                  Build app
schema:
	@apollo schema:download Sources/queries_api.json --endpoint=$(ENDPOINT) --header="Authorization: Bearer $(TOKEN)"

.PHONY: help
help : Makefile
	@sed -n 's/^##//p' $<
```
