Dibawah ini adalah saran dimana sebaiknya melakukan revalidatePath atau revalidate function lainnya, *hanya jika kamu menggunakan caching default saja*, tanpa ada set caching secara manual:
Sebaiknya tidak digunakan:
1. Route handler: karena secara default tidak ada cache disini, kecuali kamu setting sendiri
2. Halaman dengan dynamic route: seperti halaman edit

Sebaiknya digunakan:
1. Jika halaman itu static route, maka harus revalidatePath, baik itu halaman edit itu sendiri (jika ini dihalaman edit), atau halaman lain yang menggunakan data yang sama. Walaupun halaman itu dynamic rendering dan kamu tidak menggunakan Data Caching, tetap saja ada kemungkinan menampilkan data lama yang ada di Client Router Cache.