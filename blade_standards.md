# Blade Script Standards

## Standar Pemanggilan Script di Blade

### 1. Penggunaan @section('script')

Semua pemanggilan script (termasuk View::jsm(...)) harus selalu berada di dalam `@section('script')` pada file Blade:

```php
@extends('admin.layout')

@section('content')
    <!-- Content here -->
@endsection

@section('script')
    {!! View::jsm('path/to/script.js') !!}
@endsection
```

### 2. Hindari Penggunaan Manual

**Jangan gunakan** `<script src=...>` manual dan juga `@push('js')` untuk script utama modul:

```php
<!-- ❌ DILARANG -->
<script src="/assets/js/script.js"></script>

<!-- ❌ DILARANG -->
@push('js')
    <script src="/assets/js/script.js"></script>
@endpush

<!-- ✅ BENAR -->
@section('script')
    {!! View::jsm('path/to/script.js') !!}
@endsection
```

### 3. Style Tambahan

Jika ada style tambahan, letakkan di dalam `@section('script')`:

```php
@section('script')
    <style>
        .custom-style {
            background-color: #f0f0f0;
            padding: 10px;
        }
        
        .custom-button {
            border-radius: 5px;
            border: none;
        }
    </style>
    {!! View::jsm('path/to/script.js') !!}
@endsection
```

## Standar Komunikasi Data Blade ke JS

### 1. Variabel Global

Untuk komunikasi data antara Blade dan JS (misal id_biodata), gunakan variabel global seperti `app_id_biodata`:

```php
@section('script')
    <script>
        // Set variabel global dari Blade
        window.app_id_biodata = '{{ $id_biodata }}';
        window.app_user_id = '{{ $user_id }}';
        window.app_module_name = '{{ $module_name }}';
    </script>
    {!! View::jsm('path/to/script.js') !!}
@endsection
```

### 2. Akses di JavaScript

```javascript
// Di file script.js
console.log('ID Biodata:', window.app_id_biodata);
console.log('User ID:', window.app_user_id);

// Atau gunakan variabel global langsung
if (typeof app_id_biodata !== 'undefined') {
    console.log('ID Biodata:', app_id_biodata);
}
```

### 3. Larangan

**Jangan gunakan** `{id_biodata}` di dalam file JS, karena JS di-load secara noscript/dinamis dan tidak bisa akses variabel Blade secara langsung:

```javascript
// ❌ DILARANG - Tidak akan berfungsi
var id = '{id_biodata}';

// ✅ BENAR - Gunakan variabel global
var id = window.app_id_biodata;
```

## Standar Penentuan & Penampilan Foto di Template Detail

### 1. Logic Penentuan Foto

Untuk setiap template detail yang membutuhkan foto profil, gunakan logic penentuan foto seperti di `temp2/detailpersonal`:

```php
@php
    $photoPath = '';
    if (!empty($data->photo_path)) {
        $photoPath = PATH . '/' . $data->photo_path;
    } else {
        $photoPath = 'assets/uploads/default-user.png';
    }
@endphp
```

### 2. Struktur HTML

Bungkus `<img>` dengan `<a class='profile-pic'>` untuk konsistensi dan kemudahan pengembangan fitur upload/modal:

```php
<a class="profile-pic" href="{{ $photoPath }}" target="_blank">
    <img src="{{ $photoPath }}" 
         alt="Profile Photo" 
         class="img-fluid rounded-circle"
         style="width: 100px; height: 100px; object-fit: cover;">
</a>
```

### 3. Fallback Image

Selalu sediakan fallback image jika foto tidak ada:

```php
@php
    $photoPath = !empty($data->photo_path) 
        ? PATH . '/' . $data->photo_path 
        : 'assets/uploads/default-user.png';
@endphp
```

## Contoh Implementasi Lengkap

### File: views/flamboyan/admin/detail.blade.php

```php
@extends('admin.layout')

@section('content')
<div class="container-fluid">
    <div class="row">
        <div class="col-md-12">
            <div class="card">
                <div class="card-header">
                    <h4>Detail Biodata</h4>
                </div>
                <div class="card-body">
                    <div class="row">
                        <div class="col-md-3 text-center">
                            @php
                                $photoPath = !empty($biodata->photo_path) 
                                    ? PATH . '/' . $biodata->photo_path 
                                    : 'assets/uploads/default-user.png';
                            @endphp
                            
                            <a class="profile-pic" href="{{ $photoPath }}" target="_blank">
                                <img src="{{ $photoPath }}" 
                                     alt="Profile Photo" 
                                     class="img-fluid rounded-circle mb-3"
                                     style="width: 150px; height: 150px; object-fit: cover;">
                            </a>
                        </div>
                        <div class="col-md-9">
                            <h5>{{ $biodata->nama }}</h5>
                            <p><strong>ID:</strong> {{ $biodata->id_biodata }}</p>
                            <p><strong>Email:</strong> {{ $biodata->email }}</p>
                            <p><strong>Phone:</strong> {{ $biodata->phone }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
@endsection

@section('script')
    <style>
        .profile-pic:hover {
            opacity: 0.8;
            transition: opacity 0.3s ease;
        }
        
        .card {
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
    </style>
    
    <script>
        // Set variabel global untuk komunikasi dengan JS
        window.app_id_biodata = '{{ $biodata->id_biodata }}';
        window.app_user_id = '{{ $biodata->user_id }}';
        window.app_photo_path = '{{ $biodata->photo_path }}';
    </script>
    
    {!! View::jsm('flamboyan/admin/detail.js') !!}
@endsection
```

### File: script/flamboyan/admin/detail.js

```javascript
// Akses variabel global yang di-set dari Blade
document.addEventListener('DOMContentLoaded', function() {
    const idBiodata = window.app_id_biodata;
    const userId = window.app_user_id;
    const photoPath = window.app_photo_path;
    
    console.log('ID Biodata:', idBiodata);
    console.log('User ID:', userId);
    console.log('Photo Path:', photoPath);
    
    // Implementasi detail logic
    loadBiodataDetail(idBiodata);
});

function loadBiodataDetail(idBiodata) {
    // AJAX call untuk load detail data
    fetch(`/api/biodata/${idBiodata}`)
        .then(response => response.json())
        .then(data => {
            // Update UI dengan data
            updateBiodataUI(data);
        })
        .catch(error => {
            console.error('Error loading biodata:', error);
        });
}

function updateBiodataUI(data) {
    // Update UI elements
    document.getElementById('nama').textContent = data.nama;
    document.getElementById('email').textContent = data.email;
    // ... more updates
}
```

## Best Practices

### 1. Konsistensi Naming
- Gunakan prefix `app_` untuk variabel global dari Blade
- Konsisten dalam penamaan variabel

### 2. Error Handling
- Selalu cek keberadaan variabel global sebelum digunakan
- Sediakan fallback untuk data yang tidak ada

### 3. Performance
- Minimalkan jumlah variabel global
- Gunakan lazy loading untuk script yang tidak critical

### 4. Security
- Sanitasi data sebelum di-set ke variabel global
- Hindari expose sensitive data ke client-side

### 5. Maintainability
- Dokumentasikan semua variabel global
- Gunakan konstanta untuk nilai yang sering digunakan 