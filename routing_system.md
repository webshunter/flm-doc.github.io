# Routing System - FLM Builder

## 🛣️ Sistem Routing Modern

FLM Builder menggunakan sistem routing yang modern dan fleksibel yang memungkinkan developer untuk mendefinisikan route dengan mudah dan menambahkan middleware serta dependencies secara modular.

## 🔧 Basic Route Structure

### Format Dasar
```php
$route->add('/path/to/route', function(){
    // Route handler logic
    View::render('view/path');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### Komponen Route
1. **Route Definition**: `$route->add('/path', function(){})`
2. **View Rendering**: `View::render('view/path')`
3. **Dependencies**: `->use('module/file.php')`
4. **Middleware**: `->middleware('middleware_name')`

## 📍 Route Components

### 1. Route Definition
```php
$route->add('/path/to/route', function(){
    // Route handler
});
```

#### Parameter Route
```php
$route->add('/api/user/{id}', function($id){
    // $id akan berisi nilai dari parameter
    return json_encode(['user_id' => $id]);
});
```

#### Multiple Parameters
```php
$route->add('/api/user/{id}/post/{post_id}', function($id, $post_id){
    // $id dan $post_id akan berisi nilai dari parameter
    return json_encode([
        'user_id' => $id,
        'post_id' => $post_id
    ]);
});
```

### 2. View Rendering
```php
// Basic rendering
View::render('view/path');

// Rendering dengan data
View::render('view/path', [
    'data' => $value,
    'user' => $userData
]);
```

#### View Path Resolution
- **Base Path**: `views/` directory
- **Extension**: `.blade.php` (automatic)
- **Example**: `View::render('admin/dashboard')` → `views/admin/dashboard.blade.php`

### 3. Dependencies
```php
->use('vendor/autoload.php')        // Autoloader
->use('module/db.php')             // Database module
->use('module/perusahaan.php')     // Company module
->use('module/secondb.php')        // Secondary database
```

#### Path Resolution Rules
- **Root-based**: Semua path dimulai dari root folder
- **Forward slashes**: Gunakan `/` untuk directory separation
- **Case sensitive**: Perhatikan case sensitivity di Linux

### 4. Middleware
```php
->middleware('cekloginadmin')       // Admin authentication
->middleware('checkApiToken')       // API token validation
->middleware('cors')               // CORS handling
```

#### Custom Middleware
```php
$route->addMidleware('customMiddleware', function(){
    // Middleware logic
    if (!isset($_SESSION['user'])) {
        header('Location: /login');
        exit;
    }
    return true;
});
```

## 🗂️ Route Categories

### 1. Admin Routes
```php
// Dashboard
$route->add('/admin/dashboard', function(){
    View::render('admin/dashboard');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');

// Traffic monitoring
$route->add('/admin/traffic', function(){
    View::render('admin/traffic');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');

// Form builder
$route->add('/admin/formbuild', function(){
    View::render('admin/formbuild');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');

// CRUD operations
$route->add('/admin/crud/{id}', function($id = ""){
    View::render('admin/crud', ["id"=>$id]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### 2. Setting Routes
```php
// Pattern: /flamboyan/{module}/setting
$route->add('/flamboyan/bank/setting', function(){
    View::render('flamboyan/setting/bank');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');

$route->add('/flamboyan/sektor/setting', function(){
    View::render('flamboyan/setting/sektor');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### 3. Admin Module Routes
```php
// Pattern: /flamboyan/{module}/admin
$route->add('/flamboyan/agen/admin', function(){
    View::render('flamboyan/admin/agen');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');

$route->add('/flamboyan/sponsor/admin', function(){
    View::render('flamboyan/admin/sponsor');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

## 🔌 API Routes

### Basic API Structure
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

### Common API Patterns

#### GET Request (Fetch Data)
```php
$route->add('/api/users', function(){
    $db = new DB();
    $data = $db->query_result_object("SELECT * FROM users");
    
    return json_encode([
        'status' => true,
        'message' => 'Data retrieved successfully',
        'data' => $data
    ]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

#### POST Request (Create Data)
```php
$route->add('/api/users', function(){
    $db = new DB();
    $data = json_decode(file_get_contents('php://input'), true);
    
    $result = $db->insert('users', $data);
    
    return json_encode([
        'status' => true,
        'message' => 'Data created successfully',
        'data' => $result
    ]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

#### PUT Request (Update Data)
```php
$route->add('/api/users/{id}', function($id){
    $db = new DB();
    $data = json_decode(file_get_contents('php://input'), true);
    
    $result = $db->sql_update_query('users', $data, "id = '$id'");
    
    return json_encode([
        'status' => true,
        'message' => 'Data updated successfully',
        'data' => $result
    ]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

#### DELETE Request (Remove Data)
```php
$route->add('/api/users/{id}', function($id){
    $db = new DB();
    $result = $db->sql_delete_query('users', "id = '$id'");
    
    return json_encode([
        'status' => true,
        'message' => 'Data deleted successfully',
        'data' => $result
    ]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

## 📁 File Path Resolution

### Use Statement Path Resolution
1. **All paths in `use()` statements start from the root folder**
   ```php
   ->use('vendor/autoload.php')        // Root/vendor/autoload.php
   ->use('module/db.php')             // Root/module/db.php
   ->use('module/perusahaan.php')     // Root/module/perusahaan.php
   ```

2. **Common Module Paths**
   - Vendor: `vendor/`
   - Modules: `module/`
   - Views: `views/`
   - Assets: `assets/`

3. **Path Resolution Rules**
   - Always use forward slashes (/)
   - No leading slash needed
   - Paths are relative to project root
   - Case sensitive on Linux systems

4. **Example Module Structure**
   ```
   Root/
   ├── vendor/
   │   └── autoload.php
   ├── module/
   │   ├── db.php
   │   └── perusahaan.php
   ├── views/
   │   └── admin/
   └── web/
       └── admin/
   ```

## 🎨 View Path Resolution

### View::render() Function
1. **All view paths start from the `views/` folder in root**
   ```php
   View::render('admin/dashboard')     // views/admin/dashboard.blade.php
   View::render('flamboyan/setting/bank')  // views/flamboyan/setting/bank.blade.php
   ```

2. **View File Structure**
   ```
   Root/
   └── views/
       ├── admin/
       │   ├── dashboard.blade.php
       │   ├── formbuild.blade.php
       │   └── crud.blade.php
       └── flamboyan/
           ├── setting/
           │   ├── bank.blade.php
           │   ├── sektor.blade.php
           │   └── pendidikan.blade.php
           └── admin/
               ├── agen.blade.php
               └── sponsor.blade.php
   ```

3. **View Path Rules**
   - No need to include 'views/' in the path
   - No need to include '.blade.php' extension
   - Use forward slashes (/) for directory separation
   - Case sensitive on Linux systems
   - Can pass data as second parameter:
     ```php
     View::render('admin/crud', ["id" => $id]);
     ```

4. **Common View Patterns**
   - Admin views: `admin/{view}`
   - Setting views: `flamboyan/setting/{module}`
   - Admin module views: `flamboyan/admin/{module}`

## 🔒 Security Features

### Authentication Middleware
```php
function cekloginadmin() {
    // Check if user is logged in
    if (!isset($_SESSION['admin_logged_in']) || $_SESSION['admin_logged_in'] !== true) {
        header('Location: /login');
        exit;
    }
    return true;
}
```

### API Token Validation
```php
function checkApiToken() {
    $headers = getallheaders();
    $token = $headers['Authorization'] ?? '';
    
    if (empty($token)) {
        return json_encode([
            'status' => false,
            'message' => 'Unauthorized: No token provided'
        ]);
    }
    
    // Validate token logic here
    return true;
}
```

### CORS Support
```php
$route->add('/api/endpoint', function(){
    // API logic
})
->cors("all")  // Enable CORS for all origins
->use('vendor/autoload.php')
->use('module/db.php')
->middleware('cekloginadmin');
```

## 🏗️ Creating New Routes

### 1. Create New Route File
```php
// web/admin/new_route.php
<?php
use NN\Route;
use NN\Module\View;

$route->add('/your/new/route', function(){
    View::render('your/view/path');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### 2. Auto-Loading Routes
```php
// In route.php
if(APPNAME == 'usp' || APPNAME == 'acc'){
    // Load all admin routes
    foreach(Webs::map(SETUP_PATH.'web/admin/') as $pathLoad){
        include_once $pathLoad;
    };
    // Load all API routes
    foreach(Webs::map(SETUP_PATH.'web/api/flamboyan/') as $pathLoad){
        include_once $pathLoad;
    };
}
```

## 📋 Route Best Practices

### 1. Naming Conventions
- **URLs**: Use lowercase with hyphens or underscores
- **Route Names**: Be descriptive and consistent
- **File Names**: Match route functionality

### 2. Organization
- **Group related routes** in same file
- **Use consistent URL patterns**
- **Separate admin and API routes**

### 3. Security
- **Always include authentication middleware**
- **Validate input parameters**
- **Use HTTPS in production**
- **Implement proper CORS**

### 4. Performance
- **Minimize dependencies** per route
- **Use appropriate caching**
- **Optimize database queries**

### 5. Error Handling
- **Implement proper error responses**
- **Log errors appropriately**
- **Provide meaningful error messages**

## 🔧 Common Route Patterns

### 1. Setting Routes
```php
$route->add('/flamboyan/{module}/setting', function(){
    View::render('flamboyan/setting/{module}');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### 2. Admin Module Routes
```php
$route->add('/flamboyan/{module}/admin', function(){
    View::render('flamboyan/admin/{module}');
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### 3. CRUD Routes
```php
$route->add('/admin/crud/{id}', function($id = ""){
    View::render('admin/crud', ["id"=>$id]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

## 🎯 Kesimpulan

Sistem routing FLM Builder menyediakan fleksibilitas dan kemudahan dalam mendefinisikan route dengan fitur-fitur modern seperti middleware, dependencies, dan parameter routing. Dengan mengikuti best practices yang telah ditetapkan, developer dapat membuat route yang aman, efisien, dan mudah dimaintain. 