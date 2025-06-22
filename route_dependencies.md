# Route Dependencies

## Standar Dependensi Route

Setiap route di folder `web/api/flamboyan` harus selalu menambahkan dependensi berikut:

```php
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

## Contoh Penggunaan

```php
$route->add('/flamboyan/example/{param}', function($param) {
    // Route logic here
    View::render('flamboyan/example', $data);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

## Penjelasan Dependensi

### 1. vendor/autoload.php
- **Fungsi**: Autoloading class dan dependencies
- **Kebutuhan**: Wajib untuk semua route
- **Tujuan**: Memuat semua library dan class yang diperlukan

### 2. module/db.php
- **Fungsi**: Koneksi database utama
- **Kebutuhan**: Wajib untuk semua route yang membutuhkan database
- **Tujuan**: Menyediakan akses ke database utama aplikasi

### 3. module/secondb.php
- **Fungsi**: Koneksi database sekunder
- **Kebutuhan**: Wajib untuk semua route Flamboyan
- **Tujuan**: Menyediakan akses ke database sekunder untuk data TKI

### 4. module/perusahaan.php
- **Fungsi**: Data perusahaan
- **Kebutuhan**: Wajib untuk semua route Flamboyan
- **Tujuan**: Menyediakan informasi perusahaan yang diperlukan

### 5. middleware('cekloginadmin')
- **Fungsi**: Pengecekan login admin
- **Kebutuhan**: Wajib untuk semua route admin
- **Tujuan**: Memastikan hanya admin yang sudah login yang bisa mengakses

## Catatan Penting

1. **Urutan Dependensi**: Urutan dependensi harus sesuai seperti di atas
2. **Konsistensi**: Jangan menghapus atau mengubah urutan dependensi yang sudah ada
3. **Kewajiban**: Pastikan untuk selalu menambahkan dependensi ini di setiap route baru yang dibuat
4. **Pengecualian**: Hanya route yang tidak membutuhkan database yang boleh tidak menggunakan dependensi database

## Contoh Route Lengkap

```php
<?php
use NN\Route;
use NN\Module\View;
use NN\Module\DB;
use NN\Module\SECOND_DB;

$route->add('/flamboyan/biodata/{id}', function($id) {
    $db = new DB();
    $secondDb = new SECOND_DB();
    
    // Query data dari database utama
    $biodata = $db->query_result_object_row("SELECT * FROM biodata WHERE id = ?", [$id]);
    
    // Query data dari database sekunder
    $personal = $secondDb->query_result_object_row("SELECT * FROM personal WHERE id_biodata = ?", [$id]);
    
    $data = [
        'biodata' => $biodata,
        'personal' => $personal
    ];
    
    View::render('flamboyan/biodata/detail', $data);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

## Troubleshooting

### Error: Class not found
- **Penyebab**: Dependensi `vendor/autoload.php` tidak dimuat
- **Solusi**: Pastikan dependensi autoload dimuat pertama

### Error: Database connection failed
- **Penyebab**: Dependensi database tidak dimuat atau konfigurasi salah
- **Solusi**: Periksa urutan dependensi dan konfigurasi database

### Error: Unauthorized access
- **Penyebab**: Middleware `cekloginadmin` tidak dimuat
- **Solusi**: Pastikan middleware dimuat terakhir dalam chain dependensi 