## Type Data

**Serial**: serial adalah tipe data yang digunakan ketika kita ingin membuat fitur seperti auto_increment kalau di database mariadb atau mysql.

Serial adalah sebenarnya tipe data integer, jika ingin ukuran lebih besar maka bisa menggunakan bigserial (bigint) atau bisa juga smallserial (smallint).

Lebih dalam lagi mengenai serial, ketika kita menggunakan tipe data serial, di belakang layar postgre menggunakan sequence. Sequence adalah fitur dimana kita bisa membuat function auto increment.

```sql
create table admin
(
    id         serial,
    first_name varchar(100) not null,
    last_name  varchar(100),
    primary key (id)
);

# code diatas sama dengan:

create sequence admin_id_seq;

create table admin
(
    id         int default nextval('admin_id_seq'),
    first_name varchar(100) not null,
    last_name  varchar(100),
    primary key (id)
);
```

## Index

Cara kerja index: Misal kita membuat index (col1, col2, col3), artinya kita bisa melakukan pencarian secara cepat dengan komobinasi query, (col1), (col1, col2) dan (col1, col2, col3). kalau kita mengunakan kombinasi query (col3, col1), index nya tidak akan digunakan alias tidak akan memberi dampak apa-apa. Jadi jika ingin membuat index, sesuaikan dengan querynya.

Kalau kita membuat index dengan kombinasi (col1, col3), maka index akan digunakan untuk kombinasi query (col1) dan (col1, col3), tetapi sayangnya, jika kita query dengan (col3) saja, maka index tidak akan digunakan.

Ingat juga kalau kalian butuh query untuk kombinasi (col2, col3), dan kalian membuat index-nya satu persatu, index col2, terus index col3, alias tidak di kombinasikan, maka indexnya tidak akan terpakai.

Membuat index harus bijak, karena walaupun akan membuat proses pencarian data lebih cepat, tetapi akan membuat proses manipulasi data lebih lambat. Karena tiap kita melakukan update misalnya, maka postgreSQl juga akan melakukan update data di index.

> Catatan: Jika kita menggunakan LIKE ataupun ILIKE operator, walaupun kita menambahkan index, hal itu tidak akan membantu. Karena LIKE operator tidak menggunakan index

## Operator LIKE or ILIKE

Operator LIKE ataupun ILIKE itu sebenarnya tidak disarankan jika data yang kita punya itu sudah besar atau banyak. Karena operator LIKE akan membaca data dari record 1 sampai akhir, sehingga otomatis akan lambat.

## Behavior dari FOREIGN KEY

Secara default behavior dari foreign key adalah NO ACTION. Behavior dari NO ACTION mirip seperti RESCTRICT, perbedaannya hanya akan muncul jika kita mendefinisikan foreign key constraint sebagai DEFERRABLE.

|**Behavior**|**ON DELETE**|**ON UPDATE**|
|---|---|---|
|NO ACTION|Ditolak|Ditolak (jika yang ingin kita update adalah kolom yang direferensi oleh _referencing table,_ biasanya adalah kolom id).|
|RESCTRICT|Ditolak|Ditolak (sama seperti NO ACTION)|
|CASECADE|Data pada _referencing table_ akan ikut dihapus|Data pada _referencing table_ akan ikut di update, data yang di update adalah data pada kolom FOREIGN KEY (jika yang ingin kita update adalah kolom yang direferensi oleh _referencing table_, biasanya adalah kolom id).|
|SET NULL|Diubah jadi null (yang diubah menjadi null adalah data pada kolom yang kita buat sebagai FOREIGN KEY)|Diubah jadi null (jika yang kita update adalah kolom yang direferensi oleh _referencing table_), biasanya kolom id. Dan yang di update menjadi null adalah kolom yang kita buat sebagai FOREIGN KEY.|
|SET DEFAULT|Diubah menjadi default value (data yang diubah sama seperti SET NULL)|Diubah menjadi default value (saya kira hampir sama seperti SET NULL, cuman data pada kolom yang buat sebagai FOREIGN KEY di set ke DEFAULT value), ini jarang sekali saya gunakan. Jika kita tidak mendefinisikan default value dan kolom FOREIGN KEY tersebut bisa null, ketika kolom yang direferensi oleh _referencing table_ di update (biasanya kolom id), maka kolom FOREIGN KEY pada _referencing table_ akan di update menjadi null.|

## JOIN

Untuk melakukan JOIn kita harus menentukan kolom table mana yang merupakan referensi ke kolom table lain. Join cocok sekali dengan FOREIGN KEY, walaupun tidak ada aturan kalau JOIN harus ada FOREIGN KEY.

> Idealnya jangan melakukan JOIN lebih dari 5 table.

## Set Operator

Atau kalau di PostgreSQL disebut Combining Queries. Dimana ini adalah fitur di PostgreSQL yang memungkinkan kita untuk mengkombinasikan hasil di dua SELECT query.

1. UNION
	Adalah mengkombinasikan hasil dari dua SELECT query, dimana jika terdapat data yang duplikat, maka data yang duplikatnya akan dihapus, _kecuali_ kita menggunakan UNION ALL.
2. INTERSECT
    Adalah mengkombinasikan hasil dari dua query SELECT, namun data yang diambil adalah hanya data yang terdapat pada hasil query pertama dan query kedua, alias data yang duplikat. Data yang hanya ada pada salah satu query akan dihapus. Dan data yang dihasilkan tidak dalam keadaan duplikat, _kecuali_ kita menggunakan INTERSECT ALL.
3. EXCEPT
    Adalah operasi dimana query pertama akan dihilangkan oleh query kedua, maksudnya adalah jika ada data di query pertama yang sama dengan data di query kedua, maka data tersebut akan dihapus. Atau dengan kata lain data yang akan dihasilkan adalah data yang ada pada query pertama, tetapi tidak ada pada query kedua. Dan data yang dihasilkan tidak dalam keadaan duplikat, _kecuali_ kita menggunakan EXCEPT ALL.

## Transactions

Row dalam DBMS juga disebut sebagai _record_, dan Column juga disebut sebagai _field_.

## Race Condition

Race condition terjadi ketika ada lebih dari 1 proses mencoba mengupdate data yang sama dalam waktu yang sama, tetapi dalam proses update itu sebenarnya tidak hanya update yang dilakukan, tetapi perlu melakukan suatu validasi terlebih dahulu.

Misalnya dalam sistem stok pada toko online, kita user membuat pesanan, maka stok akan di update, dan sebelum stok di update tentunya perlu melakukan pengecekan stok barang terlebih dahulu. Lalu bagaimana race condition bisa terjadi?

1. Ada produk A, dengan stok=1
2. Lalu ada 2 user dalam waktu bersamaan membuat pesanan untuk produk A
3. Proses (user 1) terlebih dahulu melakukan pengecekan stok, dan ternyata stok masih ada
4. Sebelum Proses (user 1) melakukan update stok, Proses (user 2) melakukan pengecekan stok dan ternyata stok masih ada
5. Dan akhirnya Proses (user 1) selesai dan stok diupdate menjadi 0
6. Dan Proses (user 2) juga selesai dan stok diupdate menjadi 0
7. Lalu apa salahnya? jelas salah, karena stok produk A cuman tinggal 1, sehingga normalnya hanya salah satu user yang bisa melakukan pembuatan pesanan, tetapi ternyata ke-2 user tersebut bisa melakukan pembuatan pesanan.

Lalu apa solusinya?

1. Kita bisa melakukan locking pada saat melakukan SELECT untuk validasi, yaitu dengan cara `SELECT FOR UPDATE` (jika menggunakan database relation), sehingga ketika proses ke-2 ingin melakukan `SELECT FOR UPDATE` juga, maka harus menunggu proses ke-1 selesai (biasanya ketika telah dilakukan `commit` ataupun `rollback`).
2. Kita juga bisa melakukan yang namanya _optimistic locking_, yaitu ketika membuat query update, kita tidak hanya `WHERE` berdasarkan id barang tetapi juga berdasarkan data yang ingin diupdate, dalam hal ini adalah stok (yaitu data stok yang di dapatkan pada query SELECT untuk validasi). Bagaimana hal ini bisa menangani race condition?
	1. Proses (user 1) melakukan pengecekan dan stok masih ada
	2. Sebelum Proses (user 1) melakukan update stok, Proses (user 2) melakukan pengecekan dan stok masih ada
	3. Karena secara low level untuk mengupdate data yang sama, database pastikan akan memilih satu proses yang akan dijalankan terlebih dahulu (misalnya adalah Proses (user 1)), maka update stok dilakukan dengan `WHERE id barang AND stok = 1` dan pesanan akhirnya berhasil dibuat
	4. Lalu Proses (user 2) akan dijalankan dan update stok dilakukan dengan `WHERE id barang AND stok = 1`, tetapi di database sudah tidak ada lagi, produk A dengan stok = 1, yang ada adalah produk A dengan stok = 0, query akan tetap berjalan dan tidak ada error, tetapi _affected row_ dari proses query update tersebut = 0, atau dengan kata lain proses pembuatan pesanan gagal, karena tidak ada produk yang berhasil di update stok-nya, sehingga nantinya di code aplikasi, kita tinggal mengecek value dari _affected row_, ketika _affected row_ = 0, maka kita anggap proses pembuatan pesanan gagal.

## Deadlock

Deadlock adalah situasi dimana ada 2 proses yang saling menunggu _lock_ satu sama lain, sehingga proses menunggunya ini tidak akan pernah selesai.

Contoh Deadlock:

1. Proses 1 melakukan `SELECT FOR UPDATE` untuk data 001
2. Proses 2 melakukan `SELECT FOR UPDATE` untuk data 002
3. Proses 1 melakukan `SELECT FOR UPDATE` untuk data 002, diminta menunggu karena telah di lock oleh proses 2
4. Proses 2 melakukan `SELECT FOR UPDATE` untuk data 001, diminta menunggu karena telah di lock oleh proses 1
5. Akhirnya proses 1 dan 2 saling menunggu
6. Deadlock pun terjadi

Solusi untuk deadlock adalah:

1. Kita menetapkan timeout untuk _lock_ dan juga transaction (karena di transaction juga ada peluang deadlock).
2. Solusi ke-2 adalah dengan melakukan _lock_ sekaligus data yang ingin kita update, misalnya kita ingin mengupdate data-1 dan data-2 maka kita lock dua-dua-nya sekaligus. Misalnya seperti dibawah ini:

```sql
	SELECT * FROM t1 WHERE IN(1,2) FOR UPDATE
```

> Menurut saya peluang deadlock hanya akan terjadi jika dalam satu proses kita perlu meng-update lebih dari satu data atau satu record dan kita perlu untuk lock data tersebut. Serta juga lock tersebut dilakukan secara terpisah alias satu persatu.