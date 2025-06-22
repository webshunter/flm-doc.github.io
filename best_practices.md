# Best Practices - FLM Builder

## 🎯 Panduan Praktik Terbaik

Dokumen ini berisi panduan praktik terbaik untuk pengembangan aplikasi menggunakan FLM Builder, mencakup konfigurasi, performa, keamanan, dan user experience.

## ⚙️ Configuration Best Practices

### 1. Use Descriptive Field Names
```javascript
// ✅ Good
{
    title: 'Nama Lengkap',
    name: 'nama_lengkap',
    type: 'text'
}

// ❌ Bad
{
    title: 'Name',
    name: 'nm',
    type: 'text'
}
```

### 2. Implement Proper Validation
```javascript
loadCRUD({
    // Always include required fields in validation
    validasiForm: ['nama_lengkap', 'email', 'telepon', 'alamat'],
    
    // Custom validation for complex rules
    _validasi: function() {
        var email = $('#email').val();
        var telepon = $('#telepon').val();
        
        // Email validation
        if (!isValidEmail(email)) {
            alert('Format email tidak valid');
            return false;
        }
        
        // Phone validation
        if (!isValidPhone(telepon)) {
            alert('Format telepon tidak valid');
            return false;
        }
        
        return true;
    }
});
```

### 3. Configure Appropriate Permissions
```php
// Always use middleware for authentication
$route->add('/admin/secure-page', function(){
    View::render('admin/secure-page');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');  // ✅ Authentication check
```

### 4. Set Up Proper Error Handling
```javascript
loadCRUD({
    // Handle errors gracefully
    oncreate: function(result) {
        if (result.status) {
            toastr.success('Data berhasil ditambahkan');
        } else {
            toastr.error('Gagal menambahkan data: ' + result.message);
        }
    },
    
    onupdate: function(result) {
        if (result.status) {
            toastr.success('Data berhasil diupdate');
        } else {
            toastr.error('Gagal mengupdate data: ' + result.message);
        }
    },
    
    _delete: function(kode) {
        toastr.success('Data berhasil dihapus');
    }
});
```

## ⚡ Performance Best Practices

### 1. Enable Server-Side Processing for Large Datasets
```javascript
loadCRUD({
    // ✅ Enable for large datasets
    serverSide: true,
    
    // Optimize query performance
    queryTemp: 'SELECT {select} FROM table WHERE status = "1" ORDER BY id DESC',
    
    // Use appropriate pagination
    pagin: true
});
```

### 2. Use Appropriate Pagination
```javascript
loadCRUD({
    // Enable pagination for better performance
    pagin: true,
    
    // Custom pagination settings if needed
    pageLength: 25,  // Show 25 records per page
    lengthMenu: [[10, 25, 50, 100], [10, 25, 50, 100]]
});
```

### 3. Optimize Query Performance
```javascript
loadCRUD({
    // Use specific columns instead of *
    dataSelect: 'id, nama, email, status',
    
    // Add WHERE conditions to limit data
    queryTemp: 'SELECT {select} FROM users WHERE status = "1" ORDER BY id DESC',
    
    // Use custom conditions for filtering
    custcondition: function(qr) {
        var search = $('#search').val();
        if (search) {
            qr += " AND nama LIKE '%" + search + "%'";
        }
        return qr;
    }
});
```

### 4. Implement Proper Indexing
```sql
-- Add indexes on frequently queried columns
CREATE INDEX idx_users_status ON users(status);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_created_date ON users(created_date);

-- Composite indexes for complex queries
CREATE INDEX idx_users_status_date ON users(status, created_date);
```

### 5. Use Caching for Static Data
```php
// Cache frequently accessed data
function getStaticData() {
    $cacheFile = 'cache/static_data.json';
    
    if (file_exists($cacheFile) && (time() - filemtime($cacheFile)) < 3600) {
        return json_decode(file_get_contents($cacheFile), true);
    }
    
    $db = new DB();
    $data = $db->query_result_object("SELECT * FROM static_data");
    
    file_put_contents($cacheFile, json_encode($data));
    return $data;
}
```

## 🔒 Security Best Practices

### 1. Validate All Inputs
```javascript
loadCRUD({
    // Always validate required fields
    validasiForm: ['nama', 'email', 'telepon'],
    
    // Custom validation for complex rules
    _validasi: function() {
        var email = $('#email').val();
        var telepon = $('#telepon').val();
        
        // Email validation
        var emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailRegex.test(email)) {
            alert('Format email tidak valid');
            return false;
        }
        
        // Phone validation
        var phoneRegex = /^[0-9+\-\s()]+$/;
        if (!phoneRegex.test(telepon)) {
            alert('Format telepon tidak valid');
            return false;
        }
        
        return true;
    }
});
```

### 2. Use Prepared Statements
```php
// ✅ Use prepared statements in database operations
$db = new DB();
$result = $db->insert('users', [
    'nama' => $nama,
    'email' => $email,
    'telepon' => $telepon
]);

// ❌ Avoid direct SQL concatenation
// $query = "INSERT INTO users (nama, email) VALUES ('$nama', '$email')";
```

### 3. Implement Proper Access Control
```php
// Always use middleware for authentication
$route->add('/admin/secure-page', function(){
    // Check user permissions
    if (!hasPermission('admin')) {
        return json_encode([
            'status' => false,
            'message' => 'Access denied'
        ]);
    }
    
    View::render('admin/secure-page');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### 4. Sanitize Output Data
```javascript
loadCRUD({
    // Sanitize HTML output to prevent XSS
    custView: {
        'content': function(data) {
            return $('<div>').text(data).html();
        },
        'description': function(data) {
            return $('<div>').text(data).html();
        }
    }
});
```

### 5. Use HTTPS in Production
```php
// Force HTTPS in production
if ($_SERVER['HTTPS'] !== 'on' && $_SERVER['HTTP_HOST'] !== 'localhost') {
    header('Location: https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI']);
    exit;
}
```

## 🎨 User Experience Best Practices

### 1. Provide Clear Feedback
```javascript
loadCRUD({
    // Show loading indicators
    onStarted: function() {
        $('#loading').show();
    },
    
    // Success feedback
    oncreate: function(result) {
        $('#loading').hide();
        if (result.status) {
            toastr.success('Data berhasil ditambahkan');
            $('#form').trigger('reset');
        } else {
            toastr.error('Gagal menambahkan data: ' + result.message);
        }
    },
    
    onupdate: function(result) {
        $('#loading').hide();
        if (result.status) {
            toastr.success('Data berhasil diupdate');
        } else {
            toastr.error('Gagal mengupdate data: ' + result.message);
        }
    },
    
    _delete: function(kode) {
        toastr.success('Data berhasil dihapus');
    }
});
```

### 2. Implement Proper Loading States
```javascript
loadCRUD({
    // Show loading on form submission
    _insert: function() {
        $('#submitBtn').prop('disabled', true).text('Menyimpan...');
    },
    
    _update: function() {
        $('#submitBtn').prop('disabled', true).text('Mengupdate...');
    },
    
    // Reset button state after operation
    oncreate: function() {
        $('#submitBtn').prop('disabled', false).text('Simpan');
    },
    
    onupdate: function() {
        $('#submitBtn').prop('disabled', false).text('Update');
    }
});
```

### 3. Use Appropriate Field Types
```javascript
loadCRUD({
    data: [
        // Use select for options instead of text
        {
            title: 'Status',
            type: 'select',  // ✅ Better UX
            name: 'status',
            data: [
                {value: '1', text: 'Aktif'},
                {value: '0', text: 'Nonaktif'}
            ]
        },
        
        // Use number for numeric values
        {
            title: 'Umur',
            type: 'number',  // ✅ Better UX
            name: 'umur',
            min: 0,
            max: 100
        },
        
        // Use date for date values
        {
            title: 'Tanggal Lahir',
            type: 'date',  // ✅ Better UX
            name: 'tanggal_lahir'
        }
    ]
});
```

### 4. Maintain Consistent Styling
```php
<!-- Use consistent Bootstrap classes -->
<div class="content-header row">
    <div class="content-header-left col-md-6 col-12 mb-2 breadcrumb-new">
        <h3 class="content-header-title mb-0 d-inline-block">Page Title</h3>
        <div class="row breadcrumbs-top d-inline-block">
            <div class="breadcrumb-wrapper col-12">
                <ol class="breadcrumb">
                    <li class="breadcrumb-item active">Page Title</li>
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
                        <h4 class="card-title">Card Title</h4>
                    </div>
                    <div class="card-content">
                        <div class="card-body">
                            <!-- Content here -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>
```

## 📁 Code Organization Best Practices

### 1. Module/Helper Wajib OOP Class
```php
// ✅ Use OOP classes for modules/helpers
namespace NN\Module;

class NamaHelper {
    public static function fungsiStatic($param) {
        // Implementation
        return $result;
    }
    
    public static function anotherFunction($param) {
        // Implementation
        return $result;
    }
}

// Usage
use NN\Module\NamaHelper;
$result = NamaHelper::fungsiStatic($param);
```

### 2. Script Loading di Blade
```php
<!-- ✅ Always use @section('script') for scripts -->
@section('script')
    {!! View::jsm('path/to/script.js') !!}
@endsection

<!-- ❌ Avoid manual script tags -->
<!-- <script src="path/to/script.js"></script> -->
```

### 3. Komunikasi Data Blade ke JS
```php
<!-- ✅ Use global variables for Blade to JS communication -->
<script>
    var app_id_biodata = '{{ $id_biodata }}';
    var app_user_data = @json($userData);
</script>

@section('script')
    {!! View::jsm('flamboyan/admin/module') !!}
@endsection
```

```javascript
// ✅ Access global variables in JS
loadCRUD({
    queryTemp: "SELECT {select} FROM table WHERE id_biodata = '" + app_id_biodata + "'"
});

// ❌ Avoid direct Blade syntax in JS
// queryTemp: "SELECT {select} FROM table WHERE id_biodata = '{{ $id_biodata }}'"
```

### 4. Standar Foto di Template Detail
```php
<!-- ✅ Use consistent photo display logic -->
<a class='profile-pic'>
    <img src="{{ !empty($photo_path) ? PATH . '/' . $photo_path : 'assets/uploads/default-user.png' }}" 
         alt="Profile Picture" 
         class="rounded-circle" 
         width="50" 
         height="50">
</a>
```

### 5. File Organization
```
project/
├── web/
│   ├── admin/           # Admin routes
│   └── api/            # API routes
├── views/
│   ├── admin/          # Admin views
│   └── flamboyan/      # Module views
├── script/
│   └── flamboyan/      # Module scripts
├── module/             # PHP modules
└── public/             # Public assets
```

## 🔧 Development Workflow Best Practices

### 1. Follow Implementation Steps
1. **Model Analysis** - Study reference module
2. **Basic Configuration** - Set up CRUD config
3. **Table Configuration** - Define columns and queries
4. **Form Configuration** - Define form fields
5. **View Creation** - Create Blade template
6. **Route Setup** - Define routing
7. **Navigation Integration** - Add to menu

### 2. Testing Strategy
```javascript
// Test all CRUD operations
loadCRUD({
    // Test form submission
    oncreate: function(result) {
        console.log('Create test:', result);
    },
    
    // Test data updates
    onupdate: function(result) {
        console.log('Update test:', result);
    },
    
    // Test data deletion
    _delete: function(kode) {
        console.log('Delete test:', kode);
    }
});
```

### 3. Error Handling
```javascript
loadCRUD({
    // Handle validation errors
    _validasi: function() {
        try {
            // Validation logic
            return true;
        } catch (error) {
            console.error('Validation error:', error);
            return false;
        }
    },
    
    // Handle database errors
    oncreate: function(result) {
        if (!result.status) {
            console.error('Database error:', result.message);
            toastr.error('Database error: ' + result.message);
        }
    }
});
```

### 4. Documentation
```javascript
/**
 * Bank Management CRUD Configuration
 * 
 * @description Manages bank data with full CRUD operations
 * @author Developer Name
 * @version 1.0.0
 * @date 2024-01-01
 */
loadCRUD({
    title: function () { return 'BANK'; },
    titlePage: 'Bank Management',
    subTitlePage: 'Setting',
    table: "bank",
    // ... rest of configuration
});
```

## 📋 Checklist Best Practices

### Configuration Checklist
- [ ] Use descriptive field names
- [ ] Implement proper validation
- [ ] Configure appropriate permissions
- [ ] Set up proper error handling
- [ ] Use consistent naming conventions

### Performance Checklist
- [ ] Enable server-side processing for large datasets
- [ ] Use appropriate pagination
- [ ] Optimize query performance
- [ ] Implement proper indexing
- [ ] Use caching for static data

### Security Checklist
- [ ] Validate all inputs
- [ ] Use prepared statements
- [ ] Implement proper access control
- [ ] Sanitize output data
- [ ] Use HTTPS in production

### User Experience Checklist
- [ ] Provide clear feedback
- [ ] Implement proper loading states
- [ ] Use appropriate field types
- [ ] Maintain consistent styling
- [ ] Handle errors gracefully

### Code Organization Checklist
- [ ] Use OOP classes for modules/helpers
- [ ] Load scripts in @section('script')
- [ ] Use global variables for Blade-JS communication
- [ ] Follow standard photo display logic
- [ ] Organize files properly

## 🎯 Kesimpulan

Mengikuti best practices ini akan memastikan aplikasi FLM Builder yang dikembangkan memiliki performa yang baik, keamanan yang robust, user experience yang optimal, dan kode yang mudah dimaintain. Selalu ingat untuk mengikuti standar yang telah ditetapkan dan melakukan testing yang menyeluruh sebelum deployment. 