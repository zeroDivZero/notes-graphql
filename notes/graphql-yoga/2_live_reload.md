# LIVE RELOAD

Install package [nodemon](https://www.npmjs.com/package/nodemon); tool to automatically restart node app when file change detected. Replacement wrapper for node, replace word `node` on command line when executing script.

```sh
npm i nodemon --save-dev
```

Replace start script in `package.json` with:

```sh
nodemon src/index.js --exec babel-node
```
