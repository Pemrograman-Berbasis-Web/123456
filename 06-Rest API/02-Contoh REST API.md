# Contoh REST API

Berikut adalah gambaran contoh REST API untuk mengelola buku. 

API ini menyediakan berbagai _endpoint_ untuk melakukan operasi dasar seperti mengambil, menambahkan, memperbarui, dan menghapus buku.

- [Contoh REST API](#contoh-rest-api)
- [Diagram Arsitektur REST API untuk Mengelola Buku](#diagram-arsitektur-rest-api-untuk-mengelola-buku)
- [Penjelasan Diagram](#penjelasan-diagram)
- [Implementasi REST API untuk Mengelola Buku](#implementasi-rest-api-untuk-mengelola-buku)

# Diagram Arsitektur REST API untuk Mengelola Buku

```graphql
+------------------+
|     Klien        |
| (Aplikasi Web,   |
|  Mobile, dll.)   |
+------------------+
         |
         | HTTP Requests
         |
         v
+------------------+
|   REST API       |
|  (Server)        |
|                  |
| +--------------+ |
| |  Endpoint    | |
| |  /api/books  | |
| +--------------+ |
|       |          |
|       |          |
|       v          |
| +--------------+  |
| | GET         |  |  <-- Mengambil daftar semua buku
| +--------------+  |
| +--------------+  |
| | GET /{id}   |  |  <-- Mengambil detail buku berdasarkan ID
| +--------------+  |
| +--------------+  |
| | POST        |  |  <-- Menambahkan buku baru
| +--------------+  |
| +--------------+  |
| | PUT /{id}   |  |  <-- Memperbarui buku berdasarkan ID
| +--------------+  |
| +--------------+  |
| | DELETE /{id}|  |  <-- Menghapus buku berdasarkan ID
| +--------------+  |
|                  |
+------------------+
         |
         | Data Storage
         |
         v
+------------------+
|   Database       |
|  (Menyimpan data |
|   buku)          |
+------------------++------------------+
|     Klien        |
| (Aplikasi Web,   |
|  Mobile, dll.)   |
+------------------+
         |
         | HTTP Requests
         |
         v
+------------------+
|   REST API       |
|  (Server)        |
|                  |
| +--------------+ |
| |  Endpoint    | |
| |  /api/books  | |
| +--------------+ |
|       |          |
|       |          |
|       v          |
| +--------------+  |
| | GET         |  |  <-- Mengambil daftar semua buku
| +--------------+  |
| +--------------+  |
| | GET /{id}   |  |  <-- Mengambil detail buku berdasarkan ID
| +--------------+  |
| +--------------+  |
| | POST        |  |  <-- Menambahkan buku baru
| +--------------+  |
| +--------------+  |
| | PUT /{id}   |  |  <-- Memperbarui buku berdasarkan ID
| +--------------+  |
| +--------------+  |
| | DELETE /{id}|  |  <-- Menghapus buku berdasarkan ID
| +--------------+  |
|                  |
+------------------+
         |
         | Data Storage
         |
         v
+------------------+
|   Database       |
|  (Menyimpan data |
|   buku)          |
+------------------+
```
![DiagramResApBook](https://gcdnb.pbrd.co/images/K0QYg56TuHKh.png?o=1)

# Penjelasan Diagram
Diagram ini menunjukkan arsitektur dasar dari aplikasi buku yang menggunakan **REST API**.

1. **Klien**: Ini bisa berupa aplikasi web, aplikasi mobile, atau alat lain yang ingin berinteraksi dengan REST API untuk mengelola buku.
   
2. **REST API (Server)**: Ini adalah server yang menangani permintaan dari klien. REST API memiliki beberapa endpoint yang dapat diakses untuk melakukan operasi CRUD (Create, Read, Update, Delete) pada buku.

3. **Endpoint**

- `GET /api/books`: Mengambil daftar semua buku.
- `GET /api/books/{id}`: Mengambil detail buku berdasarkan ID.
- `POST /api/books:` Menambahkan buku baru.
- `PUT /api/books/{id}`: Memperbarui informasi buku berdasarkan ID.
- `DELETE /api/books/{id}`: Menghapus buku berdasarkan ID.

4. **Database**
   
   Tempat penyimpanan data buku. Data buku yang diambil atau dimodifikasi melalui REST API akan disimpan di sini.

# Implementasi REST API untuk Mengelola Buku

1. **Mengambil Daftar Semua Buku**
- **Endpoint**: `GET /api/books`
- **Deskripsi**: Mengambil daftar semua buku yang tersedia.

```json
[
    {
        "id": 1,
        "title": "Belajar REST API",
        "author": "John Doe",
        "publishedYear": 2021
    },
    {
        "id": 2,
        "title": "Pengembangan Web Modern",
        "author": "Jane Smith",
        "publishedYear": 2022
    }
]
```
2. Mengambil Detail Buku Berdasarkan  ID
   - **Endpoint** :  `GET /api/books/{id}`
   - **Deskripsi**: Mengambil  detail buku berdasarkan _ID_ yang diberikan.

```json
{
    "id": 1,
    "title": "Belajar REST API",
    "author": "John Doe",
    "publishedYear": 2024
}
```

3. Menambah Buku Baru
   
   - **Endpoint** :  `POST /api/books`
   - **Deskripsi**: Menambahkan Buku baru ke dalam daftar
   - **Contoh Permintaan**

```json
{
    "title": "Pemrograman Berbasis Web",
    "author": "Yanyan Sofiyan",
    "publishedYear": 2024
}
```
*Contoh Respons* (Buku yang baru di tambahkan)

1. **Memperbarui Informasi Buku Berdasarkan ID**
- **Endpoint** :  `PUT /api/books/{id}`
- *Deskripsi*: Memperbaharui  informasi buku berdasarkan ID yang diberikan.
- **Contoh Permintaan**

```json
{
    "title": "Belajar REST API - Edisi Kedua",
    "author": "John Doe",
    "publishedYear": 2024
}
```
-  *Contoh Respons* (Buku yang telah diperbarui)
  
 ```json
  {
    "id": 1,
    "title": "Belajar REST API - Edisi Kedua",
    "author": "John Doe",
    "publishedYear": 2022
}
 ```
5. **Menghapus Buku Berdasarkan ID**
   
- **Endpoint:** `DELETE /api/books/{id}`
- **Deskripsi**: Menghapus buku dari daftar berdasarkan ID.
- Contoh Respons
  
```json
{
    "message": "Buku dengan ID 1 telah dihapus."
}
```

Contoh di atas menggambarkan bagaimana REST API dapat digunakan untuk mengelola buku dengan berbagai operasi dasar. 

Setiap operasi dilakukan melalui `_endpoint_` yang berbeda, menggunakan metode HTTP yang sesuai. 

Data yang dikirim dan diterima biasanya dalam format `JSON`, yang mudah dibaca dan diproses.