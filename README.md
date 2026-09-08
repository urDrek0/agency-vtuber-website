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

## Rencana Fitur Talent

1. Fetch data talent satu kali.
2. Render semua card dari array talent menggunakan `map()`.
3. Simpan id talent pada setiap card.
4. Saat card diklik, ambil id card tersebut.
5. Cari satu object talent dengan `find()` berdasarkan id.
6. Isi satu modal popup menggunakan data talent terpilih.
7. Tampilkan modal.
8. Sediakan tombol close untuk menyembunyikan modal.
9. Tambahkan filter berdasarkan `status` dan `gen`.

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

## Known Issues

- Render card masih perlu memastikan hasil Promise sudah ditunggu sebelum `map()` dijalankan.
- Event click card belum menerima atau membaca id talent yang diklik.
- `talentShow()` belum terhubung dengan object talent hasil `find()`.
- Modal popup belum memiliki satu container stabil yang diisi ulang berdasarkan talent terpilih.
- Beberapa nama variabel lama masih perlu diseragamkan agar tidak mencampur nama seperti Promise, response, dan array.
- Tombol filter belum memiliki logic filter.

## Development Notes

- Gunakan Live Server atau dev server saat membaca file JSON dengan `fetch()`.
- Jangan melakukan fetch ulang untuk setiap card.
- Saat debugging, cek tipe nilai dengan `console.log()` dan bedakan apakah hasilnya `Promise`, `Array`, atau satu object talent.
- Kerjakan satu tahap alur terlebih dahulu: data selesai diambil, kemudian card dirender, lalu click card, dan terakhir popup.
