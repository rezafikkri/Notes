Di Laravel kita bisa memberikan nama pada sebuah Route. Hal ini bagus ketika kita nanti butuh misalnya membuat URL dari route tersebut (misalnya pada tombol edit, dari pada kita memasukkan routenya secara manual lebih baik kita memanfaatkan fitur ini, dengan hanya memanfaatkan nama routenya saja), atau ketika ingin melakukan redirect ke route tertentu, apalagi kalau routenya cukup panjang dan kita butuh melakukan beberapa redirect ke route tersebut, maka akan cukup repot untuk menulis routenya, juga ada kemungkinan nanti terjadi *typo*, maka dari itu kita perlu menamai route tersebut.

Dengan menambahkan nama di route nya, kita bisa menggunakan nama route saja, dan tidak perlu khawatir jika URL route nya akan berubah. 

**Note**: nama route harus selalu unik

```php
Route::get('/user/{id}/profile', function (string $id) {

// ...

})->name('profile');

$url = route('profile', ['id' => 1]);
// Contoh hasil: http://localhost:8000/user/1234/profile
```

Untuk membuat url ke route yang memiliki route parameter, sebenarnya tidak harus menggunakan assosiatif array, kita juga bisa memasukkan parameternya sesuai dengan urutan route parameternya. Contoh:

```php
Route::get('/product/{product}/item/{item}', function (string $productId, string $itemId) {

// ...

})->name('product.item.detail');

$url = route('product.item.detail', [$id, $item]);
// Contoh hasil: http://localhost:8000/product/1234/item/fgh56
```

Intinya yang perlu diingat bahwa ketika menggunakan array assosiatif, maka nama key dari array harus sama dengan nama route parameternya, sedangkan urutannya arraynya tidak berbengaruh. Namun kebalikannya ketika tidak menggunakan assosiatif array, maka urutan dari array tersebut berbengaruh, array ke-1 akan di masukkan ke route parameter ke-1, array ke-2 akan dimasukkan ke route parameter ke-2 dan begitu seterusnya.