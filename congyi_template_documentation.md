# Template DOCX Cong Yi - Dokumentasi Lengkap

## 📋 Daftar Isi
1. [Pendahuluan](#pendahuluan)
2. [Struktur Template](#struktur-template)
3. [Placeholder Reference](#placeholder-reference)
4. [Layout & Styling](#layout--styling)
5. [Dynamic Content](#dynamic-content)
6. [Image Handling](#image-handling)
7. [Implementation Guide](#implementation-guide)
8. [Best Practices](#best-practices)
9. [Troubleshooting](#troubleshooting)

## 🎯 Pendahuluan

Template `biodatacongyi.docx` adalah template Word (.docx) yang digunakan untuk menghasilkan biodata TKI (Tenaga Kerja Indonesia) dalam format khusus Taiwan/Cong Yi. Template ini dirancang untuk memenuhi standar Taiwan dalam dokumen biodata TKI.

### Fitur Utama
- ✅ **Bilingual Format** - Indonesia & Mandarin
- ✅ **Professional Layout** - Standar Taiwan
- ✅ **Dynamic Content** - Tabel pengalaman kerja dinamis
- ✅ **Multiple Images** - Foto profil & ARC
- ✅ **100+ Placeholders** - Data lengkap TKI
- ✅ **Complex Styling** - Background colors & formatting

### File Location
```
files/biodatacongyi.docx
```

## 🏗️ Struktur Template

### 1. Header Section
```
┌─────────────────────────────────────────────────────────┐
│                   崇義國際開發有限公司                    │
│              CHONG YI INTERNATIONAL DEVELOPMENT CO.,LTD. │
└─────────────────────────────────────────────────────────┘
```

### 2. Company Information
```
┌─────────────────────────────────────────────────────────┐
│              PT FLAMBOYAN GEMA JASA                     │
│                遠東國際人力有限公司                       │
│           http://www.ptflamboyanlawang.com              │
└─────────────────────────────────────────────────────────┘
```

### 3. Job Information Table
```
┌─────────────┬─────────────┬─────────────┬─────────────┐
│ 申請職位    │ 仲介公司    │ 僱主名稱    │ PL          │
│ POSISI      │ AGENT       │ NAMA MAJIKAN│             │
├─────────────┼─────────────┼─────────────┼─────────────┤
│ {kerjaposisi}│ {namaagen}  │ {namamajikan}│ {pl}        │
└─────────────┴─────────────┴─────────────┴─────────────┘
```

### 4. Personal Data Section
```
┌─────────────┬─────────────┬─────────────┬─────────────┐
│    {gambar} │ 個人資料    │ 姓名        │ {nama}      │
│             │ PERSONAL    │ NAMA        │             │
│             │ DATA        │             │             │
└─────────────┴─────────────┴─────────────┴─────────────┘
```

## 📊 Placeholder Reference

### 🔤 Data Personal

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{nama}` | Nama lengkap (Indonesia) | "Ahmad Susanto" |
| `{namamandarin}` | Nama Mandarin | "阿末·蘇珊托" |
| `{tanggaldaftar}` | Tanggal pendaftaran | "2024-01-15" |
| `{jeniskelamin}` | Jenis kelamin | "Laki-laki" |
| `{tinggibadan}` | Tinggi badan (CM) | "170 CM" |
| `{warganegara}` | Warga negara | "Indonesia" |
| `{beratbadan}` | Berat badan (KG) | "65 KG" |
| `{tanggallahir}` | Tanggal lahir | "1990-05-20" |
| `{agama}` | Agama | "Islam" |
| `{tempatlahir}` | Tempat lahir | "Jakarta" |
| `{status}` | Status pernikahan | "Menikah" |
| `{umur}` | Umur (otomatis) | "34" |
| `{tanggalstatuspernnikahan}` | Tanggal menikah/cerai | "2015-06-10" |
| `{pendidikan}` | Pendidikan terakhir | "SMA" |
| `{statuspendidikan}` | Status pendidikan | "LULUS" |
| `{alamat}` | Alamat lengkap | "Jl. Sudirman No. 123" |
| `{Provinsi}` | Provinsi | "DKI Jakarta" |

### 🌐 Kemampuan Bahasa

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{bahasamandarin}` | Label Mandarin | "國語 MANDARIN" |
| `{statusbahasamandarin}` | Kemampuan Mandarin | "Baik" |
| `{bahasainggriss}` | Label Inggris | "英語 INGGRIS" |
| `{statusbahasainggriss}` | Kemampuan Inggris | "Sedang" |

### 💉 Data Vaksinasi

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{vaksin1}` | Jenis vaksin ke-1 | "Sinovac" |
| `{vaksin1tgl}` | Tanggal vaksin ke-1 | "2021-08-15" |
| `{vaksin2}` | Jenis vaksin ke-2 | "AstraZeneca" |
| `{vaksin2tgl}` | Tanggal vaksin ke-2 | "2021-09-20" |
| `{vaksin3}` | Jenis vaksin ke-3 | "Pfizer" |
| `{vaksin3tgl}` | Tanggal vaksin ke-3 | "2022-01-10" |

### ��‍👩‍👧‍👦 Data Keluarga

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{namabapak}` | Nama bapak | "Sukarno" |
| `{umurbapak}` | Umur bapak | "65 TAHUN 嵗" |
| `{pekerjaanbapak}` | Pekerjaan bapak | "Wiraswasta" |
| `{namaibu}` | Nama ibu | "Siti Aminah" |
| `{umuribu}` | Umur ibu | "60 TAHUN 嵗" |
| `{pekerjaanibu}` | Pekerjaan ibu | "Ibu Rumah Tangga" |
| `{namaistri}` | Nama istri/suami | "Siti Fatimah" |
| `{umuristri}` | Umur istri/suami | "30 TAHUN 嵗" |
| `{pekerjaanistri}` | Pekerjaan istri/suami | "Guru" |
| `{banyaksaudaralaki}` | Jumlah saudara laki-laki | "2" |
| `{banyaksaudaralaki}` | Jumlah saudara perempuan | "1" |
| `{anaknourutke}` | Anak ke berapa | "1" |
| `{anak}` | Data anak | "Ahmad Junior" |

### 💼 Pengalaman Kerja (Dynamic Table)

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{cln.no}` | Nomor urut | "1", "2", "3" |
| `{cln.negara}` | Negara kerja | "Malaysia" |
| `{cln.posisi}` | Posisi kerja | "Operator" |
| `{cln.mulaiend}` | Tahun mulai/akhir | "2018-2020" |
| `{cln.masakerja}` | Masa kerja (tahun) | "2" |
| `{cln.masabulan}` | Masa kerja (bulan) | "24" |
| `{cln.jenisusaha}` | Jenis usaha | "Manufaktur" |
| `{cln.namaperusahaan}` | Nama perusahaan | "PT Maju Jaya" |
| `{cln.satuan}` | Satuan gaji | "USD" |
| `{cln.gaji}` | Gaji | "500" |
| `{cln.pergaji}` | Detail gaji | "500 USD/bulan" |
| `{cln.jenispekerjaan}` | Jenis pekerjaan | "Operator Mesin" |
| `{cln.alasanberhenti}` | Alasan berhenti | "Kontrak selesai" |

### 🎯 Keterampilan & Kondisi

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{keterampilan}` | Keterampilan khusus | "Mengelas, Memasak" |
| `{hoby}` | Hobi | "Membaca, Olahraga" |
| `{kemampuan}` | Kemampuan angkat | "50 KG" |
| `{yangdimakan}` | Makanan yang dimakan | "Semua jenis makanan" |
| `{pushup}` | Kemampuan push-up | "30 kali" |
| `{minumalkohol}` | Konsumsi alkohol | "Tidak" |
| `{butawarna}` | Buta warna | "Tidak" |
| `{merokok}` | Merokok | "Tidak" |
| `{banyakbatangrokok}` | Jumlah rokok | "0" |
| `{rabunjauh}` | Rabun jauh | "Tidak" |
| `{banyakrabunjauh}` | Tingkat rabun jauh | "Normal" |
| `{idiomatik}` | Kidal | "Tidak" |
| `{tanganbasah}` | Tangan basah | "Tidak 沒有" |
| `{tato}` | Tato | "Tidak" |
| `{operasi}` | Operasi | "Tidak" |
| `{alergi}` | Alergi | "Tidak" |

### 📄 Data Paspor & Medis

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{pasport}` | Status paspor | "SUDAH ADA – 有" |
| `{rencanakerjasampai}` | Rencana kerja sampai | "2024-01-15 s/d 2029-01-15" |
| `{hasilpermeriksaankesehatan}` | Hasil pemeriksaan kesehatan | "SEHAT" |

### 📞 Data Kontak

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{nohp}` | Nomor HP | "+62-812-3456-7890" |
| `{nohpkeluaraga}` | Nomor HP keluarga | "+62-812-3456-7891" |
| `{alamatkeluarga}` | Alamat keluarga | "Jl. Keluarga No. 456" |

### 📝 Data PPTK (Pernyataan)

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{pernyataan1}` | Pernyataan 1 | "YA BERSEDIA / 願意" |
| `{pernyataan2}` | Pernyataan 2 | "YA BERSEDIA / 願意" |
| `{pernyataan3}` | Pernyataan 3 | "YA BERSEDIA / 願意" |
| `{pernyataan4}` | Pernyataan 4 | "YA BERSEDIA / 願意" |
| `{pernyataan5}` | Pernyataan 5 | "YA BERSEDIA / 願意" |
| `{pernyataan6}` | Pernyataan 6 | "YA BERSEDIA / 願意" |
| `{pernyataan7}` | Pernyataan 7 | "YA BERSEDIA / 願意" |
| `{pernyataan8}` | Pernyataan 8 | "YA BERSEDIA / 願意" |

### 💼 Data Kerja

| Placeholder | Deskripsi | Contoh Data |
|-------------|-----------|-------------|
| `{usahamajikan}` | Usaha majikan | "Manufaktur" |
| `{waktukerja}` | Waktu kerja | "8 jam/hari" |
| `{kondisikerja}` | Kondisi kerja | "Indoor" |
| `{jenispekerjaandiminta}` | Jenis pekerjaan diminta | "Operator" |
| `{lokasikerja}` | Lokasi kerja | "Taipei" |
| `{lemburkerja}` | Lembur kerja | "Ya" |
| `{keterangankerja}` | Keterangan kerja | "Siap bekerja shift" |

### 🖼️ Image Placeholders

| Placeholder | Deskripsi | Ukuran | Format |
|-------------|-----------|--------|--------|
| `{gambar}` | Foto profil | 238x302 px | JPEG/PNG |
| `{arc.arc}` | Foto ARC | 500x302 px | JPEG/PNG |

## 🎨 Layout & Styling

### Font Configuration
- **Chinese Text**: DFKai-SB, 宋体 (SimSun)
- **Indonesian Text**: Arial Narrow
- **Headers**: Bold, 22pt
- **Body Text**: 22pt

### Color Scheme
- **Header Background**: #92CDDC (Light Blue)
- **Section Headers**: #FBD4B4 (Light Orange)
- **Text Color**: #000000 (Black)
- **Accent Color**: #13AC2B (Green)

### Table Structure
- **Grid Layout**: Fixed column widths
- **Cell Padding**: 108 twips
- **Border Style**: TableGrid
- **Merge Cells**: Vertical and horizontal merging

## 🔄 Dynamic Content

### Table Cloning
Template menggunakan fitur `cloneRow` untuk tabel pengalaman kerja:

```php
$datapengalaman = array(
    'no' => $no,
    'negara' => $negara,
    'posisi' => $posisi,
    'mulaiend' => $tahun,
    'masakerja' => $masa_kerja,
    'masabulan' => $masabulan,
    'jenisusaha' => $nama_jenis_usaha,
    'namaperusahaan' => $nama_perusahaan,
    'satuan' => $satuan,
    'gaji' => $gaji,
    'pergaji' => $detail_gaji,
    'jenispekerjaan' => $penjelasan,
    'alasanberhenti' => $alasan,
    'kosong' => $kosong
);

$document->cloneRow('cln', $datapengalaman);
```

### Image Processing
```php
// Foto profil
$phpWord->setImageValue('gambar', array(
    'path' => $datagambar, 
    'width' => 238, 
    'height' => 302
));

// Foto ARC
$phpWord->setImageValue('arc'.$keya, array(
    'path' => $datagambar, 
    'width' => 500, 
    'height' => 302
));
```

## 🛠️ Implementation Guide

### 1. Template Selection
```php
if ($data_majikan_agen == '64' || $kode_cong == 'ok') {
    $document = $PHPWord->loadTemplate('files/biodatacongyi.docx');
} else {
    $document = $PHPWord->loadTemplate('files/biodatabaru.docx');
}
```

### 2. Data Population
```php
// Set basic values
$document->setValue('{nama}', htmlspecialchars($personal->nama));
$document->setValue('{namamandarin}', htmlspecialchars($personal->nama_mandarin));
$document->setValue('{jeniskelamin}', htmlspecialchars($personal->jeniskelamin));

// Set calculated values
$document->setValue('{umur}', $umur_sekarang);
$document->setValue('{kodeid}', $personal->id_biodata.$review1.$review2.$review3);
```

### 3. Dynamic Table Population
```php
// Prepare data arrays
$negara = $this->modalku->buatarray('*', 'negara', 'working', $key, 'id_biodata', 'ORDER BY tahun ASC');
$posisi = $this->modalku->buatarray('*', 'posisi', 'working', $key, 'id_biodata', 'ORDER BY tahun ASC');

// Clone table rows
$document->cloneRow('cln', $datapengalaman);
```

### 4. Image Handling
```php
// Handle profile photo
if($data_foto == null) {
    $datagambar = "assets/uploads/profile.jpg";
} else {
    $datagambar = "assets/uploads/".$data_foto;
}

// Convert WebP to JPEG if needed
if (pathinfo($datagambar, PATHINFO_EXTENSION) === 'webp') {
    $image = new Imagick($datagambar);
    $image->setImageFormat('jpeg');
    $newPath = "assets/uploads/".pathinfo($personal->foto, PATHINFO_FILENAME).".jpg";
    $image->writeImage($newPath);
    $datagambar = $newPath;
}
```

### 5. File Generation
```php
// Save template
$tmp_file = 'biodata_cong_yi/gugusdata.docx';
$document->save($tmp_file);

// Generate final file
$filename = 'biodata_cong_yi/filenya.docx';
$phpWord->saveAs($filename);

// Download file
header('Content-Disposition: attachment; filename= '.$key.' '.htmlspecialchars($personal->nama).'.docx');
header('Content-Type: application/vnd.openxmlformats-officedocument.wordprocessingml.document');
readfile($filename);
```

## ✅ Best Practices

### 1. Data Validation
- Validasi semua data sebelum populate template
- Handle null/empty values dengan default values
- Sanitize input data dengan `htmlspecialchars()`

### 2. Image Optimization
- Resize images sesuai template requirements
- Convert WebP to JPEG untuk kompatibilitas
- Maintain aspect ratio untuk foto profil

### 3. Error Handling
- Check file existence sebelum processing
- Handle database connection errors
- Validate template file availability

### 4. Performance
- Use prepared statements untuk database queries
- Optimize image processing
- Clean up temporary files

### 5. Security
- Validate user permissions
- Sanitize all user inputs
- Use secure file paths

## 🔧 Troubleshooting

### Common Issues

#### 1. Template Not Found
```
Error: Template file not found
Solution: Check file path 'files/biodatacongyi.docx'
```

#### 2. Image Not Displaying
```
Error: Image placeholder not replaced
Solution: Check image path and format compatibility
```

#### 3. Dynamic Table Not Working
```
Error: Table rows not cloned properly
Solution: Verify data array structure matches template
```

#### 4. Chinese Characters Not Displaying
```
Error: Chinese text appears as boxes
Solution: Ensure proper font encoding (UTF-8)
```

#### 5. File Generation Failed
```
Error: Cannot create output file
Solution: Check directory permissions and disk space
```

### Debug Checklist
- [ ] Template file exists and accessible
- [ ] All required data available in database
- [ ] Image files exist and readable
- [ ] Directory permissions correct
- [ ] PHP memory limit sufficient
- [ ] Font files available for Chinese text

### Performance Optimization
- [ ] Use image caching
- [ ] Optimize database queries
- [ ] Implement file compression
- [ ] Use background processing for large files

## 📚 References

### Related Files
- `flamboyan-app/application/modules/biodata_cong_yi/controllers/biodata_cong_yi.php`
- `files/biodatacongyi.docx`
- `PHPWord/PHPWord.php`

### Documentation
- [PHPWord Documentation](https://github.com/PHPOffice/PHPWord)
- [Word OpenXML Specification](https://docs.microsoft.com/en-us/office/open-xml/open-xml-sdk)
- [Taiwan TKI Standards](https://www.bola.gov.tw/)

### Support
- Template Issues: Check file integrity and encoding
- Data Issues: Verify database structure and content
- Performance Issues: Monitor memory usage and optimize queries

---

**Versi Dokumentasi:** 1.0.0  
**Terakhir Update:** 2024-01-15  
**Status:** Production Ready ✅  
**Template Version:** Cong Yi v2.0
