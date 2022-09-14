# RestApi_Roket_test

##

```
cargo new hello-rocket --bin
cd hello-rocket
```

```
[dependencies]
rocket = "0.4.4"
```

```
#![feature(proc_macro_hygiene, decl_macro)]

#[macro_use] extern crate rocket;

#[get("/")]
fn index() -> &'static str {
    "Hello, world!"
}

fn main() {
    rocket::ignite().mount("/", routes![index]).launch();
}
```

##

```
[dependencies.rocket_contrib]
version = "0.4.4"
features = ["tera_templates"]
```

```rs
#![feature(proc_macro_hygiene, decl_macro)]

#[macro_use] extern crate rocket;

extern crate rocket_contrib;
use rocket_contrib::templates::Template;

#[get("/")]
fn index() -> Template {
    let mut context = ();
    Template::render("index", &context)
}

fn main() {
    rocket::ignite()
        .mount("/", routes![index])
        .attach(Template::fairing())
        .launch();
}
```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
  </head>
  <body>
    {% block body %} {% endblock body %}
  </body>
</html>
```

```html
{% extends "base" %} {% block body %}
<h1>Hello, {{ name }}.</h1>
{% endblock body %}
```

```rs
#![feature(proc_macro_hygiene, decl_macro)]

#[macro_use] extern crate rocket;

extern crate rocket_contrib;
use rocket_contrib::templates::Template;

use std::collections::HashMap;

#[get("/")]
fn index() -> Template {
    let mut context = HashMap::new();
    context.insert(
        "name".to_string(),
        "Jino Bae".to_string(),
    );
    Template::render("index", &context)
}

fn main() {
    rocket::ignite()
        .mount("/", routes![index])
        .attach(Template::fairing())
        .launch();
}
```
