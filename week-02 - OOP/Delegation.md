## Delegation

**Delegation is**
One class telling another class to do something and the other class encapsulating how to do it

Example

```
CEO
  make company efficient
          ||
          ||
          \/
      Find savings
          COO
          ||
          ||
          \/
      Fire employees
      HR manager
          ||
          ||
          \/
      Efficiency
      Employee
```
Converted to Ruby classes....

``` ruby
class CEO
  def initialize(coo = COO.new, sales_manager = SalesManager.new#, hr_manager = HRManager.new)
    @coo = coo
    @sales_manager = sales_manager
    #@hr_manager = hr_manager
  end

  def make_company_efficient
    # whole bunch of stuff
    coo.find_savings
    sales_manager.increase_sales
    # coo.hr_manager.fire_employees - this breaks something called law of Demeter - chaining through objects
  end

  class COO
    def initialize(hr_manager = HR.new)
      @hr_manager = hr_manager
    end

    def find_savings
      # whole bunch of stuff
      hr_manager.fire_employees
    end
  end

  class HRManager
    def initialize(employee_klass = Employee) #reference to class not instance of object
      @employees = [employee_klass.new, employee_klass.new]
    end

    def fire_employees
      @employees.each do |employee|
        employee.delete if employee.efficiency < 5
      end
    end
  end

  class Employee
    def efficiency
      rand(10)
    end
  end

  class SalesManager
    def increase_sales
      # whole bunch of stuff
    end
  end
```

**Read up on demeter**
