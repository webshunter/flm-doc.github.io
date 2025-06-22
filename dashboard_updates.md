# Dashboard Updates

## Dashboard Baru Flamboyan

Dashboard telah diperbarui dengan mengacu pada aplikasi Flamboyan yang ada. Fitur-fitur yang ditambahkan:

### 1. Statistik TKI per Sektor
- **Male Formal (MF)**: TKI laki-laki sektor formal
- **Male Informal (MI)**: TKI laki-laki sektor informal
- **Female Formal (FF)**: TKI perempuan sektor formal
- **Female Informal (FI)**: TKI perempuan sektor informal
- **Panti Jompo (JP)**: TKI sektor panti jompo

### 2. Data Detail per Sektor
- Total data per sektor
- Jumlah yang sudah terbang
- Jumlah MD/UNFIT (Mengundurkan Diri/UNFIT)

### 3. Chart Visualisasi
- **Pie Chart**: Persentase data TKI keseluruhan
- **Donut Chart**: Persentase data TKI per sponsor

### 4. Tabel Data Terbaru
- Data majikan terbaru
- Data passport terbaru
- Data terbang terbaru
- Data MD/UNFIT terbaru

## Route yang Ditambahkan

### Dashboard Utama
```php
$route->add('/admin/dashboard', function() {
    // Query data statistik TKI
    // Render view dashboard_flamboyan
})
->use('vendor/autoload.php')
->use('module/db.php')
->use('module/secondb.php')
->use('module/perusahaan.php')
->middleware('cekloginadmin');
```

### Route AJAX untuk Data Terbaru
```php
// Data majikan terbaru
$route->add('/admin/dashboard/majikan-terbaru', function() {
    // Return data majikan terbaru dalam format JSON
});

// Data passport terbaru
$route->add('/admin/dashboard/passport-terbaru', function() {
    // Return data passport terbaru dalam format JSON
});

// Data terbang terbaru
$route->add('/admin/dashboard/terbang-terbaru', function() {
    // Return data terbang terbaru dalam format JSON
});

// Data MD/UNFIT terbaru
$route->add('/admin/dashboard/md-unfit-terbaru', function() {
    // Return data MD/UNFIT terbaru dalam format JSON
});
```

## View yang Dibuat

### dashboard_flamboyan.blade.php
- View dashboard utama dengan statistik dan chart
- Lokasi: `views/admin/dashboard_flamboyan.blade.php`
- Menggunakan layout admin standar
- Responsive design untuk berbagai ukuran layar

## Database yang Digunakan

### Database Sekunder (SECOND_DB)
- Menggunakan `SECOND_DB` untuk query data TKI
- Tabel utama:
  - `personal` - Data personal TKI
  - `majikan` - Data majikan
  - `paspor` - Data passport
  - `visa` - Data visa
  - `datasponsor` - Data sponsor

### Filter Data
- Filter berdasarkan `id_biodata` dengan prefix sektor:
  - `MF-` untuk Male Formal
  - `MI-` untuk Male Informal
  - `FF-` untuk Female Formal
  - `FI-` untuk Female Informal
  - `JP-` untuk Panti Jompo

## Implementasi Detail

### 1. Query Statistik
```sql
-- Contoh query untuk statistik per sektor
SELECT 
    COUNT(*) as total,
    SUM(CASE WHEN status_terbang = 'Y' THEN 1 ELSE 0 END) as sudah_terbang,
    SUM(CASE WHEN status_md = 'Y' OR status_unfit = 'Y' THEN 1 ELSE 0 END) as md_unfit
FROM personal 
WHERE id_biodata LIKE 'MF-%'
```

### 2. Chart Configuration
```javascript
// Pie chart untuk persentase TKI
var pieChart = echarts.init(document.getElementById('pie-chart'));
var pieOption = {
    series: [{
        type: 'pie',
        data: [
            {value: mfCount, name: 'Male Formal'},
            {value: miCount, name: 'Male Informal'},
            {value: ffCount, name: 'Female Formal'},
            {value: fiCount, name: 'Female Informal'},
            {value: jpCount, name: 'Panti Jompo'}
        ]
    }]
};
```

### 3. DataTables Configuration
```javascript
// Tabel data terbaru dengan AJAX
$('#majikan-table').DataTable({
    ajax: '/admin/dashboard/majikan-terbaru',
    columns: [
        {data: 'nama_majikan'},
        {data: 'alamat'},
        {data: 'telepon'},
        {data: 'created_at'}
    ]
});
```

## Catatan Implementasi

1. **Dependensi Standar**: Semua route menggunakan dependensi standar Flamboyan
2. **Real-time Data**: Data diambil secara real-time dari database
3. **Chart Library**: Menggunakan library ECharts untuk visualisasi
4. **DataTables**: Tabel menggunakan DataTables dengan AJAX
5. **Responsive Design**: Design responsive untuk berbagai ukuran layar

## Troubleshooting

### Chart tidak muncul
- **Penyebab**: Library ECharts tidak dimuat
- **Solusi**: Pastikan ECharts dimuat sebelum inisialisasi chart

### Data tidak ter-update
- **Penyebab**: Cache browser atau query error
- **Solusi**: Clear cache dan periksa query database

### Tabel kosong
- **Penyebab**: AJAX endpoint tidak berfungsi atau data kosong
- **Solusi**: Periksa route AJAX dan data di database 