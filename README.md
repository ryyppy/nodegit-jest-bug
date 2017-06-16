# Version info

```
node -v
v6.9.4

npm -v
3.10.10
```


# Issues with Jest + nodegit

```
npm install
npm test
```

This will start the Jest watch mode. The first test will run fine... as soon as
you repeat the test in the same watcher instance you will get following error:

```
 FAIL  src/__tests__/foo.test.js
  ● Test suite failed to run

    TypeError: Cannot convert undefined or null to object

      at processExports (node_modules/promisify-node/index.js:44:16)
      at Object.<anonymous> (src/__tests__/foo.test.js:1:102)
      at process._tickCallback (internal/process/next_tick.js:103:7)
```

I poked around in the `promisify-node` source code and realized that at some
point, `nodegit` tries to promisify an (maybe already promisified?) object:

```
console.log node_modules/promisify-node/index.js:43
  { [Function]
    cherrypick: [Circular],
    commit: [Function],
    initOptions: [Function] }
```

On the first run, the cherrypick function is just `[Function]`.
Maybe that's an issue with module caching?


