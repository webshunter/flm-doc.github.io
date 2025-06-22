# Module Documentation - FLM Builder

## 📁 Overview

FLM Builder menyediakan sistem modul yang terstruktur untuk berbagai fungsi utilitas, database operations, file management, dan UI rendering. Semua modul berada di folder `module/` dan menggunakan namespace `NN\Module` atau `NN\Helper`.

## 🔧 Core Module Files

### 1. **array.php** - Array Extension Helper
```php
namespace NN\Helper;
class ArrayExtention {
    public static function ToInsert($data, $table = 'test', $wht = '') {
        // Converts array data to SQL INSERT statement
        // Supports bulk insert with UNION ALL
        // Handles WHERE conditions for duplicate checking
    }
}
```

**Purpose**: Mengkonversi array PHP menjadi SQL INSERT statement untuk operasi database bulk.

**Features**:
- Bulk insert dengan UNION ALL
- Handling WHERE conditions untuk duplicate checking
- Support untuk multiple table operations

**Usage**:
```php
use NN\Helper\ArrayExtention;

$data = [
    ['name' => 'John', 'email' => 'john@example.com'],
    ['name' => 'Jane', 'email' => 'jane@example.com']
];

$sql = ArrayExtention::ToInsert($data, 'users');
// Result: INSERT INTO users (`name`, `email`) SELECT a.name, a.email FROM (SELECT 'John' `name`, 'john@example.com' `email` UNION ALL SELECT 'Jane' `name`, 'jane@example.com' `email`) a
```

### 2. **cron.php** - Crontab Management
```php
namespace NN\Module;
class crontab {
    // Full crontab management system
    // Load, save, add, delete cron jobs
    // Parse cron expressions
    // Error handling and validation
}
```

**Purpose**: Sistem manajemen crontab lengkap untuk scheduling tasks pada sistem Unix-like.

**Features**:
- Load dan save crontab entries
- Add dan delete cron jobs
- Parse cron expressions
- Error handling dan validation
- Support untuk multiple users

**Usage**:
```php
use NN\Module\crontab;

$cron = new crontab();
$cron->load();
$cron->addCronJob('0 2 * * * /usr/bin/php /path/to/script.php');
$cron->saveCronTable();
```

### 3. **crop.php** - Image Cropping (Placeholder)
```php
class Croper{
    // Currently empty - placeholder for image cropping functionality
}
```

**Purpose**: Placeholder untuk fungsi image cropping (belum diimplementasikan).

### 4. **datatable.php** - DataTables Integration
```php
namespace NN\Module;
class Datatable {
    // Handles DataTables server-side processing
    // Search, pagination, ordering
    // Custom column handling
    // Front/back data manipulation hooks
}
```

**Purpose**: Server-side processing untuk DataTables jQuery plugin dengan search, pagination, dan ordering.

**Features**:
- Server-side processing
- Search functionality
- Pagination support
- Column ordering
- Custom data manipulation hooks
- JSON response formatting

**Usage**:
```php
use NN\Module\Datatable;

$datatable = new Datatable();
$datatable->query("SELECT * FROM users WHERE name LIKE '%{search}%' LIMIT {start}, {length}");
$datatable->column(['id', 'name', 'email']);
$datatable->defaultOrder('id', 'ASC');
$datatable->run();
```

### 5. **dd.php** - Debug Helper
```php
namespace NN\Module;
class DD {
    public static function view($d = null){
        saged($d); // Debug output function
    }
}
```

**Purpose**: Helper sederhana untuk debugging development.

**Usage**:
```php
use NN\Module\DD;

DD::view($variable);
```

### 6. **help.php** - Utility Functions
```php
namespace NN\Module;
class Help {
    // Excel to array conversion
    // Number to Excel column letters
    // Currency formatting (Rupiah)
    // Database query counting
    // JSON response handling
}
```

**Purpose**: Koleksi fungsi utilitas untuk operasi umum.

**Features**:
- Excel file reading dengan PhpSpreadsheet
- Konversi angka ke huruf kolom Excel
- Formatting mata uang Rupiah
- Database query counting
- JSON response handling

**Usage**:
```php
use NN\Module\Help;

// Read Excel file
$data = Help::readExcel('file.xlsx');

// Convert number to Excel column
$column = Help::numberToLetters(27); // Returns 'AA'

// Format currency
$formatted = Help::rupiah(1000000); // Returns 'Rp 1.000.000'

// Count database records
$count = Help::dbqueryNum("SELECT * FROM users WHERE status = 'active'");

// JSON response
Help::json(['status' => true, 'data' => $data]);
```

### 7. **perusahaan.php** - Company Data
```php
namespace NN\Module;
class Perusahaan {
    public static function get($kode = "nama_perusahaan"){
        // Get company profile data from database
    }
    public static function datalogin(){
        // Get current login session data
    }
}
```

**Purpose**: Manajemen data perusahaan dan session data.

**Usage**:
```php
use NN\Module\Perusahaan;

// Get company name
$companyName = Perusahaan::get('nama_perusahaan');

// Get login data
$loginData = Perusahaan::datalogin();
```

### 8. **photo_helper.php** - Photo Management
```php
namespace NN\Module;
class PhotoHelper {
    public static function copyProfilePhoto($filename, $id_biodata) {
        // Copy profile photos from FLAMBOYAN_PATH to public directory
        // Handle file permissions and error logging
        // Return public path to photo
    }
}
```

**Purpose**: Manajemen foto profil dengan file copying dan path handling.

**Features**:
- Copy foto dari FLAMBOYAN_PATH ke public directory
- Handle file permissions
- Error logging
- Default photo fallback
- File modification time checking

**Usage**:
```php
use NN\Module\PhotoHelper;

$photoPath = PhotoHelper::copyProfilePhoto('user.jpg', '123');
// Returns: 'image/profile/user.jpg' or default path
```

### 9. **uuid.php** - UUID Generation
```php
namespace NN\Module;
class Uuid {
    public static function new(){
        // Generate UUID v4 using Ramsey library
    }
}
```

**Purpose**: Generate UUID untuk unique identifiers.

**Usage**:
```php
use NN\Module\Uuid;

$id = Uuid::new(); // Returns UUID v4 string
```

### 10. **view.php** - View Rendering System
```php
namespace NN\Module;
class View {
    // Blade template rendering
    // JavaScript asset management
    // File modification time tracking
    // Multi-file JavaScript bundling
    // IP address detection
}
```

**Purpose**: Sistem rendering view lengkap dengan template engine dan asset management.

**Features**:
- Blade template rendering
- JavaScript asset management dengan cache busting
- File modification time tracking
- Multi-file JavaScript bundling
- IP address detection
- Asset versioning

**Usage**:
```php
use NN\Module\View;

// Render Blade template
View::render('template', ['data' => $data]);

// Include JavaScript with cache busting
echo View::sc('script.js');

// Get file modification time
$time = View::time();

// Multi-file JavaScript bundling
View::multijs('main.js');
```

### 11. **fm.php** - File Manager (Tiny File Manager)
```php
// Complete file management system
// Authentication, file operations
// Upload, download, edit, delete
// ZIP/TAR support
// Security features
```

**Purpose**: File manager lengkap dengan web interface (Tiny File Manager v2.5.3).

**Features**:
- Authentication system
- File upload/download
- File editing
- ZIP/TAR support
- Security features (IP whitelisting/blacklisting)
- Online document viewer
- File permissions management

## 🎯 Key Features & Patterns

### **Namespace Organization**
```php
// All modules use consistent namespaces
namespace NN\Module;  // Main modules
namespace NN\Helper;  // Helper classes
```

### **Static Method Pattern**
```php
// Most utility classes use static methods
// Easy to use without instantiation
Perusahaan::get('nama_perusahaan');
Help::rupiah(1000000);
Uuid::new();
```

### **Error Handling**
```php
// Consistent error handling across modules
try {
    // Module operations
} catch (Exception $e) {
    error_log("Module error: " . $e->getMessage());
    // Graceful fallback
}
```

### **Security Features**
- Input validation dan sanitization
- File permission management
- Authentication systems
- IP whitelisting/blacklisting
- SQL injection prevention

### **Database Integration**
```php
// Direct integration with DB class
require_once SETUP_PATH.'module/db.php';
use NN\Module\DB;

$data = DB::query_result_object("SELECT * FROM table");
```

## 🔄 Usage Patterns

### **Typical Module Usage**
```php
// In routes or controllers
use NN\Module\Help;
use NN\Module\Perusahaan;
use NN\Module\Uuid;

// Get company data
$companyName = Perusahaan::get('nama_perusahaan');

// Generate UUID
$id = Uuid::new();

// Format currency
$formatted = Help::rupiah(1000000);
```

### **DataTables Integration**
```php
$datatable = new Datatable();
$datatable->query("SELECT * FROM users WHERE name LIKE '%{search}%'");
$datatable->column(['id', 'name', 'email']);
$datatable->run();
```

### **File Management**
```php
// Photo handling
$photoPath = PhotoHelper::copyProfilePhoto('user.jpg', '123');

// View rendering
View::render('template', ['data' => $data]);
```

## 📊 Module Dependencies

### **Core Dependencies**
```php
// Required in most modules
require_once SETUP_PATH.'vendor/autoload.php';
require_once SETUP_PATH.'module/db.php';
```

### **External Libraries**
- **Ramsey UUID** - UUID generation
- **Jenssegers Blade** - Template engine
- **PhpSpreadsheet** - Excel file handling
- **Prism.js** - Syntax highlighting

## 🛠️ Development Best Practices

### **1. Module Creation**
```php
<?php
namespace NN\Module;
require_once SETUP_PATH.'vendor/autoload.php';

class NewModule {
    public static function method($param) {
        // Implementation
        return $result;
    }
}
```

### **2. Error Handling**
```php
public static function safeOperation($data) {
    try {
        // Operation logic
        return $result;
    } catch (Exception $e) {
        error_log("Module error: " . $e->getMessage());
        return false;
    }
}
```

### **3. Input Validation**
```php
public static function processData($input) {
    if (empty($input)) {
        return false;
    }
    
    // Sanitize input
    $clean = filter_var($input, FILTER_SANITIZE_STRING);
    
    // Process data
    return $result;
}
```

### **4. Database Operations**
```php
public static function getData($id) {
    $db = new DB();
    $id = (int)$id; // Type casting for security
    
    return $db->query_result_object_row(
        "SELECT * FROM table WHERE id = '$id'"
    );
}
```

## 📋 Module Checklist

### **Required Elements**
- [ ] Proper namespace (`NN\Module` or `NN\Helper`)
- [ ] Required dependencies (`vendor/autoload.php`, `db.php`)
- [ ] Error handling with try-catch
- [ ] Input validation and sanitization
- [ ] Proper documentation and comments
- [ ] Consistent method naming
- [ ] Return type consistency

### **Optional Elements**
- [ ] Static methods for utility functions
- [ ] Configuration options
- [ ] Logging functionality
- [ ] Cache mechanisms
- [ ] Performance optimizations

## 🎯 Integration with Routes

### **Module Usage in Routes**
```php
$route->add('/api/users', function(){
    use NN\Module\Help;
    use NN\Module\Uuid;
    
    $id = Uuid::new();
    $formatted = Help::rupiah(1000000);
    
    return json_encode([
        'status' => true,
        'data' => ['id' => $id, 'amount' => $formatted]
    ]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/help.php')
->use('module/uuid.php')
->middleware('cekloginadmin');
```

## 🔧 Configuration

### **Module Configuration**
```php
// In module files
define('MODULE_CONFIG', [
    'debug' => false,
    'cache' => true,
    'timeout' => 30
]);
```

### **Path Configuration**
```php
// Required constants
define('SETUP_PATH', '/path/to/project/');
define('FLAMBOYAN_PATH', '/path/to/flamboyan/');
```

## 📈 Performance Considerations

### **Optimization Tips**
1. **Use static methods** for utility functions
2. **Implement caching** for expensive operations
3. **Batch database operations** when possible
4. **Lazy loading** for heavy dependencies
5. **Memory management** for large file operations

### **Monitoring**
```php
// Add performance monitoring
$start_time = microtime(true);
// Module operation
$end_time = microtime(true);
$execution_time = ($end_time - $start_time);

if ($execution_time > 1) {
    error_log("Slow module operation: " . $execution_time . "s");
}
```

## 🎯 Conclusion

Sistem modul FLM Builder menyediakan foundation yang solid untuk berbagai operasi web development. Dengan struktur yang konsisten, error handling yang baik, dan integrasi database yang seamless, modul-modul ini memudahkan pengembangan aplikasi yang robust dan maintainable.

Setiap modul memiliki tujuan spesifik dan dapat digunakan secara independen atau dikombinasikan untuk membangun fitur yang kompleks. Pola penggunaan yang konsisten memastikan kemudahan maintenance dan pengembangan di masa depan. 