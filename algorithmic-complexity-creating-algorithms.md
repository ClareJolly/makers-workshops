# Creating your own algorithms

## Examples

### Assigning students into groups

```
Sundar
Zoe
.
.
.
Moishe
```

put each of these into groups of 4

#### How to start

- describe the problem - maybe diagram
- understand data
- input
- output

just understanding input and output you can write a **function signature** - e.g. in Ruby `def grouping(list,number)`

#### Next step

** break down into steps **

- create test cases `should be as many groups as are expected (4)` or `if list has 2 people and ask for 2 groups, result should be 1 in each group` - output and expected output should match - don't necessarily need a test framework
- think about how you would do this in real life - on paper for example (`draw 4 boxes, and run down the list putting people into the boxes in sequence`)
- write down each step in plain english

  - create empty groups
  - go down list of names
    - first name into first group, second name into second group, etc.  
    - when reach final group, go back to the start
  - when list is empty, return list of groups with their contents

- transform plain english into comments in code to refer to

- See if if can be translated into code straight away

  ```ruby
  result = []

  # - create empty groups
  number.times{result << []}
  next_column = 0
  # - go down list of names
  list.each do |name|  
  #  get next column

  # put name into it  
  group = result[next_column]
  group << name
  next_column = (next_column + 1)%number # but needs to roll over
end
  #   - when reach final group, go back to the start
  # - when list is empty, return list of groups with their contents
  return result
  ```

### Debugging algorithms

keep track of all variables

```
list = [sundar,zoe]
----
number = 2
----
result = []
----
result = [[],[]]
----
loop    | name = sundar
        | next_column = 0
        | group = [] (pointing to first array in result)
        | group = (update result to be) result = [[sundar],[]]

loop    | name = zoe
        | next_column = 1
        | group = [] (pointing to second array in result)
        | group = (update result to be) result = [[sundar],[zoe]]        

```

### Process

1. Describe the problem (sometimes a diagram helps)
2. Create test cases.
3. Think about how you would do this, given this task in real life.
4. Write down each step in plain english
5. If it is possible directly, translate each of the steps into code
6. Otherwise, think about each step separately, and repeat 3 to 6.
7. Once everything is translated to code, run your testcases
