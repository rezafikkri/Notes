Ini adalah fitur yang sama seperti pada Path Parameter ketika belajar [[PHP MVC]]. Yaitu suatu fitur yang membuat kita bisa menambahkan data pada URL, cuman di Laravel fitur ini lebih kompleks lagi dan caranya pun agak sedikit berbeda. Ini sangat berguna, misalnya ketika kita ingin membuat halaman edit produk.

Route parameter selalu terbungkus di dalam kurung kurawal `{name}`, jadi misalnya untuk route edit produk seperti ini `/product/{id}/edit`. Kita bisa menambahkan beberapa route parameter asal namanya berbeda. Data pada route parameter tersebut akan di masukkan secara otomatis ke parameter callback atau controller method nya sesuai dengan urutannya. Sedangkan nama dari parameter pada route callback atau controller method tidak harus sama dengan nama route parameternya.

```php
Route::get('/posts/{post}/comments/{comment}', function (string $postId, string $commentId) {

});
```

## Regular Expression Constraints

Kadang ada kalanya kita ingin menggunakan Route parameter namun parameternya memiliki pola atau format tertentu. Misal parameternya hanya boleh angka. Pada kasus seperti ini kita bisa menambahkan Regular expression pada Route parameter. Caranya adalah dengan menggunakan function `where()` setelah pembuatan Routenya.

```php
Route::get('/categories/{id}', function (string $categoryId) {
    return "Category $categoryId";
})->where('id', '\d+');
```

## Optional Route Parameter

Laravel juga mendukung parameter route optional, yang artinya parameternya tidak wajib diisi. Untuk membuat sebuah route parameter menjadi optional, kita bisa tambahkan ? (tanda tanya). Namun perlu diingat, jika kita menjadikan route parameter nya optional, maka kita harus menambahkan default value pada parameter di callback atau controller method nya.

```php
Route::get('/users/{id?}', function (string $userId = '404') {
    return "Users $userId";
});
```

## Routing Conflict

Saat membuat router dengan parameter, kadang terjadi conflict routing. Di Laravel jika terjadi conflict tidak akan menyebabkan error, namun Laravel akan memprioritaskan route yang pertama kali dibuat atau didefinisikan.

```php
Route::get('/conflict/{user}', function (string $name) {
    return "Username $name";
});

Route::get('/conflict/reza', function () {
    return "Username only for reza";
});
```

Pada contoh diatas request akan selalu masuk ke route yang pertama, walaupun yang diakses adalah route `/conflict/reza`. Supaya hasilnya sesuai keinginan kita, diamana kita ingin ketika route yang diakses adalah `/conflict/reza` maka request akan masuk ke route yang kedua, maka route `/conflict/reza` harus berada di atas route `/conflict/{user}`.