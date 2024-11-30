## Admin

1. Melihat persentase pengisian rapor
2. Kelola Identitas Sekolah
	- RU (Read Update)
3. Kelola Jurusan
    - CRUD (Create Read Update Delete)
4. Kelola Kelas
    - CRUD (Create Read Update Delete)
5. Kelola Wali Kelas
    - CRUD (Create Read Update Delete)
6. Kelola Siswa
    - CRUD (Create Read Update Delete)
    - Export(PDF)/Cetak data Siswa Detail
    - Import dari Excel (dengan format yang telah ditentukan)
7. Kelola Tahun Ajaran
    - CD (Create Delete)
    - Setel Tahun Ajaran Aktif
    - Unset Tahun Ajaran Aktif
8. Kelola Semester
    - Setel Semester Aktif
    - Unset Semester Aktif
9. Kelola KKM
    - CRUD (Create Read Update Delete)
11. Kelola Mata Pelajaran
    - CRUD (Create Read Update Delete)
12. Kelola Siswa Lulus
    - UD (Update Delete)
    - Export(PDF)/Cetak Transkip Nilai Dari Semua Semester
    - Export(PDF)/Cetak Surat Kelulusan
13. Kelola Siswa Keluar
    - UD (Update Delete)
    - Export(PDF)/Cetak Surat Keluar & Masuk
14. Izin Melakukan Kenaikan Kelas Terhadap Guru Wali Kelas
    - Admin perlu memastikan apakah semua data raport sudah diisi sesuai kriteria
15. Juara Umum
    - Menentukan Juara Umum per-Jurusan atau per-Jenjang Kelas
    - Export(PDF)/Cetak data Juara Umum
16. Melakukan semua yang bisa dilakukan oleh Wali Kelas dan Guru Mata Pelajaran

## Wali Kelas

1. Home Guru
    - Cek Status Akhir Semester(Naik Kelas Atau Tidak) Belum Dimasukkan
    - Cek Nilai Belum Dimasukkan
    - Menentukan Juara Kelas
    - Export(PDF)/Cetak Lembaran Serah Terima Raport
    - Melakukan Kenaikan Kelas (Kelas Siswa akan diupdate secara otomatis sesuai Status Akhir Semester: naik kelas atau tidak)
        - Ketika dilakukan kenaikan kelas, akan secara otomatis menghapus data Guru Mata Pelajaran terkait
        - Ketika kenaikan kelas, akan secara otomatis menghapus data Wali Kelas
        - Ketika kenaikan kelas, akan secara otomatis akan unset Semester dan Tahun Ajaran aktif
    - Lihat Siswa Tinggal Kelas
    - Lihat Siswa Tidak Lulus
2. Rapor Per Siswa
    - CU (Create Update Sikap Siswa)
    - CD (Create Delete Prakerin)
    - CD (Create Delete Ekstrakurikuler)
    - CD (Create Delete Prestasi)
    - CU (Create Update Ketidakhadiran)
    - CU (Create Update Catatan Wali Kelas)
    - CU (Create Update Status Akhir Semester)
    - Export(PDF)/Cetak Raport Siswa
    - Reset/Hapus data Raport

## Guru Mata Pelajaran

1. CU (Create Update Nilai per Siswa)

## Siswa (Public)

1. R (Read Rapor, fitur ini bisa bermanfaat hanya jika aplikasi di deploy (upload) ke hosting atau bisa juga dengan memanfaatkan wifi sekolah)
2. Melihat data Guru Mata Pelajaran yang mengajar pada tahun ajaran dan semester aktif saat ini