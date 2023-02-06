---
"zod-fetch": minor
---

`zod-fetch` is a simple API for building a type and runtime-safe fetcher function using [Zod](https://github.com/colinhacks/zod) schemas.

## Usage

### Using the default fetcher

You can create a fetcher using `createZodFetcher`:

```ts
import { z } from "zod";
import { createZodFetcher } from "zod-fetcher";

const fetchWithZod = createZodFetcher();

fetchWithZod(
  // The schema you want to validate with
  z.object({
    hello: "world",
  }),
  // Any parameters you would usually pass to fetch
  "/my-api",
).then((res) => {
  console.log(res);
  //          ^? { hello: string }
});
```

If you don't pass a fetcher to `createZodFetcher`, it uses a sensible default fetcher (using the `fetch` API) to grab the data needed.

### Using a custom fetcher

You can pass custom fetchers to `zod-fetcher`:

```ts
import { z } from "zod";
import { createZodFetcher } from "zod-fetcher";
import axios from "axios";

const fetchWithZod = createZodFetcher(axios.get);

fetchWithZod(
  z.object({
    data: z.object({
      name: z.string(),
    }),
  }),
  "/user",
  {
    params: {
      id: 12345,
    },
  },
).then((res) => {
  console.log(res);
  //          ^? { data: { name: string } }
});
```