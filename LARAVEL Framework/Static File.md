Entry point atau jalur masuk utama di Laravel adalah file `index.php`, yang terdapat di folder public. Ketika kita melakukan request `/hello`, sebenarnya kita mengakses `/index.php/hello`.

Untuk menambahkan static file kita bisa dengan mudah menambahkan ke dalam folder public. Secara otomatis ketika mengakses url ke file static tersebut, maka server akan mencari static filenya terlebih dahulu, jika tidak ada, maka terakhir akan dikirimkan requestnya ke `index.php`, dan tentunya karena route dengan nama file tersebut tidak ada, maka akan ditampilkan halaman error 404, namun jika file nya ada maka filenya akan load atau diambil.

## Untuk apa directory Resources ?

Di dalam directory Resources selain views ada juga css dan js, ini adalah fitur tambahan Laravel yang memanfaatkan nodeJS sehingga kita bisa melakukan compile terhadap file css dan js yang terdapat di directory resources agar di minify (supaya ukurannya kecil). Setelah di compile, file js dan css akan tetap di pindahkan ke folder public.