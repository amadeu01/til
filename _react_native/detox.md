When use `Detox`, you should also init it properly in the `init.js` file.

When use jest:

```js
const detox = require('detox');
const adapter = require('detox/runners/jest/adapter');
const specReporter = require('detox/runners/jest/specReporter');
const config = require('../package.json').detox;

jest.setTimeout(120000);
jasmine.getEnv().addReporter(adapter);
jasmine.getEnv().addReporter(specReporter);

beforeAll(async () => {
  await detox.init(config, { launchApp: false });
  await device.launchApp({ permissions: { notifications: 'YES' } });
});

beforeEach(async () => {
  await adapter.beforeEach();
});

afterAll(async () => {
  await adapter.afterAll();
  await detox.cleanup();
});
```

the `await detox.init(config, { launchApp: false });` is necessary once you want to avoid the initialization 
without set the notification permission.


[StackoverFlow](https://stackoverflow.com/questions/57057132/enable-notification-while-test-with-detox-isnt-working/57058153#57058153)

[Detox github](https://github.com/wix/Detox)
