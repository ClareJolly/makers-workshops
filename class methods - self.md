``` ruby
class User
  attr_reader :name, :email, :password

  def initialize(name, email, password)
    @name = name
    @email = email
    @password = password

  end

  def password_reminder
    "hello, here's your password"
  end

  def self.all_users
    p "getting users"
    # iterate over db object calling User.new
  end

  def get_all
    p "get all users"
  end

end

```

```
irb >
user = User.new("sam", test.com")
user.get_all
```

You have to create an instance first and then run a generic method on that instance

class methods mean I don't need to create an instance of a class and then call a method on it for something that returns all records or similar

```
irb >
User.all_users
```
