By default, Rust brings only a few types into the scope of every program in
[the *prelude*][prelude]<!-- ignore -->. If a type you want to use isn’t in the
prelude, you have to bring that type into scope explicitly with a `use`
statement. Using the `std::io` library provides you with a number of useful
features, including the ability to accept user input.
