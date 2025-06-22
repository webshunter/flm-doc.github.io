# Special Notes

## Catatan Khusus: singleData pada family.js

### Penjelasan singleData
Pada file `script/flamboyan/admin/family.js` terdapat konfigurasi `singleData: true` pada loadCRUD.

**Artinya**: Halaman ini hanya mengatur dan menampilkan satu data saja (single record) untuk setiap id_biodata, bukan list banyak data.

### Efek di CRUD
- **Tidak ada tabel list**: Hanya tampilan detail satu data (atau form tambah jika belum ada)
- **Tombol yang muncul**: 
  - "Tambah Data" (jika belum ada data)
  - "Update Data" (jika sudah ada data)
- **Query otomatis**: LIMIT 1 dan hanya mengambil satu baris data

### Kapan Menggunakan singleData
- Cocok untuk data yang memang hanya boleh satu per parent
- Contoh: data keluarga per biodata
- Data yang bersifat one-to-one relationship

### Perbedaan dengan Mode Default
Implementasi ini berbeda dengan mode default CRUD yang menampilkan banyak baris data dalam tabel.

## Query Template Notes (CRUD JS)

### Pola Penulisan Query
Pada file-file CRUD JS (misal: `script/flamboyan/admin/pengalaman.js`), terdapat pola penulisan query SQL pada properti `queryTemp` seperti berikut:

```javascript
queryTemp: "SELECT {select} FROM pengalaman WHERE id_biodata = '" + (_id('app_id_biodata').innerText)+"' || ORDER BY id_pengalaman DESC",
```

### Catatan Penting tentang Tanda `||`

**Selalu ada tanda `||` sebelum `ORDER BY` di query.**

#### Masalah dengan Tanda `||`
- Tanda `||` ini tidak umum dalam SQL standar
- Di beberapa database, `||` adalah operator konkatenasi string
- Di MySQL biasanya menggunakan `CONCAT()` atau `+`
- Jika query ini dijalankan di MySQL, `||` akan dianggap sebagai operator logika OR

#### Query yang Dihasilkan
```sql
SELECT ... FROM pengalaman WHERE id_biodata = '...' || ORDER BY id_pengalaman DESC
```

Ini bisa menyebabkan error atau hasil tidak diharapkan.

#### Kemungkinan Penggunaan
Kemungkinan besar, tanda `||` ini digunakan sebagai placeholder yang akan diproses/diganti di backend sebelum query dieksekusi.

### Saran Implementasi

#### 1. Proses Penggantian di Backend
Pastikan ada proses penggantian/parse di backend untuk menghilangkan atau mengganti `||` sebelum query dijalankan ke database.

#### 2. Perbaikan Query
Jika tidak ada proses tersebut, pertimbangkan untuk memperbaiki query agar sesuai standar SQL:

```javascript
// ❌ Dengan tanda ||
queryTemp: "SELECT {select} FROM pengalaman WHERE id_biodata = '" + (_id('app_id_biodata').innerText)+"' || ORDER BY id_pengalaman DESC",

// ✅ Tanpa tanda ||
queryTemp: "SELECT {select} FROM pengalaman WHERE id_biodata = '" + (_id('app_id_biodata').innerText)+"' ORDER BY id_pengalaman DESC",
```

### Kesimpulan
- Tanda `||` sebelum `ORDER BY` adalah pola konsisten di file CRUD JS
- Dokumentasikan alasan penggunaan tanda ini agar tidak membingungkan developer lain
- Pastikan ada mekanisme penggantian di backend atau perbaiki query sesuai standar SQL

## Contoh Implementasi singleData

### File: script/flamboyan/admin/family.js
```javascript
loadCRUD({
    title: function () { return 'FAMILY'; },
    titlePage: 'Family',
    subTitlePage: 'Setting',
    table: "family",
    idform: "containerforms",
    kode: 'id_family',
    serverSide: true,
    singleData: true,  // ← Ini yang membuat single data
    queryTemp: "SELECT {select} FROM family WHERE id_biodata = '" + (_id('app_id_biodata').innerText)+"' || ORDER BY id_family DESC",
    // ... konfigurasi lainnya
});
```

### Efek pada UI
```html
<!-- Jika belum ada data -->
<div class="text-center">
    <button class="btn btn-primary" onclick="tambahData()">
        <i class="fas fa-plus"></i> Tambah Data
    </button>
</div>

<!-- Jika sudah ada data -->
<div class="card">
    <div class="card-header">
        <h5>Data Keluarga</h5>
        <button class="btn btn-warning" onclick="updateData()">
            <i class="fas fa-edit"></i> Update Data
        </button>
    </div>
    <div class="card-body">
        <!-- Detail data keluarga -->
    </div>
</div>
```

## Troubleshooting

### Masalah dengan singleData
1. **Data tidak muncul**: Periksa apakah query WHERE clause sudah benar
2. **Tombol tidak sesuai**: Pastikan konfigurasi singleData sudah benar
3. **Multiple records**: Periksa apakah ada duplikasi data di database

### Masalah dengan Query Template
1. **SQL Error**: Periksa apakah tanda `||` sudah diproses di backend
2. **Data tidak ter-load**: Periksa query yang dihasilkan di browser console
3. **Performance issue**: Pastikan query sudah dioptimasi dengan index yang tepat

## Best Practices

### Untuk singleData
1. **Gunakan untuk data one-to-one**: Hanya untuk relasi satu-ke-satu
2. **Validasi di backend**: Pastikan hanya satu record per parent
3. **Clear UI feedback**: Berikan feedback yang jelas kepada user

### Untuk Query Template
1. **Dokumentasi**: Dokumentasikan penggunaan tanda `||`
2. **Backend processing**: Implementasikan proses penggantian di backend
3. **Testing**: Test query di berbagai database engine
4. **Fallback**: Sediakan fallback jika proses penggantian gagal 