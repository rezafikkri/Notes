Laravel mendukung grouping Route, dimana dengan melakukan grouping Route kita bisa share konfigurasi antar Route dalam satu group.

Ini lebih mudah dibandingkan membuat konfigurasi Route satu persatu.

## Route Prefix

Karavek mendukung pembuatan Route prefix, dimana kita bisa membuat prefix (awalan) untuk uri Route.

Ini sangat berguna ketika kita ingin membuat Route dengan URl yang awalannya hampir sama semua.

Kita bisa menggunakan function `Route::prefix(prefix)->group(closure)`.

## Route Middleware

Route middleware mendukung grouping berdasarkan middleware, dimana secara otomatis semua route akan memiliki middleware tersebut.

## Route Controller

Route Controller memungkinkan kita membuat route dengan controller yang sama, ini mempermudah ketika kita ingin membuat beberapa route dengan class Controller yang sama.

## Multiple Route Group

Route juga bisa menggunakan beberapa jenis group, misal kita ingin membuat group dengan prefix yang sama dan middleware yang sama, maka bisa kita lakukan juga dengan Route di Laravel. 