Sebelumnya kita sudah tahu bahwa di Route dan Controller kita bisa mengembalikan data berupa string dan view.

Laravel memilih class `Illuminate\Http\Response` yang bisa digunakan untuk representasi dari HTTP Response. Dengan class Response ini, kita bisa mengubah HTTP Response status code dan header.

Untuk membuat object Response kita bisa menggunakan function helper `response(content, status, headers)`.

## HTTP Response Header

Saar kita membuat Response kita juga bisa mengubah status dan juga header responsenya.

Kita bisa memasukkan header pada argument ketiga pada function `response(content, status, headers)`, atau kita juga bisia menggunakan method `withHeaders(arrayHeaders)` dan `header(key, value)`.

## Response Type

Sebelumnya kita sudah melakukan response JSON secara manual, sebenarnya ketika memanggil helper function `response` tanpa argument apapun, maka akan return sebuah implementasi dari interface `Illuminate\Contracts\Routing\ResponseFactory`, yang mana dalam interface tersebut ada beberapa method yang bisa digunakan untuk membuat beberapa jenis response type, yaitu:
1. `view(name, data, status, headers)`;  digunakan jika kita ingin menampilkan view, tetapi juga ingin mengubah response status code atau header.
2. `json(array, status, headers)`; digunakan untuk menampilkan JSON.
3. `file(pathToFile, headers)`; digunakan untuk menampilkan File.
4. `download(pathToFile, name, headers)`; digunakan untuk memaksa browser user untuk mendownload file.

> **Note**: inti dari Response ini adalah supaya kita bisa mengubah status code dan header dari HTTP response.