---
title: gulp ducumentation
layout: post
date: 2015-11-21
tags:
- gulp
categories:
- gulp
---

[gulp documentation](https://github.com/gulpjs/gulp/tree/master/docs)
[gulp 简体中文文档](https://github.com/lisposter/gulp-docs-zh-cn)

## getting started

- `npm install --global gulp` to install gulp globally.
- Create a `gulpfile.js` at the root of your project.
- `gulp` to run gulp.

## gulp API docs

### `gulp.src(globs[, options])`

Emits files matching provided glob or an array of globs.

```javascript
//考虑下面目录结构
client/
	a.js
	bob.js
	bad.js

//匹配`a.js`,`bad.js`
gulp.src(['client*.js', '!client/b*.js', 'client/bad.js'])

//匹配`'client/js/somedir/somefile.js'`
gulp.src('client/js/**/*.js')
```

### `gulp.dest(path[, options])`

Can be piped to and it will write files. Re-emits all data passed to it so you can pipe to multiple folders. Folders that don't exist will be created.(写入文件到其它地方，文件夹不存在自动创建)

```javascript
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```

### `gulp.task(name[, deps], fn)`

使用[`Orchestrator`](https://github.com/robrich/orchestrator)定义一个任务

```javascript
gulp.task('somename', function() {
	// Do stuff
});

// deps: 是一个数组，在运行`mytask`任务之前需要完成的任务，注意：任务并行运行，而不是顺序执行
gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
	// Do stuff
});
```

### `gulp.watch(glob[, opts], tasks` or `gulp.watch(glob [, opts, cb])`

监视文件，以及在文件发生变化的时候do something.

```javascript
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});

gulp.watch('js/**/*.js', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```
