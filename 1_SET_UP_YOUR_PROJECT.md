# How to Set up Your Project with Rspec

### 1. Install the Gems
Inside of your :test, :development group in your gemfile:

```ruby
group :test, :development do
  gem 'rspec-rails'         # This will also pull down the rspec gem
  gem 'guard-rspec'         # Run tests relevant to what you're working on automatically.
  gem 'factory_girl_rails'  # Excellent fixtures replacement
  gem 'fuubar'              # Progress bar and instant failing for your tests.
  # ...
end
```

Run the `bundle install` command to install all these gems.  You're going to need to run a couple more commands:

```bash
$ bundle exec rails generate rspec:install // Creates /spec directory, spec_helper file, replaces auto generators
$ bundle exec guard init rspec // Creates a Guardfile
```

See more details about these gems on their Github repos:

* [Guard](https://github.com/guard/guard-rspec)
* [Rspec](https://github.com/rspec/rspec-rails)
* [FactoryGirl](https://github.com/thoughtbot/factory_girl_rails). Read their documentation on how to get FactoryGirl set up with Rspec.

### 2. Clean Up The Setup

1. Remove the `/test` directory from your app.  You won't be needing it anymore, all your tests now will live under `/spec`.
2. Change your Guardfile to use fuubar.
```
guard 'rspec', cli: "--format Fuubar --color", version: 2 do
  # Don't touch the rest...
end
```

### 3. Run Your Tests
To run your tests, simply run `bundle exec guard` and Guard will start up.  It will first run all your tests, and then whatever tests are relevant to whatever you're working on.

And that's it! You should be ready to roll.

