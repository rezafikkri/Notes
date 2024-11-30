
Adalah rekomendasi yang sudah didiskusikan oleh komunitas untuk standarisasi framework atau library di PHP.

## Dependecy Injection

Sudah biasa kita membuat object yang membutuhkan object lain, namun ketika aplikasi yang kita buat sudah kompleks, kadang agak sulit melakukan management ini secara manual.

Dependency Injection adalah dimana sebuah object menerima object lainnya yang dibutuhkan. Misal Controller membutuhkan service dan service membutuhkan Repository. Untuk melakukan Dependency Injection caranya sangat mudah, kita bisa tambahkan object yang dibutuhkan menggunakan parameter constructor, tetapi ini masil kita lakukan secara manual.

Kita bisa melakukan Dependency Injection secara otomatis menggunakan PSR-11. Caranya adalah dengan menyediakan Container (tempat menyimpan object) dan kita bisa mengambil object melalui Container secara otomatis dan Container akan mengurus Dependency Injection yang dibutuhkan secara otomatis.