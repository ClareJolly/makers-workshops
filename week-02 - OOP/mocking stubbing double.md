## Mocking, stubbing and doubles

### Mocking is the umbrella term

### Stubbing
is what I have done - where you say that an object can call a method and return a value.  Overwriting the default behaviour

### Doubling
allows us to isolate our unit tests.
Unit tests should only rely on behaviour of class under test - e.g. airport spec, only test airport.  Not worry about dependencies

e.g.
```
Airport
  @weather
    weather
```
     - rather than use it this way - make this one a dummy weather object.  And then stub onto this dummy object

e.g. Bad weather double - to which other stubs can be applied

When code works, not using doubles seems fine

However - if a test is relying on another class to test, but this other class is broken.  It's unclear where the failure is

e.g.
```
airport test.....
...
plane = Plane.new
...
```

if I broke plane, then this airport specific test will break.  False positive.

Use a double to replace real plane class

could just do
```
class MockPlaneClass
end
```
and use that.  however the MockPlaneClass doesn't have any methods associated etc.  a double is useless on it's own so then also need to stub it.

This sort of replicates the doubling

```
class MockPlaneClass
  def working
    false
  end
end
```

Rspec gives us a useful way to do this

`let(:MockPlaneClass) { double(:plane)}`     - double name (:<name>) doesn't matter what it is.  Worth giving it the name of the class

`let(:MockPlaneClass) { double :plane, :working => false}`

Can either stub things out here - e.g. `:working => false` (the working method = false) or then use the other `allow` method of stubbing

could use this if you wanted doubles to be used for different methods.

That's doing the same as the `MockPlaneClass` above.

No more reliance on the plane object in the airport specs.  You can comment out the actual bike class and it will still pass as it's doubled.

#### Best practice to stub using a double rather than just stubbing alone



_READ UP ON "ANY INSTANCE OF"_
