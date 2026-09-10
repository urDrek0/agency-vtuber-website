# Aetheris ID Website

Website agensi VTuber Aetheris ID.

## Struktur Utama

- `index.html`: halaman utama dan logic JavaScript sementara.
- `src/data-talent.json`: data seluruh talent.
- `src/style.css`: styling tambahan.
- `src/output.css`: hasil build Tailwind CSS.
- `DEVELOPMENT_LOG.md`: catatan teknis dan proses pengembangan yang lebih detail.

## Menjalankan Project

Install dependency:

```bash
npm install
```

Jalankan Tailwind watcher:

```bash
npm run dev
```

Gunakan Live Server atau development server saat membuka website agar `fetch()` dapat membaca file JSON lokal.

## Alur Data Talent Dan Team

```text
fetch data-talent.json
        |
        v
response.json()
        |
        v
await Promise
        |
        v
array talents
        |
        +--> map()   membuat semua card
        +--> find()  mencari satu talent untuk modal
        +--> filter() menyaring status atau generasi
```

Data talent dan team diambil sebagai JSON array. Setiap object memiliki `id` unik, informasi profil, gambar card, dan detail masing-masing.

## Logic Card Dan Modal

1. Fetch data talent.
2. Tunggu Promise sampai menjadi array.
3. Gunakan `map()` untuk membuat semua card.
4. Kirim `talent.id` saat card diklik.
5. Gunakan `find()` untuk mendapatkan satu object talent berdasarkan id.
6. Isi modal menggunakan object talent terpilih.
7. Tombol close menyembunyikan modal tanpa menghapus card.

Alur team menggunakan pola yang sama:

1. Fetch `src/data-team.json`.
2. Tunggu Promise sampai menjadi array team.
3. Gunakan `map()` untuk membuat team card.
4. Kirim `team.id` saat team card diklik.
5. Gunakan `find()` untuk mendapatkan satu object team.
6. Isi modal team menggunakan object team terpilih.
7. Tombol close menyembunyikan modal team.

Card dan modal harus berada di container yang berbeda. Card tidak boleh ditimpa oleh isi modal.

## Data JSON

Property penting:

- `id`: id unik, contoh `talent-arvy`.
- `nama`: nama talent.
- `talent_card`: gambar card.
- `talent_image`: gambar detail.
- `desc`: deskripsi.
- `status`, `gen`, `fanbase`, `oshi_mark`: informasi profil.
- `youtube`, `instagram`, `tiktok`, `twitter`, `discord`: link sosial media.

## Changelog

### 2026-09-09

- Menyelesaikan alur card talent ke modal detail.
- Menambahkan pengiriman id talent dari card ke fungsi detail.
- Menggunakan `find()` untuk memilih satu talent berdasarkan id.
- Menggunakan object hasil pencarian untuk menampilkan nama, deskripsi, statistik, gambar, dan link sosial media.
- Menambahkan fungsi close untuk menyembunyikan modal.
- Memisahkan konsep container card dan container modal agar card tetap tersedia setelah modal ditutup.
- Menambahkan alur fetch dan render team card dari `src/data-team.json`.
- Menambahkan pola pemilihan team berdasarkan `team.id` untuk detail modal team.
- Menyelesaikan fungsi close pada modal talent dan team.
- Menetapkan filter talent dan team sebagai pekerjaan berikutnya.
- Merapikan dokumentasi project dan mencatat tanggal perubahan.

### 2026-09-10

- Menyelesaikan filter talent berdasarkan `status` menggunakan `filter()`.
- Menyelesaikan filter team berdasarkan `role` menggunakan `filter()`.
- Menambahkan opsi team `All` untuk menampilkan seluruh team card.
- Memastikan hasil filter dirender kembali menggunakan `map()`.
- Menemukan typo atribut event `onclik` yang seharusnya `onclick`, sehingga tombol filter team sebelumnya tidak menjalankan fungsi.

### 2026-09-08

- Menambahkan id unik pada setiap talent di `src/data-talent.json`.
- Menyamakan property gambar menjadi `talent_card` dan `talent_image`.
- Memisahkan proses fetch data dari proses render card.
- Memahami perbedaan Promise, array hasil Promise, dan satu object talent.
- Menetapkan penggunaan `map()`, `find()`, dan `filter()` sesuai kebutuhan masing-masing.

## Catatan Pengembangan

- `fetch()` dan `response.json()` menghasilkan Promise.
- Fungsi `async` selalu mengembalikan Promise, meskipun nilai akhirnya berupa array.
- Jalankan `map()`, `find()`, atau `filter()` setelah Promise selesai ditunggu.
- Jangan fetch ulang setiap card diklik jika data yang sama sudah tersedia.
- Filter talent tersedia untuk `Active` dan `Alum`.
- Filter team tersedia untuk `All`, `Creative`, `Production`, dan `Management`.
