
 
![breze](https://github.com/laravel/breeze/raw/2.x/art/logo.svg)
 
_Source_: [laravel/breeze](https://github.com/laravel/breeze)

- [Laravel Breeze](#laravel-breeze)
- [Installation](#installation)
 

# Laravel Breeze

 Menurut dokumentasinya dari [https://laravel.com/docs/11.x/starter-kits](https://laravel.com/docs/11.x/starter-kits)
 
 **Laravel Breeze** adalah _simple implementation_ dari semua [fitur otentikasi](https://laravel.com/docs/11.x/authentication) Laravel, termasuk `login, registrasi, pengaturan ulang kata sandi, verifikasi email, dan konfirmasi kata sandi. `
 
 Selain itu, Breeze juga menyertakan halaman `profil sederhana `di mana pengguna dapat memperbarui `nama`, `alamat email`, dan `kata sandi` mereka.

 ![breeze](https://i.imgur.com/XUiTZ8m.png)

Lapisan tampilan default Laravel Breeze terdiri dari _template_ Blade sederhana yang ditata dengan _Tailwind CSS._

Selain itu, Breeze menyediakan opsi _scaffolding_ berdasarkan _Livewire_ atau _Inertia_, dengan pilihan untuk menggunakan _Vue_ atau _React_ untuk _scaffolding_ Inertia-based.

Breeze sebagai solusi alternatif bagi temen-temen untuk membuat `autentikasi` tanpa susah payah membangunnya dari awal. 

# Installation

Jika Anda telah membuat aplikasi Laravel baru tanpa starter kit, Anda dapat menginstal Laravel Breeze secara manual menggunakan Composer:

dengan perintah berikut:

```bash
composer require laravel/breeze --dev
```

>Di _case study_ nanti kita bersama-sama akan belajar mengimplementasikan Laravel Breeze di project Laravel dan nanti kita akan menggunakan Vue.js 3 untuk kita jadikan sebagai framework sisi client-nya. 