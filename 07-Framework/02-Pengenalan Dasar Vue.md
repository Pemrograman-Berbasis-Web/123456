Pengenalan Dasar Vue JS
==========================
![vue](https://wallpapercave.com/wp/wp6878944.jpg)

- [Pengenalan Dasar Vue JS](#pengenalan-dasar-vue-js)
- [Apa itu Vue.js ?](#apa-itu-vuejs-)
- [Apa itu Vite ?](#apa-itu-vite-)
- [Requirement](#requirement)
- [Installasi Node.js](#installasi-nodejs)
- [Membuat Project Vue 3 dengan Vite](#membuat-project-vue-3-dengan-vite)
- [Menjalankan Project Vue 3 dengan Vite](#menjalankan-project-vue-3-dengan-vite)
- [Kesimpulan](#kesimpulan)


# Apa itu Vue.js ?

[Vue.js](https://vuejs.org/) merupakan framework JavaScript yang digunakan untuk membangun User Interface (UI) yang menerapkan konsep component (component based). Vue.js bersifat open source dan menjadi salah ssatu framework JavaScript yang populer karena kemudahannya dalam dipelajari untuk pemula. Vue.js juga memiliki ekosistem yang luas dan banyak komunitas yang mendukungnya.

Untuk dokumentasi lengkapnya anda bisa mengakses link berikut:
[guide/introduction](https://vuejs.org/guide/introduction.html)

# Apa itu Vite ?
> Vite the next generation frontend tooling. It's fast!

[Vite](https://vitejs.dev/) merupakan tooling yang digunakan untuk membangun aplikasi frontend dengan cepat. 

Vite dapat meningkatkan produktivitas pengembang dengan menggunakan teknologi seperti [ES modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) dan [Hot Module Replacement (HMR)](https://webpack.js.org/guides/hot-module-replacement/). 

Vite juga dapat meningkatkan kinerja aplik asi dengan menggunakan teknologi seperti [Code Splitting](https://webpack.js.org/guides/code-splitting/) dan [Tree Shaking](https://webpack.js.org/guides/tree-shaking/).

Vite dan Vue.js dibuat oleh orang yang sama, yaitu [Evan You](https://x.com/youyuxi). Beliau dahulunya merupakan mantan software engineer di _Google_ dan [Meteor.js](https://www.meteor.com/). Dan sekarang sudah fokus mengembangkan ekosistem **Vue.js** secara open source.

Sebelum Vite, dulu untuk membuat project baru di Vue.js menggunakan build tool yang bernama Vue CLI, build tool ini juga dikembangkan secara official oleh Vue.js core tim, cuman karena performa compiling-nya kurang bagus, maka kita direkomendasikan beralih menggunakan Vite. 

Tapi jangan kawatir, karena Vue CLI juga masih bisa digunakan dan terus dimaintenance.

# Requirement

Hal pertama yang harus Anda lakukan sebelum belajar Vue.js adalah menginstal [Node.js](https://nodejs.org/en/download/prebuilt-installer). Node.js adalah runtime *environment* JavaScript yang memungkinkan Anda untuk menjalankan kode JavaScript di luar browser. 

Extension tambahan:

- [vscode-extension-Vue](https://marketplace.visualstudio.com/items?itemName=Vue.volar) berguna untuk memudahkan Anda dalam mengembangkan aplikasi Vue.js. 

# Installasi Node.js

Untuk menginstal Node.js, Anda dapat mengikuti langkah-langkah berikut: 
- Silahkan unduh melalui situs resminya:
[NodeJS](https://nodejs.org/en/download/prebuilt-installer)
- Setelah itu, Anda dapat menginstal Node.js dengan cara mengikuti instruksi yang muncul di layar.
- Untuk memastikan apakah Node.js sudah berhasil terinstall, silahkan teman-teman jalankan perintah berikut ini di dalam *terminal/CMD.*

```bash
node --version
```

```bash
npm --version
```
![node/npm](https://i.imgur.com/yNKDy35.png)

Jika Anda melihat versi Node.js dan npm, maka Anda sudah berhasil menginstal Node

# Membuat Project Vue 3 dengan Vite

Silahkan teman-teman masuk ke dalam folder dimana akan menyimpan project-nya, kemudian jalankan perintah berikut ini di dalam `terminal/CMD.` atau di _visual studio code_.

```bash
npm create vite@4.2.0 vue3-crud -- --template vue
```
Jika perintah di atas berhasil dijalankan, maka kita akan membuat project Vue 3 dengan nama `vue3-crud`.

![vueinstall](https://i.imgur.com/7k7v722.png)

![vue-direk](https://i.imgur.com/9sFQC3L.png)

# Menjalankan Project Vue 3 dengan Vite

Setelah project berhasil dibuat, maka kita perlu melakukan installasi _dependency_ yang dibutuhkan sebelum menjalankan projectnya.

Silahkan jalanakn perintah berikut ini untuk masuk ke dalam _folder project-nya._

```bash
cd vue3-crud
```
Atau bisa juga dengan cara mengklik kanan pada folder projectnya, lalu pilih open with visual studio code . Jangan lupa terminal nya juga harus berada di dalam folder projectnya. 

Setelah itu, jalankan perintah berikut ini.

```bash
npm install
```
Dan silahkan teman-teman tunggu proses installasinya sampai selesai dan pastikan terhubung dengan internet.

Setelah proses installasi selesai, maka kita bisa menjalankan project-nya dengan perintah berikut ini.

```bash
npm run dev
```

![vs](https://i.imgur.com/AfVMk6k.png)

Jika berhasil, maka `project Vue 3 `kita akan dijalankan di dalam localhost menggunakan `port 5173`.

Silahkan teman-teman buka browser dan ketikkan `http://localhost:5173/`
![vue](https://i.imgur.com/sQYyHb0.png)

Kurang lebih seperti itu bagaimana cara melakukan instalasi dan menjalankan project `Vue.js 3` menggunakan `Vite.`


# Kesimpulan

Kita telah belajar bagaimana cara membuat project `Vue.js 3` menggunakan `Vite` dan menjalankan projectnya. Kita juga telah belajar bagaimana cara melakukan installasi _dependency_ yang dibutuhkan sebelum menjal ankan projectnya. 

>Berikutnya kita semua akan belajar bagaimana cara membuat _router _untuk navigasi halaman secara _SPA atau Single Page Application_ di dalam _Vue.js 3_ menggunakan `Vue Router`.
