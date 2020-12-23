<details>
 <summary>1.2.0 Error Message</summary>

```
   Compiling error-messages v0.1.0 (file:///Users/ep/src/rust/error-messages)
src/lib.rs:3:24: 3:43 error: the trait `core::ops::FnMut<(char,)>` is not implemented for the type `collections::string::String` [E0277]
src/lib.rs:3     println!("{:?}", s.find("".to_owned()));
                                    ^~~~~~~~~~~~~~~~~~~
note: in expansion of format_args!
<std macros>:2:25: 2:56 note: expansion site
<std macros>:1:1: 2:62 note: in expansion of print!
<std macros>:3:1: 3:54 note: expansion site
<std macros>:1:1: 3:58 note: in expansion of println!
src/lib.rs:3:5: 3:45 note: expansion site
src/lib.rs:3:24: 3:43 error: the trait `core::ops::FnOnce<(char,)>` is not implemented for the type `collections::string::String` [E0277]
src/lib.rs:3     println!("{:?}", s.find("".to_owned()));
                                    ^~~~~~~~~~~~~~~~~~~
note: in expansion of format_args!
<std macros>:2:25: 2:56 note: expansion site
<std macros>:1:1: 2:62 note: in expansion of print!
<std macros>:3:1: 3:54 note: expansion site
<std macros>:1:1: 3:58 note: in expansion of println!
src/lib.rs:3:5: 3:45 note: expansion site
error: aborting due to 2 previous errors
Could not compile `error-messages`.

To learn more, run the command again with --verbose.

```

</details>
