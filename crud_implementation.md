# CRUD Implementation - FLM Builder

## 📊 Sistem CRUD Modular

FLM Builder menggunakan sistem CRUD yang modular dan fleksibel yang memungkinkan developer untuk membuat interface CRUD dengan mudah melalui konfigurasi JavaScript.

## 🏗️ Core Files Structure

### File Organization
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

### Core Components
- **crud.js**: Main configuration file for CRUD operations
- **formbuild.blade.php**: View template for CRUD interface
- **dashboard.php**: Routing configuration for admin pages

## ⚙️ loadCRUD Configuration Object

### Basic Configuration
```javascript
loadCRUD({
    // Basic Configuration
    title: function() { return 'MODULE_NAME'; },
    titlePage: 'Module Name',
    subTitlePage: 'Setting',
    table: "table_name",
    idform: "containerforms",
    kode: 'primary_key',
    serverSide: true
});
```

### Complete Configuration Example
```javascript
loadCRUD({
    // Basic Configuration
    title: function() { return 'BANK'; },
    titlePage: 'Bank',
    subTitlePage: 'Setting',
    table: "bank",
    idform: "containerforms",
    kode: 'id_bank',
    serverSide: true,

    // Table Configuration
    titleTable: ['ID', 'Nama Bank', 'Kode Bank', 'Status'],
    view: ['id_bank', 'nama_bank', 'kode_bank', 'status'],
    dataSelect: 'id_bank, nama_bank, kode_bank, status',
    queryTemp: 'SELECT {select} FROM bank ORDER BY id_bank DESC',

    // Form Configuration
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
    validasiForm: ['nama_bank', 'kode_bank', 'status'],

    // Optional Features
    filldate: true,
    datekode: 'created_date',
    debug: false,
    multiAction: false,
    noAction: false,
    deleteOnly: false,
    disableEditor: false,
    pagin: true,
    orientation: 'p|l',
    columnsExport: [0,1,2],
    customeExport: [],
    custome: {},
    custView: {},
    custButton: function(d,i){},
    custcondition: function(qr){},
    groupBy: ['field1', 'field2'],

    // Callbacks
    onStarted: function(){},
    oncreate: function(){},
    onupdate: function(r){},
    onviewonly: function(obj){},
    _validasi: function(){},
    _insert: function(){},
    _update: function(){},
    _delete: function(kode){},
    _loadCust: function(){}
});
```

## 📋 Configuration Properties

### Basic Configuration
- **title**: Function yang mengembalikan judul dinamis
- **titlePage**: Judul halaman yang ditampilkan
- **subTitlePage**: Sub judul halaman
- **table**: Nama tabel database
- **idform**: ID container form
- **kode**: Nama field primary key
- **serverSide**: Enable server-side processing

### Table Configuration
- **titleTable**: Array judul kolom yang ditampilkan
- **view**: Array nama field database yang ditampilkan
- **dataSelect**: String field yang di-select dari database
- **queryTemp**: Template query SQL dengan placeholder {select}

### Form Configuration
- **data**: Array field definitions
- **validasiForm**: Array field yang wajib diisi

## 🎨 Table Features

### Data Display
```javascript
loadCRUD({
    // Server-side processing
    serverSide: true,
    
    // Custom column formatting
    custView: {
        'status': function(data) {
            return data == '1' ? 'Aktif' : 'Nonaktif';
        },
        'created_date': function(data) {
            return moment(data).format('DD/MM/YYYY');
        }
    },
    
    // Custom view rendering
    custome: {
        'action': function(data, type, row) {
            return '<button class="btn btn-sm btn-primary">Edit</button>';
        }
    }
});
```

### Filtering & Sorting
```javascript
loadCRUD({
    // Date filtering
    filldate: true,
    datekode: 'created_date',
    
    // Custom filtering
    custcondition: function(qr) {
        return qr + " AND status = '1'";
    },
    
    // Group by
    groupBy: ['category', 'status']
});
```

### Actions
```javascript
loadCRUD({
    // Enable/disable actions
    multiAction: false,
    noAction: false,
    deleteOnly: false,
    disableEditor: false,
    
    // Custom button handling
    custButton: function(data, index) {
        return '<button class="btn btn-sm btn-info">Custom</button>';
    }
});
```

## 📝 Form Handling

### Field Types
```javascript
data: [
    // Text input
    {
        title: 'Nama',
        type: 'text',
        name: 'nama',
        row: 12,
        required: true
    },
    
    // Number input
    {
        title: 'Umur',
        type: 'number',
        name: 'umur',
        row: 6,
        min: 0,
        max: 100
    },
    
    // Select dropdown
    {
        title: 'Status',
        type: 'select',
        name: 'status',
        row: 6,
        data: [
            {value: '1', text: 'Aktif'},
            {value: '0', text: 'Nonaktif'}
        ]
    },
    
    // Radio buttons
    {
        title: 'Jenis Kelamin',
        type: 'radio',
        name: 'jenis_kelamin',
        row: 6,
        data: [
            {value: 'L', text: 'Laki-laki'},
            {value: 'P', text: 'Perempuan'}
        ]
    },
    
    // Textarea
    {
        title: 'Deskripsi',
        type: 'textarea',
        name: 'deskripsi',
        row: 12,
        rows: 4
    },
    
    // Date picker
    {
        title: 'Tanggal Lahir',
        type: 'date',
        name: 'tanggal_lahir',
        row: 6
    },
    
    // File upload
    {
        title: 'Foto',
        type: 'file',
        name: 'foto',
        row: 6,
        accept: 'image/*'
    }
]
```

### Field Properties
- **title**: Label field yang ditampilkan
- **type**: Tipe input field (text/select/textarea/number/date/radio/file)
- **name**: Nama field database
- **row**: Lebar field (1-12, Bootstrap grid)
- **data**: Array opsi untuk select/radio
- **required**: Field wajib diisi
- **min/max**: Batasan nilai untuk number
- **rows**: Jumlah baris untuk textarea
- **accept**: Tipe file yang diterima

### Validation
```javascript
loadCRUD({
    // Required fields
    validasiForm: ['nama', 'email', 'status'],
    
    // Custom validation
    _validasi: function() {
        var email = $('#email').val();
        if (!isValidEmail(email)) {
            alert('Email tidak valid');
            return false;
        }
        return true;
    }
});
```

## 🔄 Integration Features

### Database Operations
```javascript
loadCRUD({
    // Automatic query building
    queryTemp: 'SELECT {select} FROM table WHERE status = "1" ORDER BY id DESC',
    
    // Custom query conditions
    custcondition: function(qr) {
        var search = $('#search').val();
        if (search) {
            qr += " AND nama LIKE '%" + search + "%'";
        }
        return qr;
    }
});
```

### UI Components
```javascript
loadCRUD({
    // Loading indicators
    onStarted: function() {
        $('#loading').show();
    },
    
    // Success callbacks
    oncreate: function() {
        toastr.success('Data berhasil ditambahkan');
    },
    
    onupdate: function(result) {
        toastr.success('Data berhasil diupdate');
    },
    
    _delete: function(kode) {
        toastr.success('Data berhasil dihapus');
    }
});
```

### Event Handling
```javascript
loadCRUD({
    // Form submission
    _insert: function() {
        // Custom logic after insert
        console.log('Data inserted');
    },
    
    _update: function() {
        // Custom logic after update
        console.log('Data updated');
    },
    
    // View only mode
    onviewonly: function(obj) {
        // Custom logic for view only
        console.log('View only mode', obj);
    }
});
```

## 📊 Single Data Mode

### Configuration
```javascript
loadCRUD({
    // Single data mode
    singleData: true,
    
    // Query template for single data
    queryTemp: "SELECT {select} FROM family WHERE id_biodata = '" + (_id('app_id_biodata').innerText)+"' || ORDER BY id_family DESC",
    
    // Form configuration
    data: [
        {
            title: 'Nama Ayah',
            type: 'text',
            name: 'nama_ayah',
            row: 6
        },
        {
            title: 'Nama Ibu',
            type: 'text',
            name: 'nama_ibu',
            row: 6
        }
    ]
});
```

### Characteristics
- **No table list**: Hanya tampilan detail satu data
- **Single record**: Query otomatis LIMIT 1
- **Add/Update buttons**: Tombol "Tambah Data" atau "Update Data"
- **Parent relationship**: Data terkait dengan parent (id_biodata)

## 🔧 Best Practices

### 1. Configuration
```javascript
// Use descriptive field names
{
    title: 'Nama Lengkap',
    name: 'nama_lengkap',
    type: 'text'
}

// Implement proper validation
validasiForm: ['nama_lengkap', 'email', 'telepon']

// Configure appropriate permissions
// Handle in middleware
```

### 2. Performance
```javascript
// Enable server-side processing for large datasets
serverSide: true

// Use appropriate pagination
pagin: true

// Optimize query performance
queryTemp: 'SELECT {select} FROM table WHERE status = "1"'

// Implement proper indexing
// Handle in database
```

### 3. Security
```javascript
// Validate all inputs
validasiForm: ['required_field1', 'required_field2']

// Use prepared statements
// Handle in backend

// Implement proper access control
// Handle in middleware

// Sanitize output data
custView: {
    'content': function(data) {
        return $('<div>').text(data).html();
    }
}
```

### 4. User Experience
```javascript
// Provide clear feedback
oncreate: function() {
    toastr.success('Data berhasil ditambahkan');
},

// Implement proper loading states
onStarted: function() {
    $('#loading').show();
},

// Use appropriate field types
{
    type: 'select',  // Instead of text for options
    data: [
        {value: '1', text: 'Aktif'},
        {value: '0', text: 'Nonaktif'}
    ]
}

// Maintain consistent styling
// Use Bootstrap classes
```

## 🛠️ Troubleshooting

### Common Issues & Solutions

#### 1. Column Name Mismatch
**Problem**: Field tidak muncul atau error
**Solution**: Verify database schema matches configuration
```javascript
// Check field names in database
// Ensure they match the 'name' property in data array
```

#### 2. Form Validation Errors
**Problem**: Form tidak bisa disubmit
**Solution**: Check validasiForm array includes all required fields
```javascript
validasiForm: ['nama', 'email', 'telepon']  // All required fields
```

#### 3. Select Field Issues
**Problem**: Dropdown tidak muncul atau kosong
**Solution**: Always provide default options and proper data structure
```javascript
{
    type: 'select',
    data: [
        {value: '', text: 'Pilih Status'},  // Default option
        {value: '1', text: 'Aktif'},
        {value: '0', text: 'Nonaktif'}
    ]
}
```

#### 4. Route Access Problems
**Problem**: Halaman tidak bisa diakses
**Solution**: Verify middleware configuration and dependencies
```php
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin')
```

### Testing Checklist
1. **Form submission** - Data berhasil disimpan
2. **Data validation** - Validasi berfungsi dengan benar
3. **Table display** - Data ditampilkan dengan benar
4. **Edit functionality** - Edit data berfungsi
5. **Delete operations** - Hapus data berfungsi
6. **Navigation access** - Menu navigasi berfungsi
7. **Server-side processing** - Pagination dan search berfungsi
8. **Error handling** - Error ditangani dengan baik

## 🎯 Implementation Example

### Complete CRUD Module

#### 1. JavaScript Configuration
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
    validasiForm: ['nama_bank', 'kode_bank', 'status'],
    
    custView: {
        'status': function(data) {
            return data == '1' ? 'Aktif' : 'Nonaktif';
        }
    },
    
    oncreate: function() {
        toastr.success('Bank berhasil ditambahkan');
    },
    
    onupdate: function(result) {
        toastr.success('Bank berhasil diupdate');
    },
    
    _delete: function(kode) {
        toastr.success('Bank berhasil dihapus');
    }
});
```

#### 2. View Template
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

#### 3. Route Definition
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

## 🎯 Kesimpulan

Sistem CRUD FLM Builder menyediakan solusi yang powerful dan fleksibel untuk membuat interface CRUD dengan mudah. Dengan konfigurasi JavaScript yang terstruktur, developer dapat membuat CRUD yang lengkap dengan fitur-fitur modern seperti server-side processing, custom validation, dan responsive design. 