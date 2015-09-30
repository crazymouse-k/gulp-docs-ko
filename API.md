## API ���� ����

�ٷ� ����:
  [gulp.src](#gulpsrcglobs-options) |
  [gulp.dest](#gulpdestpath-options) |
  [gulp.task](#gulptaskname-deps-fn) |
  [gulp.watch](#gulpwatchglob--opts-tasks-or-gulpwatchglob--opts-cb)

### gulp.src(globs[, options])

������ glob �Ǵ� glob �迭�� ���� ������ ǥ���մϴ�.
[Vinyl ����](https://github.com/wearefractal/vinyl-fs)�� [stream](http://nodejs.org/api/stream.html)�� ��ȯ�ϸ�
[������](http://nodejs.org/api/stream.html#stream_readable_pipe_destination_options)�� ���� �÷������� ����� �� �ֽ��ϴ�.

```javascript
gulp.src('client/templates/*.jade')
  .pipe(jade())
  .pipe(minify())
  .pipe(gulp.dest('build/minified_templates'));
```

`glob`�� [node-glob ����](https://github.com/isaacs/node-glob)�� �����ϰų� ���� ��θ� ���� ����� �� �ֽ��ϴ�.

#### globs
Ÿ��: `String` �Ǵ� `Array`

�о���� glob �Ǵ� glob �迭�Դϴ�.

#### options
Ÿ��: `Object`

[glob-stream]�� ���� [node-glob]�� ������ �ɼ��Դϴ�.

gulp�� [node-glob][node-glob documentation]�� [glob-stream]���� �����ϴ� �ɼ� �ܿ��� �߰� �ɼ��� �����մϴ�:

#### options.buffer
Ÿ��: `Boolean`
�⺻��: `true`

`false`�� �����ϸ� `file.contents`�� ���� ���� ��� ��Ʈ�� �������� ��ȯ�մϴ�.
���� ū ������ �ٷ� �� �����ϰ� ����� �� �ֽ��ϴ�.

**����:** �÷������� ��Ʈ���� ���� ������ �������� ���� �� �ֽ��ϴ�.

#### options.read
Ÿ��: `Boolean`
�⺻��: `true`

`false`�� �����ϸ� `file.contents`�� null�� �Ǹ� ��� ������ ���� �ʽ��ϴ�.

#### options.base
Ÿ��: `String`
�⺻��: ��� glob�� ���۵Ǵ� ��ġ ([glob2base]�� �����ϼ���)

����: `somefile.js`�� `client/js/somedir`�ȿ� ���� ��:

```js
gulp.src('client/js/**/*.js') // Matches 'client/js/somedir/somefile.js' and resolves `base` to `client/js/`
  .pipe(minify())
  .pipe(gulp.dest('build'));  // Writes 'build/somedir/somefile.js'

gulp.src('client/js/**/*.js', { base: 'client' })
  .pipe(minify())
  .pipe(gulp.dest('build'));  // Writes 'build/js/somedir/somefile.js'
```

### gulp.dest(path[, options])

�������� ��Ʈ���� ���Ϸ� ��ȯ�մϴ�.
�� �۾��� ����� ���Ŀ��� ����ؼ� ��Ʈ���� ��ȯ�ϹǷ� ���� ������ ���� �۾��� �� ���� �ֽ��ϴ�.
������ ������ ������ ���� �����մϴ�.

```javascript
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```

������ ���� ��δ� ������ ������ ��ο� ���� ��� ��θ� �߰��Ͽ� ����˴ϴ�.
���ʷ� ������ ��� ��δ� ���� base�� ������� ����˴ϴ�.
�ڼ��� ������ ���� `gulp.src`�� �����ϼ���.

#### path
Ÿ��: `String` �Ǵ� `Function`

`path`(��� ����)�� ������ �� ��θ� �����մϴ�. �Ǵ� �Լ��� �����ϸ� �Լ����� [vinyl File instance](https://github.com/wearefractal/vinyl)�� �����մϴ�.

#### options
Ÿ��: `Object`

#### options.cwd
Ÿ��: `String`
�⺻��: `process.cwd()`

������� ����� `cwd` ������ �����մϴ�. ��� ������ ��� ����� ��쿡�� �۵��մϴ�.

#### options.mode
Ÿ��: `String`
�⺻��: `0777`

��� ������ �����ϱ� ���� �ʿ��� ������ ��带 8���� ���� ���ڿ��� �����մϴ�.

### gulp.task(name[, deps], fn)

[Orchestrator]�� ����Ͽ� �۾��� �����մϴ�.

```js
gulp.task('somename', function() {
  // Do stuff
});
```

#### name

�۾��� �̸��Դϴ�. ���⼭ ������ �̸����� CLI �͹̳ο��� �۾��� ������ �� ������ ������ ���� �ȵ˴ϴ�.

#### deps
Ÿ��: `Array`

���� �۾��� ����Ǳ� ������ ���� ������ ���Ӽ� �۾��� �迭�Դϴ�.

```js
gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // Do stuff
});
```

**����:** Ȥ�� �� �۾��� ���Ӽ� �۾��� �����⵵ ���� ����˴ϱ�? �Ʒ��� �񵿱� �۾� ������ Ȯ���ϰ� ���Ӽ� �۾��� �ùٸ��� �۵��ϴ��� Ȯ���ϼ���.

#### fn

�۾��� ���� �Լ��Դϴ�. �Ϲ������� �Լ��� `gulp.src().pipe(someplugin())` ���� ������ �����ϴ�.

#### �񵿱� �۾� ����

`fn`�� �۾��� �񵿱�� ���� �� �ֽ��ϴ�. ���� �� ���� ����� �ֽ��ϴ�:

##### �ݹ� ���

```javascript
// run a command in a shell
var exec = require('child_process').exec;
gulp.task('jekyll', function(cb) {
  // build Jekyll
  exec('jekyll build', function(err) {
    if (err) return cb(err); // return error
    cb(); // finished task
  });
});
```

##### Stream ��ȯ

```js
gulp.task('somename', function() {
  var stream = gulp.src('client/**/*.js')
    .pipe(minify())
    .pipe(gulp.dest('build'));
  return stream;
});
```

##### Promise ��ȯ

```javascript
var Q = require('q');

gulp.task('somename', function() {
  var deferred = Q.defer();

  // do async stuff
  setTimeout(function() {
    deferred.resolve();
  }, 1);

  return deferred.promise;
});
```

�⺻������ �۾����� ���ü��� �ִ�� ����˴ϴ�. ���� ���� ��� �۾��� �� ���� ����Ǹ� �۾����� ��� ���� ������ �� ���� ����˴ϴ�. ���� Ư���� ������ ���� �۾��� ����ǵ��� �Ϸ��� ���� �� ������ �����ؾ� �մϴ�:

- �۾��� ���� �Ϸ�Ǵ��� ǥ���ϰ�
- �ٸ� �۾����� �ش� �۾��� �Ϸῡ ���� ǥ���ؾ� �մϴ�.

������ �������ڸ�:

���� "one"�� "two"��� �۾��� �ְ�, ������ ���� ������ ���� ���� �մϴ�:

1. Stream, �ݹ� �Ǵ� `Promise`�� ����Ͽ� �۾� "one"�� ���������� �Ϸ�Ǵ� �κ��� ǥ���մϴ�.

2. �۾� "two"���� ���� ������ �ʿ��� "one"�̶�� �۾��� ���Ӽ� �۾����� �����մϴ�.

���� �ڵ�: 

```js
var gulp = require('gulp');

// takes in a callback so the engine knows when it'll be done
gulp.task('one', function(cb) {
    // do stuff -- async or otherwise
    cb(err); // if err is not null and not undefined, the run will stop, and note that it failed
});

// identifies a dependent task must be complete before this one begins
gulp.task('two', ['one'], function() {
    // task 'one' is done now
});

gulp.task('default', ['one', 'two']);
```


### gulp.watch(glob [, opts], tasks) �Ǵ� gulp.watch(glob [, opts, cb])

������ ������ �����մϴ�. ������ `change` �̺�Ʈ�� �߻���Ű�� EventEmitter�� ��ȯ�մϴ�.

### gulp.watch(glob[, opts], tasks)

#### glob
Ÿ��: `String` �Ǵ� `Array`

������ ������ Ÿ�� �����Դϴ�. ���� glob �Ǵ� �迭�� ������ �� �ֽ��ϴ�.

#### opts
Ÿ��: `Object`

[`gaze`](https://github.com/shama/gaze)�� �Ѱ����� �ɼ��Դϴ�.

#### tasks
Ÿ��: `Array`

������ ���� �̺�Ʈ�� �߻��� ������ ȣ���� task�Դϴ�. ���� �� ������ �� �ֽ��ϴ�.

```js
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

### gulp.watch(glob[, opts, cb])

#### glob
Ÿ��: `String` �Ǵ� `Array`

������ ������ Ÿ�� �����Դϴ�. ���� glob �Ǵ� �迭�� ������ �� �ֽ��ϴ�.

#### opts
Ÿ��: `Object`

[`gaze`](https://github.com/shama/gaze)�� �Ѱ����� �ɼ��Դϴ�.

#### cb(event)
Ÿ��: `Function`

�ݹ��� ���� �̺�Ʈ���� ȣ��˴ϴ�.

```js
gulp.watch('js/**/*.js', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

�ݹ��� ���� ������ �����ϴ� `event` ��ü�� ��ȯ�մϴ�:

##### event.type
Ÿ��: `String`

�߻��� �̺�Ʈ�� Ÿ���Դϴ�. `added`, `changed`, `deleted` �� �� ������ �����˴ϴ�.

##### event.path
Ÿ��: `String`

�̺�Ʈ�� �߻��� ������ ����Դϴ�.


[node-glob documentation]: https://github.com/isaacs/node-glob#options
[node-glob]: https://github.com/isaacs/node-glob
[glob-stream]: https://github.com/wearefractal/glob-stream
[gulp-if]: https://github.com/robrich/gulp-if
[Orchestrator]: https://github.com/robrich/orchestrator
[glob2base]: https://github.com/wearefractal/glob2base
