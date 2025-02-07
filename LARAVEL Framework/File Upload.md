Laravel juga sudah menyediakan method `file(key)` di Request untuk mengambil request file upload. Method tersebut me return instance dari class `Illuminate\Http\UploadedFile`. File upload di Laravel terintegrasi dengan baik dengan [[File Storage]].

Untuk mengambil nama file kita bisa menggunakan dua method yang ada pada object UploadedFile, yaitu:
1. `getClientOriginalName()`; tetapi ini kurang aman, karena bisa diubah oleh user. Method ini cocok untuk sebagai nama file yang kita simpan di Database.
2. `hasName()`;  sedangkan method ini cocok digunakan untuk nama file yang diupload itu sendiri.

Untuk menyimpan file kita bisa menggunakan method `store()` atau `storeAs()`, perbedaannya adalah, jika menggunakan method `store()` nama file yang diupload nantinya akan random, sedangkan method `storeAs()`, kita bisa memasukkan nama file sesuai keinginan kita para argument ke-2. Selain dua method tersebut masih ada method-method yang lain.