## String
### macro
In Rust, a macro is a way to define and use custom code patterns. Macros allow developers to write code that generates more code at compile time, which can help reduce boilerplate code and improve code organization.

example

- println!
- format!
- panic!

#### macro vs function

-  macro is strictly powerful than function in every way except for
-  function you can pass it around but macro cannot.
-  macro in compiled time will be interpreted into a couple of functions (syntax sugar at compiled time)

### Integer Sizes
- i8
-  i16
-  etc.

### Statement and expressions

- an expression evaluates to a value
-  a statement does not evaluate to a value

### Tuples
``` let point:(i64,i64,i64)=(0,0,0); ```
``` let x=point.0; ```
``` let (x,y,_)=point ``` // destructure
#### unit is an empty tuple
``` let unit:()=() ```
### mutate a tuple
p.s. tuple cannot change size at the runtime

```
let mut point:(i64,i64,i64)=(0,0,0)
```
## Structs

```
struct Point{
    x: i64,
    y: i64,
    z: i64
}

fn new_point(x:i64,y:i64,z:i64) -> Point{
    Point(x,y,z)
}
```

## Array

```
let mut years:[i32;3]=[1995,2000,2005] //hard coded array length is 3
let first_year=years[0];
let [_,second_year,third_year]=years;

years[2]=2010;
years[x]=2010;  // this will panic if it is out of bounds

for year in years.iter(){
    println!("Next year: {}", year+1)
}
```
 - **Arrays can be iterated over**
 - **Tuples (and structs) cannot**


## Enum

```
enum Color{
    Green,Yellow,Red,
    Custom{
        red:u8,green:u8,blue:u8
    },
    Custom2(u8,u8,u8)
}

let go=Color::Green;
let stop=Color::Red;
let slow_down=Color::Yellow;
let purple:Color=Color::Custom:{
    red: 100, green:0, blue:250
}
```
## Pattern Matching
```
let current_color=Color::Yellow;

match current_color{
    Color:: Green =>[
        printlin!("It was green)
    ]
    Color::Yellow =>{
        printlin!("It was yellow")
    }
    Color::Custom {red,green,blue}=>{
        printlin!("{} {} {}",red,green,blue);
    }
}
```
## Method

```
enum Color {...}

impl Color{   //impl is short for implementation
 fn rgb(color: Color) ->(u8,u8,u8){...}
 fn new(r:u8,g:u8,b:u8)->Color{...}
}

let red = Color::new(255,0,0);
let purple=Color::new(100,0,255);
let (r,g,b)=Color::new(purple);
```

```
enum Color{...}

impl Color{
     fn rgb(color: Color) ->(u8,u8,u8){...}
     fn new(r:u8,g:u8,b:u8)->Self{...}
}
let red = Color::new(255,0,0);
// these two lines are doing the same thing
let purple=Color:rgb(purple);
let (r,g,b)=purple.rgb();
```
## Type Parameters
```
let last_char=my_string.pop();
let last_char:Option<char>=my_string.pop();

enum Option<T>{
    None,
    Some(T)
}

let email: Option<String> =Some(email_str);
let email:Option<String>=None;
```
**(None is Option::None - the prefix is optional for Option)**

```
enum Result<O,E>{
    Ok(O),
    Err(E)
}

let success: Result<i64,String>=Ok(42);
let failure: Result<i64,String>=Err(str);
```
**(the Result is also optional)**


## Vectors

```
let mut years:Vec<i32>= vec![1995,2000,2005];

years.push(2010); // Now years has 4 elements ending in 2010

years.push(2015);

println!("Number of years: {}",years.len());
```

## Vectors vs Arrays
```
let mut nums:[u8:3]=[1,2,3];
let mut: Vec<u8> =vec![1,2,3];

for num in nums {....}
```

**the tradeoffs here set the stage for the biggest factor in the language performance!**

## Stack memory and Heap memory








