**Model**: Bagian yang merepresentasikan data, seperti data request, response, table dan lain-lain.

**View**: Bagian yang merepresentasikan tampilan, seperti halaman web dan lain-lain.

**Controller**: Bagian yang mengurus alur kerja aplikasi atau logic aplikasi, dari mulai menerima input data, memanipulasi data, memasukkannya kedalam Model, sampai menampilkan View. Jadi angap saja Controller adalah core logic dari suatu aplikasi.

![Diagram dasar MVC](https://ik.imagekit.io/rezafikkri/diagram%20dasar%20mvc.png?updatedAt=1732930245899)
<small style="color:gray">Diagram dasar MVC</small>

![Diagram contoh Aplikasi dengan MVC](https://ik.imagekit.io/rezafikkri/Diagram%20contoh%20aplikasi%20dengan%20MVC.png?updatedAt=1732930376992)
<small style="color:gray">Diagram contoh Aplikasi dengan MVC</small>

### Routing

Adalah teknik melakukan penentuan rute dari URL ke file PHP yang akan dieksekusi.

### Path Variable

Atau disebut juga Path Parameter, adalah semacam fitur yang bisa membuat kita menambahkan data pada url. Jadi kalau biasanya ketika ingin mengirim data melalui url, kita akan menggunakan query paramater, namun dalam beberapa kasus data, kita sebaiknya menggunakan Path Variable, seperti:

`/products/1234fe`

Contoh penting yang pernah saya alami sendiri adalah, ketika membuat url untuk artikel blog dan butuh untuk menambahkan fitur share ke social media, nah dalam hal ini kita harus menggunakan Path Variable, mengapa? karena saya sendiri sudah pernah menggunakan query parameter dan akhirnya tidak bisa membuat fitur share ke social media.

Query parameter:

`/blogs?slug=membuat-query-lebih-cepat`

Path Variable:

`/blogs/membuat-query-lebih-cepat`

### Middleware

Merupakan bagian kode yang dieksekusi sebelum Controller dieksekusi.