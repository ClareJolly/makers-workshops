## Feature testing in rspec

series of instructions for interacting with the program

IRB
```
irb>
airport = Airport.new
plane = Plane.new
airport.lane(plane)
airport.planes
```
rspec - call the file something like feature_spec.rb

``` ruby
describe 'features' do
  it 'landed' do
    airport = Airport.new
    plane = Plane.new
    airport.lane(plane)
    airport.planes
    expect(airport.planes).to include(plane)
  end
end
```

MAIN DIFFERENCE IS THAT WE'D BE USING A DOUBLE IN THE UNIT TEST - WHEREAS HERE WE ARE TESTING THE OVERARCHING FEATURE
