Pengennalan Rest API
==========================
API _(Application Programming Interface)_ adalah sekumpulan aturan dan protokol yang memungkinkan satu aplikasi untuk berkomunikasi dengan aplikasi lainnya. API dapat digunakan untuk berbagai tujuan, termasuk mengakses data, mengintegrasikan layanan, dan mengotomatisasi proses.
- [Pengennalan Rest API](#pengennalan-rest-api)
  - [Rest API Cheatsheet](#rest-api-cheatsheet)
- [Apa itu REST?](#apa-itu-rest)
  - [Karakteristik REST](#karakteristik-rest)
- [Metode HTTP dalam REST](#metode-http-dalam-rest)
- [Contoh REST API](#contoh-rest-api)
- [Format Data](#format-data)
- [Keuntungan Menggunakan REST API](#keuntungan-menggunakan-rest-api)
- [Kesimpulan](#kesimpulan)

## Rest API Cheatsheet

![rest-api](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff870b253-d5f5-43bf-a6ab-667ee8ed6f8b_1280x1664.gif)
_Source_: [ByteByteGo](https://blog.bytebytego.com/p/ep94-rest-api-cheatsheet?utm_source=publication-search)

# Apa itu REST?

REST (Representational State Transfer) adalah arsitektur yang digunakan untuk membangun layanan web. REST menggunakan prinsip-prinsip yang sederhana dan berbasis HTTP untuk memungkinkan komunikasi antara klien dan server.

## Karakteristik REST

1. Stateless: Setiap permintaan dari klien ke server harus berisi semua informasi yang diperlukan untuk memahami permintaan. Server tidak menyimpan status klien antara permintaan.
   
2. Client-Server Architecture: REST memisahkan antarmuka pengguna (klien) dari penyimpanan data (server), memungkinkan pengembangan dan pengelolaan yang lebih baik.
   
3. Uniform Interface: REST menggunakan interface yang seragam untuk berinteraksi dengan sumber daya, biasanya melalui URL.

4. Resource-Based: Dalam REST, semua hal dianggap sebagai sumber daya yang dapat diakses melalui URL. Setiap sumber daya memiliki representasi yang dapat berupa JSON, XML, atau format lainnya.

5. Cacheable: Respons dari server dapat disimpan dalam cache untuk meningkatkan performa dan mengurangi latensi.

# Metode HTTP dalam REST

REST API menggunakan metode HTTP untuk melakukan operasi pada sumber daya. Berikut adalah metode yang umum digunakan:

1. **GET**: Mengambil data dari server.
2. **POST**: Mengirim data baru ke server untuk membuat sumber daya baru.
3. **PUT**: Memperbarui sumber daya yang ada di server.
4. **DELETE**: Menghapus sumber daya dari server.

# Contoh REST API

Misalkan kita memiliki _API_ untuk mengelola `buku`. Berikut adalah contoh `endpoint` dan metode yang mungkin digunakan:

1. **GET /api/books:** Mengambil daftar semua buku.
2. **GET /api/books/{id}**: Mengambil detail buku berdasarkan ID.
3. **POST /api/books:** Menambahkan buku baru.
4. **PUT /api/books/{id}**: Memperbarui informasi buku berdasarkan ID.
5. **DELETE /api/books/{id}**: Menghapus buku berdasarkan ID.

# Format Data

REST API biasanya menggunakan format _JSON_ (_JavaScript Object Notation_) untuk pertukaran data karena mudah dibaca dan ditulis oleh manusia serta mudah diproses oleh mesin.

# Keuntungan Menggunakan REST API

- S**ederhana dan Mudah Dipahami**: REST API mudah digunakan dan diimplementasikan.
- **Interoperabilitas:** REST API dapat digunakan oleh berbagai platform dan bahasa pemrograman.
- **Scalability:** REST API dapat dengan mudah diskalakan untuk menangani jumlah pengguna yang besar.

# Kesimpulan

REST API adalah cara yang kuat dan fleksibel untuk mengembangkan aplikasi yang dapat berkomunikasi dengan layanan lain. Dengan memahami prinsip-prinsip dasar REST, Anda dapat mulai membangun dan mengintegrasikan layanan web dengan lebih efektif.

