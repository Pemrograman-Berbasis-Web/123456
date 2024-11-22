Implementasi _CRUD_ dengan Laravel 11 dan Vue JS menggunakan Inertia.
==========================

- [Implementasi _CRUD_ dengan Laravel 11 dan Vue JS menggunakan Inertia.](#implementasi-crud-dengan-laravel-11-dan-vue-js-menggunakan-inertia)
- [Introduction](#introduction)
- [Step 1: Install Laravel 11](#step-1-install-laravel-11)
- [Step 2: Create Auth using Breeze](#step-2-create-auth-using-breeze)
- [Step 3: Create Migration and Model](#step-3-create-migration-and-model)
- [Step 4: Create PostController File](#step-4-create-postcontroller-file)
- [Step 5: Create Route](#step-5-create-route)
- [Step 6: Update Vue Files](#step-6-update-vue-files)
- [Run Project App](#run-project-app)
- [Pro Tips](#pro-tips)

# Introduction

Sebelum kita memulainya, mari kita pahami arti` CRUD`: `Ini adalah akronim dari dunia pemrograman komputer, yang mewakili empat fungsi penting untuk penyimpanan persisten dalam aplikasi Anda: Buat, Baca, Perbarui, dan Hapus.
`

Dalam case ini, kita akan mengimplementasikan CRUD. Membuat `tabel postingan`,`kolom judul`,`isi` `with authentication`.

![app](https://i.imgur.com/ZsotPLJ.gif)

# Step 1: Install Laravel 11

Pertama-tama, kita perlu install aplikasi Laravel 11 versi baru menggunakan perintah di bawah ini karena kita memulai dari awal. 

Jadi, buka _terminal_ atau _prompt_ Anda dan jalankan perintah di bawah ini:

```bash
composer create-project laravel/laravel example-app
```
> _example-app_ adalah nama project. Silahkan di sesuaikan dengan selera anda. 

# Step 2: Create Auth using Breeze

Sekarang, pada langkah ini, kita perlu menggunakan perintah _composer_ untuk menginstal [_breeze_]([https://](https://github.com/laravel/breeze?tab=readme-ov-file)), jadi mari kita jalankan perintah ini untuk instal _library_ ini.

```bash
composer require laravel/breeze --dev
```
![breeze](https://i.imgur.com/iL35b3p.png)

Selanjutnya, kita perlu membuat _autentikasi_ menggunakan perintah di bawah ini. Anda dapat membuat basic login, registrasi, dan verifikasi email.

```bash
php artisan breeze:install
```
![breezee](https://i.imgur.com/DoZTVzq.png)

Pilih opsi `Vue dengan Inertia`. 

Selanjutnya Kita dikasih pilihan beberapa fitur (Optional)

![optional](https://i.imgur.com/ipdYFnk.png)

Disini saya coba fitur _Dark Mode_

Selanjutnya untuk kerangka pengujian pilih `PHPUnit `
dan tunggu hingga prosesnya selesai.

![breeze](https://i.imgur.com/5O0dn5S.png)

Selanjutnya, kita perlu menjalankan perintah `migration` untuk membuat tabel **database**.

Kita Konfig dulu database-nya : 

- Open Project cari file `.env`

Konfigurasi Default nya seperti berikut
```sqlite
  DB_CONNECTION=sqlite
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=laravel
# DB_USERNAME=root
# DB_PASSWORD=
```
Ubah ke MySQL, dan untuk nama database/username silahkan di sesuaikan.

```Mysql
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```
Selanjutnya ketikan perintah berikut:

```bash
php artisan migrate
```
Jika konfigurasinya benar akan menampilkan migration seperti pada gambar berikut:

![phpartisanmigrate](https://i.imgur.com/M6mT0tD.png)

Cek juga di database nya apakah sudah ada ke 3 table di atas!

# Step 3: Create Migration and Model

Di sini, kita perlu membuat migrasi database untuk tabel postingan, dan juga kita akan membuat model untuk tabel postingan.

dengan perintah berikut:

```bash
php artisan make:migration create_posts_table
```
 ```bash
 INFO  Migration [database/migrations/2024_11_19_165042_create_posts_table.php] created successfully. 
 ```
 Selanjutnya cek folder `Migrations` pilih file berikut 
 
 ```bash
2024_11_19_165042_create_posts_table.php
 ```

>INFORMASI : nama file _Migration_ akan random sesuai dengan _tanggal dibuat-nya._

```php
<?php

// Menggunakan namespace yang diperlukan untuk migrasi di Laravel
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

// Mendeklarasikan kelas migrasi dengan anonymous class yang merupakan implementasi dari Migration
return new class extends Migration
{
    /**
     * Fungsi untuk menjalankan migrasi (membuat tabel).
     */
    public function up(): void
    {
        // Membuat tabel 'posts' dengan kolom-kolom yang sudah ditentukan
        Schema::create('posts', function (Blueprint $table) {
            // Menambahkan kolom id yang akan menjadi primary key dan auto-increment
            $table->id();
            
            // Menambahkan kolom 'title' yang bertipe string (biasanya untuk judul)
            $table->string('title');
            
            // Menambahkan kolom 'body' yang bertipe text (untuk konten atau tubuh artikel)
            $table->text('body');
            
            // Menambahkan kolom timestamps yang akan otomatis menambahkan kolom 'created_at' dan 'updated_at'
            $table->timestamps();
        });
    }

    /**
     * Fungsi untuk membatalkan migrasi (menghapus tabel).
     */
    public function down(): void
    {
        // Menghapus tabel 'posts' jika migrasi dibatalkan
        Schema::dropIfExists('posts');
    }
};
```
**Penjelasan:**

1. **Namespace dan Import:**
   - `use Illuminate\Database\Migrations\Migration;` : Mengimpor kelas `Migration` yang menyediakan fungsionalitas dasar untuk migrasi di Laravel.
   - `use Illuminate\Database\Schema\Blueprint;` : Mengimpor `Blueprint` yang memungkinkan kita mendefinisikan struktur tabel.
   - `use Illuminate\Support\Facades\Schema;` : Mengimpor `Schema` yang digunakan untuk melakukan operasi pada skema database, seperti membuat atau menghapus tabel.

2. **Fungsi `up()`**:
   - Fungsi ini digunakan untuk menjalankan operasi migrasi, dalam hal ini membuat tabel baru.
   - `Schema::create('posts', function (Blueprint $table) {...})`: Ini membuat tabel `posts` di database. Di dalam closure, kita mendefinisikan struktur tabel.
     - `$table->id()` : Menambahkan kolom `id` yang menjadi primary key dengan tipe auto-increment.
     - `$table->string('title')` : Menambahkan kolom `title` dengan tipe string, untuk menyimpan judul dari sebuah post.
     - `$table->text('body')` : Menambahkan kolom `body` dengan tipe text, yang biasanya digunakan untuk menyimpan isi artikel atau konten post.
     - `$table->timestamps()` : Menambahkan dua kolom yaitu `created_at` dan `updated_at`, yang secara otomatis mengatur waktu saat data dibuat atau diperbarui.

3. **Fungsi `down()`**:
   - Fungsi ini digunakan untuk membatalkan migrasi, dalam hal ini untuk menghapus tabel `posts` jika migrasi dibatalkan.
   - `Schema::dropIfExists('posts')`: Ini akan menghapus tabel `posts` jika tabel tersebut ada. Ini berguna untuk rollback atau mengembalikan perubahan ketika migrasi dibatalkan.

- **`up()`** digunakan untuk mendefinisikan dan menjalankan perubahan (membuat tabel).

- **`down()`** digunakan untuk membatalkan perubahan tersebut (menghapus tabel).

- Fungsi-fungsi ini sangat penting dalam migrasi database karena memungkink an pengembang untuk mengelola perubahan struktur database dengan lebih mudah dan aman.

Selanjutnya jalankan perintah berikut:

```bash
php artisan migrate
```
```bash
2024_11_19_165042_create_posts_table ......................................... 15.96ms DONE
```
Cek kembali di database apakah ada tabel post?

Sekarang kita akan membuat `model Post` dengan menggunakan perintah berikut:

```bash
php artisan make:model Post
```
Open `App/Models/Post.php` dan tambahkan kode berikut: 

```php
<?php
  
// Mendefinisikan namespace untuk model ini, yakni berada dalam folder App\Models
namespace App\Models;
  
// Mengimpor trait HasFactory yang memungkinkan penggunaan factory untuk model ini
use Illuminate\Database\Eloquent\Factories\HasFactory;

// Mengimpor kelas Model dari Eloquent yang merupakan dasar dari semua model di Laravel
use Illuminate\Database\Eloquent\Model;
  
// Mendeklarasikan kelas Post yang mewarisi dari Eloquent Model
class Post extends Model
{
    // Menggunakan trait HasFactory untuk memungkinkan pembuatan instance model menggunakan factory
    use HasFactory;
  
    // Menentukan kolom mana yang dapat diisi (mass assignment) untuk model ini
    protected $fillable = [
        'title', 'body' // Kolom 'title' dan 'body' dapat diisi secara massal
    ];
}
```
**Penjelasan:**

1. **Namespace dan Import:**
   - `namespace App\Models;` : Menetapkan namespace untuk model ini, yaitu berada di dalam folder `app/Models`.
   - `use Illuminate\Database\Eloquent\Factories\HasFactory;` : Mengimpor trait `HasFactory`, yang memungkinkan kita untuk menggunakan Factory dalam model ini. Factory sering digunakan untuk membuat data palsu (dummy data) untuk keperluan testing atau seeding.
   - `use Illuminate\Database\Eloquent\Model;` : Mengimpor kelas `Model` dari Eloquent, yang merupakan kelas dasar dari semua model di Laravel. Kelas ini memberikan banyak fungsionalitas seperti interaksi dengan database.

2. **Kelas `Post`**:
   - `class Post extends Model`: Mendeklarasikan kelas `Post`, yang merupakan model Eloquent yang berhubungan dengan tabel `posts` dalam database. Kelas ini mewarisi semua fitur yang disediakan oleh Eloquent, seperti operasi CRUD (Create, Read, Update, Delete).
   
3. **Trait `HasFactory`**:
   - `use HasFactory;` : Menggunakan trait `HasFactory`, yang menyediakan kemampuan untuk menggunakan factory, yaitu untuk membuat data model palsu secara otomatis. Hal ini sangat berguna saat melakukan seeding atau testing.

4. **Atribut `$fillable`**:
   - `protected $fillable = ['title', 'body'];`: Ini adalah array yang mendefinisikan kolom mana saja yang bisa diisi secara massal (mass assignment). Artinya, kita dapat mengisi kolom `title` dan `body` secara langsung dengan data dalam array atau saat melakukan operasi `create` atau `update`.
   
   Misalnya, jika kita ingin membuat post baru:

   ```php
   Post::create(['title' => 'Judul Post', 'body' => 'Isi post']);
   ```
   Dengan mendeklarasikan `$fillable`, kita memastikan bahwa hanya kolom `title` dan `body` yang dapat diisi melalui mass assignment, sementara kolom lain yang tidak ada dalam array ini akan dilindungi dari mass assignment, sehingga mengurangi risiko serangan **Mass Assignment Vulnerability**.

- **`HasFactory`** memungkinkan penggunaan factory untuk menghasilkan data palsu atau testing.

- **`$fillable`** digunakan untuk menentukan kolom yang dapat diisi menggunakan mass assignment, membantu menjaga keamanan data yang diinputkan ke dalam model.

- Kelas ini mewakili entitas `Post` yang berhubungan dengan tabel `posts` dalam database dan memungkinkan operasi CRUD pada data tabel tersebut.

# Step 4: Create PostController File

Sekarang, di sini kita akan membuat `PostController` menggunakan perintah `artisan.` Jadi, jalankan perintah di bawah ini untuk membuat komponen aplikasi `CRUD Post`.

```bash
php artisan make:controller PostController
```
Sekarang, buka file `PostController.php` yang baru saja dibuat dan tambahkan kode berikut untuk membuat fungsi CRUD:

`app/Http/Controllers/PostController.php`

```php
<?php

// Menentukan namespace untuk controller ini, yaitu berada dalam folder App\Http\Controllers
namespace App\Http\Controllers;

// Mengimpor kelas Request untuk menangani request HTTP
use Illuminate\Http\Request;

// Mengimpor model Post untuk mengakses data pada tabel posts
use App\Models\Post;

// Mengimpor Inertia untuk merender tampilan (views) dengan menggunakan Inertia.js
use Inertia\Inertia;

class PostController extends Controller
{
    /**
     * Menampilkan daftar semua post.
     *
     * @return response()
     */
    public function index()
    {
        // Mengambil semua data post dari database
        $posts = Post::all();

        // Menggunakan Inertia untuk merender tampilan 'Post/Index' dan mengirimkan data posts
        return Inertia::render('Post/Index', ['posts' => $posts]);
    }

    /**
     * Menampilkan form untuk membuat post baru.
     *
     * @return response()
     */
    public function create()
    {
        // Menggunakan Inertia untuk merender tampilan 'Post/Create' untuk membuat post baru
        return Inertia::render('Post/Create');
    }

    /**
     * Menyimpan post baru ke dalam database.
     *
     * @return response()
     */
    public function store(Request $request)
    {
        // Membuat instance baru dari model Post dan mengisi kolom dengan data yang dikirimkan
        $post = new Post($request->all());

        // Menyimpan data post baru ke database
        $post->save();

        // Mengarahkan pengguna ke halaman daftar post (index)
        return redirect()->route('posts.index');
    }

    /**
     * Menampilkan form untuk mengedit post yang ada.
     *
     * @return response()
     */
    public function edit(Post $post)
    {
        // Menggunakan Inertia untuk merender tampilan 'Post/Edit' dan mengirimkan data post yang ingin diedit
        return Inertia::render('Post/Edit', ['post' => $post]);
    }

    /**
     * Memperbarui post yang ada di database.
     *
     * @return response()
     */
    public function update(Request $request, Post $post)
    {
        // Memperbarui data post dengan data yang dikirimkan melalui request
        $post->update($request->all());

        // Mengarahkan pengguna kembali ke halaman daftar post setelah pembaruan
        return redirect()->route('posts.index');
    }

    /**
     * Menghapus post dari database.
     *
     * @return response()
     */
    public function destroy(Post $post)
    {
        // Menghapus post yang dipilih dari database
        $post->delete();

        // Mengarahkan pengguna kembali ke halaman sebelumnya (biasanya daftar post)
        return redirect()->back();
    }
}
```

**Penjelasan Fungsi-fungsi Controller:**

1. **`index()`**:
   - Fungsi ini digunakan untuk menampilkan daftar semua post yang ada di database. Mengambil data dari tabel `posts` menggunakan `Post::all()`, lalu mengirim data tersebut ke tampilan `Post/Index` menggunakan `Inertia::render()`.
   - **Inertia.js** memungkinkan kita untuk membuat aplikasi frontend modern dengan Vue.js atau React.js, dan controller ini mengirim data ke tampilan frontend melalui Inertia.

2. **`create()`**:
   - Fungsi ini digunakan untuk menampilkan form untuk membuat post baru. Ini akan merender tampilan `Post/Create` dengan menggunakan Inertia.

3. **`store(Request $request)`**:
   - Fungsi ini menangani penyimpanan data post baru yang dikirimkan melalui form (method POST). Data dari request diambil dengan `$request->all()` dan digunakan untuk mengisi model `Post`. Kemudian, data tersebut disimpan ke database dengan `$post->save()`.
   - Setelah berhasil menyimpan, pengguna diarahkan ke halaman daftar post (`posts.index`).

4. **`edit(Post $post)`**:
   - Fungsi ini digunakan untuk menampilkan form pengeditan untuk sebuah post yang sudah ada. Laravel akan secara otomatis mencari post berdasarkan ID yang diberikan melalui route model binding. Post yang ditemukan akan diteruskan ke tampilan `Post/Edit`.

5. **`update(Request $request, Post $post)`**:
   - Fungsi ini digunakan untuk memperbarui data post yang ada. Data yang dikirimkan melalui request akan digunakan untuk memperbarui model `Post`. Setelah data diperbarui, pengguna akan diarahkan kembali ke halaman daftar post.

6. **`destroy(Post $post)`**:
   - Fungsi ini digunakan untuk menghapus sebuah post dari database. Laravel akan mencari post berdasarkan ID yang diberikan melalui route model binding dan kemudian menghapusnya. Setelah itu, pengguna diarahkan kembali ke halaman sebelumnya.

**Inertia.js:**

Di sini, `Inertia::render()` digunakan untuk merender tampilan frontend dari Laravel Controller.


- Controller ini mengelola CRUD (Create, Read, Update, Delete) untuk model `Post` dengan menggunakan **Inertia.js** untuk menghubungkan Laravel ke frontend (Vue.js atau React).

- Fungsi-fungsi di dalam controller ini mengatur tampilan, pengambilan data, penyimpanan, dan penghapusan post, semuanya dengan alur yang bersih dan mudah dipahami.

# Step 5: Create Route

Pada langkah ini, kita akan membuat rute sumber daya untuk postingan di file `web.php.` Jadi, mari kita perbarui file tersebut:

Tambahkan kode berikut di :

`routes/web.php`

```php
<?php

// Mengimpor PostController untuk digunakan dalam routing
use App\Http\Controllers\PostController;

// Mendefinisikan resource route untuk controller PostController
Route::resource('posts', PostController::class);

```
**Penjelasan: Routing**

1. **`use App\Http\Controllers\PostController;`**:
   - Baris ini mengimpor `PostController` ke dalam file routing (biasanya `web.php` di Laravel). Dengan cara ini, kita dapat menggunakan controller tersebut untuk menangani permintaan HTTP pada rute yang terkait.

2. **`Route::resource('posts', PostController::class);`**:
   - Fungsi ini digunakan untuk mendefinisikan **resource route** di Laravel, yang secara otomatis menghubungkan berbagai URL (endpoint) dengan metode-metode dalam controller (`PostController` dalam hal ini).
   - Laravel menyediakan 7 metode default yang secara otomatis akan dihubungkan dengan rute-rute yang relevan. Metode-metode tersebut adalah:
   
   - **GET /posts** – Menampilkan daftar semua post (`index` method di controller).
   - **GET /posts/create** – Menampilkan form untuk membuat post baru (`create` method di controller).
   - **POST /posts** – Menyimpan post baru ke dalam database (`store` method di controller).
   - **GET /posts/{post}** – Menampilkan detail dari sebuah post tertentu (`show` method di controller).
   - **GET /posts/{post}/edit** – Menampilkan form untuk mengedit post yang ada (`edit` method di controller).
   - **PUT/PATCH /posts/{post}** – Memperbarui post tertentu dalam database (`update` method di controller).
   - **DELETE /posts/{post}** – Menghapus post tertentu dari database (`destroy` method di controller).

**Contoh URL dan Metode yang Dihubungkan:**

Dengan menggunakan `Route::resource('posts', PostController::class)`, berikut adalah URL dan metode controller yang akan dihasilkan secara otomatis:

| HTTP Method | URL                | Controller Method |
|-------------|--------------------|-------------------|
| GET         | /posts             | index             |
| GET         | /posts/create      | create            |
| POST        | /posts             | store             |
| GET         | /posts/{post}      | show              |
| GET         | /posts/{post}/edit | edit              |
| PUT/PATCH   | /posts/{post}      | update            |
| DELETE      | /posts/{post}      | destroy           |

**Keuntungan:**

- **Routing yang sederhana**: Menggunakan `Route::resource()` membuat routing menjadi sangat sederhana karena semua rute untuk operasi CRUD pada `Post` sudah ditangani otomatis tanpa perlu mendefinisikan satu per satu.

- **Konvensi**: Laravel mengikuti konvensi yang kuat dengan rute resource sehingga sangat memudahkan developer dalam membangun aplikasi CRUD standar.

- **Kemudahan dalam mengelola URL**: Semua rute untuk CRUD sudah terstruktur dengan jelas, dan kita dapat langsung menghubungkan aksi di controller dengan URL yang bersangkutan.


Dengan menggunakan `Route::resource('posts', PostController::class)`, Anda otomatis membuat rute untuk seluruh aksi CRUD di dalam controller `PostController` dengan cara yang sangat efisien. 

Laravel mengurus pembuatan semua rute untuk operasi dasar terhadap model `Post` secara otomatis, yang sangat menghemat waktu dan usaha dalam mendefinisikan rute secara manual.

Selanjutnya Untuk memastikan apakah `route-route` tersebut sudah _digenerate_ oleh Laravel, teman-teman bisa menjalankan perintah berikut ini di dalam terminal/CMD.

```bash
php artisan route:list
```

![route](https://i.imgur.com/ykR0dOI.png)


# Step 6: Update Vue Files

Di sini, kita akan memperbarui daftar file berikut untuk halaman daftar dan membuat file komponen vue.

Buat folder `Post `& di dalam folder post buat file baru bernama `Index.vue, Create.vue` dan `Edit.vue`. Lalu tambahkan kode berikut:

- `resources/js/Pages/Post/Index.vue`

```html
<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'; // Mengimpor layout untuk halaman yang memerlukan autentikasi
import { Head, Link, useForm } from '@inertiajs/vue3'; // Mengimpor komponen dan hook dari Inertia.js

// Mendefinisikan props yang diterima oleh komponen ini, yaitu array posts
defineProps({
    posts: {
        type: Array,
        default: () => [], // Jika tidak ada data posts, akan mengirimkan array kosong sebagai default
    },
});

// Menginisialisasi form kosong dengan hook useForm dari Inertia.js, yang akan digunakan untuk mengelola penghapusan post
const form = useForm({});

// Fungsi untuk menghapus post
const deletePost = (id) => {
    form.delete(`posts/${id}`); // Menggunakan form.delete untuk mengirim permintaan DELETE ke endpoint yang sesuai
};
</script>

<template>

    <Head title="Manage Posts" /> <!-- Mengatur title halaman melalui komponen Head dari Inertia.js -->

    <AuthenticatedLayout> <!-- Layout yang digunakan untuk tampilan halaman yang terautentikasi -->
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Manage Posts</h2> <!-- Judul halaman -->
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <!-- Tombol untuk membuat post baru, mengarah ke halaman create -->
                        <Link href="posts/create">
                            <button
                                class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded my-3">Create New
                                Post</button>
                        </Link>
                        <!-- Tabel yang menampilkan daftar posts -->
                        <table class="table-auto w-full">
                            <thead>
                                <tr>
                                    <!-- Header tabel dengan kolom ID, Title, Content, dan Action -->
                                    <th class="border px-4 py-2">ID</th>
                                    <th class="border px-4 py-2">Title</th>
                                    <th class="border px-4 py-2">Content</th>
                                    <th class="border px-4 py-2" width="250px">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <!-- Iterasi untuk menampilkan setiap post di dalam tabel -->
                                <tr v-for="post in posts" :key="post.id">
                                    <td class="border px-4 py-2">{{ post.id }}</td> <!-- Menampilkan ID post -->
                                    <td class="border px-4 py-2">{{ post.title }}</td> <!-- Menampilkan Title post -->
                                    <td class="border px-4 py-2">{{ post.body }}</td> <!-- Menampilkan Content post -->
                                    <td class="border px-4 py-2">
                                        <!-- Tombol untuk mengedit post, mengarah ke halaman edit -->
                                        <Link :href="`posts/${post.id}/edit`">
                                            <button
                                                class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Edit</button>
                                        </Link>
                                        <!-- Tombol untuk menghapus post, memanggil fungsi deletePost -->
                                        <button
                                            class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded ml-2"
                                            @click="deletePost(post.id)">Delete</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
```
**Pada Fungsi dan Bagian Kode:**

1. **`defineProps`**:
   - Menetapkan properti `posts` yang akan diterima oleh komponen ini. Properti ini bertipe array dan akan berisi daftar post yang diteruskan dari backend Laravel.
   - **Default Value**: Jika tidak ada nilai `posts` yang diteruskan, maka array kosong (`[]`) akan digunakan sebagai nilai default.

2. **`useForm`**:
   - Menginisialisasi form kosong dengan hook `useForm` dari Inertia.js. Ini digunakan untuk menangani operasi form, seperti pengiriman data dan penghapusan data.
   - Form ini dipersiapkan untuk digunakan dalam fungsi `deletePost` untuk mengirimkan permintaan HTTP `DELETE`.

3. **`deletePost`**:
   - Fungsi ini menerima `id` dari post yang ingin dihapus.
   - Menggunakan metode `form.delete()` dari `useForm` untuk mengirimkan permintaan `DELETE` ke endpoint `posts/{id}` di backend.
   - Inertia.js akan menangani permintaan tersebut dan memperbarui tampilan halaman tanpa me-refresh halaman.

4. **`<Head>`**:
   - Komponen ini digunakan untuk mengatur tag `<head>` HTML, khususnya untuk mengatur title halaman. Dalam hal ini, title halaman diatur menjadi "Manage Posts".

5. **`<AuthenticatedLayout>`**:
   - Layout ini digunakan untuk membungkus seluruh konten halaman. Layout ini menyediakan struktur dasar (seperti header, sidebar, dll.) yang konsisten di seluruh aplikasi bagi pengguna yang telah terautentikasi.

6. **`<template #header>`**:
   - Menyediakan bagian header dalam layout, yang berisi judul halaman yang mengatakan "Manage Posts". Ini memberi konteks kepada pengguna tentang halaman yang sedang mereka lihat.

7. **`<Link href="posts/create">`**:
   - Tombol ini adalah link yang mengarah ke halaman `posts/create`, memungkinkan pengguna untuk membuat post baru. Inertia.js digunakan untuk menangani navigasi antar halaman tanpa me-refresh seluruh halaman.

8. **Tabel Daftar Post**:
   - Tabel ini berfungsi untuk menampilkan daftar post yang diterima melalui props `posts`. Setiap post ditampilkan dalam baris tabel (`<tr>`) dengan kolom-kolom untuk **ID**, **Title**, dan **Content**.
   - **`v-for="post in posts"`**: Digunakan untuk mengiterasi array `posts` dan menampilkan setiap post dalam tabel.
   - **`@click="deletePost(post.id)"`**: Ketika tombol **Delete** diklik, fungsi `deletePost` dipanggil dengan ID post yang relevan. Ini akan mengirimkan permintaan untuk menghapus post tersebut.

9. **Tombol Edit**:
   - Tombol ini mengarahkan pengguna ke halaman untuk mengedit post yang bersangkutan. Navigasi dilakukan menggunakan komponen `Link` dari Inertia.js, yang memungkinkan transisi halaman tanpa me-refresh halaman.

10. **Tombol Delete**:
    - Tombol ini memungkinkan pengguna untuk menghapus post. Ketika diklik, ia memanggil fungsi `deletePost(post.id)`, yang akan menghapus post tersebut dari database menggunakan HTTP request `DELETE`.


- **Fungsi utama** dari komponen ini adalah untuk menampilkan daftar **posts**, dengan kemampuan untuk membuat, mengedit, dan menghapus post.

- **Inertia.js** digunakan untuk menangani navigasi dan pengelolaan form tanpa melakukan refresh halaman, memberikan pengalaman pengguna yang lebih responsif dan dinamis.


- `resources/js/Pages/Post/Create.vue`

```html
<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'; // Mengimpor layout yang memerlukan autentikasi
import { useForm, Link } from "@inertiajs/vue3"; // Mengimpor hook useForm untuk mengelola form dan Link untuk navigasi

// Inisialisasi form menggunakan hook useForm dari Inertia.js
// Form ini memiliki dua field: title dan body
const form = useForm({
    title: "", // Menginisialisasi field title dengan nilai kosong
    body: "",  // Menginisialisasi field body dengan nilai kosong
});

// Fungsi untuk mengirimkan form (POST request) ke backend
const submit = () => {
    form.post("/posts"); // Mengirim form melalui metode post ke endpoint '/posts'
};
</script>

<template>
    <Head title="Manage Posts" /> <!-- Mengatur title halaman menjadi "Manage Posts" menggunakan Inertia Head -->

    <AuthenticatedLayout> <!-- Menggunakan layout yang membutuhkan autentikasi untuk tampilan halaman -->
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Create Post</h2> <!-- Header halaman dengan judul "Create Post" -->
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <!-- Tombol untuk kembali ke halaman daftar posts -->
                        <Link href="/posts">
                            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded my-3">
                                Back
                            </button>
                        </Link>

                        <!-- Form untuk membuat post baru -->
                        <form @submit.prevent="submit"> <!-- Mencegah form melakukan submit tradisional dan memanggil fungsi submit -->
                            <div class="mb-4">
                                <!-- Label dan input untuk field title -->
                                <label for="title" class="block text-gray-700 text-sm font-bold mb-2">
                                    Title:
                                </label>
                                <input 
                                    type="text"
                                    id="title"
                                    v-model="form.title" <!-- Mengikat nilai input dengan form.title -->
                                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                                    placeholder="Enter Title"
                                />
                            </div>

                            <div class="mb-4">
                                <!-- Label dan textarea untuk field body -->
                                <label for="body" class="block text-gray-700 text-sm font-bold mb-2">
                                    Body:
                                </label>
                                <textarea
                                    id="body"
                                    v-model="form.body" <!-- Mengikat nilai textarea dengan form.body -->
                                    placeholder="Enter Body"
                                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                                ></textarea>
                            </div>

                            <!-- Tombol untuk mengirimkan form -->
                            <button 
                                type="submit"
                                class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded my-3">
                                Submit
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
```
**Penjelasan Pada Fungsi dan Bagian Kode:**

1. **`useForm`**:
   - **`useForm`** adalah hook dari Inertia.js yang digunakan untuk menangani form, termasuk pengiriman data. Dalam kode ini, `useForm` menginisialisasi form dengan dua field: `title` dan `body`, keduanya dimulai dengan nilai kosong.
   - **`v-model`**: Digunakan untuk mengikat input form ke properti `title` dan `body` pada objek `form`. Setiap perubahan pada input akan diperbarui secara otomatis di dalam objek `form`.

2. **`submit`**:
   - **`submit`** adalah fungsi yang dipanggil saat form disubmit.
   - Fungsi ini menggunakan metode `form.post()` dari Inertia.js untuk mengirim data form ke server pada endpoint `/posts` menggunakan HTTP method `POST`. 
   - `@submit.prevent="submit"` digunakan untuk mencegah perilaku default form (yang biasanya melakukan refresh halaman) dan menggantikannya dengan pengiriman data menggunakan Inertia.js.

3. **`<AuthenticatedLayout>`**:
   - Layout ini membungkus konten halaman dan memberikan struktur untuk tampilan halaman yang memerlukan autentikasi (misalnya, header, footer, sidebar, dsb.).

4. **`<Link href="/posts">`**:
   - Link ini memungkinkan navigasi kembali ke halaman daftar post dengan URL `/posts`. 
   - Tombol **Back** menggunakan **Inertia.js's Link** untuk berpindah halaman tanpa me-refresh seluruh halaman.

5. **`<form @submit.prevent="submit">`**:
   - Form ini menangani inputan pengguna untuk membuat post baru.
   - **`@submit.prevent="submit"`**: Mencegah form melakukan submit standar (refresh halaman), dan memanggil fungsi `submit` yang mengirimkan data ke server.

6. **`<input>` dan `<textarea>`**:
   - Form input untuk judul dan konten post.
   - **`v-model="form.title"`** dan **`v-model="form.body"`** digunakan untuk mengikat nilai input ke properti dalam objek `form`, yang memastikan bahwa perubahan pada input akan tercermin dalam objek data.

7. **Tombol Submit**:
   - Tombol ini digunakan untuk mengirimkan form.
   - Ketika diklik, tombol ini akan memanggil metode `submit()` dan mengirimkan data form ke server untuk disimpan.

**Fungsionalitas:**

- Komponen ini menyediakan formulir untuk membuat post baru dengan dua input: `title` dan `body`.

- **Inertia.js** digunakan untuk menangani pengiriman data tanpa melakukan refresh halaman. Ketika form disubmit, data akan dikirim ke endpoint backend `/posts` dengan HTTP POST request, yang dapat disertai dengan validasi atau logika lainnya di server.

- Setelah form dikirim, pengguna dapat diarahkan kembali ke halaman daftar post, yang dikelola dengan menggunakan **Inertia.js** untuk navigasi tanpa _me-refresh_ halaman.


- `resources/js/Pages/Post/Edit.vue`

```html
<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'; // Mengimpor layout yang memerlukan autentikasi
import { useForm, Link } from "@inertiajs/vue3"; // Mengimpor hook useForm dari Inertia.js untuk menangani form dan Link untuk navigasi

// Mendefinisikan properti yang diterima komponen, yaitu post
const props = defineProps({
    post: {
        type: Object, // Properti post adalah sebuah objek
        default: null, // Default nilainya adalah null
    },
});

// Inisialisasi form dengan nilai yang diambil dari props.post, untuk mengedit post yang ada
const form = useForm({
    title: props.post.title, // Menyusun nilai awal untuk field title menggunakan props.post.title
    body: props.post.body,   // Menyusun nilai awal untuk field body menggunakan props.post.body
});

// Fungsi untuk mengirimkan data form (PUT request) ke backend
const submit = () => {
    form.put(`/posts/${props.post.id}`); // Mengirim form dengan metode PUT ke URL untuk update post dengan ID yang sesuai
};
</script>

<template>

    <Head title="Manage Posts" /> <!-- Mengatur judul halaman menjadi "Manage Posts" menggunakan Inertia Head -->

    <AuthenticatedLayout> <!-- Menggunakan layout yang membutuhkan autentikasi -->
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Edit Post</h2> <!-- Header halaman dengan judul "Edit Post" -->
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <!-- Tombol untuk kembali ke halaman daftar posts -->
                        <Link href="/posts">
                            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded my-3">
                                Back
                            </button>
                        </Link>

                        <!-- Form untuk mengedit post -->
                        <form @submit.prevent="submit"> <!-- Mencegah form melakukan submit standar dan memanggil fungsi submit -->
                            <div class="mb-4">
                                <!-- Label dan input untuk field title -->
                                <label for="title" class="block text-gray-700 text-sm font-bold mb-2">
                                    Title:
                                </label>
                                <input 
                                    type="text"
                                    id="title"
                                    v-model="form.title" <!-- Mengikat input dengan form.title -->
                                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                                    placeholder="Enter Title"
                                />
                            </div>

                            <div class="mb-4">
                                <!-- Label dan textarea untuk field body -->
                                <label for="body" class="block text-gray-700 text-sm font-bold mb-2">
                                    Body:
                                </label>
                                <textarea
                                    id="body"
                                    v-model="form.body" <!-- Mengikat textarea dengan form.body -->
                                    placeholder="Enter Body"
                                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                                ></textarea>
                            </div>

                            <!-- Tombol untuk mengirimkan form -->
                            <button 
                                type="submit"
                                class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded my-3">
                                Submit
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
```
**Penjelasan Komponen dan Kode:**

1. **`defineProps`**:
   - **`defineProps`** digunakan untuk mendefinisikan properti yang diterima oleh komponen ini. Dalam hal ini, `post` adalah objek yang berisi data dari post yang akan diedit.
   - **`post`**: Diambil sebagai properti, yang akan berisi data post yang ada (title dan body) yang ingin diedit.

2. **`useForm`**:
   - **`useForm`** digunakan untuk mengelola data form dan status pengirimannya.
   - **`title: props.post.title`** dan **`body: props.post.body`**: Nilai awal dari form diambil dari data `post` yang diterima melalui properti. Ini memungkinkan pengguna untuk melihat dan mengedit data yang ada.
   - **`form.put()`**: Fungsi untuk mengirim data yang diubah ke server menggunakan metode HTTP PUT, yang mengupdate post yang ada. Endpoint yang dipanggil adalah `/posts/{id}`, di mana `id` diambil dari properti `post.id`.

3. **`submit`**:
   - Fungsi ini dipanggil saat pengguna mengirimkan form (dengan menekan tombol submit).
   - **`form.put(`/posts/${props.post.id}`)`**: Data form yang sudah diubah akan dikirim ke backend untuk mengupdate post yang sesuai dengan ID yang diberikan.

4. **`<Link href="/posts">`**:
   - Ini adalah tombol **Back**, yang menggunakan **Inertia.js's Link** untuk menavigasi kembali ke halaman daftar posts tanpa melakukan refresh halaman.

5. **`<form @submit.prevent="submit">`**:
   - **`@submit.prevent="submit"`**: Mencegah form melakukan submit standar (yang biasanya akan melakukan reload halaman), dan menggantikannya dengan pengiriman data menggunakan metode `submit` yang sudah didefinisikan.
   
6. **`<input>` dan `<textarea>`**:
   - Form input untuk mengedit data post.
   - **`v-model="form.title"`** dan **`v-model="form.body"`** mengikat input dengan properti dalam objek `form`, yang memungkinkan data form berubah secara otomatis saat pengguna melakukan perubahan pada input.

7. **Tombol Submit**:
   - Tombol ini digunakan untuk mengirimkan data form yang sudah diedit.
   - Ketika diklik, tombol ini akan memicu pengiriman data ke backend melalui metode PUT untuk mengupdate post.

**Fungsionalitas:**

- Komponen ini digunakan untuk mengedit post yang ada. Judul dan konten post yang ada dimuat ke dalam form input (judul dan body), memungkinkan pengguna untuk mengubahnya.

- Data yang diubah kemudian dikirim ke server menggunakan metode **PUT** untuk memperbarui post tersebut di backend.

- Navigasi antar halaman dikelola menggunakan **Inertia.js's Link**, memungkinkan transisi halaman tanpa refresh.


Terakhir, di dalam file `AuthenticatedLayout`, kita perlu menambahkan link pada header untuk halaman postingan.

`resources/js/Layouts/AuthenticatedLayout.vue`

```html
<!-- Add the Posts Link -->

<NavLink :href="route('posts.index')" :active="route().current('posts.index')">
    Posts
</NavLink>
```
**Penjelasan:**

1. **`<NavLink>`**:
   - Ini adalah komponen yang berfungsi untuk membuat sebuah link navigasi yang mendukung status aktif _(active state)._
   - Biasanya, komponen ini digunakan dalam sistem navigasi untuk menunjukkan link mana yang sedang aktif.

2. **`:href="route('posts.index')"`**:
   - **`:href`** adalah sintaks Vue.js untuk binding dinamis ke atribut `href`. 
   - **`route('posts.index')`**: Ini adalah fungsi dari **Inertia.js** atau Laravel yang mengembalikan URL untuk route yang dinamai `posts.index`. Dengan menggunakan `route()`, kamu tidak perlu menuliskan URL secara manual, melainkan hanya menggunakan nama route yang didefinisikan di file `web.php` (di Laravel) atau di tempat lain yang mendefinisikan route.
   - Jadi, kode ini membuat **link** menuju halaman yang memiliki route dengan nama `posts.index` (misalnya halaman daftar post).

3. **`:active="route().current('posts.index')"`**:
   - **`:active`** adalah binding dinamis untuk menentukan apakah link tersebut aktif atau tidak.
   - **`route().current('posts.index')`**: Fungsi **`route().current()`** memeriksa apakah route yang sedang diakses sesuai dengan nama route yang diberikan (dalam hal ini `posts.index`).
   - Jika halaman saat ini adalah halaman yang terhubung dengan route `posts.index`, maka properti `active` akan bernilai `true`, yang akan memberi tahu komponen `NavLink` bahwa link ini sedang aktif.
   - Biasanya, komponen `NavLink` akan menambahkan kelas CSS seperti `active` ke link tersebut agar tampil berbeda (misalnya dengan mengganti warnanya), menunjukkan bahwa link tersebut mewakili halaman yang sedang aktif.

4. **`Posts`**:
   - Ini adalah teks yang akan ditampilkan di dalam link navigasi. Ketika pengguna mengklik link ini, mereka akan diarahkan ke halaman daftar post yang didefinisikan di route `posts.index`.

**Fungsionalitas:**

Secara keseluruhan, kode ini membuat sebuah link navigasi yang mengarah ke halaman daftar post. 

Fitur utama dari `NavLink` adalah menambahkan status aktif pada link tersebut ketika pengguna berada di halaman yang sama dengan tujuan link `(posts.index)`. 

Ini membantu pengguna mengetahui halaman mana yang sedang mereka akses dalam navigasi, memberikan petunjuk visual dengan menggunakan status aktif pada link.

Dengan demikian, penggunaan `route('posts.index')` dan `route().current()` memungkinkan aplikasi untuk lebih dinamis dan responsif terhadap perubahan `URL.`

# Run Project App
 
Setelah semua langkah yang diperlukan telah dilakukan, sekarang Anda harus mengetik perintah di bawah ini dan tekan enter untuk menjalankan aplikasi

```console
php artisan serve
```

Selanjutnya, kita perlu _build_ aplikasi kita menggunakan perintah npm, jadi jalankan perintah NPM berikut:

```console
npm run build
```

Sekarang, buka browser web Anda, ketik URL yang diberikan dan lihat output dari aplikasi.


# Pro Tips

Jika aplikasinya berjalan dengan baik. Anda boleh menambahkan fitur-fitur lainnya seperti menambahkan gambar dalam post dll.

Selamat praktek dan semoga kalian semua menjadi _Programmers_ `FULL-Stack` 🔥

_Let's code and have fun!_ 💻🔥🚀
