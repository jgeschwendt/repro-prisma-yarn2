```
# Use Node.js 12 or whatever
nvm use

# Install latest version of yarn
npm i -g yarn@latest

# Install dependencies in `~/.yarn/`
yarn

# Add VSCode Yarn2 Support
yarn pnpify --sdk

yarn prisma introspect
```

This is where things start to go weird.

```
$ yarn prisma generate

✔ Generated Prisma Client to ./.yarn/unplugged/@prisma-client-virtual-a3c02a9631/node_modules/@prisma/client in 90ms

You can now start using Prisma Client in your code:


import { PrismaClient } from '@prisma/client'
// or const { PrismaClient } = require('@prisma/client')

const prisma = new PrismaClient()


Explore the full API: http://pris.ly/d/client
```

This all seems okay, however when importing the client there is a TS error at:

`import { PrismaClient } from '@prisma/client'`

```
Module '"../.yarn/unplugged/@prisma-client-virtual-a3c02a9631/node_modules/@prisma/client"' has no exported member 'PrismaClient'. ts(2305)
```

The TS error can be resolved if changed to

`import { PrismaClient } from '@prisma/client/../../.prisma/client'`

or the `@prisma/client/index.d.ts` includes this line to help the resolution.

`export * from '../../.prisma/client'`

I haven't found a way using the `package.json#resolutions` / protocols or with modifying the `.yarnrc.yml` to fix the resolution.
