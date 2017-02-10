<img alt="Screenshot embedded tweet" src="https://raw.githubusercontent.com/rob-murray/jekyll-twitter-plugin/master/media/embedded-tweet.png" align="right" />

# jekyll-twitter-plugin

A Liquid tag plugin for Jekyll blogging engine that embeds Tweets, Timelines and more from Twitter API.

[![Build Status](https://travis-ci.org/rob-murray/jekyll-twitter-plugin.svg?branch=master)](https://travis-ci.org/rob-murray/jekyll-twitter-plugin)
[![Gem Version](https://badge.fury.io/rb/jekyll-twitter-plugin.svg)](http://badge.fury.io/rb/jekyll-twitter-plugin)

---


## Description

**jekyll-twitter-plugin** is a Liquid tag plugin for [Jekyll](http://jekyllrb.com/) that enables Twitter content to be used in any pages generated by Jekyll. Content is fetched from the [Twitter Publish platform](https://publish.twitter.com).

The [Publish platform](https://publish.twitter.com) allows Twitter users to curate content for display outside of Twitter. You can display many different types of content with the familiar Twitter styling. We use this API and allow any customisation of the content that is accepted by the Twitter publish platform.

> You can now embed any Twitter content in your Jekyll powered blog!

### Here are a few examples

#### Tweet

An example of a Tweet - `{% twitter https://twitter.com/rubygems/status/518821243320287232 %}`

![Embedded tweet](https://raw.githubusercontent.com/rob-murray/jekyll-twitter-plugin/master/media/embedded-tweet.png "Screenshot of embedded tweet")

#### Timeline

An example of a Timeline - `{% twitter https://twitter.com/jekyllrb maxwidth=500 limit=5 %}`

![Embedded timeline](https://raw.githubusercontent.com/rob-murray/jekyll-twitter-plugin/master/media/embedded-timeline.png "Screenshot of embedded timeline")

#### Grid Timeline

An example of a Grid Timeline - `{% twitter https://twitter.com/TwitterDev/timelines/539487832448843776 limit=5 widget_type=grid maxwidth=500 %}`

![Embedded Grid Timeline](https://raw.githubusercontent.com/rob-murray/jekyll-twitter-plugin/master/media/embedded-grid.png "Screenshot of embedded Grid Timeline")

#### Moment

An example of a Moment - `{% twitter https://twitter.com/i/moments/650667182356082688 maxwidth=500 %}`

![Embedded moment](https://raw.githubusercontent.com/rob-murray/jekyll-twitter-plugin/master/media/embedded-moment.png "Screenshot of embedded moment")


### Features

The plugin supports the following features:

* Installed via Rubygems.
* [Customisation](#customisation) - All customisation options passed to Twitter API.
* [Authentication](#authentication) - No authentication required!
* [Caching](#caching) - Twitter API responses can be cached to speed up builds.


## Getting Started

As mentioned by [Jekyll's documentation](http://jekyllrb.com/docs/plugins/#installing-a-plugin) you have two options; manually import the source file, or require the plugin as a `gem`.


#### Require gem

Install the gem, add it to your Gemfile;

```ruby
gem 'jekyll-twitter-plugin'
```

Add the `jekyll-twitter-plugin` to your site `_config.yml` file for Jekyll to import the plugin as a gem.

```ruby
gems: ['jekyll-twitter-plugin']
```

#### Manual import

> Note: this is deprecated and support will be removed in a later version.

Just download the source file into your `_plugins` directory, e.g.

```bash
# Create the _plugins dir if needed and download project_version_tag plugin
$ mkdir -p _plugins && cd _plugins
$ wget https://raw.githubusercontent.com/rob-murray/jekyll-twitter-plugin/master/lib/jekyll-twitter-plugin.rb
```


#### Plugin tag usage

To use the plugin, in your source content use the tag `twitter` and then pass additional options to the plugin. These are passed to the API.

```liquid
{% plugin_type twitter_url *options %}

# Example for timeline of the **jekyllrb** user with a maximum of 5 Tweets and with a width of 500px
{% twitter https://twitter.com/jekyllrb maxwidth=500 limit=5 %}
```

| Argument | Required? | Description |
|---|---|---|
| `plugin_type` | Yes | Either `twitter` or `twitternocache` (same as `twitter` but does not cache api responses) |
| `twitter_url` | Yes | The Twitter URL to use, check below for supported URLs. |
| `*options` | No | Parameters for the API separated by spaces. Refer below and to respective Twitter API documentation for available parameters. |


### Supported Twitter URLs

The Twitter URLs that are supported depend on Twitter. We pass the url and all parameters to the API - check [Twitter Publish platform](https://publish.twitter.com) for availability. Here is documentation for some common types:

##### Tweet:

* [https://dev.twitter.com/web/overview](https://dev.twitter.com/web/overview)
* [https://dev.twitter.com/rest/reference/get/statuses/oembed](https://dev.twitter.com/rest/reference/get/statuses/oembed)
* [https://dev.twitter.com/web/embedded-tweets/parameters](https://dev.twitter.com/web/embedded-tweets/parameters)

##### Timeline:

* [https://dev.twitter.com/web/embedded-timelines/oembed](https://dev.twitter.com/web/embedded-timelines/oembed)

##### Moments:

* [https://dev.twitter.com/web/embedded-moments/oembed](https://dev.twitter.com/web/embedded-moments/oembed)

### Customisation

All pairs of options and values after the URL are passed to the API. The parameters must be in pairs with the option as the key: `option=value`.

For example, if you want to limit the width of the embedded content then the API supports a `maxwidth` option so you could construct the tag as below to limit it to a value of 500 (pixels).

```liquid
{% twitter https://twitter.com/jekyllrb maxwidth=500 %}
```

### Authentication

The API does not require any authentication.

> Previous version of this library used an API that required API keys (TWITTER_CONSUMER_KEY TWITTER_CONSUMER_SECRET TWITTER_ACCESS_TOKEN TWITTER_ACCESS_TOKEN_SECRET). We now print a warning if these are detected as a reminder that you can safely remove them.


### Output

All content will be rendered inside a div with the class `jekyll-twitter-plugin`.

```html
<div class='jekyll-twitter-plugin'>
    -- content from API --
</div>
```

If something goes wrong, then a basic error message will be displayed:

> Tweet could not be processed

If we receive an error from the API then a message will be cached and rendered something like this below. If it's a 404, then this suggests the Tweet is protected or deleted.  I will not be fetched again and again. If the Tweet is restored, then simply delete the cached response from `.tweet-cache` directory and build again.

```html
<div class='jekyll-twitter-plugin'>
    <p>There was a '{error name}' error fetching Tweet '{Tweet status url}'</p>
</div>
```

### Caching

Twitter API responses can be cached to speed up Jekyll site builds. The reponses will be cached in a directory within your Jekyll project called `.tweet-cache`. This should not be committed to source control.

Caching is enabled by using the `twitter` tag.

It is possible to disable caching by using the specific `twitternocache` tag.

```liquid
{% twitternocache twitter_url *options %}

# Example
{% twitternocache https://twitter.com/rubygems/status/518821243320287232 %}

```


### Contributions

I've tried hard to keep all code in the one `lib/jekyll-twitter-plugin.rb` file so that people can just grab this file and include in their Jekyll `_plugins` directory if they do not want to install with Rubygems. This will be dropped in a later version.

Please use the GitHub pull-request mechanism to submit contributions.

There is a quick integration test in `spec/integration_tests.rb` that will use the **jekyll-twitter-plugin** public api and output a file `output_test.html`. Run this with the following command:

```ruby
$ ruby spec/integration_tests.rb && open output_test.html
```


## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/rob-murray/jekyll-twitter-plugin/tags).


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
