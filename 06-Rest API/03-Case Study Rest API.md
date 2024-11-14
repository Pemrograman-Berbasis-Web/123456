Implementasi REST API
=============================

Untuk implementasi lebih lanjut dan pengaturan yang lebih lengkap, berikut adalah langkah-langkah tambahan yang perlu Anda lakukan untuk menyelesaikan **aplikasi Todo List** dengan **API** sederhana menggunakan **PHP**. 

- [Implementasi REST API](#implementasi-rest-api)
- [Struktur Folder Project](#struktur-folder-project)
- [Membuat API `(api.php)`](#membuat-api-apiphp)
- [Modifikasi `TodoController.php`](#modifikasi-todocontrollerphp)
- [Modifikasi `index.php `](#modifikasi-indexphp-)
- [Memodifikasi `listTodos.php`](#memodifikasi-listtodosphp)
- [Pengaturan `.htaccess `](#pengaturan-htaccess-)
- [Periksa Kode `api.php`](#periksa-kode-apiphp)
- [Tes API](#tes-api)
- [Eksplorasi REST API](#eksplorasi-rest-api)
- [Pro Tips](#pro-tips)

# Struktur Folder Project

Sebelum melanjutkan, mari kita tentukan struktur folder yang jelas untuk proyek Anda:

```graphql
/todolist_project
├── index.php                    # Entry point aplikasi
├── api.php                      # API endpoint untuk operasi CRUD
├── .htaccess                    # Konfigurasi untuk URL rewriting
├── core
│   └── Database.php             # Koneksi database menggunakan PDO
├── models
│   ├── Todo.php                 # Model Todo yang merepresentasikan setiap task
│   └── TodoModel.php            # Model untuk operasi CRUD pada database
├── controllers
│   └── TodoController.php       # Controller untuk mengatur logika bisnis Todo
├── views
│   └── listTodos.php            # Template/view untuk menampilkan daftar Todo
└── assets
    ├── css
    │   └── style.css            # file CSS untuk styling
    └── js
        └── script.js            #  file JavaScript untuk interaksi tambahan
```

Pada struktur project di atas kita menambahkan 2 file baru yaitu `api.php` dan `.htaccess`

Penjelasan :

- **api.php**:

  - File ini berfungsi sebagai _endpoint_ API untuk operasi CRUD _(Create, Read, Update, Delete)_. Di sini Anda menangani permintaan _HTTP_ yang datang dan menjalankan logika yang sesuai berdasarkan metode permintaan (`GET, POST, PUT, DELETE)`.

- **.htaccess**:

  - File ini digunakan untuk mengonfigurasi pengaturan URL pada server Apache, misalnya untuk mengarahkan semua permintaan yang datang ke `index.php` untuk routing atau ke `API` yang sesuai.
- 
  - `.htaccess` sangat berguna untuk `clean URLs` atau mengarahkan permintaan berdasarkan aturan tertentu.


# Membuat API `(api.php)`

Pertama, buat file baru bernama `api.php `di root direktori proyek Anda:

Selanjutnya tambahkan kode berikut:

```php
<?php

// Aktifkan error reporting untuk memudahkan debugging
ini_set('display_errors', 1);
error_reporting(E_ALL);

// api.php

require_once 'controllers/TodoController.php';
header('Content-Type: application/json');

// Menangani aksi berdasarkan parameter 'action'
$action = $_GET['action'] ?? null;
$controller = new TodoController();

switch ($action) {
    case 'list':
        // Mendapatkan daftar semua todos
        echo json_encode($controller->index());
        break;
    case 'add':
        // Menambahkan todo baru
        if ($_SERVER['REQUEST_METHOD'] === 'POST') {
            $inputData = json_decode(file_get_contents('php://input'), true);
            if (isset($inputData['task']) && !empty($inputData['task'])) {
                $controller->add($inputData['task']);
                echo json_encode(['message' => 'Todo added successfully']);
            } else {
                echo json_encode(['message' => 'Task is required']);
            }
        } else {
            echo json_encode(['message' => 'Invalid request method. Use POST to add a todo.']);
        }
        break;
    case 'complete':
        // Menandai todo sebagai selesai
        $inputData = json_decode(file_get_contents('php://input'), true);
        if (isset($inputData['id'])) {
            $controller->markAsCompleted($inputData['id']);
            echo json_encode(['message' => 'Todo marked as completed']);
        } else {
            echo json_encode(['message' => 'ID is required']);
        }
        break;
    case 'delete':
        // Menghapus todo
        $inputData = json_decode(file_get_contents('php://input'), true);
        if (isset($inputData['id'])) {
            $controller->delete($inputData['id']);
            echo json_encode(['message' => 'Todo deleted']);
        } else {
            echo json_encode(['message' => 'ID is required']);
        }
        break;
    default:
        echo json_encode(['message' => 'Invalid action']);
}
``` 

# Modifikasi `TodoController.php` 

Untuk mengembalikan data dalam format yang sesuai untuk API:

```php
<?php
// controllers/TodoController.php

require_once 'models/TodoModel.php';

class TodoController {
    private $model;

    /**
     * Konstruktor untuk inisialisasi model Todo
     */
    public function __construct() {
        $this->model = new TodoModel();
    }

    /**
     * Fungsi untuk mengambil semua todo
     * @return array
     */
    public function index() {
        return $this->model->getAllTodos();
    }

    /**
     * Fungsi untuk menambahkan todo baru
     * @param string $task
     * @return mixed
     */
    public function add($task) {
        return $this->model->createTodo($task);
    }

    /**
     * Fungsi untuk menandai todo sebagai selesai
     * @param int $id
     * @return mixed
     */
    public function markAsCompleted($id) {
        return $this->model->updateTodoStatus($id, 1);
    }

    /**
     * Fungsi untuk menghapus todo
     * @param int $id
     * @return mixed
     */
    public function delete($id) {
        return $this->model->deleteTodo($id);
    }
}
```
# Modifikasi `index.php `

Untuk menggunakan API

```php
<?php
// index.php

require_once 'controllers/TodoController.php';

$controller = new TodoController();
$todos = $controller->index();

// Merender tampilan
require 'views/listTodos.php';
```
# Memodifikasi `listTodos.php`

Bagian _front-end_ menggunakan AJAX untuk berinteraksi dengan API.

```php
<!DOCTYPE html>
<html>
<head>
    <title>Todo List</title>
    <link rel="stylesheet" type="text/css" href="assets/css/style.css">
    <!-- Menyertakan jQuery dari CDN -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            // Fungsi untuk memuat semua todo dari API
            function loadTodos() {
                $.get('api.php?action=list', function(data) {
                    var todoList = $('#todo-list');
                    todoList.empty();
                    data.forEach(function(todo) {
                        var li = $('<li>').text(todo.task);
                        if (!todo.is_completed) {
                            li.append(' <a href="#" class="complete" data-id="' + todo.id + '">Mark as completed</a>');
                        }
                        li.append(' <a href="#" class="delete" data-id="' + todo.id + '">Delete</a>');
                        todoList.append(li);
                    });
                });
            }

            // Add todo
            // Fungsi untuk menambahkan todo baru
            $('#add-form').submit(function(e) {
                e.preventDefault();
                var task = $('#task').val();
                $.post('api.php?action=add', JSON.stringify({task: task}), function() {
                    $('#task').val('');
                    loadTodos();
                });
            });

            // Complete todo
            // // Fungsi untuk menandai todo sebagai selesai
            $(document).on('click', '.complete', function() {
                var id = $(this).data('id');
                $.ajax({
                    url: 'api.php?action=complete',
                    type: 'PUT',
                    data: JSON.stringify({id: id}),
                    success: loadTodos
                });
            });

            // Delete todo
            // Fungsi untuk menghapus todo
            $(document).on('click', '.delete', function() {
                var id = $(this).data('id');
                $.ajax({
                    url: 'api.php?action=delete',
                    type: 'DELETE',
                    data: JSON.stringify({id: id}),
                    success: loadTodos
                });
            });

            // Initial load
            // Memuat todos pada inisialisasi awal
            loadTodos();
        });
    </script>
</head>
<body>
    <h1>Todo List</h1>
    <ul id="todo-list"></ul> <!-- Daftar todo ditampilkan di sini -->
    <form id="add-form">
        <input type="text" id="task" placeholder="New Task"> <!-- Input untuk menambah todo -->
        <button type="submit">Add</button> <!-- Tombol untuk mengirim form -->
    </form>
</body>
</html>
```
# Pengaturan `.htaccess `

Jika server Anda tidak mendukung metode HTTP `PUT` dan `DELETE`, Anda bisa menggunakan `.htaccess` untuk mengkonfigurasi _routing_ HTTP. 

Buat file `.htaccess` di root folder proyek Anda dan tambahkan kode berikut:

```bash
# RewriteEngine On
# RewriteCond %{REQUEST_METHOD} ^(PUT|DELETE)
# RewriteRule ^(.*)$ api.php [QSA,L]

RewriteEngine On

# Mengarahkan semua permintaan ke index.php
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php [L]

# Mengatur akses ke api.php
RewriteRule ^api/(.*)$ api.php [L,QSA]
```
Pastikan Apache mendukung `mod_rewrite` di server Anda.

# Periksa Kode `api.php`

Pastikan kode `api.php` sudah sesuai dengan contoh kode di atas. 

Ketika Anda mengakses URL `api.php?action=add` melalui browser, itu berarti Anda hanya mengakses API tanpa mengirim data melalui `POST` (yang diperlukan untuk menambahkan todo). 

Namun, kode di `api.php` yang menangani `action=add` belum memeriksa apakah metode `HTTP` yang digunakan adalah `POST`.

Pada kode di atas, `action=add` hanya dapat ditangani dengan metode `POST`, dan kita memeriksa apakah metode yang digunakan adalah `POST`. 

Jika Anda mengaksesnya dengan `GET` (misalnya dengan mengetik URL langsung di browser), Anda akan mendapatkan pesan error yang mengatakan bahwa metode yang digunakan salah.

Output: 

```json
{"message":"Invalid request method. Use POST to add a todo."}
```
# Tes API

Untuk menguji API secara lebih tepat, Anda sebaiknya menggunakan alat seperti [Postman](https://www.postman.com/downloads/) atau [cURL](https://terminalcheatsheet.com/id/guides/curl-rest-api) alih-alih browser karena browser default menggunakan metode `GET`, sedangkan untuk menambahkan `todo`, Anda memerlukan metode `POST`.

- Panduan Install `POSTMAN` : 
  - [Windows](https://apidog-com.translate.goog/blog/download-install-postman/?_x_tr_sl=en&_x_tr_tl=id&_x_tr_hl=id&_x_tr_pto=tc&_x_tr_hist=true)
  - [MacOs](https://apidog-com.translate.goog/blog/download-install-postman/?_x_tr_sl=en&_x_tr_tl=id&_x_tr_hl=id&_x_tr_pto=tc&_x_tr_hist=true)

- **Menggunakan Postman:**

  - Buka Postman.
  - Set method menjadi `POST`.
  - URL: `http://localhost/todo-app/api.php?action=add` (_atau URL yang sesuai dengan server Anda_).
  - Pilih tab `Body` dan pilih `raw` dengan format `JSON`.
  - Masukkan `data JSON` berikut di `Body`:

```json
{
    "task": "Belajar REST API"
}
```
  - Klik Send, dan Anda harus menerima `respons JSON` seperti berikut:

```json
{
    "message": "Todo added successfully"
}
```

- **Menggunakan `cURL` di Terminal:**
  
Jika Anda tidak ingin menggunakan `Postman`, Anda juga bisa menggunakan `cURL` di `terminal` atau `command prompt`.

Panduan install `cURL`: 

- [Windows](https://kb.naverisk.com/en/articles/5569958-how-to-install-curl-in-windows)
- [MacOs](https://formulae.brew.sh/formula/curl)

Disini saya menggunakan `cURL` di terminal VsCode untuk melakukan pengujian API.

![yys](https://i.imgur.com/eUtd6JY.png)


```bash
# Tes API dengan cURL
# Gunakan perintah berikut untuk menguji API dengan cURL
# Untuk URL , ganti dengan URL server Anda

curl -X POST http://localhost:8888/todolist_project/api.php?action=add -H 'Content-Type: application/json' -d '{"task":"Belajar REST API"}'
```

Jika berhasil, Anda akan mendapatkan `respons JSON` yang menunjukkan bahwa `todo` telah `berhasil ditambahkan`.

```json
{
    "message": "Todo added successfully"
}
```
Selanjutnya lihat di browser dengan mengakses URL project dan database Anda, Anda harus melihat bahwa `todo` baru telah `berhasil ditambahkan`.

![alt text](https://i.imgur.com/hjqcaL8.png)

![db](https://i.imgur.com/NObZa6u.png)

Perintah `curl` ini digunakan untuk mengirim permintaan `POST` ke server lokal Anda yang menjalankan `todolist_project` pada `api.php` dengan tindakan `add.` Ini akan menambahkan item baru ke daftar tugas `(todo list)` dengan tugas `"Belajar REST API"`.

**Penjelasan Potongan Baris Perintah:**

1. `curl:`
   
- curl adalah alat baris perintah yang digunakan untuk mentransfer data dari atau ke server menggunakan berbagai protokol, dalam hal ini HTTP.

2. `-X POST:`
   
- `-X `digunakan untuk menentukan metode HTTP yang akan digunakan. Dalam contoh ini, `POST` digunakan untuk mengirim data ke server.

3. `http://localhost:8888/todolist_project/api.php?action=add:`
   
- `URL` tujuan untuk mengirimkan permintaan. `localhost` merujuk pada server lokal (komputer Anda), `8888` adalah nomor `port`, `todolist_project/api.php` adalah lokasi `endpoint API`, dan `action=add` adalah _parameter query_ yang menandakan bahwa aksi yang diminta adalah penambahan data.
  
4. `-H 'Content-Type: application/json':`
   
  - `-H `digunakan untuk mengatur header permintaan. `Content-Type: application/json` menunjukkan bahwa data yang dikirim memiliki tipe konten JSON.
  
5. `-d '{"task":"Belajar REST API"}':`
   
- `-d` digunakan untuk menentukan data yang dikirim dalam permintaan. Dalam contoh ini, data JSON yang dikirim adalah `{"task":"Belajar REST API"}`. Ini berarti bahwa sebuah objek dengan `properti task` dan nilai `"Belajar REST API"` sedang dikirim ke server.

# Eksplorasi REST API

Selanjutnya , silahkan anda boleh menggunakan `Postman` dan `cURL` untuk menguji API.

HTTP Methods berikut:

- `GET api.php?action=list` - Mendapatkan semua todo
- `POST api.php?action=add` - Menambahkan todo baru
- `PUT api.php?action=complete` - Menandai todo sebagai selesai
- `DELETE api.php?action=delete` - Menghapus todo


# Pro Tips

- Pastikan `server localhost` kalian jalan dulu ya, baru mulai `menguji API` ini dan itu. Gak mau kan kalo udah semangat malah gagal di tengah jalan? 😄

- Jangan lupa tes `masing-masing perintah` biar makin paham dan jago!

- Cek kembali` URL` dan `metode` yang digunakan untuk memastikan tidak ada `typo` atau kesalahan.

- Jika menggunakan `Postman`, Postman juga menyediakan fitur untuk menyimpan _request_ dan _collection_, sangat berguna untuk pengujian berulang.

Selamat praktek dan semoga kalian semua makin jago `nge-REST API!` 

_Let's code and have fun!_ 💻🔥🚀