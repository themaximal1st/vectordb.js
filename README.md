# vectordb.js

<img src="logo.png" />

<div class="badges" style="text-align: center; margin-top: 0px;">
<img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/themaximal1st/vectordb.js">
<img alt="NPM Downloads" src="https://img.shields.io/npm/dt/%40themaximalist%2Fvectordb.js">
<img alt="GitHub code size in bytes" src="https://img.shields.io/github/languages/code-size/themaximal1st/vectordb.js">
<img alt="GitHub License" src="https://img.shields.io/github/license/themaximal1st/vectordb.js">
</div>
<br />


`VectorDB.js` is the simplest way to do text similarity in Node.js

-   Uses [hnswlib-node](https://github.com/yoshoku/hnswlib-node) for simple vector search
-   Uses [Embeddings.js](https://embeddingsjs.themaximalist.com) for simple text embeddings
-   Supports OpenAI, Mistral and local embeddings
-   Caches embeddings to `embeddings.cache.json`
-   Automatically resizes database size
-   Store objects with embeddings
-   MIT license



## Usage

```javascript
import VectorDB from "@themaximalist/vectordb.js"
const db = new VectorDB();

await db.add("orange");
await db.add("blue");

const result = await db.search("light orange");
// [ { input: 'orange', distance: 0.3109036684036255 } ]
```



## Installation

```bash
npm install @themaximalist/vectordb.js
```

To use local embeddings, install the transformers library:

```bash
npm install @xenova/transformers
```



## Switch Embedding Models

VectorDB.js works with OpenAI, Mistral or local embeddings.

To use OpenAI (`text-embedding-ada-002`) or Mistral (`mistral-embed`), either pass an `apikey` to the `embeddings` options or set the `OPENAI_API_KEY` or `MISTRAL_API_KEY` environment variable.

```javascript
import VectorDB from "@themaximalist/vectordb.js"

const db = new VectorDB({ dimensions: 1536, embeddings: { service: "openai"});

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
const db = new VectorDB({ dimensions: 1024, embeddings: { service: "mistral"});
// ...
```

With Local embeddings:

```javascript
const db = new VectorDB(); // defaults to local embeddings
// ...
```

Being able to easily switch embeddings providers ensures you don't get locked in!



## Embeddings with Objects

`vectordb.js` can store any valid JavaScript object along with the embedding.

```javascript
import VectorDB from "@themaximalist/vectordb.js"
const db = new VectorDB();

await db.add("orange", "oranges");
await db.add("blue", ["sky", "water"]);
await db.add("green", { "grass": "lawn" });
await db.add("purple", { "flowers": 214 });

const results = await db.search("light green", 1);
assert(results[0].object.grass == "lawn");
```



## Deploy Embeddings

`vectordb.js` can work by itself, but was built side-by-side to work with [Model Deployer](https://github.com/themaximal1st/modeldeployer).

Model Deployer is an easy way to deploy your LLM and Embedding models in production. You can monitor usage, rate limit users, generate API keys with specific settings and more.

It's especially helpful in offering options to your users. They can download and run models locally, they can use your API, or they can provide their own API key.

It works out of the box with vectordb.js.

```javascript
import VectorDB from "@themaximalist/vectordb.js"
const db = new VectorDB({
  embeddings: {
    service: "modeldeployer",
    model: "model-api-key-goes-here",
  }
});

await db.add("orange", "oranges");
await db.add("blue", ["sky", "water"]);
await db.add("green", { "grass": "lawn" });
await db.add("purple", { "flowers": 214 });

const results = await db.search("light green", 1);
assert(results[0].object.grass == "lawn");
```




## Projects

`VectorDB.js` is currently used in the following projects:

-   [AI.js](https://aijs.themaximalist.com) — simple AI library
-   [Model Deployer](https://modeldeployer.com) — deploy AI models in production
-   [HyperType](https://hypertypelang.com) — knowledge graph toolkit
-   [HyperTyper](https://hypertyper.com) — multidimensional mind mapping



## Author

-   [The Maximalist](https://themaximalist.com/)
-   [@themaximal1st](https://twitter.com/themaximal1st)


## License

MIT
