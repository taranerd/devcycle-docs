---
title: DevCycle Node.js SDK Getting Started
sidebar_label: Getting Started
sidebar_position: 2
---

To use the DVC Server SDK in your project, import the `@devcycle/nodejs-server-sdk` package and 
call `initialize` with your DVC SDK server key. You may optionally `await` for the client
to be initialized.

JS Example:
```javascript
const DVC = require('@devcycle/nodejs-server-sdk')

const dvcClient = await DVC.initialize('<DVC_SDK_SERVER_KEY>').onClientInitialized()
```

Typescript Example:
```typescript
import { initialize } from '@devcycle/nodejs-server-sdk'

const dvcClient = await initialize('<DVC_SDK_SERVER_KEY>').onClientInitialized()
```
