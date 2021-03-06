# hapi-swagger


This is a [Swagger UI](https://github.com/wordnik/swagger-ui) plug-in for [HAPI](http://spumko.github.io/). When installed it will self document HTTP API interface in a project.


## Install

You can add the module to your HAPI using npm:

    $ npm install hapi-swagger --save,

    
## Adding the plug-in into your project

In the .js file where you create the HAPI `server` object add the following code after you have created the `server` object:

    var pack = require('../package'),
        swaggerOptions = {
            basePath: 'http://localhhost:8000',
            apiVersion: pack.version
        };

    server.pack.require({'hapi-swagger': swaggerOptions}, function (err) {
        if (!err && err !== null) {
            server.log(['error'], 'Plugin "hapi-swagger" load error: ' + err) 
        }else{
             server.log(['start'], 'swagger interface loaded')
        }
    });


## Tagging your API routes
As a project may be a mixture of web pages and API endpoints you need to tag the routes you wish Swagger to document. Simply add the `tags: ['api']` property to the route object for any endpoint you want documenting.


    {
        method: 'GET',
        path: '/todo/{id}/',
        config: {
            handler: handlers.mapUsername,
            description: 'Get todo',
            notes: 'Returns a todo item by the id passed in the path',
            tags: ['api'],        
            validate: { 
                path: {
                    username: hapi.types.String()
                            .required()
                            .description('the id for the todo item'),
                }
            }
        },
    }
    
## Adding interface into a page
The plugin adds all the resources needed to build the interface into your project. All you need to do is add some javascript into the header of a web page and add two elements into the HTML where you wish it to render.


### Adding the javascript

The doc directory and all the files in the URLs below are added by the plugin


    <link href='https://fonts.googleapis.com/css?family=Droid+Sans:400,700' rel='stylesheet' type='text/css'/>
    <link href='/docs/swaggerui/css/highlight.default.css' media='screen' rel='stylesheet' type='text/css'/>
    <link href='/docs/swaggerui/css/screen.css' media='screen' rel='stylesheet' type='text/css'/>
    <script src="/docs/swaggerui/lib/shred.bundle.js" type="text/javascript"></script>
    <script src='/docs/swaggerui/lib/jquery-1.8.0.min.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/lib/jquery.slideto.min.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/lib/jquery.wiggle.min.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/lib/jquery.ba-bbq.min.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/lib/handlebars-1.0.0.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/lib/underscore-min.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/lib/backbone-min.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/lib/swagger.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/swagger-ui.js' type='text/javascript'></script>
    <script src='/docs/swaggerui/lib/highlight.7.3.pack.js' type='text/javascript'></script>
    <script type="text/javascript">
      $(function () {
        window.swaggerUi = new SwaggerUi({
          url: window.location.protocol + '//' + window.location.host + '/docs',
          dom_id: "swagger-ui-container",
          supportedSubmitMethods: ['get', 'post', 'put', 'delete'],
          onComplete: function(swaggerApi, swaggerUi){
            log("Loaded SwaggerUI")
            $('pre code').each(function(i, e) {hljs.highlightBlock(e)});
          },
          onFailure: function(data) {
            log("Unable to Load SwaggerUI");
          },
          docExpansion: "list"
        });
        window.swaggerUi.load();
      });
    </script>

  
### Adding the HTML elements

Place the HTML code below into the body fo web page where you wish the interface to render

<pre>
&lt;section id=&quot;swagger&quot;&gt;
    &lt;h1 class=&quot;entry-title api-title&quot;&gt;API&lt;/h1&gt;
    &lt;div id=&quot;message-bar&quot; class=&quot;swagger-ui-wrap&quot;&gt;&lt;/div&gt;
    &lt;div id=&quot;swagger-ui-container&quot; class=&quot;swagger-ui-wrap&quot;&gt;&lt;/div&gt;
&lt;/section&gt;
</pre>


### Error Status Codes
You can add HTTP error status codes to each of the endpoints. As HAPI routes don not directly have a property for error status codes so you need to add them to the notes. The status codes need to be added to the end of the notes array starting with array item with the value "Error status codes". Each error code should be added as string with code first follow by its description:

    config: {
        handler: handlers.add,
        description: 'Add',
        tags: ['api'],
        jsonp: 'callback',
        notes: [
            'Adds together two numbers and return the result',
            'Error status codes',
            '400, bad request'
        ],
        validate: { 
            path: {
                a: hapi.types.Number()
                    .required()
                    .description('the first number'),

                b: hapi.types.Number()
                    .required()
                    .description('the second number')
            }
        }
    }


### Mocha test
The project has a small number integration and unit tests. To run the test within the project type the following command.

    $ mocha --reporter list

If you are considering sending a pull request please add tests for the functionality you add or change.


### Thanks
I would like to thank [Brandwatch](http://www.brandwatch.com/) who allow me to open this code up as part of the work on this plugin was done during a contract with them.

### Contributors
* Joshua McGinnis (https://github.com/joshua-mcginnis)
* David Waterston (https://github.com/davidwaterston)
* Jozz (https://github.com/jozzhart)
* John Oliva (https://github.com/joliva)
* thiagogalesi4e (https://github.com/thiagogalesi4e)
* HughePaul (https://github.com/HughePaul)


### This is a work in progress
If you find any issue please file here on github and I will try and fix them.