---
title: "Working with folders in Node"
description: "How to interact with folders using Node"
date: 2018-08-24T07:00:00+02:00
updated: 2019-07-07T07:00:00+02:00
tags: node
tags_weight: 69
path: node-folders
---

The Node.js `fs` core module provides many handy methods you can use to work with folders.

## Check if a folder exists

Use `fs.access()` to check if the folder exists and Node can access it with its permissions.

## Create a new folder

Use `fs.mkdir()` or `fs.mkdirSync()` to create a new folder.

```js
const fs = require('fs')

const folderName = '/Users/flavio/test'

try {
  if (!fs.existsSync(dir)){
    fs.mkdirSync(dir)
  }
} catch (err) {
  console.error(err)
}
```

## Read the content of a directory

Use `fs.readdir()` or `fs.readdirSync` to read the contents of a directory.

This piece of code reads the content of a folder, both files and subfolders, and returns their relative path:

```js
const fs = require('fs')
const path = require('path')

const folderPath = '/Users/flavio'

fs.readdirSync(folderPath)
```

You can get the full path:

```js
fs.readdirSync(folderPath).map(fileName => {
  return path.join(folderPath, fileName)
}
```

You can also filter the results to only return the files, and exclude the folders:

```js
const isFile = fileName => {
  return fs.lstatSync(fileName).isFile()
}

fs.readdirSync(folderPath).map(fileName => {
  return path.join(folderPath, fileName).filter(isFile)
})
```

## Rename a folder

Use `fs.rename()` or `fs.renameSync()` to rename folder. The first parameter is the current path, the second the new path:

```js
const fs = require('fs')

fs.rename('/Users/flavio', '/Users/roger', err => {
  if (err) {
    console.error(err)
    return
  }
  //done
})
```

`fs.renameSync()` is the synchronous version:

```js
const fs = require('fs')

try {
  fs.renameSync('/Users/flavio', '/Users/roger')
} catch (err) {
  console.error(err)
}
```

## Remove a folder

Use `fs.rmdir()` or `fs.rmdirSync()` to remove a folder.

Removing a folder that has content can be more complicated than you need.

In this case I recommend installing the `fs-extra` module, which is very popular and well maintained, and it's a drop-in replacement of the `fs` module, providing more features on top of it.

In this case the `remove()` method is what you want.

Install it using

`npm install fs-extra`

and use it like this:

```js
const fs = require('fs-extra')

const folder = '/Users/flavio'

fs.remove(folder, err => {
  console.error(err)
})
```

It can also be used with promises:

```js
fs.remove(folder).then(() => {
  //done
}).catch(err => {
  console.error(err)
})
```

or with async/await:

```js
async function removeFolder(folder) {
  try {
    await fs.remove(folder)
    //done
  } catch (err) {
    console.error(err)
  }
}

const folder = '/Users/flavio'
removeFolder(folder)
```