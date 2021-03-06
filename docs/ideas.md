The hien-lang Language Specs

### let, var, and type inference

```
let a = 3 # immutable
var b = 3 # mutable

a = 5 # compile error
b = 5 # ok
```

Compiled

```c++
#line 1 "var.hi"
const int a = 3;
#line 2 "var.hi"
int b = 3;

#line 4 "var.hi"
a = 5; // compile error
#line 5 "var.hi"
b = 5;

```

### Functions

```
def sum(from: Int, to: Int){
  [from..to].reduce(:+) # range and reduce
}

def doubles(list: List<Int>){
  list.map(n => n*2) # anonymous function
}

def fib(n){
  match(n){ # pattern match
    0 => 0
    1 => 1
    else => fib(n-1) + fib(n-2)
  }
}

def hello(n: Int) => puts n # short hand
let hello = (n:Int => puts n) # anonymous function
```

Compiled

```c++
int sum(int from, int to){
  return reduce(arrayRange(from, to), plus);
}

const int __anon1(int n){
  return n*2;
}

List<int> doubles(List<int> list){
  return map(list, __anon1);
}

int fib(int n){
  if(n == 0){
    return 0;
  }else if(n == 1){
    return 1;
  }else{
    return fib(n-1) + fib(n-2);
  }
}
```

### Parallelism

```
let list = [1,3,5,7]
list.paraMap(n => n*n)
list.paraEach(n => puts n)
```

Compiled

```c++
const Array<int> list = {1,3,5,7};

const int __anon1(int n){
  return n*n;
}

const int __anon2(int n){
  printf("%d",n);
}

pararellMap(list, __anon1);
pararellEach(list, __anon2);

```

### Data Structures

```
let arr = [1,4,6] # array
let arr2 = List(1,4,6).to_a # array
let list = List(1,4,6) # list
let list2 = [1,4,6].to_l # list

let hash = {1: "apple", 2: "banana"} # hash map

type AddressBook = {name: Str, age: Int, address: Str}
let record = {name: "Tom", age: 26, address: "Tokyo, Japan"} // record
```

Compiled

```c++
const Array<int> arr = {1,4,6};
const Array<int> arr2 = List<int>(1,4,6).toArray();
const List<int> list = List<int>(1,4,6);
const List<int> list2 = Array<int>(1,4,6).toList();

const Map<int, Any> = {1,"apple",2,"banana"};

class AddressBook : public Record {
public:
  string name;
  int age;
  string address;

  AddressBook(string name, int age, string address){
    this.name = name;
    this.age = age;
    this.address = address;
  }
}

const AddressBook record = new AddressBook("Tom", 26, "Tokyo, Japan");

```

### String Manipulation

```
let s = "world"
let hello = "hello, #{s}" # hello, world
let result = "result: #{ Math.sqrt(4) }" # result: 2.0
let heredoc = <<EOF
This is here doc.
You can write multiline document.
EOF
```

Compiled


```c++
const string s = "world";
const string hello = "hello, " + s;
const string result = "result: " + sqrt(4);
const heredoc = "This is heredoc.\nYou can write multiline document.\n";
```

### Classes

```
# modern style
class Contact (let name: Str, let address: Str){
  def print(){
    puts "name: #{name}, address: #{address}"
  }
}
let contact = Contact("Yamada","Osaka")
puts contact.name # Yamada
puts contact.address # Osaka
contact.print # name: Yamada, address: Osaka

# old style
class Animal {
  def initialize(type: Str, name: Str){
    this.type = type
    this.name = name
  }

  def print(){
    puts "type: #{type}, name: #{name}"
  }
}

let animal = Animal("Dog","Pochi")
puts animal.type # Dog
puts animal.name # Pochi
animal.print # type: Dog, name: Pochi
```

### Loops

```
# old style
var arr = [];
var i = 0
while(i < 5){
  arr.push i
}

puts arr # [0,1,2,3,4]

# best
let arr = [0..4]
puts arr # [0,1,2,3,4]

# for each
for(let n : arr)[
  puts n; # 0\n1\n2\n ...
}

arr.each(puts _) # best

arr.size.times(n => puts n) # another code
```

### Macros

```
macro def nil!(e:Var<Any>){
  `%e = nil`
}

macro def incl(e:Var<Number>){
  `++%e`
}
```