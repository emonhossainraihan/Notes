For better auto complete:

```js
// common.js
import { PrismaClient } from "@prisma/client";

export const prisma = new PrismaClient();

// src/resolver
import { prisma } from "../common";
```
