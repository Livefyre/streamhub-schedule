# streamhub-schedule

streamhub-schedule is a streamhub-sdk plugin takes a set of schedule objects and displays them in a horizontal calendar.

## Views:
The streamhub-schedule provides ```ScheduleView``` which is constructed with an array of event data objects which are
expected to have the following structure:

    event = { pk: "12345", fields: {
        start_date: "1/30/2013 9:00",
        end_date: "1/30/2013 12:00",
        title: "My event",
        description: "Great event description",
        sort_order: 12345  // optional
      }
    };
    
## To run the example site:

    $ git clone git@github.com:genehallman/streamhub-schedule.git
    $ cd streamhub-schedule
    $ npm install
    $ npm start

+ To see the demo, browse to [localhost:8080](http://localhost:8080)
+ To run the tests, browse to [localhost:8080/tests/index.html](http://localhost:8080/tests/index.html)
+ To see the docs, browse to [localhost:8080/docs/index.html](http://localhost:8080/docs/index.html)

## To install on your site:
The easiest way to use the streamhub-schedule is to install it via [bower](http://twitter.github.com/bower/) and [requirejs](http://requirejs.org/):

### Install via Bower
Bower can be used to automatically download streamhub-schedule and its dependency tree.

```
$ bower install git://github.com/genehallman/streamhub-schedule.git
```

#### Use via Require.js
Once you've called bower install, you'll have a suite of components available to you in the ```./components``` directory. These can be accessed via Require.js, as shown below.

    <!-- Get requirejs -->
    <script src="components/requirejs/require.js" type="text/javascript"></script>
    <!-- Get Livefyre sdk loader -->
    <script src="http://zor.t402.livefyre.com/wjs/v3.0.sdk/javascripts/livefyre.js"></script>
    <script type="text/javascript">
      require.config({
        baseUrl: 'components',
        paths: {
          jquery: 'jquery/jquery',
          text: 'requirejs-text/text',
          backbone: 'backbone/backbone',
          underscore: 'underscore/underscore',
          mustache: 'mustache/mustache',
          isotope: 'isotope/jquery.isotope',
          fyre: 'http://zor.t402.livefyre.com/wjs/v3.0/javascripts/livefyre',
        },
        packages: [ {
          name: 'streamhub-backbone',
          location: 'streamhub-backbone'
        },
        {
          name: "streamhub-ticker",
          location: "streamhub-ticker/src"
        }],
        shim: {
          backbone: {
              deps: ['underscore', 'jquery'],
              exports: 'Backbone'
          },
          underscore: {
              exports: '_'
          },
          isotope: {
              deps: ['jquery']
          },
          fyre: {
              exports: 'fyre'
          },
        }
      });
      // Now to load the example
      require(['streamhub-backbone', 'streamhub-ticker/views/TickerView'],
          function(Hub, View) {
              fyre.conv.load({network: "network.fyre.co"}, [{app: 'sdk'}], function(sdk) {
              var col = window.col = new Hub.Collection().setRemote({
                  sdk: sdk,
                  siteId: "12345",
                  articleId: "article_1"
              });
              
              var feedCol = window.feedCol = new Hub.Collection();
              
              col.on('initialDataLoaded', function() {
                  feedCol.setRemote({
                      sdk: sdk,
                      siteId: "12345",
                      articleId: "article_2"
                  });
              }, this);
              
              var view = new View({
                  collection: col,
                  el: document.getElementById("example"),
                  feedCollection:feedCol
              });
              view.render();
          });
      });
    </script>
