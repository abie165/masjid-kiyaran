# Sistem Informasi Masjid Al-Muqarrabillah Kiyaran — Revisi V5

Paket aplikasi web statis untuk GitHub Pages + Firebase Authentication + Cloud Firestore.

## Isi paket
- `index.html` — struktur halaman, navigasi, dan formulir.
- `style.css` — desain responsif.
- `app.js` — logika aplikasi dan login admin tunggal.
- `firestore.rules` — aturan keamanan Firestore.
- `README.md` — panduan pemasangan.

## Perbaikan V5
1. Memperbaiki bug yang membuat callback login berhenti sebelum tombol pengelolaan ditampilkan. Pada versi sebelumnya, kode mencoba mengubah elemen `loginInfo` yang tidak ada di HTML; error tersebut menghentikan proses penampilan tombol.
2. Status pengelola kini ditentukan konsisten dari akun Firebase Authentication yang emailnya cocok dengan satu email admin yang ditetapkan di `app.js`.
3. Setelah admin masuk, tombol tambah, ubah, dan hapus ditampilkan pada Kas Masjid, Takmir, Khatib & Imam, TPA, serta Kegiatan Masjid. Jamaah yang tidak login tetap hanya dapat melihat.
4. Login tetap cukup menggunakan password; email admin ditetapkan di dalam kode aplikasi.
5. Firestore Rules diselaraskan: hanya akun Firebase dengan email admin yang ditentukan dapat menambah, mengubah, atau menghapus data.
6. Tombol Unduh Excel (CSV) dan Cetak/PDF dipindahkan ke bawah rincian transaksi Kas Masjid.

## Cara memasang
1. Ekstrak ZIP.
2. Ganti `index.html`, `style.css`, dan `app.js` pada repositori GitHub Pages dengan file V5.
3. Buka Firebase Console → Firestore Database → Rules.
4. Ganti seluruh isi Rules dengan file `firestore.rules` dari V5, lalu klik **Publish**. Langkah ini wajib agar tombol yang sudah muncul dapat menyimpan perubahan.
5. Tunggu GitHub Pages selesai deployment.
6. Buka situs dan lakukan hard refresh (`Ctrl + Shift + R`) atau hapus cache situs di HP.
7. Login melalui tombol Login Pengelola menggunakan password akun admin Firebase.

## Pengaturan admin dan keamanan
- Email admin tetap: `munzifani@gmail.com`.
- Password tidak disimpan di HTML atau JavaScript; Firebase Authentication yang memverifikasinya.
- Tidak diperlukan lagi dokumen `pengelola/{UID}` untuk menentukan tampilan hak admin. Hak tulis di database ditentukan langsung oleh Firebase Authentication melalui Firestore Rules.
- Pastikan akun dengan email admin tersebut memang akun yang Anda kendalikan. Jangan membuat atau membagikan akses akun admin kepada orang lain.
- Jangan pernah menaruh password di file aplikasi atau repositori GitHub.
- Data masjid dapat dibaca publik sesuai rancangan. Hindari memasukkan data pribadi sensitif, khususnya data anak TPA.

## Troubleshooting
- **Tombol tambah/ubah/hapus belum muncul:** pastikan deployment V5 selesai, lakukan hard refresh, lalu pastikan login menggunakan akun admin yang emailnya sesuai.
- **Tombol muncul tetapi simpan/hapus gagal:** hampir selalu Firestore Rules belum diganti atau belum di-Publish. Periksa tab Rules dan pastikan isinya sama dengan file V5.
- **Login gagal:** pastikan Email/Password aktif pada Firebase Authentication dan akun admin tersedia pada proyek Firebase yang sama dengan konfigurasi `app.js`.
- **Data tidak tampil:** pastikan `firebaseConfig` di `app.js` masih menunjuk proyek Firebase yang benar.
