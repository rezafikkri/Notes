Redirect itu sendiri di dalam Laravel di representasikan dalam class `Illuminate\Http\RedirectResponse`. Untuk membuat object redirect, kita bisa menggunakan helper function `redirect(to)`.

## Redirect to Named Route

Ketika kita memanggil helper function `redirect` tanpa argument apapun, maka akan return sebuah instance dari class `Illuminate\Routing\Redirector`, yang mana dari instance atau object `Redirector` tersebut  kita bisa memanggil method `route(name, params)` untuk membuat `RedirectResponse` ke sebuah named route.

Salah satu keuntungan kita menggunakan ini adalah kita bisa menambahkan parameter tanpa harus manual membuat pathnya.

## Redirect to Controller Action

Setiap method yang ada di dalam Class Controller Laravel disebut sebagai "action" , jadi maksud dari kata "Controller Action" adalah merujuk ke method-method yang ada di dalam class Controller yang bertugas untuk menangani request dan mengembalikan response.

Selain menggunakan Named Routes, kita juga bisa redirect ke Controller Action dan Laravel akan otomatis tahu URL target redirect nya yang mana dari routes-nya. Untuk melakukan itu kita bisa menggunakan method `action([controller, actionName], params)` yang ada di object `Illuminate\Routing\Redirector`.

## Redirect to External Domain

Jika kita ingin melakukan redirect ke domain lain, kita bisa menggunakan method `away(url)`, yang mana method tersebut akan membuat sebuah `RedirectResponse` tanpa ada tambahan URL encoding, validation, ata verification apapun:

```php
return redirect()->away('https://www.google.com');
```

