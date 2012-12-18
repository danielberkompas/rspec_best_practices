# How to Test External Services

Many apps rely on external services.  Often when you write your tests, you need to set up data in those external services and query them for it in order for your tests to be accurate.

"Out of the box," this has two main disadvantages:

1. Your tests will be slow.  Making a lot of requests to external services is expensive.
2. If these external services are micro-services written by you, you will not be able to use a continuous integration service such as RailsOnFire or Semaphore.

## The Solution: VCR

The [VCR gem](http://rubygems.org/vcr) is a great gem which can record your requests to external services for use in later tests.  Your tests only have to run against the real external services once; all later tests use recordings of those interactions.

### Configuration

**Gemfile**
```ruby
group :test do
  # Use either of these gems, as you find one or the
  # other works better for you.  They block your app
  # from making external requests that VCR isn't
  # monitoring.
  gem "webmock", "~> 1.8.0"
  gem "fakeweb" 
  gem "vcr"
end
```

Run `bundle install` to install these gems.

**spec_helper.rb**
Inside your `spec_helper.rb` file, you'll need to add the following configuration block:

```ruby
require 'vcr'

VCR.configure do |c|
  c.cassette_library_dir = 'vcr_cassettes'
  c.hook_into :webmock # or :fakeweb, if you want to use that
  c.configure_rspec_metadata!
  c.ignore_localhost = true # don't record interactions with localhost
  c.default_cassette_options = { :record => :new_episodes }
end
```

### Use VCR in a Test
This is how you use VCR in a test:

```ruby
require "spec_helper"

describe SomeClass do
  use_vcr_cassette # you can pass options into this method; read the VCR docs
  # ...
end
```

### Important Notes

1. Avoid using the [Faker](http://faker.rubyforge.org/) gem to create test data in your factories or anything else.  Using it defeats the purpose of VCR, because new requests are made to the external services every time the tests run.

