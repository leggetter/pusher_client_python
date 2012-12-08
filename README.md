# Pusher Python module

## Installation

This module has been tested with Python 2.5 and 2.6.

You can install this module using your package management method or choice, normally `easy_install` or `pip`. For example:

    pip install pusher

## Getting started

Register at <http://pusher.com> for a Pusher account.

### Global Configuration

    pusher.app_id = 'your-pusher-app-id'
    pusher.key = 'your-pusher-key'
    pusher.secret = 'your-pusher-secret'

### Create a Pusher instance

    my_pusher = pusher.Pusher(app_id='your-pusher-app-id', key='your-pusher-key', secret='your-pusher-secret')

## Triggering Events

Trigger an event. Channel and event names may only contain alphanumeric characters, '-' and '_':

    p['a_channel'].trigger('an_event', {'some': 'data'})

You can also specify `socket_id` as a separate argument, as described in <http://pusherapp.com/docs/duplicates>:

    my_pusher['a_channel'].trigger('an_event', {'some': 'data'}, socket_id)

## Generic requests to the Pusher REST API

Aside from triggering events, the REST API also supports a number of operations for querying the state of the system. A reference of the available methods is available at http://pusher.com/docs/rest_api.

All requests must be signed by using your secret key, which is handled automatically using these methods:

    my_pusher.get( 'path', params )

Examples of interactions:

### All channels within your application

    response = my_pusher.get( '/channels' )
    if response['statusCode'] == 200:
        channels = response['result']
        # Loop through channels

### Information on a single channel

    response = my_pusher.get( '/channels/test_channel' )
    if response['statusCode'] == 200:
        occupied = response['occupied']       

### Information on a single Presence channel

    response = my_pusher.get( '/channels/presence-channel', { 'info': 'user_count' } )
    if response['statusCode'] == 200:
        occupied = response['occupied']
        user_count = response['result']['user_count'] 

## Heroku

If you're using Pusher as a Heroku add-on, you can just get the config informat

    p = pusher.pusher_from_url()

## Tornado

To use the Tornado web server to trigger events, set `channel_type`:

    pusher.channel_type = pusher.TornadoChannel

To see this functionality in action, look at `examples/tornado_channel.py`.

## Google AppEngine

To force the module to use AppEngine's urlfetch, do the following on setup:

    pusher.channel_type = pusher.GoogleAppEngineChannel

I haven't been able to test this though. Can somebody confirm it works? Thanks! `:-)`

## Running the tests

The `pusher` directory contains the following test files:

* test.py
* acceptance_test.py

The tests can be run using [nose](http://readthedocs.org/docs/nose/en/latest/). You will need to run them individually using `nosetests <filename>` e.g `nosetests acceptance_test.py`
  
The tests defined in `acceptance_test.py` execute against the Pusher service. For these to run you must rename the `test_config.example.py` file to `test_config.py` and update the values in it to valid Pusher application credential files.

## Python 2.4 - 2.6 support

The Pusher Python library now depends on the OrderedDict collection that was introduced in Python 2.7. To run GenSON with Python 2.4 - 2.6, install the ordereddict package with pip or visit the following links:

http://code.activestate.com/recipes/576693/
http://pypi.python.org/pypi/ordereddict

## Special thanks

Special thanks go to [Steve Winton](http://www.nixonmcinnes.co.uk/people/steve/), who implemented a Pusher module in Python for the first time, with focus on AppEngine. This module borrows from his contribution at <http://github.com/swinton/gae-pusherapp>

## Copyright

Copyright (c) 2012 Pusher Ltd. See LICENSE for details.
