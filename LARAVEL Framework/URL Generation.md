Laravel menyediakan beberapa cara untuk membuat link di aplikasi kita, ini sangat berguna ketika kita butuh membuat link di View atau Response.

## Current URL

Kadang kita ingin mengakses url saat ini, sebenarnya kita bisa menggunakan object request, namun jika dalam keadaan tidak ada object Request kita bisa menggunakan class`Illuminate\Routing\UrlGenerator`. Untuk membuat class tersebut kita bisa menggunakan helper method `url()` atau menggunakan facade `URL`. `url()->current()` untuk mendapatkan url saat ini tanpa query param dan `url()->full()` untuk mendapatkan url saat ini dengan query param.

## URL untuk named Route

`URLGenerator` juga bisa digunakan untuk membuat link menuju named routes
Kita bisa menggunakan method `route(name, parameters)` atau `URL::route(name, parameters)` atau `url()->route(name, parameters)`.

## URL untuk Controller action

`URLGenerator` juga bisa digunakan untuk membuat link menuju controller action,
Laravel secara otomatis akan mencarikan path yang sesuai di route dengan controller action tersebut.
Kita bisa menggunakan method `action(controllerAction, parameters)`, atau `URL::action(controllerAction, parameters)`, atau `url()->action(controllerAction, parameters)`.