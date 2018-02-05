## 1. 識別子 (identify)
### 命名規則 (rules of identify)
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

```
foo = 1

def call_variable
  puts foo
end

call_variable → raise exception
```

(2)

```
$foo = 1

def call_variable
  puts $foo
end

call_variable → 1
```

(3)

```
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
```

(4)

```
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
```

## 2. Numeric (数値)

### All types in ruby
- Numeric (数値)
- Boolean (論理値)
- String (文字列)
- Symbol (シンボル)
- Array (配列)
- Hash (ハッシュ)
- Range (範囲)
- Regular Expression (正規表現)
- Command Output (コマンド出力) ???

### 2.1. Numeric

- Normal
```
3.0e2 → 300.0
3e2 → 300.0
3e-2 → 0.03
3321321321e100 → 3.321321321e+109
33333333e-110 → 3.3333333e-103
```

- Binary, octal, decimal, hexa (indicator - 基数指示子)

| Indicator | pattern |
|--------|-------|
| Binary | 0b |
| Octal | 0o or 0 |
| Decimal | 0d |
| Hexa | 0x |

Example:

  ```
  0b1001 → 9
  0o70 → 56
  070 → 56
  0d20 → 20
  0x1F → 31
  0xf → 15

  079 → raise error
  0b20 → raise error
  0xZ → raise error
  ```

- Use number with underscore
```
1_000_000_000
→ 1000000000
```

- Get string code of character → use  「?」
```
?R → "R"
?\C-v → "\u0016" # (Code of Control C + v)
?\M-\C-m → "\x8D" # (Code of Alt + Control C + m)
```

### 2.2. Operators (演算子)

- `+, - , *, /, %`
- `==, !=, <, >=`
- UFO Operator: `<=>`
  ```
    100 <=> 1 → 1
    1 <=> 1 → 0
    1 <=> 100 → -1
  ```

  ```
    def <=>(a, b)
      return 0 if a == b
      a > b ? 1 : -1
    end
  ```

- `=, +=, -=, +=, /=, **=`

```
a = 5
a *= 2 → 10
a **= 5 → 3125 # = a^5
```

- `::, ?:`

```
Abc::Constant
```

- `.., ...`

```
(1..9).to_a → [1,2,3,4,5,6,7,8]
(1...9).to_a → [1,2,3,4,5,6,7,8]
```

- `&&, and, ||, or, !, not`

```
1 && 2 → 2
2 && 1 → 1
1 and 2 → 2
2 and 1 → 1
1 || 2 → 1
2 || 1 → 2
1 or 2 → 1
2 or 1 → 2
```

#####  Range of numeric classes

| Class name | Max | Min |
| --- | --- | --- |
| Fixnum | (2**(0.size * 8 -2) -1) | -(2**(0.size * 8 -2)) |
| BigNum | ? | ? |
| Float | Float::MAX | Float::MIN |

##### Rational

```
Rational(0.5) → (1/2)
```
