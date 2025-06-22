# Implementation Steps - FLM Builder

## 🚀 Langkah-langkah Implementasi

### 1. Model Analysis

#### Location
- **Path**: `flamboyan-app/application/modules/[nama_modul]/models/`
- **Purpose**: Analisis struktur data dan business logic dari aplikasi referensi

#### Identifikasi Komponen
- **Table name**: Nama tabel database
- **Column structure**: Struktur kolom dan tipe data
- **Primary key**: Kunci primer tabel
- **Required fields**: Field yang wajib diisi
- **Foreign keys**: Relasi dengan tabel lain
- **Business rules**: Aturan bisnis yang berlaku

#### Contoh Analisis Model
```php
// Contoh model dari flamboyan-app
class M_bank extends CI_Model {
    public function get_all() {
        return $this->db->get('bank')->result();
    }
    
    public function get_by_id($id) {
        return $this->db->where('id_bank', $id)->get('bank')->row();
    }
}
```

### 2. Basic Configuration

#### CRUD Configuration Object
```javascript
loadCRUD({
    // Basic Configuration
    title: function () { return 'MODULE_NAME'; },
    titlePage: 'Module Name',
    subTitlePage: 'Setting',
    table: "table_name",
    idform: "containerforms",
    kode: 'primary_key',
    serverSide: true,
    
    // Optional Configuration
    debug: false,
    multiAction: false,
    noAction: false,
    deleteOnly: false,
    disableEditor: false
});
```

#### Configuration Properties
- **title**: Function yang mengembalikan judul dinamis
- **titlePage**: Judul halaman yang ditampilkan
- **subTitlePage**: Sub judul halaman
- **table**: Nama tabel database
- **idform**: ID container form
- **kode**: Nama field primary key
- **serverSide**: Enable server-side processing

### 3. Table Configuration

#### Column Definition
```javascript
loadCRUD({
    // Table Configuration
    titleTable: ['ID', 'Nama Bank', 'Kode Bank', 'Status'],
    view: ['id_bank', 'nama_bank', 'kode_bank', 'status'],
    dataSelect: 'id_bank, nama_bank, kode_bank, status',
    queryTemp: 'SELECT {select} FROM bank ORDER BY id_bank DESC'
});
```

#### Table Properties
- **titleTable**: Array judul kolom yang ditampilkan
- **view**: Array nama field database yang ditampilkan
- **dataSelect**: String field yang di-select dari database
- **queryTemp**: Template query SQL dengan placeholder {select}

#### Advanced Table Features
```javascript
loadCRUD({
    // Pagination
    pagin: true,
    
    // Export Options
    orientation: 'p|l',  // Portrait or Landscape
    columnsExport: [0,1,2],  // Columns to export
    
    // Custom View Formatting
    custView: {
        'status': function(data) {
            return data == '1' ? 'Aktif' : 'Nonaktif';
        }
    },
    
    // Custom Query Conditions
    custcondition: function(qr) {
        return qr + " AND status = '1'";
    }
});
```

### 4. Form Configuration

#### Field Definition
```javascript
loadCRUD({
    data: [
        {
            title: 'Nama Bank',
            type: 'text',
            name: 'nama_bank',
            row: 12,
            required: true
        },
        {
            title: 'Kode Bank',
            type: 'text',
            name: 'kode_bank',
            row: 6,
            required: true
        },
        {
            title: 'Status',
            type: 'select',
            name: 'status',
            row: 6,
            data: [
                {value: '1', text: 'Aktif'},
                {value: '0', text: 'Nonaktif'}
            ]
        }
    ],
    validasiForm: ['nama_bank', 'kode_bank', 'status']
});
```

#### Field Types
- **text**: Input text biasa
- **number**: Input angka
- **select**: Dropdown selection
- **radio**: Radio button
- **textarea**: Text area
- **date**: Date picker
- **file**: File upload

#### Field Properties
- **title**: Label field yang ditampilkan
- **type**: Tipe input field
- **name**: Nama field database
- **row**: Lebar field (1-12, Bootstrap grid)
- **data**: Array opsi untuk select/radio
- **required**: Field wajib diisi

### 5. View Creation

#### File Location
- **Path**: `views/flamboyan/setting/[namamodul].blade.php`
- **Template**: Extend dari admin template

#### Basic View Structure
```php
@extends('temp.admin')
@php
use NN\Module\DB;
use NN\Module\View;
@endphp

@section("content")
<div class="content-header row">
    <div class="content-header-left col-md-6 col-12 mb-2 breadcrumb-new">
        <h3 class="content-header-title mb-0 d-inline-block">Module Name</h3>
        <div class="row breadcrumbs-top d-inline-block">
            <div class="breadcrumb-wrapper col-12">
                <ol class="breadcrumb">
                    <li class="breadcrumb-item active">Module Name</li>
                </ol>
            </div>
        </div>
    </div>
</div>

<div class="content-body">
    <section id="section-name">
        <div class="row">
            <div class="col-12">
                <div class="card">
                    <div class="card-header">
                        <h4 class="card-title">Module Title</h4>
                    </div>
                    <div class="card-content">
                        <div class="card-body">
                            <div id="containerforms"></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>
@endsection

@section('script')
{!! View::jsm('flamboyan/admin/modulename') !!}
@endsection
```

#### View Components
- **Content Header**: Breadcrumb dan judul halaman
- **Card Container**: Wrapper untuk konten
- **Form Container**: Container untuk CRUD form
- **Script Loading**: Load JavaScript CRUD

### 6. Route Setup

#### File Location
- **Path**: `web/admin/dashboard.php`
- **Purpose**: Konfigurasi routing untuk admin pages

#### Route Definition
```php
$route->add('/flamboyan/bank/setting', function(){
    View::render('flamboyan/setting/bank');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

#### Route Components
- **URL Path**: `/flamboyan/{module}/setting`
- **View Rendering**: `View::render('flamboyan/setting/{module}')`
- **Dependencies**: Autoloader, database, perusahaan module
- **Middleware**: Admin authentication check

#### Route Categories
1. **Setting Routes**: `/flamboyan/{module}/setting`
2. **Admin Routes**: `/flamboyan/{module}/admin`
3. **CRUD Routes**: `/admin/crud/{id}`

### 7. Navigation Integration

#### File Location
- **Path**: `views/temp/nav/usp.blade.php`
- **Purpose**: Menu navigasi admin

#### Menu Item Structure
```php
<li class="nav-item">
    <a href="/flamboyan/bank/setting" class="nav-link">
        <i class="fa fa-university"></i>
        <span>Bank</span>
    </a>
</li>
```

#### Menu Properties
- **href**: URL ke halaman module
- **icon**: Font Awesome icon
- **span**: Label menu item
- **nav-item**: Bootstrap class untuk menu item
- **nav-link**: Bootstrap class untuk link

#### Menu Organization
```php
<!-- Setting Menu -->
<li class="nav-item has-sub">
    <a href="#" class="nav-link">
        <i class="fa fa-cog"></i>
        <span>Setting</span>
    </a>
    <ul class="menu-content">
        <li class="nav-item">
            <a href="/flamboyan/bank/setting" class="nav-link">Bank</a>
        </li>
        <li class="nav-item">
            <a href="/flamboyan/sektor/setting" class="nav-link">Sektor</a>
        </li>
    </ul>
</li>
```

## 🔧 Best Practices

### 1. Naming Conventions
- **File Names**: lowercase dengan underscore (snake_case)
- **Class Names**: PascalCase
- **Function Names**: camelCase
- **Database Tables**: lowercase dengan underscore
- **Database Columns**: lowercase dengan underscore

### 2. Code Organization
- **Models**: Business logic dan data access
- **Views**: Presentation layer
- **Routes**: URL mapping dan middleware
- **Scripts**: Client-side functionality

### 3. Security Considerations
- **Input Validation**: Validate semua input user
- **SQL Injection**: Gunakan prepared statements
- **XSS Prevention**: Sanitize output data
- **Access Control**: Implement proper authentication

### 4. Performance Optimization
- **Database Indexing**: Index pada kolom yang sering di-query
- **Query Optimization**: Optimize SQL queries
- **Caching**: Implement caching untuk data statis
- **Lazy Loading**: Load data sesuai kebutuhan

## 📋 Testing Checklist

### 1. Form Functionality
- [ ] Form submission berhasil
- [ ] Data validation berfungsi
- [ ] Required fields validation
- [ ] Error messages ditampilkan
- [ ] Success messages ditampilkan

### 2. Table Display
- [ ] Data ditampilkan dengan benar
- [ ] Pagination berfungsi
- [ ] Sorting berfungsi
- [ ] Search/filter berfungsi
- [ ] Export berfungsi

### 3. CRUD Operations
- [ ] Create new record
- [ ] Read existing records
- [ ] Update record
- [ ] Delete record
- [ ] Bulk operations (jika ada)

### 4. Navigation
- [ ] Menu item accessible
- [ ] URL routing berfungsi
- [ ] Breadcrumb navigation
- [ ] Back button functionality

### 5. Error Handling
- [ ] Database connection errors
- [ ] Validation errors
- [ ] Permission errors
- [ ] Network errors
- [ ] Server errors

## 🎯 Implementation Example

### Complete Module Implementation

#### 1. Model Analysis (Reference)
```php
// flamboyan-app/application/modules/bank/models/M_bank.php
class M_bank extends CI_Model {
    public function get_all() {
        return $this->db->get('bank')->result();
    }
}
```

#### 2. CRUD Configuration
```javascript
// script/flamboyan/admin/bank.js
loadCRUD({
    title: function () { return 'BANK'; },
    titlePage: 'Bank',
    subTitlePage: 'Setting',
    table: "bank",
    idform: "containerforms",
    kode: 'id_bank',
    serverSide: true,
    
    titleTable: ['ID', 'Nama Bank', 'Kode Bank', 'Status'],
    view: ['id_bank', 'nama_bank', 'kode_bank', 'status'],
    dataSelect: 'id_bank, nama_bank, kode_bank, status',
    queryTemp: 'SELECT {select} FROM bank ORDER BY id_bank DESC',
    
    data: [
        {
            title: 'Nama Bank',
            type: 'text',
            name: 'nama_bank',
            row: 12,
            required: true
        },
        {
            title: 'Kode Bank',
            type: 'text',
            name: 'kode_bank',
            row: 6,
            required: true
        },
        {
            title: 'Status',
            type: 'select',
            name: 'status',
            row: 6,
            data: [
                {value: '1', text: 'Aktif'},
                {value: '0', text: 'Nonaktif'}
            ]
        }
    ],
    validasiForm: ['nama_bank', 'kode_bank', 'status']
});
```

#### 3. View Template
```php
<!-- views/flamboyan/setting/bank.blade.php -->
@extends('temp.admin')
@php
use NN\Module\DB;
use NN\Module\View;
@endphp

@section("content")
<div class="content-header row">
    <div class="content-header-left col-md-6 col-12 mb-2 breadcrumb-new">
        <h3 class="content-header-title mb-0 d-inline-block">Bank</h3>
        <div class="row breadcrumbs-top d-inline-block">
            <div class="breadcrumb-wrapper col-12">
                <ol class="breadcrumb">
                    <li class="breadcrumb-item active">Bank</li>
                </ol>
            </div>
        </div>
    </div>
</div>

<div class="content-body">
    <section id="bank-section">
        <div class="row">
            <div class="col-12">
                <div class="card">
                    <div class="card-header">
                        <h4 class="card-title">Data Bank</h4>
                    </div>
                    <div class="card-content">
                        <div class="card-body">
                            <div id="containerforms"></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>
@endsection

@section('script')
{!! View::jsm('flamboyan/admin/bank') !!}
@endsection
```

#### 4. Route Definition
```php
// web/admin/dashboard.php
$route->add('/flamboyan/bank/setting', function(){
    View::render('flamboyan/setting/bank');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

#### 5. Navigation Menu
```php
<!-- views/temp/nav/usp.blade.php -->
<li class="nav-item">
    <a href="/flamboyan/bank/setting" class="nav-link">
        <i class="fa fa-university"></i>
        <span>Bank</span>
    </a>
</li>
```

## 🎯 Kesimpulan

Implementasi module di FLM Builder mengikuti pola yang konsisten dan terstruktur. Dengan mengikuti 7 langkah implementasi ini, developer dapat membuat module baru dengan cepat dan efisien sambil memastikan konsistensi dengan arsitektur sistem yang ada. 