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

### 2.3. String (文字列)

```
a = "abcd"
p a　→ "abcd"
b = "ab""cd"
p b → "abcd"
b = "ab" 'cd'
p b → "abcd"
```

```
a = 1
p "a = #{a}"
→ a = 1
p 'a = #{a}'
→ a = \#{a}
```

```
"100".to_i → 100
"1.9".to_f → 1.9
"10a5".to_i → 10
"10a5".to_f → 10.0
"1.9".to_i → 1
"1.9.a".to_f → 1.9
"1.9.9".to_f → 1.9
```

##### Use `\` notation

| Content | Means |
| --- | --- | --- |
| \x | ? |
| \n | new line |
| \r | ? |
| \f | ? |
| \a | bell |
| \s | ? |
| \nnn | Octor notation (n: 0 ~ 7) |
| \xnn | Hexa notation (n: 0-9, a-f) |
| \C-x | Control + x (x is ASCII chacter) |
| \M-x | Meta + x (x is ASCII chacter) |
| \M-\C-x | Meta + Control + x (x is ASCII chacter) |


```
"\101" → "A" (in octal)
"\x41" → "A" (in hexa)
```


##### Different between `p`, `puts`, `print`

```
2.3.0 :035 > p "a\b"
"a\b"
 => "a\b"

2.3.0 :036 > print "a\nb"
a
b => nil

2.3.0 :037 > puts "a\nb"
a
b
 => nil
```

##### Here document

```
2.3.0 :037 > a = 1
2.3.0 :038 > query = <<-SQL
2.3.0 :039">   SELECT * FROM users where id = #{a};
2.3.0 :040"> SQL
 => "SELECT * FROM users where id = 1;\n"
2.3.0 :041 >
2.3.0 :042 >   puts query
  SELECT * FROM users where id = 1;
 => nil
2.3.0 :043 > p query
"SELECT * FROM users where id = 1;\n"
```

Different betwwen double quotes and single quotes is explained in here too.

```
2.3.0 :009 > a = 1

2.3.0 :010 >   query = <<-'SQL'
2.3.0 :011'> SELECT * FROM users
2.3.0 :012'> where id = #{a};
2.3.0 :013'> SQL
 => "SELECT * FROM users\nwhere id = \#{a};\n"
2.3.0 :014 > puts query
SELECT * FROM users
where id = #{a};
 => nil


 2.3.0 :015 > query = <<-"SQL"
 2.3.0 :016"> SELECT * FROM users
 2.3.0 :017"> where id = #{a};
 2.3.0 :018"> SQL
  => "SELECT * FROM users\nwhere id = 1;\n"
 2.3.0 :019 > puts query
 SELECT * FROM users
 where id = 1;
  => nil
```

##### Percent notation
```
2.3.0 :028 > a = %*abcdef*
 => "abcdef"
2.3.0 :029 > p a
"abcdef"
 => "abcdef

2.3.0 :026 > a = %*"abcdef"*
 => "\"abcdef\""
2.3.0 :027 > p a
"\"abcdef\""
 => "\"abcdef\""
```

```
2.3.0 :035 > a = %[abcdef]
 => "abcdef"
2.3.0 :036 > p a
"abcdef"
 => "abcdef"

2.3.0 :037 > a = %["abcdef"]
=> "\"abcdef\""
2.3.0 :038 > p a
"\"abcdef\""
=> "\"abcdef\""
```

All percent notation formats:

| Format | The value will be generated | Example |
| --- | --- | --- |
| % | Double quote | (1) |
| %Q | Double quote | (2) |
| %q | Single quote | (3) |
| %s | Symbol | (4) |
| %W | Array | (5) |
| %w | Array (Same with %W) | (6) |
| %s | Export command | (7) |
| %r | Regular expression | (8) |

```
a = 1

①
2.3.0 :056 > test = %*"#{a}"*
 => "\"1\""
2.3.0 :057 > p test
"\"1\""

2.3.0 :058 > test = %*'#{a}'*
 => "'1'"
2.3.0 :059 > p test
"'1'"

②
2.3.0 :060 > test = %Q!#{a}!
 => "1"
2.3.0 :061 > p test
"1"

2.3.0 :064 > test = %Q!'#{a}'!
 => "'1'"
2.3.0 :065 > p test
"'1'"

③
2.3.0 :066 > test = %q!'#{a}'!
 => "'\#{a}'"
2.3.0 :067 > p test
"'\#{a}'"

2.3.0 :068 > test = %q!"#{a}"!
 => "\"\#{a}\""
2.3.0 :069 > p test
"\"\#{a}\""

④
2.3.0 :070 > test = %s!"#{a}"!
 => :"\"\#{a}\""
2.3.0 :071 > p test
:"\"\#{a}\""

⑤
2.3.0 :074 > test = %W('a' "b" 'c' "d")
 => ["'a'", "\"b\"", "'c'", "\"d\""]

⑥
2.3.0 :075 > test = %w('a' "b" 'c' "d")
 => ["'a'", "\"b\"", "'c'", "\"d\""]

⑦
2.3.0 :084 > %x*ls*
 => "chapter3.md\n"
 (Run ls command and print the output)

⑧
2.3.0 :086 > %r*(a-9)+*
 => /(a-9)+/
```

#### Operater

```
2.3.0 :087 > a = "ru" + "by"
 => "ruby"
2.3.0 :088 > a * 3
 => "rubyrubyruby"
2.3.0 :089 > 3 * a
TypeError: String can't be coerced into Fixnum

2.3.0 :090 > a = "ru"
 => "ru"
2.3.0 :091 > a << "by"
 => "ruby"
2.3.0 :092 > p a
"ruby"
```

**Comparison**

```
"a" < "b" → true
"ab" < "ac" → true
"ab" < "abc" → true
"Ab" == "Ab" → true
"Ab" <=> "Ab" → 0
```

**Length and size of string**

*NOTE: There are some differents between different encoding*

```
"abcdef".length → 6
"日本語".encoding → #<Encoding:UTF-8>
"日本語".length → 3
```

##### Print with format:  sprintf

Same with `printf` of C language

```
# Radices format
sprintf("result: %#b", 12) → "result: 0b1100" (binary)
sprintf("result: %#o", 12) → "result: 014" (octor)
sprintf("result: %#x", 12) → "result: 0xC" (hexa)
sprintf("result: %#X", 12) → "result: 0xC" (Hexa)

# Number of digits
sprintf("result: %02d", 1) → "result: 01"
sprintf("result: %02d", 123) → "result: 123"
sprintf("result: %05.5f", 123.2345678) → "result: 123.23457"  → rounded
```

Other way: use `%` symbol

```
"result: %#b" % 12  → "result: 0b1100"
"result: %05.5f" % 123.2345678 → "result: 123.23457"  → rounded
```

### 2.4. Symbol

##### Generation ways:

```
foo1 = :"foo1"  → :foo1
foo2 = :"#{foo1}foo2" → :foo1foo2
foo3 = :'foo3' → foo3
foo4 = :foo4 → foo4
foo5 = :"foo1 foo2" → :"foo1 foo2"
foo6 = %s*foo6* → :foo6
foo7 = "foo7".to_sym → :foo7
```

##### Different between symbol and string

```
"foo1".object_id → 70300729983460 # 1st time
"foo1".object_id → 70300733581760 # 2nd time
→ Always create new string object after called string

"foo1" == "foo1" → true
"foo1".equal? "foo1" → false


:foo1.object_id → 1154268 # 1st time
:foo1.object_id → 1154268 # 2nd time
→ Create symbol object only 1 time

:foo1 == :foo1 → true
:foo1.equal? :foo1 → true
```

★ `eql?`

```
"foo1".eql? "foo1" → true
1.0 == 1 → true
(1.0).eql? 1 → false
(1.0).eql? 1.0 → true

"foo1".equal? "foo1" → false
(1.0).equal? 1 → false
(1.0).equal? 1.0 → true
```

### 2.5. Variables and value of its

```
v1 = "test"
v2 = v1

v1.equal? v2 → true

v1 += "test 2"
p v1 → "testtest 2"
p v2 → "test"
```

**Parameter's value in function**

```
①
def func v1
  v1.object_id
end

v1 = "test"
v1.object_id → 70300726236340
func v1 → 70300726236340

②
def func v1
  p v1.object_id
  v1 += "test 2"
  v1.object_id
end

v1 = "test"
v1.object_id → 70300737948960
func v1 → 70300726215520
v1 → "test"
```

**Destruction method sample**

```
v1 = "test1"
v2 = v1
v1.chop
p v1 → "test1"
p v2 → "test1"

v1.chop!
p v1 → "test"
p v2 → "test"
```

### 2.6. Array (配列)

```
①
array = Array.new(2){|index| index + 1}
→ [1, 2]

②
array = Array.new(2, "a")
→ ["a", "a"]
array[0].object_id → 70300726074660
array[1].object_id → 70300726074660
array[0].replace("b")
p a
→ ["b", "b"]

③
array = ["a", "a"]
array[0].object_id → 70300725957160
array[1].object_id → 70300725957140
array[0].replace("b")
p array
→ ["b", "a"]
```

##### Subscript operator(添字演算子)

```
①
array = [1, 2, 3, 4]
array[-1] → 4
array[3] → 4
array[-4] → 1
array[0] → 1

②
array = [0, 1, 2, 3, 4, 5]
array1 = array[2, 3] # 2: from_index, 3: length of new array
→ [2, 3, 4]
p array
→ [0, 1, 2, 3, 4, 5]
p array1
→ [2, 3, 4]

③
a = [0, 1, 2, 3, 4]
a[1, 3] = "a" # 1: from_index, 3: number of elements will be earsed
p a
→ [0, "a", 4]

④
a = [0, 1, 2, 3, 4]
a[1, 3] = ["a", "b"]
p a
→ [0, "a", "b", 4]

⑤
a = [0, 1, 2, 3, 4]
a[1, 3] = "a", "b", "c", "d", "e"
p a
→ [0, "a", "b", "c", "d", "e", 4]
```

##### Some ways to set value

```
①
2.3.0 :197 > a, b, c = 1, 2, 3
 => [1, 2, 3]
2.3.0 :198 > a
 => 1
2.3.0 :199 > b
 => 2
2.3.0 :200 > c
 => 3

②
2.3.0 :201 > def foo
2.3.0 :202?>     return 1, 2, 3
2.3.0 :203?>   end
 => :foo
2.3.0 :205 >   a, b, c = foo
 => [1, 2, 3]
2.3.0 :206 > a
 => 1
2.3.0 :207 > b
 => 2
2.3.0 :208 > c
 => 3
2.3.0 :209 > a, b, c, d = foo
=> [1, 2, 3]
2.3.0 :210 > d

③
2.3.0 :211 > a, b, c = [1, 2, 3]
 => [1, 2, 3]
2.3.0 :212 > a
 => 1
2.3.0 :213 > b
 => 2
2.3.0 :214 > c
 => 3

2.3.0 :215 > (a, b), c = [1, 2], 3
=> [[1, 2], 3]
2.3.0 :216 > a
=> 1
2.3.0 :217 > b
=> 2
2.3.0 :218 > c
=> 3

④
2.3.0 :219 > a = 1, 2, 3
 => [1, 2, 3]
2.3.0 :220 > a
 => [1, 2, 3]

⑤
2.3.0 :221 > a, *b = 1, 2, 3, 4
 => [1, 2, 3, 4]
2.3.0 :222 > a
 => 1
2.3.0 :223 > b
 => [2, 3, 4]

2.3.0 :224 > a, *b, c = 1, 2, 3, 4
=> [1, 2, 3, 4]
2.3.0 :225 > a
 => 1
2.3.0 :226 > b
 => [2, 3]
2.3.0 :227 > c
 => 4

a, *b, *c = 1, 2, 3, 4, 5, 6
→ SyntaxError

⑥
def foo1 a, *b
  foo2 *b
end

def foo2 c, *d
  d
end

foo1(1, 2, 3) → [3]
foo1(1, 2, 3, 4) → [3, 4]
```

##### Operate with array

```
a = [1, 1, 2, 2]
b = [2, 2, 3, 4]

①
a & b → [2]
a | b → [1, 2, 3, 4]
b | a → [2, 3, 4, 1]

②
a + b → [1, 1, 2, 2, 2, 2, 3, 4]
b + a → [2, 2, 3, 4, 1, 1, 2, 2]
a - b → [1, 1]
b - a → [3, 4]

a - [1] → [2, 2]

③
[1, 2] * 3 → [1, 2, 1, 2, 1, 2]
[1, 2, 3] * "/" → "1/2/3"
[1, 2, 3].join("/") → "1/2/3"
```

###### `for` and `each`

```
①
for i in [1, 2, 3]
  p i
end
→ ok

②
for i, j in [[1, 2], [3, 4]]
  p i+ j
end

→ ok

③
for i in [1, 2, 3]
  bar = i
end
p bar
→ 3

④
[1, 2, 3].each do |i|
  bar = i
end
p bar
→ NameError
```

### 2.7. Hash

```
①
2.3.0 :005 > hash = Hash.new(4) # 4 is default value
 => {}
2.3.0 :006 > hash
 => {}
2.3.0 :007 > hash[:test]
 => 4
2.3.0 :008 > hash[:test1]
 => 4

2.3.0 :012 > a = Hash[:test1, 1, :test2, 2]
=> {:test1=>1, :test2=>2}1, 1, :test2, 2]

②
def foo a, b, c, d
  c
end

foo(1, 2, foo1: 1, foo2: 1)
→ ArgumentError: wrong number of arguments (given 3, expected 4)

foo(1, 2, {foo1: 1, foo2: 2}, "d")
→ {:foo1 => 1, :foo2 => 2}
```

### 2.8. Range

```
①
(1..5).include? 3 → true
(1..5).include? 6 → false
(1..5).include? 5 → true
(1...5).include? 5 → false
(1...5).include? 1 → true

②
(1..5) == 3 → false
(1..5) === 3 → true
(1..5) === 5 → true
(1..5) === 6 → false
(1...5) === 5 → false
(1...5) === 1 → true

③
a = "abcdef"
a[1] → "b"
a[1..2] → "bc"
a[1..2] → "b"
```

##### `case`

```
①
case condition
when value1
  ...
when value2
  ...
else
  ...
end

②
case condition
when value1 then; <<command>>
when value2 then; <<command>>
else <<command>>
end

Sample:

case 7
when 1..5 then; p 1
when 5..10 then; p 2
end
→ 2
```

##### `while`, `until`

```
①
while <<condition>> do
  ...
end

②
begin
  ...
end while <<condition>>

Sample:

i = 0
begin
  p i
  i += 1
end while(1..4) === i

③
until <<condition>> do
  ...
end
```

### 2.9. RegularExpression (正規表現)

```
①
/Ruby/ → /Ruby/
%r(Ruby) → /Ruby/
Regexp.new "Ruby" → /Ruby/

②
/Ruby/ === "I love Ruby"
→ true
"I love Ruby" === /Ruby/
→ false

③
/Ruby/ =~ "Version of Ruby: 2.3"
→ 11
"Version of Ruby: 2.3" =~ /Ruby/
→ 11

$`  → "Version of "
$&  → "Ruby"
$'  → ": 2.3"
```

### 2.10. Output command (コマンド出力)

```
`date`
%x(date)
```

### 2.11. Block and Proc
##### Block

```
def func x
  x + yield
end

func(3){ 2 }
→ 5

func(3)
→ LocalJumpError: no block given (yield)

def func x
  x
end

func(3) {2}
→ 3
```

```
def func y
  y + yield
end

func(1){x = 2}

p x
→ NameError: undefined local variable or method `x'

x = 2
func(1){x += 2} → 5

x → 4
```

```
def func a, b
  a + yield(b, 3)
end

func(1, 2){|x, y| x + y}
→ 6
```

```
def func
  return 1 if block_given?
  0
end

func(){} → 1
func → 0
```

##### Proc

Block become object  → proc

```
proc = Proc.new{|x| p x}
proc.call(1)
→ 1
```

- Use proc in block

```
def func x
  x + yield
end

proc = Proc.new{2}
func(2, &proc)
→ 4
```

- Use block in proc

```
def func x, &proc
  x + proc.call
end

func(1){2}
→ 3
```

- lambda

```
lmd = lambda{|x| p x}
lmd.call(3)
→ 3
```

★ Different between `lambda` and `proc`

```
①
def func
  proc = Proc.new{return 1}
  proc.call
  2  # do not run to here
end

func  → 1

def func2
  lmd = lambda{return 1}
  lmd.call
  2 # run to here
end

func2  → 2

②
p1 = Proc.new{|x, y| y}
p p1.call(1)
→ nil

l1 = lambda{|x, y| y}
p l1.call(1)
→ ArgumentError
```

##### Some method using block
- `each`
- `each_with_index`
- `each_key`
- `upto`, `downto`
- `times`

### Thread

```
t = Thread.new do
  p "start"
  sleep 5
  p "end"
end

p "wait"
t.join
```

```
3.times do |i|
  Thread.start(i) do |index|
    p "Thread-#{index} start"
  end
end
```

### 2.12. Loop break systax (脱出構文)
- `next`
- `redo`

### 2.13. Exception

```
raise ArgumentError, "message"
→ ArgumentError: message

raise ArgumentError.new, "message"
→ ArgumentError: message

err = ArgumentError.new("message")
raise err
→ ArgumentError: message

raise "message"
→ RuntimeError: message
```

- Catch exception

```
①
begin
  1 / 0
  1
rescue
  0
end

→ 0

②
b2.3.0 :141 > begin
2.3.0 :142 >       p 1
2.3.0 :143?>   rescue
2.3.0 :144?>     p 0
2.3.0 :145?>   else
2.3.0 :146 >       p 2
2.3.0 :147?>   ensure
2.3.0 :148 >       p 3
2.3.0 :149?>   end
1
2
3
 => 2

③
1 / 0 rescue p 1
→ 1
```

```
2.3.0 :151 > begin
2.3.0 :152 >       1 / 0
2.3.0 :153?>   rescue ZeroDivisionError
2.3.0 :154?>     p $!.class
2.3.0 :155?>     raise
2.3.0 :156?>   end
ZeroDivisionError
ZeroDivisionError: divided by 0
```

```
a = 0
begin
  b = 1 / a
rescue ZeroDivisionError
  a += 1
  retry
ensure
  p b
end

 => 1
```

##### Exception class structure

- Exception
  - ScriptError
    - SyntaxError
  - SignalException
  - StandardError
    - ArgumentError
    - RuntimeError
    - NameError
      - NoMethodError
    - ZeroDivisionError

##### Catch, throw

```
def foo
  throw :exit
end

catch(:exit) {
  a = 5
  foo
  a = 3 → this is not executed
}

p a
→ NameError: undefined local variable or method `a'
```
