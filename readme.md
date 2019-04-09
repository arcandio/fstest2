I'm working on a desktop app using node, create-react-app, and electron. The following line, [recommended here](https://electronjs.org/docs/api/dialog) 

```javascript
const { dialog } = require('electron').remote
```

when added to my `store` react component (and thus not in the electron main process), causes the following error:

```
TypeError: fs.existsSync is not a function
getElectronPath
F:/freelance/repos/creative-ontology-editor/node_modules/electron/index.js:8
   5 | var pathFile = path.join(__dirname, 'path.txt');
   6 | 
   7 | function getElectronPath() {
>  8 |   if (fs.existsSync(pathFile)) {
   9 |     var executablePath = fs.readFileSync(pathFile, 'utf-8');
  10 | 
  11 |     if (process.env.ELECTRON_OVERRIDE_DIST_PATH) {
```

I believe this is because React or create-react-app specifically blocks/nullifies certain node.js modules like `fs`, but I could be wrong. Note that this error happens *inside* the `electron` module when I include the above line, not in my own code.

My goal is to have my desktop app able to save and load files to the user's machine, the way something like Word or Excel does.

I've called `const fs = window.require('fs');` in my react component, which I think I'm not supposed to do, but also, since this is in the actual `electron` module's `index.js`, I made sure that it also calls it, which it does: `var fs = require('fs')`. There was no change in behavior when I switched my call in the react component to `const fs = window.require('fs')`.

I've also made sure to set `webPreferences.nodeIntegration` to true in my electron main process, to no avail.

# Replication

1. clone/download the project in windows
2. run `npm run electron-dev`
3. Observe error

See my question here: https://stackoverflow.com/questions/55581152/how-do-i-use-use-dialogs-in-an-electron-react-environment