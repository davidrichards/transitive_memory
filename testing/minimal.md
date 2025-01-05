# Minimal

## Unit Tests

The most basic way to run unit test is:

- pull test classes from the ObjectSpace (Ruby)
- from class to instance to test list
- call methods in a begin / rescue loop
- create feedback

Here's a GPT-suggested example:

```ruby
module SimpleTest
  class AssertionError < StandardError; end

  def self.run
    test_classes = ObjectSpace.each_object(Class).select { |klass| klass < SimpleTest::TestCase }
    test_classes.each do |test_class|
      test_instance = test_class.new
      test_class.instance_methods.grep(/^test_/).each do |method|
        begin
          test_instance.send(method)
          puts "#{method} passed"
        rescue AssertionError => e
          puts "#{method} failed: #{e.message}"
        end
      end
    end
  end

  class TestCase
    def assert(condition, message = "Assertion failed")
      raise AssertionError, message unless condition
    end

    def assert_equal(expected, actual, message = nil)
      message ||= "Expected #{expected.inspect}, got #{actual.inspect}"
      assert(expected == actual, message)
    end

    def assert_not_equal(expected, actual, message = nil)
      message ||= "Did not expect #{expected.inspect}, but got #{actual.inspect}"
      assert(expected != actual, message)
    end
  end
end
```

From there, I can create a test class:

```ruby
require_relative 'simple_test'

class ExampleTest < SimpleTest::TestCase
  def test_addition
    assert_equal(4, 2 + 2, "Addition test failed")
  end

  def test_subtraction
    assert_equal(0, 2 - 2, "Subtraction test failed")
  end
end

SimpleTest.run
```

Minitest offers this basic construct:

```ruby
class MyTest < Minitest::Test
  def setup
    @resource = Resource.new
  end

  def teardown
    @resource.cleanup
  end
end
```
