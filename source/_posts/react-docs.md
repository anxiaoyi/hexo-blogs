---
title: ReactJS Docs
layout: post
date: 2015-11-21
tags:
- react
categories:
- javascript
---

# [ReactJS Docs](http://facebook.github.io/react/docs/getting-started.html)

## Gettings Started

- `<head></head>`引入`react.js`,`react-dom.js`文件

- `src/helloworld.js`:

```javascript
ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
);
```

- Offline Transform

```javascript
npm install --global babel-cli
npm install babel-preset-react

babel --presets react src --watch --out-dir build
```

- Then reference it from `helloworld.html`:

```javascript
<script type="text/babel" src="src/helloworld.js"></script>
```
