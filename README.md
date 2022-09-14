# RestApi_Roket_test

## Web Framework

- Actix :숙련자용 가장빠름

- Rocket :입문자용

- Yew:WASM에 적합함 컴포넌트 재사용 가능.미래지향적

## Rocket

## 로켓폴더생성

```
cargo new rocket_test --bin
cd hello-rocket
```

## 2022 년 9월기준

```
[dependencies]
rocket = "0.4.10"
```

- mainrs

```rs
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

## Template

```
[dependencies.rocket_contrib]
version = "0.4.4"
features = ["tera_templates"]
```

- mainrs

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

- base.html.tera

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

- index.html.tera

```html
{% extends "base" %} {% block body %}
<h1>Hello, {{ name }}.</h1>
{% endblock body %}
```

- main.rs

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
