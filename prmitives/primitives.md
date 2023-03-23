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
