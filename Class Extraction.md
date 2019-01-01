## Class Extraction

- Understand what the code is doing
- you could run test to see what it's testing for
- Go through methods and decide if it should sit in a to do list

- run the Great rspec to see how far it got to - recreate anything that's no longer there - **the unit tests**
- pick an easy one and start there

``` ruby
class TodoList
  def initialize
    @todos = []
  end

  def add(description)
    @todos << { description: description, complete: false }
  end

  def set_complete(index)
    get(index)[:complete] = true
  end

  def to_string
    all.each_with_index.map { |todo, index|
      todo_to_string(todo, index + 1)
    }.join("\n")
  end

  def get(index)
    all[index]
  end

  private

  def all
    @todos
  end

  def todo_to_string(todo, index)
    description = todo[:description]
    complete = todo_completion_to_string(todo)
    "#{index}. #{description} #{complete}"
  end

  def todo_completion_to_string(todo)
    todo[:complete] ? "complete" : "not complete"
  end
end

```

Splitting out

``` ruby
class TodoList
  def initialize # OK
    @todos = []
  end

  def add(description) # OK but no longer want to use a hash
    @todos << { description: description, complete: false }
  end

  def set_complete(index)
    get(index)[:complete] = true
  end

  def to_string # should move out
    all.each_with_index.map { |todo, index|
      todo_to_string(todo, index + 1)
    }.join("\n")
  end

  def get(index) # OK
    all[index]
  end

  private

  def all #OK
    @todos
  end

  def todo_to_string(todo, index) # to do or a presentation class
    description = todo[:description]
    complete = todo_completion_to_string(todo)
    "#{index}. #{description} #{complete}"
  end

  def todo_completion_to_string(todo) # to do or a presentation class
    todo[:complete] ? "complete" : "not complete"
  end
end

```

so we want to have a todo class and a presentation method

WRITE TEST FIRST

``` ruby
class Todo
  def initialize(description)
    @complete = false
  end
end

```
