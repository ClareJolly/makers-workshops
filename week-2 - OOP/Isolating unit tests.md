## Isolating Unit Tests

If we don't isolate our tests we create dependencies in the tests as well as in the code.  The code is fine but supposing we made a change to a class that a test was relying on, we wouldn't be able to tell where the error was, in the main class, just because it has a dependency, or in the actual place where the bug is

``` ruby
# should be like this with no cross dependencies in the test
----------------------         -----------
Docking station object  ====   bike object
----------------------         -----------
    ||                            ||
-------                        ---------
DS test                        Bike test
-------                        ---------
```

look for `.new` in your tests as a good way of spotting this


``` ruby

#in the test file you could add

class BrokenBike
  def working
    false
  end
end

class WorkingBike
  def working
    true
  end
end

#and then use these in place of any .new references in the test

#instead of
bike = Bike.new

#use
bike = WorkingBike.new

```

Better way of doing it is

``` ruby
  let(:broken_bike)   {double :bike, :working => false}

  # broken bike can receive a call to working and return false

  let(:working_bike)  {double :bike}

  # and then in the test

  describe 'dock'
    it 'stores the bike' do
      # using just a double plus a stub
      allow(working_bike).to receive(:working) {true}
      #
      station.dock(bike)
      expect(station.bikes).to include(bike)
    end
  end
```
Another example

``` ruby
class ObjectUnderTest
    def initialize
      @other_object = OtherObject.new
    end

    def delegated_method
      @other_object.dangerous_operation
    end
  end

class OtherObject
  def dangerous_operation
    "DANGER!!!!!!"
  end
end

describe ObjectUnderTest do
  subject(object_under_test) { ObjectUnderTest.new }

  #as part of calling ObjectUnderTest - there is a call to create another object inside initialize here that we can't get around

  describe 'delegated_method' do
    it 'returns some value' do
      expect(object_under_test.delegated_method).to eq "DANGER!!!!!!"
    end
  end
end

```

### Dependency injection
This helps with isolating the test but also makes it reusable

``` ruby
class ObjectUnderTest
    def initialize(other_object = OtherObject.new)
      @other_object = other_object
      # we are no longer tied to using an object
    end

    def delegated_method
      @other_object.dangerous_operation
    end
  end

class OtherObject
  def dangerous_operation
    "DANGER!!!!!!"
  end
end

describe ObjectUnderTest do
  subject(object_under_test) { ObjectUnderTest.new(mock_other_object) }
  # adding a double and updating the subject above to use this double
  let(:mock_other_object)     { double :other_object, :dangerous_operation => "DANGER!!!!!!"  }

  describe 'delegated_method' do
    it 'returns some value' do
      #NEED TO CHANGE THE TEST AS THIS IS JUST TESTING SOMETHING WE KNOW WILL WORK AS WE TOLD IT TO
      # expect(object_under_test.delegated_method).to eq "DANGER!!!!!!"
      #so change to....
      expect(mock_other_object).to receive(:dangerous_operation)
      object_under_test.delegated_method
    end
  end
end

```

** read up on **
``` ruby
#argument hash
def initialize(other_object = OtherObject.new, capacity = 10)

how do we default the otherobject, while still adding a capacity
```
