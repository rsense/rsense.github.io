## About

RSense is a tool for doing static analysis of Ruby source code. Rsense is used in conjunction with an editor plugin.

RSense is currently under heavy development and ready for testing.  Currently we need to improve the homepage and develop plugins for code_completion.  In the near future we'll also be ready to implement some of the other basic features like `find-definition`. After that, there's plenty to do in the long term.  See the waffle link below to find out where you can pitch in. It would be awesome if you helped get things done.

![Rsense in Atom editor](https://cloud.githubusercontent.com/assets/1395968/3344028/5b3c2f0a-f8a6-11e3-8952-c0f7155cb19e.gif)

## Installation

Add this line to your application's Gemfile:

    gem 'rsense'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rsense

## Usage

Install one of these plugins:
-  [rsense/atom-rsense](https://atom.io/packages/rsense)

The Rsense server is started from the commandline with:

    $ rsense start

It takes two potential options: `--port` and `--path`.

Currently only the Atom plugin is available, but a Sublime Text plugin will follow shortly. After that, if the community has not already contributed Vim and Emacs plugins, we will do so.

**When using Rsense, please be patient. After starting the server and triggering the initial completion, Rsense can be slow to load your projects dependencies into it's graph. It's quite fast after this initial completion and in the future we will optimize this.**

## Plugin Authors

Rsense plugins are easy to implement.  First your plugin may want to ensure the Rsense server has been started.  It can do this by shelling out to the command line with `rsense start`. Alternately, you can instruct your users to start it from the command line. The server can optionally take a port number like this: `rsense start --port 12345`. The default port is `47367`. It also takes a project path, in case the user has a `.rsense` config file there.  For now, this config file is not very useful, but it may become so in the future.

The rsense server will be running at `http://localhost:47367` (or an alternate port if you specify one).  It communicates via json.  You need to send it json like the following example:

```json
{
    "command": "code_completion",
    "project": "spec/fixtures/test_gem",
    "file": "spec/fixtures/test_gem/lib/sample.rb",
    "code": "require \"sample/version\"\n\nmodule Sample\n  class Sample\n    attr_accessor :simple\n\n    def initialize\n      @simple = \"simple\"\n    end\n\n    def another\n      \"another\"\n    end\n  end\nend\n\nsample = Sample::Sample.new\nsample",
    "location": {
        "row": 18,
        "column": 7
    }
}
```

For now, `code_completion` is the only command available, but this will change in the future. Project is the root dir of the user's project. This is needed for finding information about project dependencies.  Code is the text from the file where a completion is being triggered. Location should be obvious.

Rsense will return json that looks like the below:

```json
{
  "completions":
  [
    {
      "name":"taint",
      "qualified_name":"Object#taint",
      "base_name":"Object",
      "kind":"METHOD"
    },
    {
      "name":"methods",
      "qualified_name":"Object#methods",
      "base_name":"Object",
      "kind":"METHOD"
    }
  ]
}

```

## Contributing

Contributions can only be accepted if they include tests.

1. Fork it ( https://github.com/[my-github-username]/rsense/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## License

RSense is distributed under the term of
[GPLv3+](http://gplv3.fsf.org/).

## Credits

Rsense was originally designed and implemented by [Matsuyama Tomohiro(@m2ym)](https://github.com/m2ym/), and his hard work is still at the core of rsense today.  All of the algorithms for type-detection were implemented by him, with inspiration from multiple places.  You can read about his original version at [Rsense: A Ruby Development tools for Emacs, Vim and Others](http://cx4a.org/software/rsense/)

In 2013, a major undertaking by @edubkendo to bring it current and improve its usefullness to rubyists was sponsored by the @jruby organization as a Google Summer of Code project.

Special thanks belongs to [Tom Enebo (@enebo)](https://github.com/enebo) who provided excellent mentorship, code, architectural suggestions and more throughout the course of the update.

## Community

[![Gitter chat](https://badges.gitter.im/rsense/rsense.png)](https://gitter.im/rsense/rsense)

[![Stories in Ready](https://badge.waffle.io/rsense/rsense.png?label=ready&title=Ready)](https://waffle.io/rsense/rsense)

[Github](https://github.com/rsense/rsense)
