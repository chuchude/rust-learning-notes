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
  