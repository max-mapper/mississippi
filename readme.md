# mississippi

a collection of useful stream utility modules. learn how the modules work using this and then pick the ones you want and use them individually

the goal of the modules included in mississippi is to make working with streams easy without sacrificing speed, error handling or composability.

## usage

```js
var miss = require('mississippi')
```

## methods

- [pipe](#pipe)
- [pipeline](#pipeline)
- [duplex](#duplex)
- [through](#through)
- [concat](#concat)

### pipe

##### `miss.pipe(stream1, stream2, stream3, ..., cb)`

Pipes streams together and destroys all of them if one of them closes. Calls `cb` with `(error)` if there was an error in any of the streams.

When using standard `source.pipe(destination)` the source will _not_ be destroyed if the destination emits close or error. You are also not able to provide a callback to tell when then pipe has finished.

`miss.pipe` does these two things for you, ensuring you handle stream errors 100% of the time (unhandled errors are probably the most common bug in most node streams code)

#### original module

`miss.pipe` is provided by [`require('pump')`](https://npmjs.org/pump)

#### example

```js
// lets do a simple file copy
var fs = require('fs')

var read = fs.createReadStream('./original.zip')
var write = fs.createWriteStream('./copy.zip')

// use miss.pipe instead of read.pipe(write)
miss.pipe(read, write, function (err) {
  if (err) return console.error('Copy error!', err)
  console.log('Copied successfully')
})
```

### pipeline

##### `var pipeline = miss.pipeline(stream1, stream2, stream3, ...)`

builds a pipeline from all the transform streams passed in as arguments by piping them together and returning a single stream object that lets you write to the first stream and read from the last stream

if any of the streams in the pipeline emits an error or gets destroyed, or you destroy the stream it returns, all of the streams will be destroyed and cleaned up for you

#### original module

`miss.pipeline` is provided by [`require('pumpify')`](https://npmjs.org/pumpify)

#### example

```js
// first create some transform streams (note: these two modules are fictional)
var imageResize = require('image-resizer-stream')({width: 400})
var pngOptimizer = require('png-optimizer-stream')({quality: 60})

// instead of doing a.pipe(b), use pipelin
var resizeAndOptimize = miss.pipeline(imageResize, pngOptimizer)
// `resizeAndOptimize` is a transform stream. when you write to it, it writes
// to `imageResize`. when you read from it, it reads from `pngOptimizer`.
// it handles piping all the streams together for you

// use it like any other transform stream
var fs = require('fs')

var read = fs.createReadStream('./image.png')
var write = fs.createWriteStream('./resized-and-optimized.png')

miss.pipe(read, resizeAndOptimize, write, function (err) {
  if (err) return console.error('Image processing error!', err)
  console.log('Image processed successfully')
})
```

### duplex

##### `var duplex = miss.duplex([writable, readable, opts])`

take two separate streams, a writable and a readable, and turn them into a single duplex (readable and writable) stream

the returned stream will emit data from the readable, and when you write to it, it writes to the writable

you can either choose to supply the writable and the readable at the time you create the stream, or you can do it later using the `.setWritable` and `.setReadable` methods, and data written to the stream in the meantime will be buffered for you

#### original module

`miss.duplex` is provided by [`require('duplexify')`](https://npmjs.org/duplexify)

#### example

```js
// lets spawn a process and take its stdout and stdin and combine them into 1 stream
var child = require('child_process')

// @- tells it to read from stdin, --data-binary sets 'raw' binary mode
var curl = child.spawn('curl -X POST --data-binary @- http://foo.com')

// duplexCurl will write to stdin and read from stdout
var duplexCurl = miss.duplex(curl.stdin, curl.stdout)
```

### through

todo

### concat

todo

