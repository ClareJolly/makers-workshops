```
As a mathematician
so i can add numbers
I want to add two numbers on my calculator
```

ctrl + l to clear IRB
pwd check directory

take user story and split into objects and functions
useful method - nouns and verbs

e.g.
Nouns (object/class)
user - mathematician
calculator
numbers

Verbs (method)
add

Start with feature test - what does the user want
This tells us when our work is done.  Will guide you through process

IRB for now to test feature tests - makes it an easy distinction

irb>
require './calculator.rb'
calculator = Calculator.new
calculator.add(2,3)

run the test

errors

create a unit test - reproduce IRB error with unit test.  They need to fail for the same reason as the IRB failure

initialise rspec (rspec --init)
touch ./spec/calculator_spec.rb (or echo '#test' > ./spec/calculator_spec.rb)

in new file
require 'calculator'
describe Calculator do
end

then just run rspec - just type rspec, or rspec <filepath><filename>

result of this doesn't match irb error so in this example, need to add require... into the irb to get a similar test failure

NEED TO GET FEATURE TEST AND UNIT TEST TO FAIL THE SAME WAY

Failed unit test - go and fix the failure.

create file

new error, uninitialized constant

create class

run test - test is green

return to feature test and re-run

it 'adds 1 and 2 and returns 3' do
  expect(Calculator.add(1,2)).to eq(3)
end

feature test matches unit test - so fix it

in calculator class.
def add()

end

to know when to stop, run the test until it gets green

error now is expecting 2 arguments

def add(x,y)
  3
end

simplest solution to pass the test

Add more unit tests until it passes

NEVER USE OPERATIONS IN TEST

Add new unit test with another example with values

it 'adds 3 and 5 and returns 8' do
  expect(Calculator.add(3,5)).to eq(8)
end

def add(x,y)
#could do
#if x == 1 and y == 2 etc
#should be - REFACTOR
  x + y
end

REFACTOR - make code simpler while passing test

additional user stories - start cycle again.  if previous feature tests now fail but new tests are green.  Go back and make the previous tests green OR write a new method for the new user story

require './lib/calculator.rb'
calculator = Calculator.new
calculator.add(1,2)
