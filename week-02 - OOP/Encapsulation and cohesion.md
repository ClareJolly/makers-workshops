## Encapsulation and cohesion

Grouping of related behaviour

``` ruby
class Note
  def initialize(title, body)
    @title = title
    @body = body
  end

  def title
    @title
  end

  def body
    @body
  end

  def display
    puts @title
    puts "---"
    puts @body
  end

  def say_hi_to_kay
    puts "Hi Kay!"
  end
end
```

**Related methods**
- Title
- Body
- Display - but could potentially be split out

- say_hi_to_kay - unrelated to Note.  doesn't belong

**This class is therefore not cohesive**

Should separate out into another class - also display

``` ruby
class KayGreeter
  def say_hi_to_kay
    puts "Hi Kay!"
  end
end

class Note
  def initialize(title, body)
    @title = title
    @body = body
  end

  def title
    @title
  end

  def body
    @body
  end

  def display

end

class DisplayNote
  def display
    puts @title
    puts "---"
    puts @body
  end
end
```

Display like this won't work as can't access the variables in the Note class

Good way to do it while maintaining some way of displaying the note is to call the display note and create a DisplayNote instance in the initialize

``` ruby
class Note
  def initialize(title, body)
    @title = title
    @body = body
    @display_note = DisplayNote.new
  end

  def title
    @title
  end

  def body
    @body
  end

  def display
    @display_note.display(self)
  end

end

class DisplayNote
  def display
    puts @title
    puts "---"
    puts @body
  end
end

```

**Rule of thumb for determining if cohesive**

look at class and describe.  If you have to use the work "AND" then may be problem.  "OR" or "BUT" then definitely a problem
