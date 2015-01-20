# Grunt and the Builds Training

> Grunt, Bower and how they interact with the build system. How to use grunt as developers

## Bower

It is a package management system for the web. It is a key-value pair for an endpoint in github. You can also have your own bower server. TR is hoping to have one of these inside their ecosystem.

Enables the ability to download all third-party dependencies from a single location and ensure the correct version. 

* .bowerrc - config file that tells bower where to download the packages
* bower.json - tells bower which dependencies you want to download, including versions. 

You can run **bower install** to get latest. Currently we push our bower components up to TFS, but ideally if we had our own bower server, this would not be necessary. Grunt tasks will automatically get latest and updated index.html with the latest references.

## Grunt

A JavaScript task runner. It allows you to automate common tasks using JavaScript. There is another task runner out there called Gulp -- the difference is that Grunt is configuration based where as Gulp is pipe-based. Grunt is considered to be a lot more mature at the moment, but Gulp is making progress. Gulp might be considered easier for developers because it is inline with how we think. 

One of the things that make Grunt very powerful are the plugins -- there is a ton of functionality already written. gruntjs.com/plugins 

### How we are using Grunt

We have a package.json file that tells the system what all of our grunt tasks (and grunt itself) that we depend on. 

First thing you do is run **npm install**. It will look at the dependencies in package.json and download all of them into the node_modules folder. 

You will notice some tilde's on the version in package.json. Tilde indicates to grunt that you only want the specified version up to the last minor version (ex. ~0.0.14 = 0.0.15<). You can also specify '<' or '>' depending on whatever you need. This becomes very important to lock down versions of things like angular and jquery.

#### Adding a new dependency

> npm install <package-name> --save-dev

This will save the development version into our node_modules folder and update the package.json file.  

#### Gruntfile.js

This is the default convention for grunt. Specifies the grunt configuration. ** went to the restroom for most of this, watch recording**

grunt.initConfig:

* pkg: identifies the package.json file
* yeoman: variables we want to use in the system. 
	* app: file path to the distribution creation of the build system
	* dist: can be used to define unique environments, here we do not really have dev/qa/prod builds
* watch: task that says to watch the style files, and if we do, execute copy and autoprefixer

##### Grunt Build

1. clean: 
	* cleans up .tmp and yeoman dist files. 
2. useminPrepare
	* targets index.html and do what it needs to do to prepare itself usemin
	* it will look for build tags in the HTML (build:css styles/main.css, build:js js/moduals.js)
	* we have divided the build into three js files, and one css file. 
	* will run concat and uglify against our targets
3. concurrent:dist
	* it will concurrently copy and minify our html and images.
4. autoprefixer
	* Goes through our css and adds our vendor prefix if we don't already have it. For example, if you added a rounded corner, it will automatically add the -moz and -webkit for you
5. concat
6. copy:dist
	* cop various files to distribution directory
7. cdnify
	* swap out vendor libraries to CDN references, is bower aware and will keep versions in sync
8. ngmin
	* minification with angular can blow up if you don't use the array version.
	* ng-min does this for you, change the declarations to be viable for minification
9. cssmin
	* minifies css styles
10. uglify
	* obfuscates the code and adds banner for Thomson Reuters. 
11. rev
	* will look at images/styles/js and add version number prefix. Then goes into index.html and changes them to that version.
	* This is really handy because it changes the names of the files, this prevents some of the caching issues that I've seen in the past. 
12. usemin
	* minifies and bundles everything

htmlmin is currently commented out. It will remove comments, collapse booleans, etc. In order to compress the HTML. 

In our app we only have one build. We could add more tasks and deploy to other locations if we needed to. 

Ideally jshint would be running in the build as well, but we currently have too many errors. 

On the CI server they will run the build even if the test fails. Right now, every 15 minutes the build service will go out and check TFS. If a change is detected, it will clean and pull latest, and running 'test' and then 'build'. He is hoping to add 'sonar' and 'reports' task in the next couple of days. 

#### Karma.js

coverage node will create a coverage report using istanbul and will dump the html report. 

sonar node will run the coverage reports, create the configuration files, then push up to sonar server for management

unit node specifies how grunt should process the unit tests and where they are defined, etc

#### Grunt Plugins We Use

##### load-grunt-tasks

It will go into package.json file and look for all of the grunt-tasks you have installed and load them automatically. Now we don't need to remember to explicitly declare the tasks. 

##### time-grunt

At the end of all your tasks, it gives you a summary of how long each took.

## FAQ

* No coverage of ng-templates plugin. 
	* No we are not using it, and doesn't plan on it. He worries about going forward in the next 2 years he thinks we might have to rip it back out. It basically caches your templates.
* Pattern: put various attributes in the package file. Why not just make it in the grunt file?
	* he really only uses name and version, he does it out of habit. If we had other needs he would recommend Gruntfile.js for other more specific properties. 
	* He is also worried about the version in package.json, we are not using it because they are not sure how they want to use it. It can be versioned differently than your web services now because they are separate dependencies. grunt-bump does the version bumping for you. 
* bowerInstall task - watch bower file and automatically update deps.
* Why use jasmine instead of karma?
	* Jasmine is a headless test and is lighter, but it is non-standard.  
	* PhantomJs is a headless browser as well, but it is webkit. 

