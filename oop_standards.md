# OOP Class Standards

## Standar Baru: Module/Helper Wajib OOP Class

### 1. Struktur Class Helper

Semua module/helper di folder `module/` harus berupa class dengan namespace `NN\Module`:

```php
<?php
namespace NN\Module;

class NamaHelper {
    public static function fungsiStatic($param1, $param2) {
        // Implementation
        return $result;
    }
    
    public static function fungsiLain($param) {
        // Implementation
        return $result;
    }
}
```

### 2. Fungsi Utility

Fungsi utility (seperti UUID, PhotoHelper, dsb) harus menggunakan static function:

```php
<?php
namespace NN\Module;

class UUID {
    public static function generate() {
        return uniqid();
    }
    
    public static function validate($uuid) {
        // Validation logic
        return preg_match('/^[a-f0-9]{13}$/', $uuid);
    }
}

class PhotoHelper {
    public static function upload($file, $path) {
        // Upload logic
        return $uploadedPath;
    }
    
    public static function resize($image, $width, $height) {
        // Resize logic
        return $resizedImage;
    }
}
```

### 3. Cara Penggunaan di Route/File Lain

#### Import Class
```php
<?php
use NN\Module\NamaHelper;
use NN\Module\UUID;
use NN\Module\PhotoHelper;
```

#### Panggil Static Function
```php
// Panggil dengan static method
$result = NamaHelper::fungsiStatic($param1, $param2);
$uuid = UUID::generate();
$photoPath = PhotoHelper::upload($file, '/uploads/');
```

#### Load Module di Route
```php
$route->add('/example/route', function() {
    // Route logic
})
->use('vendor/autoload.php')
->use('module/nama_helper.php')  // Load module file
->use('module/db.php')
->middleware('cekloginadmin');
```

### 4. Contoh Implementasi Lengkap

#### File: module/string_helper.php
```php
<?php
namespace NN\Module;

class StringHelper {
    public static function slugify($string) {
        $string = strtolower($string);
        $string = preg_replace('/[^a-z0-9\s-]/', '', $string);
        $string = preg_replace('/[\s-]+/', '-', $string);
        return trim($string, '-');
    }
    
    public static function truncate($string, $length = 100) {
        if (strlen($string) <= $length) {
            return $string;
        }
        return substr($string, 0, $length) . '...';
    }
    
    public static function randomString($length = 10) {
        $characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
        $randomString = '';
        for ($i = 0; $i < $length; $i++) {
            $randomString .= $characters[rand(0, strlen($characters) - 1)];
        }
        return $randomString;
    }
}
```

#### Penggunaan di Route
```php
<?php
use NN\Module\StringHelper;

$route->add('/api/create-slug', function() {
    $title = $_POST['title'] ?? '';
    $slug = StringHelper::slugify($title);
    
    return json_encode([
        'status' => true,
        'slug' => $slug
    ]);
})
->use('vendor/autoload.php')
->use('module/string_helper.php')
->use('module/db.php')
->middleware('cekloginadmin');
```

### 5. Keuntungan OOP Class

1. **Tidak ada error redeclare**: Setiap class hanya dideklarasikan sekali
2. **Lebih mudah autoload**: Autoloader dapat dengan mudah memuat class
3. **Konsisten dengan best practice OOP PHP**: Mengikuti standar modern PHP
4. **Namespace isolation**: Menghindari konflik nama function
5. **Better organization**: Kode lebih terorganisir dan mudah maintain

### 6. Larangan

**Tidak boleh lagi menggunakan function procedural global** di module/helper:

```php
// ❌ DILARANG - Function procedural global
function generateUUID() {
    return uniqid();
}

function uploadPhoto($file) {
    // Upload logic
}

// ✅ BENAR - Static method dalam class
class UUID {
    public static function generate() {
        return uniqid();
    }
}

class PhotoHelper {
    public static function upload($file) {
        // Upload logic
    }
}
```

### 7. Best Practices

1. **Naming Convention**: Gunakan PascalCase untuk nama class
2. **File Naming**: Nama file harus sama dengan nama class (case sensitive)
3. **Namespace**: Selalu gunakan namespace `NN\Module`
4. **Static Methods**: Gunakan static methods untuk utility functions
5. **Documentation**: Dokumentasikan setiap method dengan PHPDoc
6. **Error Handling**: Implementasikan proper error handling

### 8. Contoh PHPDoc

```php
<?php
namespace NN\Module;

/**
 * Helper class untuk operasi string
 */
class StringHelper {
    /**
     * Mengubah string menjadi slug URL-friendly
     * 
     * @param string $string String yang akan diubah
     * @return string Slug yang sudah dibersihkan
     */
    public static function slugify($string) {
        // Implementation
    }
    
    /**
     * Memotong string dengan panjang tertentu
     * 
     * @param string $string String yang akan dipotong
     * @param int $length Panjang maksimal string
     * @return string String yang sudah dipotong
     */
    public static function truncate($string, $length = 100) {
        // Implementation
    }
}
```

### 9. Migration dari Function Global

Jika ada function global yang sudah ada, migrasikan ke class:

```php
// ❌ Sebelum - Function global
function formatCurrency($amount) {
    return 'Rp ' . number_format($amount, 0, ',', '.');
}

// ✅ Sesudah - Static method dalam class
class CurrencyHelper {
    public static function format($amount) {
        return 'Rp ' . number_format($amount, 0, ',', '.');
    }
}
``` 