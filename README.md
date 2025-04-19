# TLDR;

`aide is more popular, less brittle, less boiler plate, more future proof, more composable for axum aganist utoipa`


# Context

You created HTTP API over simple domain in Rust over `axum`. 

Simple means API is over non recursive and non graph based data,
with static data model.

You want to provide some documentation for TypeScript develop to use API.
So you write simple free form text for that.

And got next issue:
- somebody needs to maintain docs to be up to code
- somebody need to write TypeScript from that free form

Writing docs is brittle as long as there is no deep review of them to be correct 
and writing TS on top of possibly ambiguild description requires experemnitation.

CI does not complains about any of desycn of docs, TypeScript and backend code.

So you want to `ensure that API documentation does not desyncs with code and allows to auotmate some TS client generation`.

Additionally you have WS APO which shares some data shaping with HTTP.

## Solution

Use some formal langauge to describe API.
After research you find that `OpenAPI 3.1` semantics fit most of you HTTP API well.
Additionally [openapi is 100% json schema adopted](https://www.openapis.org/blog/2021/02/18/openapi-specification-3-1-released),
so usage of industirally accepted well integrated JSON Schema format crate.

We need to have a way to define API not to be brittle and easy detect when formal defintion desyncs with code.


## Variants

We search internet, and fined that `aide` and `utoipa-axum` can help us define OpenAPI over our HTTP.

Let look into each one


### Utoipa

- requies to redefine `path`, `params`, `method`, repeating what already was  
- uses its own schema instead of industrial standard, so you cannot use existing schema dev knowledge outther, not WS support
- requires repeat docs on top of rust docs
- return results data and codes are not compile time imposed by
- supports several web frameworks, which leads to tendency to be lowest common denomincato or all of them, 
leading to worths ergonomics and large amount of boilerpalte

So utoipa increases amount of work needed to be done, is very brittle, not compile time enforsable and bad ergonomics.


### Aide

- uses `schemars` which is de facto standartd to define OpenAPI 3.1 data (JSON Schema)
- substantial part of our code already has `schemars` support,
- is best for axum, ingegrates with WS and router, not redefiniton of these just for docs
- allows to make errors to compose and check return bodies and codes, so making error typed.
- goes in line with Axum desing, so easy to extend without forking

**So we get less code written for supprot and less chances docs will desync with implementation, WS,
and ability to extend using idiomatic axum approach**

## Result

We reasonable people and just use `aide`

## Some stats 

- utoipa targets 3 web frameworks, so stats to be divided by populatiry of each, that makes axum only framework start euqivalent to utoipa

[![Star History Chart](https://api.star-history.com/svg?repos=tamasfe/aide,juhaku/utoipa&type=Date)](https://www.star-history.com/#tamasfe/aide&juhaku/utoipa&Date)

![GitHub Repo stars](https://img.shields.io/github/stars/juhaku/utoipa) ![GitHub contributors](https://img.shields.io/github/contributors/juhaku/utoipa)


![GitHub Repo stars](https://img.shields.io/github/stars/tamasfe/aide) ![GitHub contributors](https://img.shields.io/github/contributors/tamasfe/aide)  


[![crates.io version](https://img.shields.io/crates/v/aide.svg)](https://crates.io/crates/aide)
[![crates.io downloads](https://img.shields.io/crates/d/aide.svg)](https://crates.io/crates/aide)


[![crates.io version](https://img.shields.io/crates/v/utoipa-axum.svg)](https://crates.io/crates/utoipa-axum)
[![crates.io downloads](https://img.shields.io/crates/d/utoipa-axum.svg)](https://crates.io/crates/utoipa-axum)



