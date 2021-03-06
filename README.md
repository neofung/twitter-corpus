General Info
------------

Collects all tweets from the sample Public stream using Twitter's
streaming API, and writes them to stdout, which you can redirect to a
file for later use as a corpus.

The sample Public stream "Returns a small random sample of all public
statuses. The Tweets returned by the default access level are the same,
so if two different clients connect to this endpoint, they will see the
same Tweets."

This module consumes tweets from the sample Public stream and putes them
on a queue. The tweets are then consumed from the queue by writing them
to a file in JSON format as sent by twitter, with one tweet per line.
This file can then be processed and filtered as necessary to create a
corpus of tweets for use with Machine Learning, Natural Language Processing,
and other Human-Centered Computing applications.

Usage
-----

First, you will need to configure the script by supplying tokens that
are generated by Twitter for your application.

-   Copy the `config-sample.yaml` file to `config.yaml`.
-   Follow the instructions in the config file to generate the
    necessary keys and tokens for your Twitter app, and add them to the
    config file.
-   Edit the script as needed. Use the following examples as a guide so
    that you may connect to the appropriate stream in the `main()`
    function.
-   Create a virtualenv and install the dependencies from the
    requirements file.
-   Run the script, redirecting stdout as needed e.g.:

        python twitter_corpus.py > tweet-stream.json

Status and error information will be written to stderr.

### Statuses/Sample

By default, the script is configured to connect to the *sample* stream,
which "returns a small random sample of all public statuses".

    stream.sample()

### Statuses/Filter

If you would like to filter tweets by location boxes then be sure to
read the [location](https://dev.twitter.com/docs/streaming-apis/parameters#locations)
parameter information from the Twitter API. Below is an example to
filter tweets for the continental United States.

    LOCATIONS = [-124.85, 24.39, -66.88, 49.38,]
    stream.filter(locations=LOCATIONS)

If you would like to filter by keywords instead, use the `track`
parameter. Below is an example to filter for some example emoticons.

    EMOTICONS = ">:] :-) :) :o) :] :3 :c) :> =] 8) =) :} :^) "
    EMOTICONS = EMOTICONS.strip().split(' ')
    stream.filter(track=EMOTICONS)

### More Information

Please refer to the `streaming.py` module from the *Tweepy* library.

Process vs. stdout
------------------

If you would like to modify the application to process tweets as they
are received instead of writing them to stdout, edit the `worker()`
function as needed.

Relevant Twitter API Documentation
----------------------------------

* [Public streams](https://dev.twitter.com/docs/streaming-apis/streams/public),
which describe the types of streams available.
* [Statuses/filter](https://dev.twitter.com/docs/api/1.1/post/statuses/filter),
which describes the limits for the number of keywords, users, and
location boxes that you are allowed to use with the filter. Pay special
note to the fact that all filters are combined through the **OR**
operator and not the AND operator. For example, specifying both location
and track parameters will return a Tweet object that matches either
criteria, and not necessarily both.
* [Tweet](https://dev.twitter.com/docs/platform-objects/tweets) objects,
which are returned by the streams. Describes all the fields present.
* [User](https://dev.twitter.com/docs/platform-objects/users) objects,
which are also embedded into each Tweet object. Describes all the fields
present.
* [Location](https://dev.twitter.com/docs/streaming-apis/parameters#locations)
parameter information.
* [Track](https://dev.twitter.com/docs/streaming-apis/parameters#track)
(keyword) parameter information.

Required Libraries
------------------

### Tweepy
* A Python library for accessing the Twitter API.
* Does all the heavy lifting of connecting to the sample Public stream.
* Available at: <https://github.com/tweepy/tweepy>
