
Inertia
==========================

![inertia](https://cdn.deliciousbrains.com/content/uploads/2019/11/29120635/inertia-js-featured-img.jpg.webp)
_Source_: [deliciousbrains.com](https://)

Bagi para web programmer, pemilihan teknologi yang tepat dapat menjadi kunci keberhasilan suatu proyek. Terutama dengan banyaknya pilihan jenis teknologi, terkadang kita dituntut menggunakan teknologi terbaru, salah satunya teknologi _Single Page Application (SPA)._

_Single Page Application (SPA)_ menjadi butuh _effort_ lebih dalam penerapannya karena biasanya memiliki dua bagian arsitektur, _Backend_ dan _Frontend_. 

Laravel yang sudah mumpuni dibagian backend, sementara pada bagian _frontend_ harus dibantu teknologi berbasis javasript seperti, _Vuejs_ atau _Reactjs_.

Untuk `programmer fullstack developer`, yang mengerjakan _frontend_ dan _backend_ sekaligus sangat terbantu. Kita tidak lagi harus membuat coding terpisah dan tetap merasakan framework Laravel sebagai mesin utamanya.

Oleh karena itu lahirlah sebuah library baru untuk menjebatani kedua hal tersebut dalam satu buah framework, yang diberi nama [Inertiajs](https://inertiajs.com/).

- [Inertia](#inertia)
- [Apa itu Inertia.js?](#apa-itu-inertiajs)
- [_Inertia.js_ Bukan Sebuah Framework !](#inertiajs-bukan-sebuah-framework-)
- [Gambaran Umum Arsitektur Inertia.js](#gambaran-umum-arsitektur-inertiajs)
- [Contoh Implementasi Kode](#contoh-implementasi-kode)

# Apa itu Inertia.js?

Kalau kita baca keterangan di website resminya kurang lebih seperti berikut ini :

>Build single-page apps, `without building an API.`
Create `modern single-page React`, `Vue`, and` Svelte apps `using classic _server-side routing_. 

> Works with any backend — tuned for Laravel.

Sederhananya adalah sebuah pendekatan baru untuk membuat sebuah aplikasi web berbasis SPA atau _Single Page Application_ menggunakan `React`, `Vue` dan `Svelte` tanpa perlu membuat sebuah `Rest API`. Dan disini dia menyebutnya `The Modern Monolith`.

Inertia.js memungkinkan kita membuat aplikasi satu halaman (SPA) yang dirender sepenuhnya di sisi klien, tanpa banyak kerumitan yang umumnya saat mengembangkan SPA secara umum. Ini dilakukan dengan memanfaatkan framework di sisi server yang ada.

Inertia.js tidak memiliki sebuah routing kusus dan juga tidak membutuhkan sebuah Rest API, Kita cukup membuat controller dan route di dalam Laravel dan merendernya melalui Inertia.js

# _Inertia.js_ Bukan Sebuah Framework !

Inertia.js bukanlah sebuah framework dan juga bukan pengganti framework dari sisi server dan sisi client, melainkan Inertia.js berperan sebagai penghubung antara `framework sisi server (Laravel dan Rails)` dan `famework sisi client (React, Vue dan Svelte).`

# Gambaran Umum Arsitektur Inertia.js

**Arsitektur Backend (Laravel)**

1. `Routes:` Mendefinisikan rute-rute di r`outes/web.php.`
2. `Controllers:` Menggunakan pengontrol Laravel untuk menangani logika bisnis dan mengembalikan tampilan menggunakan Inertia.js.
3. `Middleware`: Mengelola middleware untuk autentikasi, otorisasi, dll.
4. `Views:` Menggunakan Inertia untuk merender komponen frontend.

**Arsitektur Frontend (Vue.js)**

1. `Pages:` Menggunakan komponen Vue sebagai halaman utama.
2. `Components:` Membuat komponen Vue yang digunakan di dalam halaman.
3. `Inertia Link`: Menggunakan Inertia Link untuk navigasi tanpa reload.
4. `API Calls:` Jika diperlukan, memanggil API untuk data tambahan.

**Ilustrasi arsitektur Inertia.js dapat digambarkan sebagai berikut:**

![inertia](https://i.imgur.com/UIXRLYa.png)


# Contoh Implementasi Kode

```php
// Backend (Laravel)
use Inertia\Inertia;

class PostController extends Controller
{
    public function index()
    {
        // Mengambil data dari database
        $posts = Post::all();

        // Mengirimkan data ke frontend menggunakan Inertia
        return Inertia::render('Posts/Index', [
            'posts' => $posts
        ]);
    }
}
```


```javascript
// Frontend (Vue.js)
<template>
    <div>
        <h1>Posts</h1>
        <ul>
            <li v-for="post in posts" :key="post.id">
                {{ post.title }}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        posts: Array
    }
}
</script>
```

```html
<!--Inertia Link:

komponen InertiaLink untuk melakukan navigasi antara halaman tanpa harus memuat ulang seluruh halaman. InertiaLink bekerja serupa dengan tag anchor (<a>), namun tanpa melakukan refresh halaman. -->

<inertia-link href="/posts/1">Lihat Post</inertia-link>
```

>Di _case study_ nanti kita bersama-sama akan belajar mengimplementasikan Inertia.js di dalam project Laravel dan nanti kita akan menggunakan Vue.js 3 untuk kita jadikan sebagai framework sisi client-nya. 