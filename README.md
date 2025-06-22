# FLM Builder - Dokumentasi Lengkap & Panduan Implementasi

## 📋 Daftar Isi
1. [Pendahuluan](#pendahuluan)
2. [Core Components](#core-components)
3. [Implementation Steps](#implementation-steps)
4. [Routing System](#routing-system)
5. [Database Connection](#database-connection)
6. [CRUD Implementation](#crud-implementation)
7. [View Module & Blade Integration](#view-module--blade-integration)
8. [API Development](#api-development)
9. [Best Practices](#best-practices)
10. [Troubleshooting](#troubleshooting)
11. [MySQL Functions](#mysql-functions)
12. [Route Dependencies](#route-dependencies)
13. [Dashboard Updates](#dashboard-updates)
14. [OOP Class Standards](#oop-class-standards)
15. [Blade Script Standards](#blade-script-standards)

## 🎯 Pendahuluan

FLM Builder adalah sistem modern untuk membangun aplikasi web dengan arsitektur modular yang menggunakan routing modern, Blade template engine, dan sistem CRUD yang fleksibel.

### Fitur Utama
- ✅ Modern Routing System
- ✅ Blade Template Engine
- ✅ Modular CRUD Implementation
- ✅ Dual Database Support
- ✅ API Development Framework
- ✅ Security Features
- ✅ Environment Configuration
- ✅ Error Handling

## 🔧 Core Components

### 1. crud.js
- Main configuration file for CRUD operations
- Handles form titles, table operations, and data management
- Controls server-side processing and form validation

### 2. formbuild.blade.php
- View template for CRUD interface
- Extends admin layout
- Includes table templates and CRUD JavaScript

### 3. dashboard.php
- Routing configuration for admin pages
- Handles URL mapping to CRUD views

## 🚀 Implementation Steps

### 1. Model Analysis
- Location: flamboyan-app/application/modules/[nama_modul]/models/
- Identify:
  - Table name
  - Column structure
  - Primary key
  - Required fields

### 2. Basic Configuration
```javascript
loadCRUD({
    title: function () { return 'MODULE_NAME'; },
    titlePage: 'Module Name',
    subTitlePage: 'Setting',
    table: "table_name",
    idform: "containerforms",
    kode: 'primary_key',
    serverSide: true
});
```

### 3. Table Configuration
- Define column titles and database mappings
- Configure SQL query template
- Set up server-side processing

### 4. Form Configuration
- Required field validation
- Field definitions with properties:
  - title: Field label
  - type: input type (text/select/textarea)
  - name: database column name
  - row: width (12 for full width)
  - data: options for select fields

### 5. View Creation
- Create in views/flamboyan/setting/[namamodul].blade.php
- Extend admin template
- Include necessary JavaScript

### 6. Route Setup
- Add to web/admin/dashboard.php
- Include middleware and dependencies
- Configure URL mapping

### 7. Navigation Integration
- Add to views/temp/nav/usp.blade.php
- Place in appropriate menu section
- Configure menu item properties

## ⚙️ Routing System

### Basic Route Structure
```php
$route->add('/path/to/route', function(){
    View::render('view/path');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### Route Components
1. Route Definition
   - `$route->add('/path/to/route', function(){})`
   - First parameter: URL path
   - Second parameter: Callback function

2. View Rendering
   - `View::render('view/path')`
   - Renders the specified view template
   - Can pass data as second parameter: `View::render('view/path', ["data" => $value])`

3. Dependencies
   - `->use('vendor/autoload.php')` - Autoloader
   - `->use('module/db.php')` - Database module
   - `->use('module/perusahaan.php')` - Company module

4. Middleware
   - `->middleware('cekloginadmin')` - Admin authentication check

### Route Categories
1. Admin Routes
   - `/admin/dashboard` - Main dashboard
   - `/admin/traffic` - Traffic monitoring
   - `/admin/formbuild` - Form builder
   - `/admin/crud/{id}` - CRUD operations

2. Setting Routes
   - Pattern: `/flamboyan/{module}/setting`
   - Example: `/flamboyan/bank/setting`
   - Renders: `flamboyan/setting/{module}`

3. Admin Module Routes
   - Pattern: `/flamboyan/{module}/admin`
   - Example: `/flamboyan/agen/admin`
   - Renders: `flamboyan/admin/{module}`

## 🗄️ Database Connection

### Primary Database (db.php)
```php
namespace NN\Module;
class DB {
    public $host;
    public $user;
    public $pass;
    public $db;
    private $pdo;
}
```

### Secondary Database (secondb.php)
```php
namespace NN\Module;
class SECOND_DB {
    public $host;
    public $user;
    public $pass;
    public $db;
    private $pdo;
}
```

### Usage in Routes
```php
$route->add('/path', function(){
    // Route handler
})
->use('vendor/autoload.php')
->use('module/db.php')        // Primary database
->use('module/secondb.php')   // Secondary database
->middleware('cekloginadmin');
```

## 📊 CRUD Implementation

### Core Files Structure
```
script/
├── crud.js              # Main CRUD functionality
├── form/
│   ├── input.js        # Input field handling
│   ├── number.js       # Number field handling
│   ├── radio.js        # Radio button handling
│   └── select.js       # Select field handling
├── tool/
│   └── terbilang.js    # Number to text conversion
├── tools.js            # Utility functions
├── forms.js            # Form management
├── callform.js         # Form callbacks
└── button.js           # Button handling
```

### loadCRUD Configuration Object
```javascript
loadCRUD({
    // Basic Configuration
    title: function() { return 'MODULE_NAME'; },
    titlePage: 'Module Name',
    subTitlePage: 'Setting',
    table: "table_name",
    idform: "containerforms",
    kode: 'primary_key',
    serverSide: true,

    // Table Configuration
    titleTable: ['Column1', 'Column2'],
    view: ['field1', 'field2'],
    dataSelect: 'field1, field2',
    queryTemp: 'SELECT {select} FROM table',

    // Form Configuration
    data: [
        {
            title: 'Field Label',
            type: 'text|select|number|date|radio|textarea',
            name: 'field_name',
            row: 12,
            data: []
        }
    ],
    validasiForm: ['required_field1', 'required_field2']
});
```

## 🎨 View Module & Blade Integration

### View Class Structure
```php
namespace NN\Module;
use Jenssegers\Blade\Blade;
use NN\Files;

class View {
    public static function render($name = '', $arg = [])
    public static function tailwind($name = '', $arg = [])
    public static function sc($file, $type="script")
    public static function js($path = "")
    public static function jsm($path = "")
    public static function jsx($path = "")
}
```

### Basic View Structure
```php
@extends('temp.admin')
@php
use NN\Module\DB;
use NN\Module\View;
@endphp

@section("content")
    <!-- Main content here -->
@endsection

@section('css')
    <!-- Custom CSS if needed -->
@endsection

@section('script')
    <!-- Custom scripts if needed -->
@endsection
```

## 🔌 API Development

### Basic API Route Structure
```php
$route->add('/api/[endpoint]', function(){
    return json_encode([
        'status' => true,
        'message' => 'Success',
        'data' => $data
    ]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### API Response Format
```php
// Success Response
return json_encode([
    'status' => true,
    'message' => 'Success',
    'data' => $data
]);

// Error Response
return json_encode([
    'status' => false,
    'message' => 'Error message',
    'error' => $error_details
]);
```

### API File Structure
```
Root/
├── web/
│   ├── admin/           # Admin routes
│   └── api/            # API routes
│       └── flamboyan/  # API endpoints
│           ├── users.php
│           ├── auth.php
│           └── ...
```

## 📋 Best Practices

### 1. Configuration
- Use descriptive field names
- Implement proper validation
- Configure appropriate permissions
- Set up proper error handling

### 2. Performance
- Enable server-side processing for large datasets
- Use appropriate pagination
- Optimize query performance
- Implement proper indexing

### 3. Security
- Validate all inputs
- Use prepared statements
- Implement proper access control
- Sanitize output data

### 4. User Experience
- Provide clear feedback
- Implement proper loading states
- Use appropriate field types
- Maintain consistent styling

## 🛠️ Troubleshooting

### Common Issues & Solutions
1. Column Name Mismatch
   - Solution: Verify database schema matches configuration

2. Form Validation Errors
   - Solution: Check validasiForm array includes all required fields

3. Select Field Issues
   - Solution: Always provide default options and proper data structure

4. Route Access Problems
   - Solution: Verify middleware configuration and dependencies

### Testing Checklist
1. Form submission
2. Data validation
3. Table display
4. Edit functionality
5. Delete operations
6. Navigation access
7. Server-side processing
8. Error handling

## 📦 Required Packages and Dependencies

### Core Framework Components
```json
{
    "illuminate/bus": "v8.83.27",
    "illuminate/collections": "v8.83.27",
    "illuminate/container": "v8.83.27",
    "illuminate/contracts": "v8.83.27",
    "illuminate/events": "v8.83.27",
    "illuminate/filesystem": "v8.83.27",
    "illuminate/macroable": "v8.83.27",
    "illuminate/pipeline": "v8.83.27",
    "illuminate/support": "v8.83.27",
    "illuminate/view": "v8.83.27"
}
```

### Template Engine
```json
{
    "jenssegers/blade": "v1.4.0"
}
```

### Database and Query Helpers
```json
{
    "gugusd999/db_query_helper": "v0.0.2",
    "brick/math": "0.9.3",
    "doctrine/inflector": "2.0.10"
}
```

### File Processing
```json
{
    "phpoffice/phpword": "^1.3",
    "phpoffice/phpexcel": "^1.8",
    "mpdf/mpdf": "^8.2"
}
```

## 🔄 Environment Configuration

### Environment File Structure
```
.env
├── Database Configuration
│   ├── DB_HOST
│   ├── DB_USER
│   ├── DB_PASS
│   └── DB_NAME
├── Application Settings
│   ├── APP_NAME
│   ├── APP_URL
│   └── APP_ENV
├── Path Variables
│   ├── {APP} - Application root path
│   ├── {ROOT} - Project root path
│   └── FLAMBOYAN_PATH - Path to CI3 reference application
└── Other Settings
    ├── HOST
    ├── USERNAME
    ├── PASSWORD
    └── DATABASE
```

### Path Variables
1. Application Paths:
   ```
   FLAMBOYAN_PATH=../../flamboyan/    # Path to CI3 reference application
   {APP}                              # Current application root
   {ROOT}                             # Project root directory
   ```

## 🎯 Project Structure Overview

### Project Components
```
Root/
├── flamboyan-app/           # Reference Application (CI3 HMVC)
│   └── application/
│       └── modules/         # Source code reference
│           ├── [module1]/   # Used as reference for new modules
│           ├── [module2]/
│           └── ...
├── web/                     # New Application Routes
├── views/                   # New Application Views
├── module/                  # New Application Modules
└── public/                  # Public Assets
```

### Reference Application (flamboyan-app)
1. Purpose
   - Serves as reference for business logic
   - Source of module requirements
   - Database structure reference
   - NOT part of the new application

2. Module Reference
   ```
   modules/
   ├── [module_name]/        # Reference module structure
   │   ├── controllers/      # Business logic reference
   │   ├── models/          # Data structure reference
   │   ├── views/           # UI/UX reference
   │   └── config/          # Configuration reference
   ```

### New Application Development
1. Independent Structure
   - Modern routing system
   - Blade template engine
   - Modular CRUD implementation
   - Enhanced security features

2. Module Implementation
   - Create new modules based on reference
   - Implement using new CRUD system
   - Maintain data compatibility
   - Improve upon existing features

## 📝 Standar Baru

### Module/Helper Wajib OOP Class
```php
namespace NN\Module;
class NamaHelper {
    public static function fungsiStatic(...) { ... }
}
```

### Script Loading di Blade
```php
@section('script')
    {!! View::jsm('path/to/script.js') !!}
@endsection
```

### Komunikasi Data Blade ke JS
- Gunakan variabel global seperti `app_id_biodata`
- Hindari `{id_biodata}` langsung di JS

### Standar Foto di Template Detail
- Logic fallback: `photo_path` → `assets/uploads/default-user.png`
- Bungkus dengan `<a class='profile-pic'>`

## 🎯 Kesimpulan

FLM Builder adalah sistem yang powerful untuk membangun aplikasi web modern dengan arsitektur yang modular dan fleksibel. Dokumentasi ini mencakup semua aspek yang diperlukan untuk development, maintenance, dan troubleshooting sistem. 

## 🗄️ MySQL Functions

### 1. nama_majikan Function
```sql
DELIMITER $$

CREATE FUNCTION nama_majikan(p_id_biodata VARCHAR(50))
RETURNS VARCHAR(255)
DETERMINISTIC
BEGIN
    DECLARE v_namamajikan VARCHAR(255);

    SELECT IFNULL(NULLIF(NULLIF(m.namamajikan, ''), '0'), d.nama)
    INTO v_namamajikan
    FROM majikan m
    LEFT JOIN datamajikan d ON m.kode_majikan = d.id_majikan
    WHERE m.id_biodata = p_id_biodata
    LIMIT 1;

    RETURN v_namamajikan;
END$$

DELIMITER ;
```

#### Function Description:
- **Purpose**: Retrieves the employer's name (nama majikan) based on biodata ID with fallback logic
- **Parameter**: 
  - `p_id_biodata` (VARCHAR(50)): The biodata ID to look up
- **Return**: VARCHAR(255) - The employer's name
- **Logic Flow**:
  1. First checks `m.namamajikan` from majikan table
  2. If empty or '0', falls back to `d.nama` from datamajikan table
  3. Uses LEFT JOIN to ensure all records are returned even without matching datamajikan
- **Usage**: 
  ```sql
  SELECT nama_majikan('BIODATA123'); -- Returns employer name for biodata ID 'BIODATA123'
  ```
- **Notes**:
  - Function is DETERMINISTIC (always returns same result for same input)
  - Uses IFNULL and NULLIF for fallback logic
  - Joins majikan and datamajikan tables
  - Returns NULL if no matching record found
  - Used in various queries to get employer information
  - Handles empty strings and '0' values as NULL

## 🔗 Route Dependencies

### Standar Dependensi Route
Setiap route di folder web/api/flamboyan harus selalu menambahkan dependensi berikut:
```php
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### Contoh Penggunaan
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

### Catatan Penting
1. Pastikan untuk selalu menambahkan dependensi ini di setiap route baru yang dibuat
2. Dependensi ini diperlukan untuk:
   - Autoloading class dan dependencies (vendor/autoload.php)
   - Koneksi database utama (module/db.php)
   - Koneksi database sekunder (module/secondb.php)
   - Data perusahaan (module/perusahaan.php)
   - Pengecekan login admin (middleware cekloginadmin)
3. Urutan dependensi harus sesuai seperti di atas
4. Jangan menghapus atau mengubah urutan dependensi yang sudah ada

## 📊 Dashboard Updates

### Dashboard Baru
Dashboard telah diperbarui dengan mengacu pada aplikasi Flamboyan yang ada. Fitur-fitur yang ditambahkan:

1. **Statistik TKI per Sektor:**
   - Male Formal (MF)
   - Male Informal (MI) 
   - Female Formal (FF)
   - Female Informal (FI)
   - Panti Jompo (JP)

2. **Data Detail per Sektor:**
   - Total data per sektor
   - Jumlah yang sudah terbang
   - Jumlah MD/UNFIT (Mengundurkan Diri/UNFIT)

3. **Chart Visualisasi:**
   - Pie chart untuk persentase data TKI keseluruhan
   - Donut chart untuk persentase data TKI per sponsor

4. **Tabel Data Terbaru:**
   - Data majikan terbaru
   - Data passport terbaru
   - Data terbang terbaru
   - Data MD/UNFIT terbaru

### Route yang Ditambahkan
```php
// Dashboard utama dengan data statistik
$route->add('/admin/dashboard', function() {
    // Query data statistik TKI
    // Render view dashboard_flamboyan
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');

// Route AJAX untuk data terbaru
$route->add('/admin/dashboard/majikan-terbaru', function() {
    // Return data majikan terbaru dalam format JSON
});

$route->add('/admin/dashboard/passport-terbaru', function() {
    // Return data passport terbaru dalam format JSON
});

$route->add('/admin/dashboard/terbang-terbaru', function() {
    // Return data terbang terbaru dalam format JSON
});

$route->add('/admin/dashboard/md-unfit-terbaru', function() {
    // Return data MD/UNFIT terbaru dalam format JSON
});
```

### View yang Dibuat
- `views/admin/dashboard_flamboyan.blade.php` - View dashboard utama dengan statistik dan chart

### Database yang Digunakan
- Menggunakan `SECOND_DB` untuk query data TKI
- Tabel utama: `personal`, `majikan`, `paspor`, `visa`, `datasponsor`
- Filter berdasarkan `id_biodata` dengan prefix sektor (MF-, MI-, FF-, FI-, JP-)

### Catatan Implementasi
1. Semua route menggunakan dependensi standar Flamboyan
2. Data diambil secara real-time dari database
3. Chart menggunakan library ECharts
4. Tabel menggunakan DataTables dengan AJAX
5. Responsive design untuk berbagai ukuran layar

## 🏗️ OOP Class Standards

### Standar Baru: Module/Helper Wajib OOP Class

1. **Semua module/helper di folder `module/` harus berupa class** (menggunakan namespace `NN\Module`).
2. **Fungsi utility** (seperti UUID, PhotoHelper, dsb) harus menggunakan static function.
3. **Contoh struktur class helper:**

```php
namespace NN\Module;
class NamaHelper {
    public static function fungsiStatic(...) { ... }
}
```

4. **Cara penggunaan di route/file lain:**
   - Tambahkan di atas file:
     ```php
     use NN\Module\NamaHelper;
     ```
   - Panggil dengan:
     ```php
     NamaHelper::fungsiStatic(...);
     ```
   - Pastikan file module di-load dengan:
     ```php
     ->use('module/nama_helper.php')
     ```

5. **Tidak boleh lagi menggunakan function procedural global** di module/helper.
6. **Keuntungan:**
   - Tidak ada error redeclare
   - Lebih mudah autoload
   - Konsisten dengan best practice OOP PHP

## 📝 Blade Script Standards

### Standar Pemanggilan Script di Blade
- Semua pemanggilan script (termasuk View::jsm(...)) harus selalu berada di dalam @section('script') pada file Blade.
- Hindari penggunaan <script src=...> manual dan juga @push('js') untuk script utama modul.
- Contoh yang benar:

```php
@section('script')
    {!! View::jsm('path/to/script.js') !!}
@endsection
```

- Jika ada style tambahan:

```php
@section('script')
    <style>
        /* custom style */
    </style>
    {!! View::jsm('path/to/script.js') !!}
@endsection
```

### Standar Komunikasi Data Blade ke JS
- Untuk komunikasi data antara Blade dan JS (misal id_biodata), gunakan variabel global seperti `app_id_biodata` yang di-set di JS dari Blade.
- Jangan gunakan `{id_biodata}` di dalam file JS, karena JS di-load secara noscript/dinamis dan tidak bisa akses variabel Blade secara langsung.
- Pastikan variabel global di-set sebelum script JS utama di-load.

### Standar Penentuan & Penampilan Foto di Template Detail
- Untuk setiap template detail yang membutuhkan foto profil, gunakan logic penentuan foto seperti di temp2/detailpersonal:
    - Jika photo_path ada, gunakan PATH/photo_path
    - Jika tidak, fallback ke assets/uploads/default-user.png
- Bungkus <img> dengan <a class='profile-pic'> untuk konsistensi dan kemudahan pengembangan fitur upload/modal.
- Standar ini wajib diikuti saat membuat config/template baru.

## 🔧 Special Notes

### Catatan Khusus: singleData pada family.js
- Pada file `script/flamboyan/admin/family.js` terdapat konfigurasi `singleData: true` pada loadCRUD.
- Artinya: **halaman ini hanya mengatur dan menampilkan satu data saja (single record) untuk setiap id_biodata, bukan list banyak data**.
- Efek di CRUD:
  - Tidak ada tabel list, hanya tampilan detail satu data (atau form tambah jika belum ada).
  - Tombol yang muncul hanya "Tambah Data" (jika belum ada) atau "Update Data" (jika sudah ada).
  - Query otomatis LIMIT 1 dan hanya mengambil satu baris data.
- Cocok untuk data yang memang hanya boleh satu per parent (misal: data keluarga per biodata).
- Implementasi ini berbeda dengan mode default CRUD yang menampilkan banyak baris data dalam tabel.

### Query Template Notes (CRUD JS)
- Pada file-file CRUD JS (misal: script/flamboyan/admin/pengalaman.js), terdapat pola penulisan query SQL pada properti `queryTemp` seperti berikut:
  
  ```js
  queryTemp: "SELECT {select} FROM pengalaman WHERE id_biodata = '" + (_id('app_id_biodata').innerText)+"' || ORDER BY id_pengalaman DESC",
  ```
- **Catatan penting:**
  - Selalu ada tanda `||` sebelum `ORDER BY` di query.
  - Tanda `||` ini tidak umum dalam SQL standar. Di beberapa database, `||` adalah operator konkatenasi string, namun di MySQL biasanya menggunakan `CONCAT()` atau `+`.
  - Jika query ini dijalankan di MySQL, `||` akan dianggap sebagai operator logika OR, sehingga query menjadi:
    ```sql
    SELECT ... FROM pengalaman WHERE id_biodata = '...' || ORDER BY id_pengalaman DESC
    ```
    Ini bisa menyebabkan error atau hasil tidak diharapkan.
  - Kemungkinan besar, tanda `||` ini digunakan sebagai placeholder yang akan diproses/diganti di backend sebelum query dieksekusi.
- **Saran:**
  - Pastikan ada proses penggantian/parse di backend untuk menghilangkan atau mengganti `||` sebelum query dijalankan ke database.
  - Jika tidak ada proses tersebut, pertimbangkan untuk memperbaiki query agar sesuai standar SQL.
- **Kesimpulan:**
  - Tanda `||` sebelum `ORDER BY` adalah pola konsisten di file CRUD JS.
  - Dokumentasikan alasan penggunaan tanda ini agar tidak membingungkan developer lain. 