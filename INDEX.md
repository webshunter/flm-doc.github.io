# FLM Builder - Overview

## 🎯 Tentang FLM Builder

FLM Builder adalah sistem modern untuk membangun aplikasi web dengan arsitektur modular yang menggunakan routing modern, Blade template engine, dan sistem CRUD yang fleksibel.

### 🚀 Fitur Utama

- **Modern Routing System** - Sistem routing yang fleksibel dan mudah dikonfigurasi
- **Blade Template Engine** - Template engine yang powerful untuk view rendering
- **Modular CRUD Implementation** - Sistem CRUD yang modular dan dapat dikustomisasi
- **Dual Database Support** - Dukungan untuk database primer dan sekunder
- **API Development Framework** - Framework untuk pengembangan API yang terstruktur
- **Security Features** - Fitur keamanan yang komprehensif
- **Environment Configuration** - Konfigurasi environment yang fleksibel
- **Error Handling** - Penanganan error yang robust

## 📚 Struktur Dokumentasi

### 1. **Documentation** - Dokumentasi lengkap sistem
   - Core Components
   - Implementation Steps
   - Routing System
   - Database Connection
   - CRUD Implementation
   - View Module & Blade Integration
   - API Development
   - Best Practices
   - Troubleshooting

### 2. **Implementation Steps** - Langkah-langkah implementasi
   - Model Analysis
   - Basic Configuration
   - Table Configuration
   - Form Configuration
   - View Creation
   - Route Setup
   - Navigation Integration

### 3. **Routing System** - Sistem routing modern
   - Basic Route Structure
   - Route Components
   - Route Categories
   - Parameter Route
   - Middleware
   - Dependencies

### 4. **CRUD Implementation** - Implementasi CRUD
   - Core Files Structure
   - loadCRUD Configuration
   - Table Features
   - Form Handling
   - Integration Features
   - Best Practices

### 5. **API Development** - Pengembangan API
   - Basic API Route Structure
   - API Response Format
   - Common API Patterns
   - API Security Features
   - API Best Practices
   - Testing API Endpoints

### 6. **Template Documentation** - Dokumentasi template dokumen
   - **Cong Yi Template** - Template DOCX untuk biodata TKI Taiwan
     - Placeholder Reference (100+ placeholders)
     - Dynamic Content & Table Cloning
     - Image Handling & Processing
     - Implementation Guide
     - Best Practices & Troubleshooting

### 7. **Best Practices** - Praktik terbaik
   - Configuration
   - Performance
   - Security
   - User Experience
   - Code Organization

### 8. **Troubleshooting** - Pemecahan masalah
   - Common Issues & Solutions
   - Testing Checklist
   - Debug Tools
   - Error Handling

## 🏗️ Arsitektur Sistem

### Project Structure
```
Root/
├── flamboyan-app/           # Reference Application (CI3 HMVC)
│   └── application/
│       └── modules/         # Source code reference
├── web/                     # New Application Routes
├── views/                   # New Application Views
├── module/                  # New Application Modules
└── public/                  # Public Assets
```

### Core Components
- **crud.js** - Main configuration file for CRUD operations
- **formbuild.blade.php** - View template for CRUD interface
- **dashboard.php** - Routing configuration for admin pages

## 🔧 Teknologi yang Digunakan

### Core Framework
- **Laravel Components** (Illuminate packages v8.83.27)
- **Blade Template Engine** (Jenssegers/Blade v1.4.0)
- **PDO Database** - Database abstraction layer

### File Processing
- **PHPWord** - Word document generation
- **PHPExcel** - Excel file handling
- **mPDF** - PDF generation

### Utilities
- **Carbon** - Date and time manipulation
- **UUID** - Unique identifier generation
- **Collections** - Data structure handling

## 🎯 Quick Start

### 1. Environment Setup
```bash
# Copy environment file
cp .env.example .env

# Configure database settings
DB_HOST=localhost
DB_USER=root
DB_PASS=password
DB_NAME=database
```

### 2. Install Dependencies
```bash
composer install
```

### 3. Basic Route Example
```php
$route->add('/admin/dashboard', function(){
    View::render('admin/dashboard');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### 4. CRUD Configuration
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

## 📋 Standar Baru

### Module/Helper OOP Class
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

## 🔗 Referensi

- **Reference Application**: `flamboyan-app/` - CodeIgniter 3 HMVC reference
- **Documentation**: `agent/notes.txt` - Complete technical documentation
- **Examples**: Various implementation examples throughout the documentation

## 📞 Support

Untuk bantuan dan dukungan:
1. Cek dokumentasi lengkap di section "Documentation"
2. Lihat troubleshooting guide untuk masalah umum
3. Ikuti best practices untuk implementasi yang optimal

---

**Versi:** 1.0.0  
**Status:** Production Ready ✅  
**Framework:** Modern PHP with Laravel Components 