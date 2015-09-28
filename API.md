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
Ÿ��: `String` or `Array`

�о���� glob �Ǵ� glob �迭�Դϴ�.

#### options
Ÿ��: `Object`

[glob-stream]�� ���� [node-glob]�� ������ �ɼ��Դϴ�.

gulp�� [node-glob][node-glob documentation]�� [glob-stream]���� �����ϴ� �ɼ� �ܿ��� �߰� �ɼ��� �����մϴ�:

#### options.buffer
Ÿ��: `Boolean`
�⺻��: `true`

`false`�� �����ϸ� `file.contents`�� ���� ���� ��� ��Ʈ�� �������� ��ȯ�մϴ�. ���� ū ������ �ٷ� �� �����ϰ� ����� �� �ֽ��ϴ�.
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
�� �۾��� ����� ���Ŀ��� ����ؼ� ��Ʈ���� ��ȯ�ϹǷ� ���� ������ ���� �۾��� �� ���� �ֽ��ϴ�. ������ ������ ������ ���� �����մϴ�.

```javascript
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```

The write path is calculated by appending the file relative path to the given
destination directory. In turn, relative paths are calculated against the file base. 
See `gulp.src` above for more info.

#### path
Ÿ��: `String` or `Function`

The path (output folder) to write files to. Or a function that returns it, the function will be provided a [vinyl File instance](https://github.com/wearefractal/vinyl).

#### options
Ÿ��: `Object`

#### options.cwd
Ÿ��: `String`
�⺻��: `process.cwd()`

`cwd` for the output folder, only has an effect if provided output folder is relative.

#### options.mode
Ÿ��: `String`
�⺻��: `0777`

Octal permission string specifying mode for any folders that need to be created for output folder.

### gulp.task(name[, deps], fn)

Define a task using [Orchestrator].

```js
gulp.task('somename', function() {
  // Do stuff
});
```

#### name

The name of the task. Tasks that you want to run from the command line should not have spaces in them.

#### deps
Ÿ��: `Array`

An array of tasks to be executed and completed before your task will run.

```js
gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // Do stuff
});
```

**Note:** Are your tasks running before the dependencies are complete?  Make sure your dependency tasks are correctly using the async run hints: take in a callback or return a promise or event stream.

#### fn

The function that performs the task's operations. Generally this takes the form of `gulp.src().pipe(someplugin())`.

#### Async task support

Tasks can be made asynchronous if its `fn` does one of the following:

##### Accept a callback

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

##### Return a stream

```js
gulp.task('somename', function() {
  var stream = gulp.src('client/**/*.js')
    .pipe(minify())
    .pipe(gulp.dest('build'));
  return stream;
});
```

##### Return a promise

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

**Note:** By default, tasks run with maximum concurrency -- e.g. it launches all the tasks at once and waits for nothing. If you want to create a series where tasks run in a particular order, you need to do two things:

- give it a hint to tell it when the task is done,
- and give it a hint that a task depends on completion of another.

For these examples, let's presume you have two tasks, "one" and "two" that you specifically want to run in this order:

1. In task "one" you add a hint to tell it when the task is done.  Either take in a callback and call it when you're
done or return a promise or stream that the engine should wait to resolve or end respectively.

2. In task "two" you add a hint telling the engine that it depends on completion of the first task.

So this example would look like this:

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
Ÿ��: `String` or `Array`

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
Ÿ��: `String` or `Array`

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
