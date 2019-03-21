# mississippi Change Log
All notable changes to this project will be documented in this file.
This project adheres to [Semantic Versioning](http://semver.org/).

## 4.0.0 - 2019-03-21

* Update through2 to 3.0.1 (from 2.0.0) BREAKING: readable-stream 2 -> 3
* Update flush-write-stream to 2.0.0 (from 1.0.0) BREAKING: readable-stream ^2.3.6 -> ^3.1.1
* Update concat-stream to 2.0.0 (from 1.5.0) BREAKING: readable-stream ^2.2.2 -> ^3.0.2
* Update duplexify to 4.0.0 (from 3.4.2) BREAKING: readable-stream ^2.0.0 -> ^3.1.1

## 3.0.0 - 2018-02-26

* Update to pump@3.0.0.  Returns the last stream the pipeline to enable chaining.  (Use the individual modules to avoid potentially unnecessary major updates in your project).

## 2.0.0 - 2018-01-30

* Update to pump@2.0.1.  (Use the individual modules to avoid potentially unnecessary major updates in your project)
* Pin engines support to >= Node 4.0.0.  Run Node LTS or greater.
