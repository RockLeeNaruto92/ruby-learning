# 識別子 (identify)
### 命名規則:
  - 0..9
  - A..Z, a..z
  - underscore
  - Number is not placed in the first
  - Do not same with key of ruby

Sample of wrong cases:
    - 1_to_10
    - abc?
    - abc-1

### List of keys of ruby

```
1. nil
2. true
3. false
4. not
5. or
6. and
7. BEGIN
8. END
9. begin
10. end
11. do
12. then
13. yield
14. rescue
15. ensure
16. class
17. module
18. def
19. defined?
20. alias
21. undef
22. super
23. self
24. return
25. while
26. until
27. for
28. in
29. break
30. next
31. redo
32. retry
33. case
34. when
35. if
36. unless
37. else
38. elsif
39. __LINE__
40. __FILE__
41. __ENCODING__ (only 1.9)
```

### Scope of variable, constant

|  Type       |  Indentify rules           | Scope  | Value when do not init |
| ------------- |:-------------:| -----:|
| Locale variable | - first character: alphabeta or underscore | The nearest scope (where variable is defined) (1) | If defined in above → nil, else → raise exception |
| Global variable | - first character: $ | All  | nil |
| Class variable | - first character: @@ | All instances of class can see (2) | raise exception
| Instance variable | - first character: @ | Only in that instance (3) | nil |
| Constant | - First character must be upcase   | (4)  | raise exception  |

(1)

foo = 1

def call_variable
  puts foo
end

call_variable → raise exception

(2)

$foo = 1

def call_variable
  puts $foo
end

call_variable → 1

(3)

class Test
  @@class_var_1 = 100

  def instance_method_call_defined_variable
    puts @@class_var_1
  end

  def instance_method_call_undefined_variable
    puts @@class_var_2
  end

  class << self
    def class_method_call_defined_variable
      puts @@class_var_1
    end
  end
end

Test.new.instance_method_call_defined_variable → 100
Test.new.instance_method_call_undefined_variable → raise exception
Test.class_method_call_defined_variable → 100

(4)

module TestConstant
  TEST_CONSTANT = 1

  def instance_call_constant
    puts TEST_CONSTANT
  end

  class << self
    def class_call_constant
      puts TEST_CONSTANT
    end
  end
end

class TestConstantClass
  include TestConstant
end

TestConstantClass.class_call_constant → raise exception
TestConstant.class_call_constant → 1
TestConstantClass.new.instance_call_constant → 1
TestConstantClass::TEST_CONSTANT → 1
TestConstant::TEST_CONSTANT → 1
