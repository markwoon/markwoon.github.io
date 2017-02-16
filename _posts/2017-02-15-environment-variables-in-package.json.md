---
layout: post
title: Environment Variables in package.json
---

One of the many, many "joys" of working with npm on Windows is having to deal with environment variables in `package.json`.  I used to just ignore the problem but have recently found a couple excellent packages that solves the problem:

* [`cross-env`](https://www.npmjs.com/package/cross-env), which allows you to set environment variables, and
* [`cross-var`](https://www.npmjs.com/package/cross-var), which allows you to expand them.

Here's what used to happen:

```json
"scripts": {
  "foo":     "some-command $ENV_VAR/helloworld",
  "foo:win": "some-command %ENV_VAR%\\helloworld",
  "bar": "NODE_PATH=$NODE_PATH:./app/scripts some-command",
  "bar:win": "set NODE_PATH=%NODE_PATH%;.\\app\\scripts && some-command"
}
```

And now:

```json
"scripts": {
  "foo": "cross-env some-command $ENV_VAR/helloworld",
  "bar": "cross-var cross-env NODE_PATH=$NODE_PATH:./app/scripts some-command"
}
```
