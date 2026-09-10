# Development Log - Aetheris ID Website

Dokumen ini berisi catatan perubahan, keputusan teknis, dan hal penting selama pengembangan website Aetheris ID.

## Project Overview

- Website agensi VTuber Aetheris ID.
- Halaman utama saat ini berada di `index.html`.
- Data talent disimpan di `src/data-talent.json`.
- JavaScript sementara masih berada di bagian `<script>` dalam `index.html`.
- Styling menggunakan Tailwind CSS dan `src/style.css`.

## Data Talent

Setiap talent disimpan sebagai satu object di dalam array JSON. Property penting yang digunakan:

- `id`: identitas unik talent, contoh `talent-arvy`.
- `nama`: nama talent.
- `talent_card`: gambar card talent.
- `talent_image`: gambar detail talent.
- `desc`: deskripsi talent.
- `status`: status talent.
- `gen`: generasi talent.
- `fanbase`: nama fanbase.
- `oshi_mark`: mark talent.
- `youtube`, `instagram`, `tiktok`, `twitter`, `discord`: informasi sosial media.

Catatan:

- Semua `id` harus unik.
- Nama property JSON harus konsisten dengan property yang dipanggil JavaScript.
- `talents` berarti seluruh array, sedangkan `talent` berarti satu object.
- Data gambar menggunakan underscore, yaitu `talent_card` dan `talent_image`.

## Alur Data

```text
fetch data-talent.json
        |
        v
response.json()
        |
        v
Promise berisi array talents
        |
        v
await Promise
        |
        v
array talents
        |
        +--> map()   membuat card
        +--> find()  mencari talent untuk popup
        +--> filter() menyaring status atau generasi
```

File JSON cukup di-fetch sekali. Promise atau hasil array yang sama dapat digunakan kembali untuk render card, filter, dan detail popup.

## Catatan Async JavaScript

- `fetch()` mengembalikan Promise.
- `response.json()` juga mengembalikan Promise.
- Fungsi yang diberi `async` selalu mengembalikan Promise.
- `await` mengambil nilai di dalam Promise, tetapi tidak mengubah return value fungsi `async` menjadi nilai sinkron di luar fungsi.
- `returnPromise()` dapat mengembalikan Promise yang hasil akhirnya adalah array talent.
- Pemakai hasil tersebut harus menunggu dengan `await` atau memakai `.then()` sebelum menjalankan `map()`, `find()`, atau `filter()`.
- `resolve` dan `reject` manual tidak diperlukan karena `fetch()` sudah mengelola Promise-nya.

## Status Fitur Talent

- [x] Fetch data talent.
- [x] Render semua talent card dengan `map()`.
- [x] Kirim id talent dari card ke fungsi detail.
- [x] Cari talent terpilih dengan `find()`.
- [x] Isi modal berdasarkan object talent terpilih.
- [x] Tampilkan dan tutup modal talent.
- [x] Filter talent berdasarkan `status`.

## Status Fitur Team

- [x] Fetch data team dari `src/data-team.json`.
- [x] Render semua team card dengan `map()`.
- [x] Siapkan pengiriman id team untuk detail.
- [x] Siapkan modal detail team dan fungsi close.
- [x] Filter team berdasarkan `role`.

## Change Log

### 2026-09-08

- Menambahkan property `id` unik pada setiap talent di `src/data-talent.json`.
- Menyamakan nama property gambar menjadi `talent_card` dan `talent_image`.
- Menambahkan pemahaman bahwa data JSON lokal dapat diproses dengan pola yang sama seperti response API.
- Membuat fungsi pengambilan data secara terpisah dari fungsi render.
- Menghubungkan `fetch()` dengan `response.json()` melalui Promise.
- Memahami perbedaan antara Promise dan array hasil Promise.
- Menentukan bahwa data talent sebaiknya di-fetch sekali dan digunakan kembali.
- Menentukan rencana penggunaan `map()`, `find()`, dan `filter()` untuk fitur talent.

### 2026-09-09

- Menyelesaikan render card talent dari data JSON.
- Menyelesaikan pemilihan talent berdasarkan id dan pengisian modal detail.
- Menyelesaikan fungsi close modal talent.
- Menambahkan pola fetch, render, dan detail untuk team card dari `src/data-team.json`.
- Menyelesaikan pemisahan container card dan modal agar card tidak tertimpa saat detail dibuka.
- Menetapkan filter talent dan filter team sebagai pekerjaan berikutnya.

### 2026-09-10

- Menyelesaikan filter talent berdasarkan `status` dengan `filter()`.
- Menyelesaikan filter team berdasarkan `role` dengan `filter()`.
- Menambahkan render ulang hasil filter menggunakan `map()`.
- Menambahkan opsi `All` untuk mengembalikan seluruh team card.
- Menemukan dan mencatat typo `onclik` yang seharusnya `onclick` pada tombol filter team.

## Next Tasks

- Merapikan CSS modal dan card setelah logic filter selesai.

## Development Notes

- Gunakan Live Server atau dev server saat membaca file JSON dengan `fetch()`.
- Jangan melakukan fetch ulang untuk setiap card.
- Saat debugging, cek tipe nilai dengan `console.log()` dan bedakan apakah hasilnya `Promise`, `Array`, atau satu object talent.
- Kerjakan satu tahap alur terlebih dahulu: data selesai diambil, kemudian card dirender, lalu click card, dan terakhir popup.
