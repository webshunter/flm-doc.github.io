# Template Documentation Summary

## 📋 Daftar Template yang Didokumentasikan

### 1. **Cong Yi Template** (`congyi_template_documentation.md`)
- **File Template**: `files/biodatacongyi.docx`
- **Controller**: `flamboyan-app/application/modules/biodata_cong_yi/controllers/biodata_cong_yi.php`
- **Fungsi**: `print_bio($key="", $kode_cong="")`

#### Fitur Utama:
- ✅ **Bilingual Format** - Indonesia & Mandarin
- ✅ **100+ Placeholders** - Data lengkap TKI
- ✅ **Dynamic Tables** - Pengalaman kerja dinamis
- ✅ **Multiple Images** - Foto profil & ARC
- ✅ **Professional Layout** - Standar Taiwan

#### Konten Dokumentasi:
- **Placeholder Reference** - 100+ placeholder dengan deskripsi dan contoh
- **Layout & Styling** - Font, warna, dan struktur tabel
- **Dynamic Content** - Table cloning dan image processing
- **Implementation Guide** - Step-by-step implementasi
- **Best Practices** - Optimasi dan keamanan
- **Troubleshooting** - Masalah umum dan solusi

#### Placeholder Categories:
1. **Data Personal** - Nama, tanggal lahir, alamat, dll
2. **Kemampuan Bahasa** - Mandarin dan Inggris
3. **Data Vaksinasi** - 3 dosis vaksin COVID-19
4. **Data Keluarga** - Orang tua, pasangan, anak
5. **Pengalaman Kerja** - Tabel dinamis dengan cloneRow
6. **Keterampilan & Kondisi** - Hobi, kesehatan, kebiasaan
7. **Data Paspor & Medis** - Status dokumen dan kesehatan
8. **Data Kontak** - HP dan alamat keluarga
9. **Data PPTK** - 8 pernyataan kesediaan
10. **Data Kerja** - Informasi pekerjaan dan majikan
11. **Image Placeholders** - Foto profil dan ARC

## 🎯 Penggunaan Template

### Trigger Template Selection:
```php
if ($data_majikan_agen == '64' || $kode_cong == 'ok') {
    $document = $PHPWord->loadTemplate('files/biodatacongyi.docx');
}
```

### Parameter Kunci:
- **`$key`** - ID biodata TKI
- **`$kode_cong`** - Parameter "ok" untuk template Cong Yi

### Output:
- **Format**: Microsoft Word (.docx)
- **Nama File**: `{id_biodata} {nama_tki}.docx`
- **Konten**: Biodata lengkap dalam format Taiwan

## 📊 Statistik Template

| Metric | Value |
|--------|-------|
| Total Placeholders | 100+ |
| Dynamic Tables | 1 (Pengalaman Kerja) |
| Image Placeholders | 2 (Profil + ARC) |
| Language Support | 2 (ID + Mandarin) |
| File Size | ~15KB |
| Complexity | High |

## 🔗 Referensi Terkait

### File Template:
- `files/biodatacongyi.docx` - Template utama
- `files/biodatabaru.docx` - Template alternatif

### Controller & Logic:
- `flamboyan-app/application/modules/biodata_cong_yi/controllers/biodata_cong_yi.php`
- Fungsi `print_bio()` - Main generation function

### Dependencies:
- `PHPWord/PHPWord.php` - Word document library
- `Imagick` - Image processing (WebP to JPEG)

### Database Tables:
- `biodata` - Data personal TKI
- `working` - Pengalaman kerja
- `family` - Data keluarga
- `vaccine` - Data vaksinasi
- `medical` - Data medis

## 🚀 Quick Reference

### Generate Document:
```php
// URL: /biodata_cong_yi/print_bio/{id_biodata}/ok
// Method: GET
// Output: Download .docx file
```

### Key Placeholders:
```php
{nama}           // Nama lengkap
{namamandarin}   // Nama Mandarin
{gambar}         // Foto profil
{cln.negara}     // Negara kerja (dynamic)
{pernyataan1-8}  // Pernyataan PPTK
```

### Image Requirements:
- **Foto Profil**: 238x302 px, JPEG/PNG
- **Foto ARC**: 500x302 px, JPEG/PNG
- **Format**: Auto-convert WebP to JPEG

## 📝 Notes

### Template Version:
- **Current**: Cong Yi v2.0
- **Last Update**: 2024-01-15
- **Status**: Production Ready ✅

### Compatibility:
- **PHP**: 7.4+
- **PHPWord**: Latest version
- **Imagick**: Required for WebP conversion
- **Encoding**: UTF-8 for Chinese characters

### Performance:
- **Memory Usage**: ~50MB per document
- **Processing Time**: ~2-5 seconds
- **File Size**: ~100-500KB output

---

**Dokumentasi Lengkap**: [congyi_template_documentation.md](congyi_template_documentation.md)  
**Status**: Complete ✅  
**Maintainer**: Development Team 