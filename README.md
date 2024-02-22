# VectorDB.js

<img src="public/logo.png" alt="VectorDB.js — Simple in-memory vector database for Node.js" class="logo" />

<div class="badges" style="text-align: center; margin-top: -10px;">
<a href="https://github.com/themaximal1st/vectordb.js"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/themaximal1st/vectordb.js"></a>
<a href="https://www.npmjs.com/package/@themaximalist/vectordb.js"><img alt="NPM Downloads" src="https://img.shields.io/npm/dt/%40themaximalist%2Fvectordb.js"></a>
<a href="https://github.com/themaximal1st/vectordb.js"><img alt="GitHub code size in bytes" src="https://img.shields.io/github/languages/code-size/themaximal1st/vectordb.js"></a>
<a href="https://github.com/themaximal1st/vectordb.js"><img alt="GitHub License" src="https://img.shields.io/github/license/themaximal1st/vectordb.js"></a>
</div>
<br />

`VectorDB.js` is a simple in-memory vector database for Node.js. It's an easy way to do text similarity. 

-   Works 100% locally and in-memory by default
-   Uses [hnswlib-node](https://github.com/yoshoku/hnswlib-node) for simple vector search
-   Uses [Embeddings.js](https://embeddingsjs.themaximalist.com) for simple text embeddings
-   Supports OpenAI, Mistral and local embeddings
-   Caches embeddings
-   Automatically resizes database size
-   Store objects with embeddings
-   MIT license


## Installation

Install `VectorDB.js` from NPM:

```bash
npm install @themaximalist/vectordb.js
```

For local embeddings, install the transformers library:

```bash
npm install @xenova/transformers
```

For remote embeddings like OpenAI and Mistral, add an API key to your environment.

```bash
export OPENAI_API_KEY=...
export MISTRAL_API_KEY=...
```

## Usage

To find similar strings, add a few to the database, and then search.

```javascript
import VectorDB from "@themaximalist/vectordb.js"
const db = new VectorDB();

await db.add("orange");
await db.add("blue");

const result = await db.search("light orange");
// [ { input: 'orange', distance: 0.3109036684036255 } ]
```


## Embedding Models

By default `VectorDB.js` uses a local embeddings model.

To switch to another model like OpenAI, pass the service to the `embeddings` config.

```javascript
const db = new VectorDB({
  dimensions: 1536,
  embeddings: {
    service: "openai"
  }
});

await db.add("orange");
await db.add("blue");
await db.add("green");
await db.add("purple");

// ask for up to 4 embeddings back, default is 3
const results = await db.search("light orange", 4);
assert(results.length === 4);
assert(results[0].input === "orange");
```

With Mistral Embeddings:

```javascript
const db = new VectorDB({
  dimensions: 1536,
  embeddings: {
    service: "mistral"
  }
});

// ...
```

Being able to easily switch embeddings providers ensures you don't get locked in!

`VectorDB.js` was built on top of [Embeddings.js](https://embeddingsjs.themaximalist.com/), and passes the full `embeddings` config option to `Embeddings.js`.


## Embeddings with Objects

`VectorDB.js` can store any valid JavaScript object along with the embedding.

```javascript
const db = new VectorDB();

await db.add("orange", "oranges");
await db.add("blue", ["sky", "water"]);
await db.add("green", { "grass": "lawn" });
await db.add("purple", { "flowers": 214 });

const results = await db.search("light green", 1);
assert(results[0].object.grass == "lawn");
```

This makes it easy to store metadata about the embedding, like an object id, URL, etc...


## VectorDB.js in Production

`VectorDB.js` works great by itself, but was built side-by-side to work with [Model Deployer](https://modeldeployer.com).

Model Deployer is an easy way to deploy your [LLM](https://llmjs.themaximalist.com) and Embedding models in production. You can monitor usage, rate limit users, generate API keys with specific settings and more.

It's especially helpful in offering options to your users. They can download and run models locally, they can use your API, or they can provide their own API key.

It works out of the box with VectorDB.js.

```javascript
const db = new VectorDB({
  embeddings: {
    service: "modeldeployer",
    model: "api-key",
  }
});

await db.add("orange", "oranges");
await db.add("blue", ["sky", "water"]);
await db.add("green", { "grass": "lawn" });
await db.add("purple", { "flowers": 214 });

const results = await db.search("light green", 1);
assert(results[0].object.grass == "lawn");
```

Learn more about [deploying embeddings with Model Deployer](https://modeldeployer.themaximalist.com).


## Projects

`VectorDB.js` is currently used in the following projects:

-   [AI.js](https://aijs.themaximalist.com) — simple AI library
-   [Model Deployer](https://modeldeployer.com) — deploy AI models in production
-   [HyperType](https://hypertypelang.com) — knowledge graph toolkit
-   [HyperTyper](https://hypertyper.com) — multidimensional mind mapping



## License

MIT


## Author

Created by [The Maximalist](https://twitter.com/themaximal1st), see our [open-source projects](https://themaximalist.com/products).

