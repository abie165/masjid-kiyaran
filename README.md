# Sistem Informasi Masjid Al-Muqarrabillah Kiyaran

Aplikasi web statis untuk GitHub Pages + Firebase Authentication + Cloud Firestore. Proyek Firebase dan repositori GitHub harus BARU dan khusus masjid; jangan gunakan konfigurasi aplikasi RT.

## Fitur
- Beranda: ringkasan kas, tanggal Masehi/Hijriah, jadwal sholat otomatis untuk Sawit, Boyolali, dan agenda.
- Kas: transaksi masuk/keluar, kategori, keterangan, tautan bukti Drive, filter, ekspor CSV yang dapat dibuka di Excel, cetak/simpan PDF.
- Takmir: nama, jabatan, masa khidmat, tautan foto.
- Khatib & Imam: jadwal Jumat, imam rawatib, tarawih, kegiatan khusus.
- TPA: data ringkas santri/kelompok, pengajar, bahan ajar dan tautan Drive.
- Hak akses: jamaah hanya membaca; hanya UID yang didaftarkan di koleksi `pengelola` yang dapat mengubah data.

## 1. Buat proyek Firebase baru
1. Buka https://console.firebase.google.com/ lalu klik **Add project**.
2. Beri nama misalnya `masjid-almuqarrabillah-kiyaran`. Pastikan ini proyek baru, bukan `kas-rt-01-kiyaran`.
3. Setelah proyek dibuat, buka **Build > Authentication > Get started > Sign-in method** lalu aktifkan **Email/Password**.
4. Di **Authentication > Users > Add user**, buat akun pengelola pertama dengan email dan password yang kuat. Simpan password dengan aman.
5. Buka **Build > Firestore Database > Create database**. Mulai di production mode, pilih lokasi terdekat yang tersedia dan sesuai kebutuhan. Selesaikan pembuatan.
6. Buka **Project settings** (ikon roda gigi) > **General** > bagian **Your apps** > ikon Web `</>` > daftarkan aplikasi web. Salin objek `firebaseConfig`.
7. Buka file `app.js`, cari `const firebaseConfig=...` paling atas. Ganti semua nilai `ISI_...` dengan nilai dari Firebase. Jangan menempelkan password ke file kode.

## 2. Pasang Firestore Security Rules
1. Firebase Console > Firestore Database > Rules.
2. Ganti isi rules dengan seluruh isi file `firestore.rules` dalam paket ini.
3. Klik **Publish**.

## 3. Daftarkan UID pengelola (langkah penting)
1. Firebase Console > Authentication > Users. Klik akun pengelola yang baru dibuat dan salin **User UID**.
2. Firestore Database > Data > Start collection.
3. Collection ID: `pengelola`.
4. Document ID: tempel UID persis seperti dari Authentication.
5. Tambahkan field `role` bertipe string dengan nilai `admin`.
6. Tambahkan field `aktif` bertipe boolean dengan nilai `true`.
7. Klik Save. Untuk bendahara tambahan, buat akun Authentication baru lalu buat dokumen `pengelola/{UID}` dengan `role` = `bendahara` dan `aktif` = `true`.

> Role hanya dibuat dari Firebase Console. Jangan pernah memberi jamaah hak tulis ke koleksi `pengelola`. Email yang sama saja tidak otomatis memberi hak pengelola.

## 4. Buat repositori GitHub baru
1. Masuk https://github.com/ lalu klik **New repository**.
2. Nama contoh: `masjid-al-muqarrabillah-kiyaran`. Pilih **Public** (diperlukan untuk GitHub Pages gratis), lalu Create repository.
3. Upload empat file: `index.html`, `style.css`, `app.js`, `firestore.rules` (README opsional). Letakkan langsung di root repositori, bukan di folder lain.
4. Buka **Settings > Pages**. Pada **Build and deployment**, pilih **Deploy from a branch**, branch `main`, folder `/ (root)`, lalu Save.
5. Tunggu proses publikasi. URL akan berbentuk `https://NAMA-AKUN.github.io/masjid-al-muqarrabillah-kiyaran/`.

## 5. Google Drive
- Buat folder Drive misalnya `Dokumen Masjid Al-Muqarrabillah`.
- Untuk setiap dokumen, gunakan **Share > General access** dan pilih akses yang sesuai. Untuk dokumen yang boleh dibaca jamaah, pilih `Anyone with the link – Viewer` bila kebijakan masjid mengizinkan.
- Salin tautan dan tempel ke kolom tautan pada aplikasi. Aplikasi hanya menyimpan tautannya; file tetap berada di Drive.
- Jangan membuat dokumen sensitif (nomor HP, alamat, data keluarga, data pribadi anak) menjadi publik.

## 6. Cara memakai aplikasi
1. Buka alamat GitHub Pages.
2. Jamaah dapat langsung melihat beranda, laporan kas, takmir, jadwal dan informasi TPA.
3. Pengelola klik **Login Pengelola**, masukkan email dan password Firebase.
4. Jika login berhasil tetapi tombol tambah tidak muncul, pastikan UID telah didaftarkan pada koleksi `pengelola` dengan `aktif=true` dan role yang benar.
5. Pada Kas, isi tanggal, jenis, kategori, nominal, keterangan dan (opsional) tautan bukti Drive.
6. Tombol **Unduh Excel (CSV)** menghasilkan file CSV yang dapat dibuka dengan Microsoft Excel/LibreOffice. Tombol **Cetak/PDF** membuka dialog cetak browser; pilih **Save as PDF**.
7. Tambahkan data takmir, jadwal ibadah, dan data TPA melalui tombol tambah saat login.

## Catatan dan batasan penting
- Jadwal sholat menggunakan API AlAdhan dengan pencarian kota Sawit, Indonesia dan metode 20. Perbedaan metode/perhitungan dapat terjadi. Takmir sebaiknya mencocokkan dengan jadwal resmi masjid/Kemenag sebelum dijadikan acuan jamaah.
- Tanggal Hijriah otomatis memakai kalender Umm al-Qura browser; dapat berbeda satu hari dari penetapan pemerintah.
- Jadwal otomatis membutuhkan internet. Bila API tidak merespons, jadwal tidak tampil.
- Data pada koleksi `tpa` dapat dibaca publik. Masukkan hanya data yang memang layak diumumkan (misalnya jumlah santri, nama pengajar dengan izin, dan tautan bahan ajar). Jangan masukkan identitas rinci atau data pribadi anak.
- File Drive harus dibagikan dengan izin yang benar agar jamaah dapat membukanya.
- Ekspor CSV berformat pemisah titik koma agar lebih mudah dibuka di Excel berlokal Indonesia; jika kolom tidak terpisah, gunakan menu Data > From Text/CSV.
- Security Rules membatasi tulis ke akun yang UID-nya terdaftar. Namun laporan kas memang publik sesuai rancangan transparansi.

## Troubleshooting singkat
- `ISI_API_KEY` atau layar kosong: konfigurasi Firebase belum diganti dengan konfigurasi proyek baru.
- `Missing or insufficient permissions`: periksa rules sudah dipublikasikan dan UID akun sudah terdaftar benar.
- Login gagal: pastikan Email/Password aktif dan akun dibuat di Authentication proyek masjid yang baru.
- GitHub Pages 404: periksa file berada di root, branch `main`, folder `/root`, dan tunggu beberapa menit.
- Jadwal sholat gagal: periksa koneksi internet atau layanan API; cocokkan jadwal dengan sumber lokal.
