Pengenalan Laravel
=================
![Laraver](https://i.imgur.com/XrJNZn3.png)

- [Pengenalan Laravel](#pengenalan-laravel)
- [Versi Laravel](#versi-laravel)
  - [PHP 8.2](#php-82)
  - [Kerangka Aplikasi yang lebih Sederhana](#kerangka-aplikasi-yang-lebih-sederhana)
  - [Tampilan Welcome Screen Baru](#tampilan-welcome-screen-baru)
  - [Memahami Struktur Folder Laravel](#memahami-struktur-folder-laravel)
    - [Folder Controller dan model](#folder-controller-dan-model)
    - [View](#view)
    - [Apa sih itu routes dan migration?](#apa-sih-itu-routes-dan-migration)
    - [Latihan: Menampilkan Kode Routes](#latihan-menampilkan-kode-routes)
- [Kesimpulan](#kesimpulan)

**Laravel** adalah sebuah framework PHP yang open source dan dirancang untuk memudahkan pengembangan aplikasi web. Framework ini mengikuti pola arsitektur Model-View-Controller (MVC) dan menyediakan berbagai fitur serta alat yang berguna untuk mempercepat proses pengembangan web.

**Laravel** sangat populer di kalangan pengembang web karena dokumentasinya yang lengkap, komunitas yang aktif, dan ekosistem paket yang kaya yang memungkinkan pengembang untuk memperluas fungsi dasar framework sesuai kebutuhan.

**Framework**: adalah kumpulan program berupa file pustaka (libraries) atau class-class yang mendukung dalam pengembangan aplikasi secara terstruktur dan independen terhadap aplikasi. Software Framework adalah sebuah desain yang bisa digunakan berulang-ulang (re-usable design) untuk sebuah sistem atau sub sistem piranti lunak.

Manfaat framework:

1. Mempercepat proses pembuatan aplikasi baik itu aplikasi berbasis desktop, mobile ataupun web.
2. Membantu para developer dalam perencanaan, pembuatan dan pemeliharaan sebuah aplikasi.
3. Aplikasi yang dihasilkan menjadi lebih stabil dan handal, hal ini dikarenakan Framework sudah melalui proses uji baik itu stabilitas dan juga keandalannya.
4. Memudahkan para developer dalam membaca code program dan lebih mudah dalam mencari bugs.
5. Memiliki tingkat keamanan yang lebih, hal ini dikarenakan Framework telah mengantisipasi cela – cela keamanan yang mungkin timbul.
6. Mempermudah developer dalam mendokumentasikan aplikasi-aplikasi yang sedang dibangun.
7. Memudahkan developer dalam melakukan perubahan dan pengembangan aplikasi yang sudah ada

Arsitektur Laravel didasarkan pada pola arsitektur **Model-View-Controller (MVC)** yang memisahkan logika aplikasi menjadi tiga komponen utama: `Model`, `View`, dan `Controller`.

![laravelMVC](https://i.imgur.com/Csn9gBM.png)
***Source***: _luckymedia.dev_

1. **Model** bertanggung jawab untuk menangani data dan logika bisnis aplikasi. Dalam Laravel, model biasanya berinteraksi dengan database menggunakan Eloquent ORM. Model mewakili tabel dalam database dan menyediakan cara yang elegan untuk berinteraksi dengan data.

2. **View** adalah komponen yang bertanggung jawab untuk menyajikan data kepada pengguna. Dalam Laravel, tampilan dibuat menggunakan Blade template engine. Blade memungkinkan penggunaan logika dalam template dengan sintak yang bersih dan mudah dipahami.

3. **Controller** bertindak sebagai perantara antara Model dan View. Controller menerima input dari pengguna melalui HTTP request, memprosesnya (misalnya, mengambil data dari model), dan kemudian mengembalikan respon yang sesuai, biasanya berupa tampilan.

Arsitektur Laravel yang berbasis MVC memisahkan tanggung jawab di antara berbagai komponen, membuat aplikasi lebih terstruktur, mudah dipahami, dan mudah dikembangkan.

Dapat disimpulkan bahwa data dari `model (database)` hanya dapat berinteraksi dengan c`ontroller`. `Controller` kemudian berinteraksi dengan `view`, membawa data dari` model/database`, dan `view `menampilkannya ke `web`.

# Versi Laravel

Laravel memiliki beberapa versi yang telah dirilis, dengan setiap versi memiliki fitur-fitur baru dan perbaikan. Berikut adalah beberapa versi Laravel yang paling populer:

Jika kita lihat pada [kebijakan dukungan](https://laravel.com/docs/11.x/releases#support-policy) Laravel, maka Laravel 11 dirilis pada Q1 tahun 2024, yaitu tepatnya di bulan Maret 2024.

| Version | PHP (*) | Release | Bug Fixes Until | Security Fixes Until |
|---------|---------|---------|-----------------|----------------------|
| 9       | 8.0 - 8.2 | February 8th, 2022 | August 8th, 2023 | February 6th, 2024 |
| 10      | 8.1 - 8.3 | February 14th, 2023 | August 6th, 2024 | February 4th, 2025 |
| 11      | 8.2 - 8.3 | March 12th, 2024 | September 3rd, 2025 | March 12th, 2026 |
| 12      | 8.2 - 8.3 | Q1 2025 | Q3, 2026 | Q1, 2027 |

Kemudian apa saja fitur-fitur dan perubahan yang terjadi di **Laravel 11** ? mari kita akan membahasnya satu-persatu.

Laravel 11 melanjutkan perbaikan yang dilakukan di Laravel 10.x dengan memperkenalkan struktur aplikasi yang disederhanakan, *per-second rate limiting, health routing, graceful encryption key rotation, queue testing improvements, [Resend]([https://](https://resend.com/)) mail transport, Prompt validator integration, new Artisan commands*, dan masih banyak lagi.

Selain itu, Laravel juga merilis sebuah library yang bernama **Laravel Reverb**. Library ini merupakan server _WebSocket_ yang memberikan kemampuan _real-time_ yang baik pada aplikasi yang kita buat.

## PHP 8.2

Untuk PHP dengan versi `8.1` kini sudah tidak akan bisa digunakan lagi di Laravel 11. Untuk teman-teman yang akan memulai membuat project baru di Laravel 11, maka harus menggunakan PHP versi `8.2` atau `8.3`.

Berikut ini informasi terkait dukungan PHP 8.1 yang dihentikan di Laravel 11.

[11.x Drop PHP 8.1 support](https://github.com/laravel/framework/pull/45526)

## Kerangka Aplikasi yang lebih Sederhana

Laravel 11 akan hadir dengan struktur folder yang disederhanakan. Jadi sekarang saat teman-teman ketika membuat project baru di Laravel 11, maka di dalam folder app hanya terdapat 3 folder saja, yaitu `Http`, `Models` dan `Providers`, kurang lebih seperti ini.

```graphql
app
├── Http
│   └── Controllers
│       └── Controller.php
├── Models
│   └── User.php
└── Providers
    └── AppServiceProvider.php
```
Struktur aplikasi baru ini dimaksudkan untuk memberikan pengalaman yang lebih ramping dan _modern_, dengan tetap mempertahankan banyak konsep yang sudah familiar bagi pengembang Laravel sebelumnya.

> INFORMASI : jika sebelumnya teman-teman menggunakan Laravel versi 10 ke bawah dan ingin upgrade ke Laravel 11, maka kita tidak harus mengadopsi struktur project baru ini. Artinya, struktur project Laravel lama masih tetap bisa digunakan.

## Tampilan Welcome Screen Baru

Di Laravel 11 untuk tampilkan welcome screen kini telah diubah, kurang lebih seperti berikut ini.

![Laravel 11](https://i.imgur.com/AYKHvuz.png)

## Memahami Struktur Folder Laravel

Laravel 11 memiliki struktur folder yang lebih sederhana, seperti yang telah disebut kan sebelumnya. Berikut ini adalah struktur folder Laravel 11. 

### Folder Controller dan model

`Controller` berada di folder `App/Http/Controller`. Nanti kode logikanya mau ke mana akan ada di sini.

`Model` berada di `App/model` Nanti database nya berada di sini.

### View

`View` berada di folder `resources/views`. Nanti nya di sini kita akan koding mengkoding untuk tampilan nya.

![struktur-folder](https://i.imgur.com/usUAVw0.png)


Nah ada yang belom kita bahas di awal yaitu `routes` dan `migration`.

perhatikan gambar di atas!

### Apa sih itu routes dan migration?

`Routes` adalah penjembatani antara `controller` dan `view`

Dan `migration` adalah kode yang akan kita gunakan untuk membuat tabel di `database`.

Di dalam di folder `routes/`, terdapat dua file:

- `console.php` adalah `route` untuk menjalankan `controller` dari `CMD` dan
- `web.php` adalah `route` untuk menjalankan `controller` yang diakses dari `web` atau `HTTP`.

Folder migration berada di folder `database/migration.`

Nah di sini sudah ada `migration` yang di buatkan oleh laravel nya langsung.


![laravel](https://www.petanikode.com/img/laravel-11/mvc-route.avif)

seperti sebelumnya kita belum membahas routes, yaitu fungsi nya untuk menjembatani antara controller dan view atau program nya akan ke arah mana, apakah ke view dulu atau ke controller.

### Latihan: Menampilkan Kode Routes

Coba buka file `routes/web.php`, di sini akan terdapat definisi routes dari aplikasi kita. Semua routes dari aplikasi wajib ditulis di ini.

Di setiap route terdapat parameter `URI `yang menyatakan alamat URL dan `action` adalah fungsi/`controller` untuk di jalankan saat alamat tersebut dibuka.

Sebagai contoh:

Di sini terdapat route menuju URL `/ `(root) atau home page. 

Lalu di sana terdapat fungsi yang akan dijalankan saat route` /` dibuka. 

Fungsi tersebut akan membuka file view dari `resource/view/welcome.blade.php.`

```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('welcome');
});
```
Oke, sekarang mari kita latihan!

Buatlah folder dengan nama `latihan` dan buat file `hello.blade.php` di dalamnya.

Setiap membuat file di view harus menggunakan `blade.php` agar bisa terbaca oleh laravel nya dikarenakan laravel menggunakan bahasa _blade_ untuk _template engine_.

Dan masukan kode ini ke dalam `hello.blade.php`. 

Disini kita coba membuat kan hello untuk test saja apakah nampil apa tidak.

```html
<h1>Hello Word</h1>
```
Kemudian di `routes` nya kita akan membuat `routes/web.php` nya dengan memberikan *url* nya **latihan**.

```php
Route::get('/latihan', function () {
  return view('latihan.hello');
});
```
Sekarang jalankan perintah berikut ini di dalam terminal/CMD untuk menjalankan _server-nya._

```bash
php artisan serve
```

Oke kembali lagi ke web nya tetapi di` url-nya` di tambahkan `/latihan`.

```bash
http://127.0.0.1:8000/latihan
```

Jika berhasil maka akan terlihat tulisan `Hello Word` di dalam webnya.


Di sini kita sudah bisa menampilkannya dan `view` ini kita sudah bisa membuat web statis loh tanpa menggunakan `controller` dan `model (database).`


# Kesimpulan

Kita dapat menyimpulkan bahwa Laravel 11 memiliki beberapa perubahan dan fitur baru yang memudahkan pengembangan aplikasi web. Struktur folder yang lebih sederhana, tampilan welcome screen baru, dan penjelasan tentang routes dan migration membuat pengembangan aplikasi menjadi lebih mudah dipahami. Selain itu, kita juga dapat membuat aplikasi web statis dengan menggunakan view tanpa harus menggunakan controller dan model (database). Dengan demikian, Laravel 11 dapat menjadi pilihan yang tepat bagi pengembang web yang ingin membuat aplikasi web yang lebih cepat dan mudah dipahami. 