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

```ruby
guard 'rspec', cli: "--format Fuubar --color", version: 2 do
  # Don't touch the rest...
end
```

#### Tips for Guard
Guard has some annoying tendencies.  By default, it runs all of your tests when you run `bundle exec guard` and it does so again whenever the tests you're currently working on pass.

Also, you may want to group your tests so that you can run all your unit tests and acceptance (capybara) tests separately.  Here's a sample `Guardfile` that addresses these issues:

```ruby
# Creating an Rspec "group" allows you to run the tests in this group
# by themselves.  This is nice as you get a larger test suite.
# 
# How to use:
#
#   bundle exec guard
#   all unit
#
group "unit" do
  # The :spec_paths option limits actually limits the scope where this 
  # group will look for specs to run.
  #
  # The :all_on_start option should be set to false, so you can start 
  # up Guard and then immediately start testing whatever you want.
  #
  # Similarly, the :all_after_pass method should also be set to false.
  # This will prevent Guard from running all your tests after the 
  # current thing you're working on is passing.  
  #
  guard 'rspec', spec_paths: ['spec/models', 'spec/controllers', 'spec/actors', 'spec/decorators', 'spec/workers', 'spec/mailers'], cli: "--format Fuubar --color", all_on_start: false, all_after_pass: false do
    watch(%r{^spec/.+_spec\.rb$})
    watch(%r{^lib/(.+)\.rb$})     { |m| "spec/lib/#{m[1]}_spec.rb" }
    watch('spec/spec_helper.rb')  { "spec" }

    # Rails example
    watch(%r{^app/(.+)\.rb$})                           { |m| "spec/#{m[1]}_spec.rb" }
    watch(%r{^app/(.*)(\.erb|\.haml)$})                 { |m| "spec/#{m[1]}#{m[2]}_spec.rb" }
    watch(%r{^app/controllers/(.+)_(controller)\.rb$})  { |m| ["spec/routing/#{m[1]}_routing_spec.rb", "spec/#{m[2]}s/#{m[1]}_#{m[2]}_spec.rb", "spec/acceptance/#{m[1]}_spec.rb"] }
    watch(%r{^spec/support/(.+)\.rb$})                  { "spec" }
    watch('config/routes.rb')                           { "spec/routing" }
    watch('app/controllers/application_controller.rb')  { "spec/controllers" }
    
  end
end

group "acceptance" do
  guard 'rspec', spec_paths: ['spec/acceptance'], cli: "--format Fuubar --color", all_on_start: false, turnip: true do
    # Capybara request specs
    watch(%r{^app/views/(.+)/.*\.(erb|haml)$})          { |m| "spec/requests/#{m[1]}_spec.rb" }
    
    # Turnip features and steps
    watch(%r{^spec/acceptance/(.+)\.feature$})
    watch(%r{^spec/acceptance/steps/(.+)_steps\.rb$})   { |m| Dir[File.join("**/#{m[1]}.feature")][0] || 'spec/acceptance' }
  end
end
```

### 3. Run Your Tests
To run your tests, simply run `bundle exec guard` and Guard will start up. 

```bash
07:04:36 - INFO - Guard is now watching at '/Users/dberkom/Sites/ManageMyProperty/MyForks/mentor'
07:04:36 - INFO - Guard::RSpec is running
07:04:36 - INFO - Guard::RSpec is running
[1] guard(main)> 
```

And that's it! You should be ready to roll.  Guard will automatically run any relevant spec for a file you're working on, and if you want to run all your tests you can type one of the following into the Guard console:

```bash
  all
  all group_name
```

