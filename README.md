# gulp-vsts-bump
Gulp plugin to bump the version of VSTS tasks  (still in preview, may be broken!)  
[![npmjs version Badge][npmjs-version-badge]][npmjs-pkg-url]
[![Circle CI Badge][circle-ci-badge]][circle-ci-url]
[![AppVeyor Status][appveyor-badge]][appveyor-url]
[![Coverage Status][coveralls-badge]][coveralls-url]
[![Sonar Quality Gate][sonar-quality-gate-badge]][sonar-url]  

## About
Gulp plugin that supports bumping the versions of VSTS tasks. The VSTS task manifest files maintain the version as an Object which differs from the traditional semver string used to represent the version found in other files like package.json (note that the values of Major, Minor, Patch can be strings OR numbers).

VSTS task manifest example:
```json
{
    "id": "923e6d5c-0b14-462b-922e-813cbd2ef4cc",
    "name": "2018vsts",
    "friendlyName": "Sample Task",
    "description": "vsts",
    "author": "me",
    "version": {
        "Major": 0,
        "Minor": 1,
        "Patch": 1
    },
}
```

The VSTS task version cannot be bumped using other gulp plugins without writing a lot of extra code, so we wrote this plugin to provide simple support specifically for VSTS tasks.  

This plugin should only be used for bumping VSTS task manifest files. For bumping any other standard version string in any other type file (like in a package.json file) you should *not* use this plugin, and you should use something like [gulp-bump][gulp-bump-pkg-url] instead.

## Usage
**Simple Usage (bumps patch version by default)**
```js
const gulp = require('gulp');
const vstsBump = require('gulp-vsts-bump');

gulp.task('tasks:bump', function () {
    return gulp.src(['./tasks/**/task.json'], { base: './' })
        .pipe(vstsBump())
        .pipe(gulp.dest('./'));
});
```

**Specific Bump Type**
```js
const gulp = require('gulp');
const vstsBump = require('gulp-vsts-bump');

gulp.task('tasks:bump', function () {
    return gulp.src(['./tasks/**/task.json'], { base: './' })
        .pipe(vstsBump({ type: 'minor' }))
        .pipe(gulp.dest('./'));
});
```

## Options

- **type**: string (default value: `'patch'`)  
Use to specify the release type you want to bump. Expected values are `'major'`, `'minor'`, or `'patch'` (default value). Technically any valid semver type (including prerelease, etc.) will be accepted, but you shouldn't use anything other than `major`, `minor`, or `patch` since that is all VSTS tasks can store.

- **quiet**: boolean (default value: `false`)  
Set this to `true` if you want to supress the log output. For example:

```js
    .pipe(vstsBump({ quiet: true }))

```  

- **versionPropertyType**: string (default value: `'number'`)  
Allowed values: `'number'` (default) and `'string'`. Some VSTS tasks specify the values for the version Major, Minor, and Patch properties as a number while others store it as a string (VSTS supports both apparently). By default the plugin will emit the bumped version values as numbers in the task.json file(s), but if you would prefer those values to be strings instead then set this property to `'string'` in the configuration options: 

```js
    .pipe(vstsBump({ versionPropertyType: 'string' }))
```  

- **indent**: number (default value: `2`) OR string  
Allowed values: any positive whole number between `1` and `10` inclusive, or the tab character `'\t'`. This controls the spacing indent value to use in the updated task.json file(s). If a number is specified, each level in the json file will be indented by that amount of space characters. Alternatively, if the tab `'\t'` character is specified, then each level will be indented with a tab. 

For example to indent by 4 spaces:  
```js
    .pipe(vstsBump({ indent: 4 }))
```  

Or if you prefer a tab:  
```js
    .pipe(vstsBump({ indent: '\t' }))
```

## License
MIT - see license details [here][license-url] 

## Contributing
Need to open an issue? Click the below links to create one:

- [Report a bug][create-bug-url]
- [Request an enhancement][create-enhancement-url]
- [Ask a question][create-question-url]

See the [developing guidelines][contrib-dev-url] for more info.

[npmjs-version-badge]: https://img.shields.io/npm/v/gulp-vsts-bump.svg
[npmjs-pkg-url]: https://www.npmjs.com/package/gulp-vsts-bump
[circle-ci-badge]: https://circleci.com/gh/swellaby/gulp-vsts-bump.svg?style=shield
[circle-ci-url]: https://circleci.com/gh/swellaby/gulp-vsts-bump
[appveyor-badge]: https://ci.appveyor.com/api/projects/status/8574rkisuw157e8h?svg=true
[appveyor-url]: https://ci.appveyor.com/project/swellaby/gulp-vsts-bump
[sonar-quality-gate-badge]: https://sonarcloud.io/api/project_badges/measure?project=swellaby%3Agulp-vsts-bump&metric=alert_status
[sonar-url]: https://sonarcloud.io/dashboard?id=swellaby%3Agulp-vsts-bump
[gulp-bump-pkg-url]: https://www.npmjs.com/package/gulp-bump
[coveralls-badge]: https://coveralls.io/repos/github/swellaby/gulp-vsts-bump/badge.svg?branch=master
[coveralls-url]: https://coveralls.io/github/swellaby/gulp-vsts-bump?branch=master
[license-url]: LICENSE
[vsts-task-manifest-url]: https://raw.githubusercontent.com/Microsoft/vsts-task-lib/master/tasks.schema.json
[create-bug-url]: https://github.com/swellaby/gulp-vsts-bump/issues/new?template=BUG_TEMPLATE.md&labels=bug,unreviewed&title=Bug:%20
[create-question-url]: https://github.com/swellaby/gulp-vsts-bump/issues/new?template=QUESTION_TEMPLATE.md&labels=question,unreviewed&title=Q:%20
[create-enhancement-url]: https://github.com/swellaby/gulp-vsts-bump/issues/new?template=ENHANCEMENT_TEMPLATE.md&labels=enhancement,unreviewed&title=E:%20
[contrib-dev-url]: .github/CONTRIBUTING.md#developing
