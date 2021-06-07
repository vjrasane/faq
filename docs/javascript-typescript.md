# JavaScript / TypeScript

## Math

### XYZ vector from latitude and longitude

```typescript
const ratio: number = Math.PI / 180.0;
const vector = {
  x: Math.cos(latitude * ratio) * Math.cos(longitude * ratio),
  y: Math.sin(latitude * ratio),
  z: Math.cos(latitude * ratio) * Math.sin(longitude * ratio)
};
```

## Misc

### Typescript value of type

```typescript
type ValueOf<T> = T[keyof T];
```

### Read package.json version with Bash

```shell
jq -r '.version' package.json
```

## React

### Compose context providers

```JSX
import React, { FunctionComponent } from 'react';

type Component<P = unknown> = FunctionComponent | ProviderTuple<P>;

type ProviderTuple<P> = [FunctionComponent<P>, P];

const Compose: FunctionComponent<{ providers: Component<any>[] }> = ({ providers, children }) => (
<>
{ providers.reduceRight((acc, curr) => {
const [Provider, props] = Array.isArray(curr) ? [curr[0], curr[1]] : [curr, {}];
return <Provider {...props}>{acc}</Provider>;
}, children)}
</>
);

export { Compose, ProviderTuple };
```

## Testing

### Supertest download zip

```typescript
const response = await request(app)
    .get("/download/zip")
    .set('Authorization', `Bearer <bearer token>`)
    .parse((res, callback) => {
        let data = '';
        res.setEncoding('binary');
        res.on('data', (chunk) => {
            data += chunk;
        });
        res.on('end', () => callback(null, Buffer.from(data, 'binary')));
    })
    .end((err: Error, res: Response) => {
        if (err) reject(err);
        resolve(res);
    })
);

expect(response.status).toBe(200);
fs.writeFileSync("/tmp/download.zip", response.body);
const zip = new AdmZip("/tmp/download.zip");
const zipEntries = zip.getEntries();
```

## Electron

### Spectron test case

```javascript
const Application = require('spectron').Application
const assert = require('assert')

describe("Application launch", function () {
    this.timeout(10000);

    let app;

    beforeEach(async () => {
        app = new Application({
            path: "path/to/executable"
        })
        await app.start();
        await app.client.windowByIndex(0);
    })

    afterEach(() => {
        if (app && app.isRunning()) {
            return app.stop()
        }
    })

    it('shows an initial window', async () => {
        const moduleName = await app.client.$(".module-text");
        const text = await moduleName.getText();
        assert.equal(text, "Home")
        app.client.saveScreenshot("./screenshot.png")
    })
})
```

### Playwright test case

```typescript
import { electron, ElectronApplication } from 'playwright-electron';

describe("Application launch", () => {
  let app: ElectronApplication;
  beforeEach(async () => {
    app = await electron.launch("path/to/executable");
  });

  afterEach(async () => {
    await app.close();
  });

  it('window title', async () => {
    const page = await app.firstWindow();
    expect(await page.title()).toEqual('title');
  });
});
```
