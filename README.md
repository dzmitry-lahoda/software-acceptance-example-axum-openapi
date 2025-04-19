# TLDR

`aide is more popular, less brittle, has less boilerplate, is more future-proof, and more composable for axum compared to utoipa.`

# Context

You have created an HTTP API over a simple domain in Rust using `axum`.

Simple means the API is over a non-recursive and non-graph-based data model with a static structure.

You want to provide documentation for TypeScript developers to use the API, so you write simple free-form text for that.

This leads to the following issues:
- Someone needs to maintain documentation to keep it up to date with the code.
- Someone needs to write TypeScript from that free-form documentation.

Writing documentation is brittle unless it is thoroughly reviewed for correctness, and writing TypeScript on top of potentially ambiguous descriptions requires experimentation.

CI does not catch any desynchronization between documentation, TypeScript, and backend code.

You want to **ensure that API documentation does not get out of sync with code and allows automating some TypeScript client generation**.

Additionally, you have a WebSocket API that shares some data shapes with HTTP.

## Solution

Use a formal language to describe the API.  
After research, you find that `OpenAPI 3.1` semantics fit most of your HTTP API well.  
Additionally, [OpenAPI is 100% JSON Schema adopted](https://www.openapis.org/blog/2021/02/18/openapi-specification-3-1-released), so you can use an industry-accepted, well-integrated JSON Schema format crate.

You need a way to define the API, definition not to be brittle and easy it easy to detect when the formal definition is out of sync with the Rust code.

## Variants

Searching the internet, you find that `aide` and `utoipa-axum` can help define OpenAPI for your HTTP API.

Let's look at each one:

### Utoipa

- Requires redefining `path`, `params`, and `method`, repeating what is already present.
- Uses its own schema instead of the industry standard, so you cannot leverage existing schema knowledge or WebSocket support.
- Requires repeating documentation on top of Rust codes.
- Return result data and HTTP codes are not compile-time enforced.
- Supports several web frameworks, which leads to a lowest-common-denominator approach and more boilerplate.

So, utoipa increases the amount of work needed to have API in sync, is brittle, not compile-time enforceable, and has poor DX.
All this makes inital goal to keep things in sync hard.

### Aide

- Uses `schemars`, which is the de facto standard to define OpenAPI 3.1 data (JSON Schema).
- A substantial part of your code already has `schemars` support.
- Best suited for axum, integrates with WebSockets and the router, does not require to redefine things just for docs.
- Allows errors to be composed and return bodies and HTTP codes to be checked, making errors typed.
- Aligns with Axum design, so it is easy to extend without forking.

**This results in less code to support, fewer chances for documentation to get out of sync with implementation or WebSockets, and the ability to extend using idiomatic axum approaches.**

## Result

Given these considerations, it is reasonable to use `aide`.

## Some stats

Utoipa targets 3 web frameworks, so stats should be weighted by the popularity of each, making aide(axum-only) framework stats equivalent to utoipa(or better).
Also single framework crates usually faster upgrade and adop features of target, which leads to neat seamless integration and getting fresh juice.

[![Star History Chart](https://api.star-history.com/svg?repos=tamasfe/aide,juhaku/utoipa&type=Date)](https://www.star-history.com/#tamasfe/aide&juhaku/utoipa&Date)

**aide**

![GitHub Repo stars](https://img.shields.io/github/stars/tamasfe/aide) ![GitHub contributors](https://img.shields.io/github/contributors/tamasfe/aide)  
[![crates.io version](https://img.shields.io/crates/v/aide.svg)](https://crates.io/crates/aide)
[![crates.io downloads](https://img.shields.io/crates/d/aide.svg)](https://crates.io/crates/aide)


**axum**

![GitHub Repo stars](https://img.shields.io/github/stars/juhaku/utoipa) ![GitHub contributors](https://img.shields.io/github/contributors/juhaku/utoipa)
[![crates.io version](https://img.shields.io/crates/v/utoipa-axum.svg)](https://crates.io/crates/utoipa-axum)
[![crates.io downloads](https://img.shields.io/crates/d/utoipa-axum.svg)](https://crates.io/crates/utoipa-axum)



