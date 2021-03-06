# grunt-connect-proxy

> Provides an http proxy as middleware for the grunt-contrib-connect plugin. 

## Getting Started
This plugin requires Grunt `~0.4.0`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-connect-proxy --save-dev
```

One the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-connect-proxy');
```

## Adapting the  "connect" task

### Overview

#### Proxy Configuration
In your project's Gruntfile, add a section named `proxies` to your existing connect definition.

```js
grunt.initConfig({
    connect: {
            options: {
                port: 9000,
                hostname: 'localhost'
            },
            proxies: [
                {
                    context: '/cortex',
                    host: '10.10.2.202',
                    port: 8080,
                    https: false,
                    changeOrigin: false
                }
            ]
        }
})
```
####
Adding the middleware
Expose the proxy function to use in the middleware, at the top of the grunt file:
```js
var proxySnippet = require('grunt-connect-proxy/lib/utils').proxyRequest;
```

Add the middleware call from the connect option middleware hook
```js
        connect: {
            livereload: {
                options: {
                    middleware: function (connect) {
                        return [
                            proxySnippet
                        ];
                    }
                }
            }
        }
```
### Adding the configureProxy task to the server task
For the server task, add the configureProxies task before the connect task
```js
    grunt.registerTask('server', function (target) {
        grunt.task.run([
            'clean:server',
            'compass:server',
            'configureProxies',
            'livereload-start',
            'connect:livereload',
            'open',
            'watch'
        ]);
    });
```


### Options
The available configuration options from a given proxy are generally the same as what is provided by the underlying [httpproxy](https://github.com/nodejitsu/node-http-proxy) library

#### options.context
Type: `String`

The context to match requests against. Matching requests will be proxied. Should start with /. Should not end with /

#### options.host
Type: `String`

The host to proxy to. Should not start with the http/https protocol.

#### options.port
Type: `Number`
Default: 80

The port to proxy to. 

#### options.https
Type: `Boolean`
Default: false

Whether to proxy with https

#### options.changeOrigin
Type: `Boolean`
Default: false

Whether to change the origin on the request to the proxy, or keep the original origin. 

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History
0.1.0 Initial release
