# How to Test Classes
Here's a sample template for how to describe a class.

### spec/[type of class]/class_name_spec.rb
```ruby
require "spec_helper" # Pulls in all of Rspec's goodness. The rest of your files should be auto-loaded.

describe ClassName do

  # Describe each method on your class.
  # Note that the `describe` call limits the scope of everything inside it.
  
  # This is how you describe a "class method", defined in your class like this:
  # def self.class_method
  #   ...
  # end
  # 
  # Results in a failure that looks like this:
  #
  #   ClassName.class_method failed ...
  #
  describe ".class_method" do
    # ...
  end
  
  # This is how you describe an "instance method", defined like this:
  # def instance_method
  #   ...
  # end
  #
  # Results in a failure that looks like this:
  #
  #   ClassName#instance_method failed ...
  #
  describe "#instance_method" do
    # Do setup for the method with "let" syntax. Your method is
    # going to need an instance of your class to operate on, for
    # example.
    let(:class_name) { ClassName.new }
    let(:some_var) { some_value }
    # etc...
    
    # Set the subject
    subject { class_name }
    
    # Run any additional setup you need. `before` runs before every 
    # "it" block in the current scope.
    before { do_some_more_setup }
    
    # When you're testing expectations like should_receive, 
    # you might find `after` useful.  It runs after every
    # "it" block in the current scope.
    after { call_some_method_that_should_trigger_expectations }
    
    # "it" and "its" implicitly refer to the subject
    it { should be_something }
    
    # Use "its" to test methods on the subject
    its(:attribute) { should be == some_value }

    # When you have test that needs more explanation, use a
    # wordy "it" block like so:
    it "does some complicated thing" do
      # test here
    end
    
    # Does your method do different things in different
    # scenarios?  If so, use `context`.  
    context "when some criteria is true" do
      # then actually make the criteria the case using a "let" block
      # then add your tests in that scenario
    end
    
    context "when some criteria is not true" do
      # etc
    end
    
    # NOTE: Context also limits everything inside to that context.
  end
end
```

## How Thorough Should I Be?

Start by being **very** thorough.  Test all the methods in your class.  Once you get familiar with the toolchain and the rspec syntax, you can start easing off and only test the truly important methods.
