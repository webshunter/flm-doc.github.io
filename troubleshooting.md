# Troubleshooting - FLM Builder

## 🛠️ Panduan Pemecahan Masalah

Dokumen ini berisi panduan lengkap untuk mengatasi masalah umum yang mungkin terjadi saat menggunakan FLM Builder.

## 🔍 Common Issues & Solutions

### 1. Column Name Mismatch

#### Problem
Field tidak muncul di tabel atau form, atau terjadi error saat submit data.

#### Symptoms
- Field kosong di tabel
- Error "Column not found" di database
- Form tidak bisa disubmit

#### Solution
```javascript
// ✅ Verify database schema matches configuration
loadCRUD({
    // Check field names in database
    view: ['id_bank', 'nama_bank', 'kode_bank'],  // Must match database columns
    
    data: [
        {
            name: 'nama_bank',  // Must match database column name
            type: 'text'
        }
    ]
});

// ❌ Common mistakes
// view: ['id', 'nama', 'kode'],  // Wrong column names
// name: 'nama',  // Wrong field name
```

#### Debug Steps
1. Check database table structure
2. Verify column names in `view` array
3. Verify field names in `data` array
4. Ensure case sensitivity matches

### 2. Form Validation Errors

#### Problem
Form tidak bisa disubmit karena validation error.

#### Symptoms
- Form tidak submit saat klik tombol
- Error message muncul
- Data tidak tersimpan

#### Solution
```javascript
// ✅ Include all required fields in validation
loadCRUD({
    validasiForm: ['nama_bank', 'kode_bank', 'status'],  // All required fields
    
    data: [
        {
            title: 'Nama Bank',
            name: 'nama_bank',
            required: true
        },
        {
            title: 'Kode Bank',
            name: 'kode_bank',
            required: true
        }
    ]
});

// ❌ Missing required fields
// validasiForm: ['nama_bank'],  // Missing kode_bank and status
```

#### Debug Steps
1. Check `validasiForm` array includes all required fields
2. Verify field names match exactly
3. Check if fields have `required: true` property
4. Test with custom validation function

### 3. Select Field Issues

#### Problem
Dropdown select tidak muncul atau kosong.

#### Symptoms
- Select field kosong
- Options tidak muncul
- Error saat memilih option

#### Solution
```javascript
// ✅ Always provide default options and proper data structure
loadCRUD({
    data: [
        {
            title: 'Status',
            type: 'select',
            name: 'status',
            data: [
                {value: '', text: 'Pilih Status'},  // Default option
                {value: '1', text: 'Aktif'},
                {value: '0', text: 'Nonaktif'}
            ]
        }
    ]
});

// ❌ Missing data or wrong structure
// data: []  // Empty data
// data: ['1', '0']  // Wrong structure
```

#### Debug Steps
1. Check `data` array is not empty
2. Verify data structure has `value` and `text` properties
3. Add default option for better UX
4. Test with static data first

### 4. Route Access Problems

#### Problem
Halaman tidak bisa diakses atau muncul error 404.

#### Symptoms
- Error 404 Not Found
- Blank page
- Redirect loop

#### Solution
```php
// ✅ Verify middleware configuration and dependencies
$route->add('/flamboyan/bank/setting', function(){
    View::render('flamboyan/setting/bank');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

#### Debug Steps
1. Check route URL is correct
2. Verify view file exists
3. Check all dependencies are included
4. Verify middleware is working
5. Check file permissions

### 5. Database Connection Issues

#### Problem
Tidak bisa koneksi ke database atau query error.

#### Symptoms
- Database connection error
- Query execution failed
- Data tidak muncul

#### Solution
```php
// ✅ Check database configuration
// .env file
DB_HOST=localhost
DB_USER=root
DB_PASS=password
DB_NAME=database

// ✅ Verify database connection in route
$route->add('/api/test', function(){
    try {
        $db = new DB();
        $result = $db->query_result_object("SELECT 1 as test");
        return json_encode(['status' => true, 'data' => $result]);
    } catch (Exception $e) {
        return json_encode(['status' => false, 'error' => $e->getMessage()]);
    }
})
->use('vendor/autoload.php')
->use('module/db.php');
```

#### Debug Steps
1. Check database credentials in `.env`
2. Verify database server is running
3. Test connection with simple query
4. Check database permissions
5. Verify table exists

### 6. Script Loading Issues

#### Problem
JavaScript tidak dimuat atau error di console.

#### Symptoms
- CRUD tidak berfungsi
- JavaScript errors in console
- Page not responsive

#### Solution
```php
<!-- ✅ Always use @section('script') for scripts -->
@extends('temp.admin')

@section('content')
    <!-- Content here -->
@endsection

@section('script')
    {!! View::jsm('flamboyan/admin/bank') !!}
@endsection

<!-- ❌ Avoid manual script tags -->
<!-- <script src="path/to/script.js"></script> -->
```

#### Debug Steps
1. Check script file exists
2. Verify script path is correct
3. Check browser console for errors
4. Ensure script is loaded after DOM
5. Test with simple script first

### 7. View Rendering Issues

#### Problem
View tidak muncul atau error saat render.

#### Symptoms
- Blank page
- Template error
- Missing content

#### Solution
```php
// ✅ Verify view structure
@extends('temp.admin')
@php
use NN\Module\DB;
use NN\Module\View;
@endphp

@section("content")
    <!-- Content here -->
@endsection

@section('script')
    {!! View::jsm('path/to/script') !!}
@endsection
```

#### Debug Steps
1. Check view file exists in correct location
2. Verify template inheritance
3. Check for syntax errors in Blade
4. Verify data is passed correctly
5. Test with simple view first

## 🧪 Testing Checklist

### 1. Form Submission Testing
```javascript
// Test form submission
loadCRUD({
    oncreate: function(result) {
        console.log('Create test result:', result);
        if (result.status) {
            console.log('✅ Form submission successful');
        } else {
            console.log('❌ Form submission failed:', result.message);
        }
    }
});
```

**Checklist:**
- [ ] Form dapat disubmit
- [ ] Data tersimpan di database
- [ ] Success message muncul
- [ ] Form reset setelah submit
- [ ] Validation berfungsi

### 2. Data Validation Testing
```javascript
// Test data validation
loadCRUD({
    _validasi: function() {
        console.log('Validation test running...');
        var email = $('#email').val();
        var telepon = $('#telepon').val();
        
        if (!email) {
            console.log('❌ Email validation failed');
            return false;
        }
        
        if (!telepon) {
            console.log('❌ Phone validation failed');
            return false;
        }
        
        console.log('✅ Validation passed');
        return true;
    }
});
```

**Checklist:**
- [ ] Required fields validation
- [ ] Format validation (email, phone)
- [ ] Custom validation rules
- [ ] Error messages muncul
- [ ] Form tidak submit jika validation gagal

### 3. Table Display Testing
```javascript
// Test table display
loadCRUD({
    onStarted: function() {
        console.log('Table loading started...');
    },
    
    oncreate: function() {
        console.log('✅ Table data loaded successfully');
    }
});
```

**Checklist:**
- [ ] Data ditampilkan dengan benar
- [ ] Pagination berfungsi
- [ ] Sorting berfungsi
- [ ] Search/filter berfungsi
- [ ] Export berfungsi

### 4. Edit Functionality Testing
```javascript
// Test edit functionality
loadCRUD({
    onupdate: function(result) {
        console.log('Edit test result:', result);
        if (result.status) {
            console.log('✅ Edit successful');
        } else {
            console.log('❌ Edit failed:', result.message);
        }
    }
});
```

**Checklist:**
- [ ] Edit button berfungsi
- [ ] Form terisi dengan data lama
- [ ] Update berhasil
- [ ] Success message muncul
- [ ] Table terupdate

### 5. Delete Operations Testing
```javascript
// Test delete functionality
loadCRUD({
    _delete: function(kode) {
        console.log('Delete test for ID:', kode);
        console.log('✅ Delete successful');
    }
});
```

**Checklist:**
- [ ] Delete button berfungsi
- [ ] Konfirmasi delete muncul
- [ ] Data terhapus dari database
- [ ] Success message muncul
- [ ] Table terupdate

### 6. Navigation Access Testing
```javascript
// Test navigation
console.log('Testing navigation access...');
console.log('Current URL:', window.location.href);
console.log('Menu items:', $('.nav-link').length);
```

**Checklist:**
- [ ] Menu item accessible
- [ ] URL routing berfungsi
- [ ] Breadcrumb navigation
- [ ] Back button functionality
- [ ] No 404 errors

### 7. Server-Side Processing Testing
```javascript
// Test server-side processing
loadCRUD({
    serverSide: true,
    onStarted: function() {
        console.log('Server-side processing started...');
    },
    
    oncreate: function() {
        console.log('✅ Server-side processing successful');
    }
});
```

**Checklist:**
- [ ] Data loading cepat
- [ ] Pagination berfungsi
- [ ] Search berfungsi
- [ ] No timeout errors
- [ ] Large datasets handled

### 8. Error Handling Testing
```javascript
// Test error handling
loadCRUD({
    oncreate: function(result) {
        if (!result.status) {
            console.log('❌ Error occurred:', result.message);
            // Test error handling
            toastr.error('Error: ' + result.message);
        }
    }
});
```

**Checklist:**
- [ ] Database errors handled
- [ ] Network errors handled
- [ ] Validation errors handled
- [ ] User-friendly error messages
- [ ] No blank pages on error

## 🔧 Debug Tools

### 1. Enhanced var_dump
```php
// Use Debug class for better debugging
use NN\Module\Debug;

Debug::var_dump($variable);
Debug::var_dump_plain($variable);
Debug::htmlentities($variable);
```

### 2. Console Logging
```javascript
// Add console logging for debugging
loadCRUD({
    onStarted: function() {
        console.log('CRUD started');
        console.log('Configuration:', this);
    },
    
    _validasi: function() {
        console.log('Validation running');
        console.log('Form data:', $('#form').serialize());
        return true;
    }
});
```

### 3. Network Monitoring
```javascript
// Monitor AJAX requests
$(document).ajaxStart(function() {
    console.log('AJAX request started');
});

$(document).ajaxComplete(function(event, xhr, settings) {
    console.log('AJAX request completed');
    console.log('Response:', xhr.responseText);
});
```

### 4. Error Reporting Control
```php
// Control error reporting
error_reporting(E_ALL);
ini_set('display_errors', 1);

// For production
error_reporting(0);
ini_set('display_errors', 0);
```

## 📋 Common Error Codes

### Database Errors
- **1045**: Access denied for user
- **1049**: Unknown database
- **1146**: Table doesn't exist
- **1054**: Unknown column

### PHP Errors
- **E_ERROR**: Fatal error
- **E_WARNING**: Warning
- **E_NOTICE**: Notice
- **E_PARSE**: Parse error

### HTTP Errors
- **404**: Not found
- **500**: Internal server error
- **403**: Forbidden
- **401**: Unauthorized

## 🎯 Performance Monitoring

### 1. Query Performance
```php
// Monitor query execution time
$start_time = microtime(true);

$db = new DB();
$result = $db->query_result_object("SELECT * FROM large_table");

$end_time = microtime(true);
$execution_time = ($end_time - $start_time);

if ($execution_time > 1) {
    error_log("Slow query detected: {$execution_time}s");
}
```

### 2. Memory Usage
```php
// Monitor memory usage
$memory_usage = memory_get_usage(true);
$memory_peak = memory_get_peak_usage(true);

error_log("Memory usage: " . ($memory_usage / 1024 / 1024) . "MB");
error_log("Peak memory: " . ($memory_peak / 1024 / 1024) . "MB");
```

### 3. Response Time
```javascript
// Monitor response time
var startTime = performance.now();

// Your operation here

var endTime = performance.now();
console.log('Operation took ' + (endTime - startTime) + ' milliseconds');
```

## 🔍 Troubleshooting Workflow

### 1. Identify the Problem
- What is the expected behavior?
- What is the actual behavior?
- When does the problem occur?
- What steps reproduce the problem?

### 2. Gather Information
- Check browser console for errors
- Check server logs for errors
- Check database logs for errors
- Gather system information

### 3. Analyze the Problem
- Review relevant code
- Check configuration settings
- Verify dependencies
- Test with minimal setup

### 4. Implement Solution
- Apply fix based on analysis
- Test the solution
- Document the fix
- Monitor for recurrence

### 5. Verify Resolution
- Test all related functionality
- Ensure no new issues introduced
- Update documentation if needed
- Share solution with team

## 🎯 Kesimpulan

Troubleshooting adalah bagian penting dari development process. Dengan mengikuti panduan ini dan menggunakan tools debugging yang tersedia, developer dapat mengatasi masalah dengan cepat dan efisien. Selalu ingat untuk:

1. **Systematic approach** - Ikuti langkah-langkah yang terstruktur
2. **Document everything** - Catat semua masalah dan solusi
3. **Test thoroughly** - Pastikan solusi benar-benar menyelesaikan masalah
4. **Learn from issues** - Gunakan pengalaman untuk mencegah masalah serupa
5. **Share knowledge** - Bagikan solusi dengan tim untuk pembelajaran bersama 