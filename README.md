# api documentation for  [gulp-grunt (v0.5.5)](http://github.com/gratimax/gulp-grunt)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-grunt.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-grunt) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-grunt.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-grunt)
#### Run grunt tasks from gulp

[![NPM](https://nodei.co/npm/gulp-grunt.png?downloads=true)](https://www.npmjs.com/package/gulp-grunt)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-grunt/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-grunt_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-grunt/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-grunt/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-grunt/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "gratimax",
        "email": "max@ovsankin.com"
    },
    "bugs": {
        "url": "https://github.com/gratimax/gulp-grunt/issues"
    },
    "dependencies": {
        "grunt": "^1.0.1"
    },
    "description": "Run grunt tasks from gulp",
    "devDependencies": {
        "chai": "^3.3.0",
        "gulp": "^3.9.0",
        "gulp-mocha": "^2.1.3",
        "mocha": "^2.3.3"
    },
    "directories": {},
    "dist": {
        "shasum": "a06692df0d0da1c30cee01f340bbc17ce6947577",
        "tarball": "https://registry.npmjs.org/gulp-grunt/-/gulp-grunt-0.5.5.tgz"
    },
    "engines": {
        "node": ">= 0.9.0"
    },
    "gitHead": "bd13f068dd1b8bb7d89c3bb8bcab7896541938f7",
    "homepage": "http://github.com/gratimax/gulp-grunt",
    "keywords": [
        "gulpfriendly",
        "grunt"
    ],
    "license": "MIT",
    "main": "./index.js",
    "maintainers": [
        {
            "name": "gratimax",
            "email": "max@ovsankin.com"
        }
    ],
    "name": "gulp-grunt",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/gratimax/gulp-grunt.git"
    },
    "scripts": {
        "test": "mocha -R spec"
    },
    "version": "0.5.5"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-grunt](#apidoc.module.gulp-grunt)
1.  [function <span class="apidocSignatureSpan">gulp-grunt.</span>tasks (options)](#apidoc.element.gulp-grunt.tasks)



# <a name="apidoc.module.gulp-grunt"></a>[module gulp-grunt](#apidoc.module.gulp-grunt)

#### <a name="apidoc.element.gulp-grunt.tasks"></a>[function <span class="apidocSignatureSpan">gulp-grunt.</span>tasks (options)](#apidoc.element.gulp-grunt.tasks)
- description and source-code
```javascript
tasks = function (options) {
  var opt = makeOptions(options);

  var oldCwd = process.cwd();
  var cwd = opt.base != null ? opt.base : oldCwd;

  grunt.file.setBase(cwd);

  var gruntCliDir = opt.base ? (opt.base + "/") : "";

  grunt.task.init([]);

  process.chdir(oldCwd);

  var gruntTasks = grunt.task._tasks,
    finalTasks = {};

  var registerGruntTask = function (name) {
    finalTasks[opt.prefix + name] = function (cb) {
      if (opt.verbose) {
        console.log('[grunt-gulp] Running Grunt "' + name + '" task...');
      }
      var args = opt.force ?  [name, '--force', '--verbose=' + opt.verbose] : [name, '--verbose=' + opt.verbose];
      for (var key in opt) {
        if (key != 'base' && key != 'prefix') {
          args = args.concat('--' + key + '=' + opt[key]);
        }
      }
      var child = spawn(
        gruntCliDir + gruntCmd,
        args,
        {cwd: cwd}
      );
      child.stdout.on('data', function (d) {
        grunt.log.write(d);
      });
      child.stderr.on('data', function (d) {
        grunt.log.error(d);
      });
      child.on('close', function (code) {
        if (opt.verbose) {
          grunt.log.ok('[grunt-gulp] Done running Grunt "' + name + '" task.');
        }
        if (code != 0) {
    	     grunt.fail.warn('[grunt-gulp] Failed running Grunt "' + name + '" task.')
    	  }
        cb();
      });
    };
  }

  for (var name in gruntTasks) {
    if (gruntTasks.hasOwnProperty(name)) {
      // add tasks
      registerGruntTask(name);
      // also add target-specific tasks
      for (var target in grunt.config.get(name)) {
        registerGruntTask(name + ':' + target);
      }
    }
  }

  return finalTasks;
}
```
- example usage
```shell
...
  base: null, // this is just the directory that your Gulpfile is in
  prefix: 'grunt-',
  verbose: false,
  force: true
}
'''

### gulp-grunt.tasks()
__Takes__ '(options)'

This just returns all the grunt tasks found, along with their associated functions.
Calling is essentially the same as with the main function:
'''js
var gulp_grunt = require('gulp-grunt')
var tasks = gulp_grunt.tasks({
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
