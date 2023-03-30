## Ownership 

```
fn get_years()->Vec<i32>{
    let years = vec![1995,2000,2005,2010];
    //allo (this scops "owns" years)
    return years;  // !!!transfer ownership to main scope because it return.
} //deallocate(years) because it went out of scope

fn main(){
    let years=get_years(); // take ownership from get_years()
} // deallocate(years)
```

**<span style="color:red">the owner</span> is the scope where the allocation created**


#### example 
```
               // take ownership of years from caller
fn print_years(years:Vec<i32>){ 
    for year in years.iter(){
        println!("Year: {}",year);
    }
} // deallocate(years)

fn main(){
    let years=vec![1990,1995,2000,2010]; 
    print_years(years); // years ownership is transferred to print_years function
    print_years(years); // use-after-free bug, in the compile you will get a "value used here after move error"

    // This workaround can be done like this below
    let years2=print_years(years);
    let years3=print_years(years2);
}

```

#### .Clone is your friend
another solution to fix the issue above
```
               // take ownership of years from caller
fn print_years(years:Vec<i32>){ 
    for year in years.iter(){
        println!("Year: {}",year);
    }
} // deallocate(years)

fn main(){
    let years=vec![1990,1995,2000,2010]; 
    print_years(years); // years ownership is transferred to print_years function
    print_years(years); // use-after-free bug, in the compile you will get a "value used here after move error"

    // use the .clone to fix the issue, but .clone is the friend for beginner even though 
    // it has performance cost
    print_years(years.clone());
    print_years(years);
}

```

### conclusion of ownership

- manual dealloc(), use-after-free, double free
-  automatic  garbage collector vs Rust's scope-based heuristics


## Borrowing && References

``` 
fn print_years(years: Vec<i32>){ 
    for year in years.iter(){
        println!("Year:{}",year);
    }
}  // deallocate years

fn main(){
    let years= vec![10990,1995,2000,2010];
    print_years(years); // complie error, use-after-free
    print_years(years); 
}

```

Fix 

``` 
fn print_years(years: &Vec<i32>){  // reference type, means the funciton doesn't own years, only can look at the years memory, similar to pointer
    for year in years.iter(){
        println!("Year:{}",year);
    }
}  // deallocate years

fn main(){
    let years= vec![10990,1995,2000,2010];
    // to fix this
    print_years(&years); // & will fix the compile error
    print_years(&years);  // temporarily give print_years access to years (&year - "borrow years")

}


```
example
**fn len(&self) -> usize**

**You can't turn off the borrow checker" in Rust**

### Mutable References

```
let mut years: Vec<i32>=vec![1990,1995];
let mutable_years: &mut Vec<i32>=&mut years; // when it is borrowed, it is ok to change it when you return it.
let length=mutable_years.len();
mutable_years.clear(); // clear() removes all elements from the Vec

fn clear(&mut self){
    // set self's length to 0
}
fn len(&self)->usize

```

### Mutable Reference Restrictions

```
let years: Vec<i32>=vec![1990,1995];

let years_ref1:&Vec<i32>=&years;
let years_ref2:&Vec<i32>=&years;

```
#### mutable refennces VS inmutable refennces
- **for mutable references, you can only have one reference**
- **for immutable references, you can have as many immutable references as you want**
- **you can not have mutable borrow and immutable borrow as the same time**
  for example
  ```
   let mut years: Vec<ii32>=vec![1990,1995];

   let years2:&Vec<i32>=&years;
   let years3:&mut Vec<i32>=&mut years;
  ```
### Slices
-  slice doesn't own the elements, just references them

``` 
let nums=vec![1,2,3];
let slice=&num[0...2]; // get part of the vector

```

## Lifetimes

``` 
let years: Vec<i64>=vec![1980,1985,1990,1995,2000,2005,2010];

let eighties: &[i64]=&years[0..2];
let nineties:&[i64]=&years[2::4];

println!("We have {} years in the nineties",nineies.len());

Release{
    years,eighties,nineties
}
```

```
struct Releases{ // rust doesn't allow to do this way without lifetime annotation when using slice,you need to explicitly define annotation.
    years: &[i64],
    eighties: &[i64],
    nineties: &[i64],
}

fn jazz_releases(years:&[i64])->Release{...}

let releases={ // anonymous scope
    let all_years: Vec<i64>=vec![1980,1985,1990,1995,2000,2005,2010];
    jazz_releases(&all_years);
} // deallocate all_years

let eighties=releases.eighties; // use-after-free!!!!!!!!

// The fix is to use lifetime annotation
// a lifttime parameter named a
fn jazz_releases<'a>(years:&'a[i64])->Release{ //you can choose any name, not just "a"
   let eighties:&'a[i64]=&years[0..2];
   let nineies:&'a[i64]=&years[2...4];

   Releases{
    years,
    eighties,
    nineies
   }
 }

or

struct Releases<'y>{
    years:&'y.[i64],
    eighties:&'y.[i64],
    nineies:&'y.[i64]
}
```

### Lifetime Elision

### The static lifetime
**things that have the static lifetime, they don't get allocated and deallocated, they always exist. it is in the binary**
```
let name="Sam"; = let name:&'static str="Sam"; // these two are the same thing.
let name:&str="Sam";
