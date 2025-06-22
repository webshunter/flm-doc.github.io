# API Development - FLM Builder

## 🔌 Framework API Development

FLM Builder menyediakan framework yang terstruktur untuk pengembangan API dengan fitur-fitur modern seperti middleware, authentication, dan response formatting yang konsisten.

## 🏗️ Basic API Route Structure

### Format Dasar
```php
$route->add('/api/[endpoint]', function(){
    // API logic here
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

### Komponen API Route
1. **Route Definition**: `$route->add('/api/endpoint', function(){})`
2. **API Logic**: Business logic dan data processing
3. **Response Format**: JSON response yang konsisten
4. **Dependencies**: Database dan module dependencies
5. **Middleware**: Authentication dan security

## 📤 API Response Format

### Success Response
```php
return json_encode([
    'status' => true,
    'message' => 'Success',
    'data' => $data
]);
```

### Error Response
```php
return json_encode([
    'status' => false,
    'message' => 'Error message',
    'error' => $error_details
]);
```

### Response Properties
- **status**: Boolean indicating success/failure
- **message**: Human-readable message
- **data**: Response data (for success)
- **error**: Error details (for failure)

## 🔄 Common API Patterns

### 1. GET Request (Fetch Data)
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

### 2. POST Request (Create Data)
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

### 3. PUT Request (Update Data)
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

### 4. DELETE Request (Remove Data)
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

## 🔐 API Security Features

### Authentication Middleware
```php
function cekloginadmin() {
    // Check if user is logged in
    if (!isset($_SESSION['admin_logged_in']) || $_SESSION['admin_logged_in'] !== true) {
        return json_encode([
            'status' => false,
            'message' => 'Unauthorized: User not logged in'
        ]);
    }
    return true;
}
```

### Token Validation
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
    $validToken = validateToken($token);
    if (!$validToken) {
        return json_encode([
            'status' => false,
            'message' => 'Unauthorized: Invalid token'
        ]);
    }
    
    return true;
}

// Usage in route
$route->add('/api/secure-endpoint', function(){
    // API logic
})
->use('vendor/autoload.php')
->use('module/db.php')
->middleware('checkApiToken')
->middleware('cekloginadmin');
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

## 📁 API Folder Structure

### Directory Organization
```
Root/
├── web/
│   ├── admin/           # Admin routes
│   └── api/            # API routes
│       └── flamboyan/  # API endpoints
│           ├── users.php
│           ├── auth.php
│           ├── detailfamily.php
│           ├── detailinterview.php
│           ├── detailkettugas.php
│           ├── detailpengalaman.php
│           ├── detailrequest.php
│           ├── detailskill.php
│           ├── detailtugas.php
│           ├── detailvaksin.php
│           ├── detailworking.php
│           └── upload_arc.php
```

### Auto-Loading Implementation
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

## 🛠️ Creating New API Endpoints

### 1. Create New API File
```php
// web/api/flamboyan/users.php
<?php
use NN\Route;
use NN\Module\DB;

// GET all users
$route->add('/api/v1/users', function(){
    $db = new DB();
    $data = $db->query_result_object("SELECT * FROM users");
    
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

// GET single user
$route->add('/api/v1/users/{id}', function($id){
    $db = new DB();
    $data = $db->query_result_object_row("SELECT * FROM users WHERE id = '$id'");
    
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

### 2. API File Organization
```php
<?php
// 1. Namespace imports
use NN\Route;
use NN\Module\DB;

// 2. Helper functions
function validateUserData($data) {
    $errors = [];
    
    if (empty($data['name'])) {
        $errors[] = 'Name is required';
    }
    
    if (empty($data['email'])) {
        $errors[] = 'Email is required';
    } elseif (!filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
        $errors[] = 'Invalid email format';
    }
    
    return $errors;
}

// 3. Route definitions
$route->add('/api/v1/users', function(){
    $method = $_SERVER['REQUEST_METHOD'];
    $db = new DB();
    
    switch($method) {
        case 'GET':
            $data = $db->query_result_object("SELECT * FROM users");
            return json_encode([
                'status' => true,
                'message' => 'Users retrieved successfully',
                'data' => $data
            ]);
            
        case 'POST':
            $data = json_decode(file_get_contents('php://input'), true);
            
            // Validate data
            $errors = validateUserData($data);
            if (!empty($errors)) {
                return json_encode([
                    'status' => false,
                    'message' => 'Validation failed',
                    'error' => $errors
                ]);
            }
            
            $result = $db->insert('users', $data);
            return json_encode([
                'status' => true,
                'message' => 'User created successfully',
                'data' => $result
            ]);
    }
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

## 🔍 Parameter Route

### Basic Parameter Route
```php
$route->add('/api/nama/{nama}', function($nama){
    // $nama akan berisi nilai dari parameter
    // Contoh: jika URL = /api/nama/gugus
    // Maka $nama = "gugus"
    return json_encode([
        'status' => true,
        'message' => 'Sukses',
        'data' => $nama
    ]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### Multiple Parameters
```php
$route->add('/api/user/{id}/post/{post_id}', function($id, $post_id){
    // $id dan $post_id akan berisi nilai dari parameter
    // Contoh: jika URL = /api/user/1/post/123
    // Maka $id = "1" dan $post_id = "123"
    return json_encode([
        'status' => true,
        'message' => 'Sukses',
        'data' => [
            'user_id' => $id,
            'post_id' => $post_id
        ]
    ]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### Parameter Validation
```php
$route->add('/api/data/{id}', function($id){
    if (empty($id)) {
        return json_encode([
            'status' => false,
            'message' => 'ID tidak boleh kosong'
        ]);
    }
    
    if (!is_numeric($id)) {
        return json_encode([
            'status' => false,
            'message' => 'ID harus berupa angka'
        ]);
    }
    
    // Proses data
    $db = new DB();
    $data = $db->query_result_object_row("SELECT * FROM data WHERE id = '$id'");
    
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

## 📊 Example API Implementation

### Complete API Structure
```php
// web/api/flamboyan/users.php
<?php
use NN\Route;
use NN\Module\DB;

$route->add('/api/v1/users', function(){
    $method = $_SERVER['REQUEST_METHOD'];
    $db = new DB();
    
    switch($method) {
        case 'GET':
            $data = $db->query_result_object("SELECT * FROM users");
            return json_encode([
                'status' => true,
                'message' => 'Users retrieved successfully',
                'data' => $data
            ]);
            
        case 'POST':
            $data = json_decode(file_get_contents('php://input'), true);
            $result = $db->insert('users', $data);
            return json_encode([
                'status' => true,
                'message' => 'User created successfully',
                'data' => $result
            ]);
    }
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('checkApiToken')
->middleware('cekloginadmin');

$route->add('/api/v1/users/{id}', function($id){
    $method = $_SERVER['REQUEST_METHOD'];
    $db = new DB();
    
    switch($method) {
        case 'GET':
            $data = $db->query_result_object_row("SELECT * FROM users WHERE id = '$id'");
            return json_encode([
                'status' => true,
                'message' => 'User retrieved successfully',
                'data' => $data
            ]);
            
        case 'PUT':
            $data = json_decode(file_get_contents('php://input'), true);
            $result = $db->sql_update_query('users', $data, "id = '$id'");
            return json_encode([
                'status' => true,
                'message' => 'User updated successfully',
                'data' => $result
            ]);
            
        case 'DELETE':
            $result = $db->sql_delete_query('users', "id = '$id'");
            return json_encode([
                'status' => true,
                'message' => 'User deleted successfully',
                'data' => $result
            ]);
    }
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('checkApiToken')
->middleware('cekloginadmin');
```

### API Documentation
```php
/**
 * @api {get} /api/v1/users Get all users
 * @apiName GetUsers
 * @apiGroup User
 * @apiVersion 1.0.0
 *
 * @apiSuccess {Boolean} status Status of the request
 * @apiSuccess {String} message Success message
 * @apiSuccess {Array} data Array of users
 *
 * @apiSuccessExample {json} Success-Response:
 *     HTTP/1.1 200 OK
 *     {
 *       "status": true,
 *       "message": "Users retrieved successfully",
 *       "data": [...]
 *     }
 */
```

## 🧪 Testing API Endpoints

### Using cURL
```bash
# GET request
curl -X GET http://localhost/api/v1/users

# POST request
curl -X POST http://localhost/api/v1/users \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}'

# PUT request
curl -X PUT http://localhost/api/v1/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"John Updated"}'

# DELETE request
curl -X DELETE http://localhost/api/v1/users/1
```

### Using Postman
1. Create new request
2. Set method and URL
3. Add headers and body
4. Send request

### Testing with Authentication
```bash
# With token
curl -X GET http://localhost/api/v1/users \
  -H "Authorization: Bearer your_token_here"

# With session (for browser testing)
# Login first, then access API endpoint
```

## 📈 API Monitoring and Logging

### Request Logging
```php
function logApiRequest($endpoint, $method, $data) {
    $log = [
        'timestamp' => date('Y-m-d H:i:s'),
        'endpoint' => $endpoint,
        'method' => $method,
        'data' => $data,
        'ip' => $_SERVER['REMOTE_ADDR'] ?? 'unknown'
    ];
    
    file_put_contents(
        'logs/api.log',
        json_encode($log) . "\n",
        FILE_APPEND
    );
}

// Usage in route
$route->add('/api/endpoint', function(){
    logApiRequest('/api/endpoint', $_SERVER['REQUEST_METHOD'], $_POST);
    // API logic
});
```

### Performance Monitoring
```php
function monitorApiPerformance($start_time) {
    $end_time = microtime(true);
    $execution_time = ($end_time - $start_time);
    
    if ($execution_time > 1) { // Log slow requests
        logApiRequest('performance', 'warning', [
            'execution_time' => $execution_time
        ]);
    }
}

// Usage in route
$route->add('/api/endpoint', function(){
    $start_time = microtime(true);
    
    // API logic here
    
    monitorApiPerformance($start_time);
});
```

## 🔧 Best Practices

### 1. Route Organization
- **Group related endpoints** in same file
- **Use consistent naming** conventions
- **Version your APIs** (v1, v2, etc.)
- **Document endpoints** properly

### 2. Error Handling
```php
try {
    // API logic
    $result = $db->query_result_object("SELECT * FROM table");
    
    return json_encode([
        'status' => true,
        'message' => 'Success',
        'data' => $result
    ]);
} catch (Exception $e) {
    return json_encode([
        'status' => false,
        'message' => 'Error occurred',
        'error' => $e->getMessage()
    ]);
}
```

### 3. Security
- **Implement authentication** for all endpoints
- **Validate input data** thoroughly
- **Sanitize output** data
- **Use HTTPS** in production
- **Enable CORS** properly

### 4. Performance
- **Use proper indexing** on database
- **Implement caching** for static data
- **Optimize queries** for better performance
- **Limit response size** for large datasets

### 5. Response Consistency
```php
// Always use consistent response format
function apiResponse($status, $message, $data = null, $error = null) {
    $response = [
        'status' => $status,
        'message' => $message
    ];
    
    if ($data !== null) {
        $response['data'] = $data;
    }
    
    if ($error !== null) {
        $response['error'] = $error;
    }
    
    return json_encode($response);
}

// Usage
return apiResponse(true, 'Success', $data);
return apiResponse(false, 'Error occurred', null, $error);
```

## 📋 Dependencies Route Flamboyan

### Standar Dependensi Route
Setiap route di folder `web/api/flamboyan` harus selalu menambahkan dependensi berikut:
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
    return json_encode(['data' => $param]);
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### Catatan Penting
1. **Pastikan untuk selalu menambahkan dependensi** ini di setiap route baru yang dibuat
2. **Dependensi ini diperlukan untuk**:
   - Autoloading class dan dependencies (vendor/autoload.php)
   - Koneksi database utama (module/db.php)
   - Koneksi database sekunder (module/secondb.php)
   - Data perusahaan (module/perusahaan.php)
   - Pengecekan login admin (middleware cekloginadmin)
3. **Urutan dependensi** harus sesuai seperti di atas
4. **Jangan menghapus atau mengubah** urutan dependensi yang sudah ada

## 🎯 Kesimpulan

Framework API Development FLM Builder menyediakan solusi yang terstruktur dan aman untuk pengembangan API. Dengan fitur-fitur seperti middleware, authentication, response formatting yang konsisten, dan auto-loading, developer dapat membuat API yang robust dan mudah dimaintain. 