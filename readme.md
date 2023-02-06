# Zod Fetch

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

## Why does this exist?

For teams that want to combine runtime-safety with a fetcher (an extremely common use case), you often have a choice:

1. Use a big, all-encompassing solution like [tRPC](https://trpc.io/)
2. Hack something together on your own

I hope that this small API offers a nice compromise for teams looking to bridge that gap. Or, at least, offers some pretty example code you can copy-and-paste into your projects.