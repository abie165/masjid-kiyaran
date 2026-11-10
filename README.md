# Sistem Informasi Masjid Al-Muqarrabillah Kiyaran — Revisi

Paket aplikasi web statis untuk GitHub Pages + Firebase Authentication + Cloud Firestore.

## Isi paket
- `index.html` — struktur halaman dan formulir.
- `style.css` — desain responsif dengan latar gradasi berbeda pada tiap halaman.
- `app.js` — fungsi aplikasi, login admin tunggal, data kas dan konten masjid.
- `firestore.rules` — aturan keamanan Firestore.
- `README.md` — panduan pemasangan.

## Perubahan pada versi ini
- Desain diperbarui dengan warna gradasi dan latar berbeda untuk Beranda, Kas, Takmir, Khatib & Imam, dan TPA.
- Login menampilkan kolom password saja. Email admin ditetapkan di kode sebagai `munzifani@gmail.com`.
- Hak tulis dibatasi ke satu akun admin tersebut, dengan dokumen admin aktif pada Firestore.
- Kalender Hijriah menampilkan nama bulan dan akhiran `H`.
- Kartu Jumlah Transaksi di Beranda dihapus.
- Tombol tambah kegiatan, transaksi kas, takmir, jadwal imam, dan data TPA ditampilkan setelah admin terverifikasi.
- Tombol ubah/hapus untuk data terkait tersedia bagi admin.
- Rincian kas tidak menampilkan kolom Keterangan dan Bukti; formulir transaksi juga disederhanakan.
- Perbaikan editor ubah data TPA dan fungsi render halaman.

## Penting: aktivasi akun admin
Saya tidak dapat mengubah akun atau password pada proyek Firebase Anda dari file ZIP. Agar aplikasi aktif, lakukan satu kali pengaturan berikut di Firebase Console.

1. Buka Firebase Console: https://console.firebase.google.com/
2. Pilih proyek yang dipakai aplikasi ini (`masjid-kiyaran`, sesuai konfigurasi yang sudah ada di `app.js`).
3. Buka **Authentication → Sign-in method** dan aktifkan **Email/Password**.
4. Buka **Authentication → Users**. Pastikan hanya ada satu akun admin dengan email `munzifani@gmail.com`. Jika belum ada, buat akun tersebut dengan password pilihan Anda.
5. Demi keamanan, karena password admin sempat dikirim melalui percakapan, sebaiknya gunakan password baru yang kuat dan jangan menaruh password di file kode atau membagikannya.
6. Buka **Firestore Database → Rules**, ganti seluruh rules dengan isi file `firestore.rules`, lalu klik **Publish**.
7. Buka **Firestore Database → Data**. Buat koleksi `pengelola` jika belum ada.
8. Salin **UID** akun admin dari Authentication → Users. Buat dokumen di koleksi `pengelola` dengan **Document ID persis UID tersebut**.
9. Tambahkan field:
   - `role` — string — `admin`
   - `aktif` — boolean — `true`
10. Simpan. Jangan buat dokumen admin aktif untuk akun lain. Rules akan menolak semua akun selain email admin yang ditetapkan.

## Cara menerapkan file revisi
1. Ekstrak ZIP.
2. Ganti file lama `index.html`, `style.css`, `app.js`, dan `firestore.rules` dengan file dari paket ini.
3. Jika menggunakan GitHub Pages, unggah empat file aplikasi ke root repositori (folder utama).
4. Tunggu deployment selesai, lalu buka ulang situs. Jika tampilan lama masih muncul, lakukan hard refresh (`Ctrl + Shift + R`).
5. Login dari tombol **Login Pengelola** menggunakan password akun admin yang sudah dibuat di Firebase. Email tidak perlu dimasukkan di halaman login.

## Catatan keamanan
- Password tidak ditanam di HTML atau JavaScript. Firebase Authentication tetap memverifikasi password terhadap akun admin.
- Login password-saja di tampilan menggunakan alamat admin tetap yang ada di `app.js`; ini bukan pengganti keamanan server. Pembatasan sebenarnya dilakukan oleh Firestore Security Rules.
- Jangan menghapus dokumen `pengelola/{UID}` atau menonaktifkan `aktif`, karena tombol pengelolaan akan hilang dan operasi tulis ditolak.
- Data kas, takmir, kegiatan, jadwal, dan TPA masih dapat dibaca publik sesuai rancangan aplikasi. Jangan memasukkan data pribadi sensitif, khususnya data anak.
- Kalender Hijriah menggunakan kalender Umm al-Qura pada browser dan dapat berbeda satu hari dari penetapan resmi setempat.
- Jadwal sholat mengambil data dari AlAdhan untuk Sawit, Boyolali. Cocokkan dengan jadwal resmi masjid.

## Troubleshooting
- **Login berhasil tetapi tombol tambah tidak muncul:** pastikan email akun tepat, UID dokumen `pengelola` sama persis, `role=admin`, dan `aktif=true`.
- **Login gagal:** pastikan Email/Password aktif dan akun admin sudah dibuat di proyek Firebase yang sama.
- **Missing or insufficient permissions:** pastikan isi `firestore.rules` sudah dipublikasikan.
- **Halaman kosong:** pastikan `firebaseConfig` pada `app.js` sesuai proyek Firebase dan file berada di root situs.
