# Praktikum 11 PBD - Export Schema (No. 4) & Restore Database (No. 5)

* **Database Studi Kasus:** `ojek_online`
* **Dikerjakan oleh:** Irawan Prasetyo (L0125016)
* **Catatan:** Sesuaikan path folder backup (`D:\Backup\...`) dengan lokasi penyimpanan di laptop masing-masing.

---

## Nomor 4: Export Schema Database

Mengekspor **hanya struktur database** (tabel, kolom, constraint, index, dll.) tanpa menyertakan isi data menggunakan opsi `-s` (*schema-only*) dan `-F p` (*plain SQL*).

### 1. Jalankan Export Schema (CMD)
```cmd
pg_dump -U postgres -h localhost -p 5432 -d ojek_online -s -F p -f "D:\Backup\PBD11_schema_ojek_online.sql"
```

### 2. Verifikasi File Hasil Export (CMD)
```cmd
:: Cek keberadaan dan ukuran file
dir "D:\Backup\PBD11_schema_ojek_online.sql"

:: Pastikan struktur tabel berhasil diekspor
findstr /C:"CREATE TABLE" "D:\Backup\PBD11_schema_ojek_online.sql"

:: Pastikan tidak ada isi data tabel (output harus kosong)
findstr /C:"COPY" "D:\Backup\PBD11_schema_ojek_online.sql"
```

> **Cek Manual:** Buka file `PBD11_schema_ojek_online.sql` menggunakan Notepad untuk memastikan bagian `CREATE TABLE` dan *constraint*-nya tertulis lengkap tanpa blok data.

---

## Nomor 5: Restore Database

Memulihkan database ke database baru menggunakan file *full backup* (format custom `.backup`) yang dihasilkan pada **Nomor 3**.

> *(Opsional — Jika file backup dari Nomor 3 belum ada, buat dulu untuk uji coba lewat CMD):*
> ```cmd
> pg_dump -U postgres -h localhost -p 5432 -d ojek_online -F c -f "D:\Backup\ojek_online_full.backup"
> ```

### 5a. Buat Database Tujuan Restore
Buat database baru dengan nama yang berbeda dari database asal.
**Penting:** Jalankan perintah ini **sendirian** (satu *statement*) sambil terkoneksi ke database `postgres` (bisa lewat HeidiSQL, pgAdmin, atau `psql`):

```sql
CREATE DATABASE ojek_online_restore;
```

### 5b. Restore Database dengan `pg_restore` (CMD)
```cmd
pg_restore -U postgres -h localhost -p 5432 -d ojek_online_restore -v "D:\Backup\ojek_online_full.backup"
```
> *Catatan: Jika muncul error terkait role atau owner, tambahkan opsi `--no-owner --no-privileges` sebelum path file backup.*

### 5c. Verifikasi Struktur dan Data
Jalankan query di bawah ini sambil terkoneksi ke database **`ojek_online_restore`**, lalu jalankan ulang di database asal **`ojek_online`** untuk membandingkan hasilnya.

**1. Cek daftar tabel yang berhasil dipulihkan:**
```sql
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public' AND table_type = 'BASE TABLE'
ORDER BY table_name;
```

**2. Cek jumlah baris tiap tabel (`ANALYZE` dijalankan dulu supaya statistik akurat):**
```sql
ANALYZE;

SELECT relname AS tabel, n_live_tup AS jumlah_baris
FROM pg_stat_user_tables
ORDER BY relname;
```

**3. Cek contoh isi data pada salah satu tabel:**
```sql
SELECT * FROM pesanan LIMIT 5;
```

Jika daftar tabel, jumlah baris tiap tabel, dan sampel datanya sama persis antara `ojek_online` dan `ojek_online_restore`, maka proses restore dinyatakan berhasil.

---

## Checklist Screenshot untuk Laporan PDF

**Nomor 4:**
- [ ] Perintah `pg_dump` (export schema) yang sukses di CMD
- [ ] Output `dir` yang menunjukkan file `.sql` berhasil dibuat
- [ ] Output `findstr /C:"CREATE TABLE"`
- [ ] Output `findstr /C:"COPY"` yang kosong (bukti tanpa data)
- [ ] Tampilan isi file `PBD11_schema_ojek_online.sql` di Notepad

**Nomor 5:**
- [ ] Hasil eksekusi `CREATE DATABASE ojek_online_restore;`
- [ ] Perintah dan log proses `pg_restore` di CMD
- [ ] Hasil query daftar tabel di `ojek_online_restore`
- [ ] Perbandingan jumlah baris tiap tabel (`ojek_online` vs `ojek_online_restore`)
- [ ] Hasil query `SELECT * FROM pesanan LIMIT 5;`

---

## Skrip untuk File SQL Kelompok (`PBD11_KelompokX.sql`)

Salin bagian di bawah ini ke dalam file SQL gabungan kelompok:

```sql
-- ============================================================
-- PRAKTIKUM 11 PBD - DCL, BACKUP AND RESTORE
-- Bagian: Nomor 4 (Export Schema) dan Nomor 5 (Restore Database)
-- Database studi kasus: ojek_online
-- ============================================================


-- ============================================================
-- NOMOR 4: Export Schema Database
-- Dikerjakan oleh: Irawan Prasetyo (L0125016)
-- ============================================================

-- Perintah CMD (schema-only, format plain):
-- pg_dump -U postgres -h localhost -p 5432 -d ojek_online -s -F p -f "D:\Backup\PBD11_schema_ojek_online.sql"

-- Verifikasi file hasil export (CMD):
-- dir "D:\Backup\PBD11_schema_ojek_online.sql"
-- findstr /C:"CREATE TABLE" "D:\Backup\PBD11_schema_ojek_online.sql"
-- findstr /C:"COPY" "D:\Backup\PBD11_schema_ojek_online.sql"   -- hasil kosong = tidak ada data tabel


-- ============================================================
-- NOMOR 5: Restore Database
-- Dikerjakan oleh: Irawan Prasetyo (L0125016)
-- ============================================================

-- 5a. Buat database tujuan restore (namanya harus berbeda dari database asal).
--     Jalankan SENDIRIAN (satu statement) sambil terkoneksi ke database "postgres".
CREATE DATABASE ojek_online_restore;

-- 5b. Restore full backup (hasil Nomor 3) dengan pg_restore (CMD):
-- pg_restore -U postgres -h localhost -p 5432 -d ojek_online_restore -v "D:\Backup\ojek_online_full.backup"

-- 5c. Verifikasi hasil restore.
--     Jalankan query di bawah sambil terkoneksi ke database "ojek_online_restore",
--     lalu jalankan ulang di database asal "ojek_online" untuk dibandingkan.

-- Daftar tabel yang berhasil dipulihkan
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public' AND table_type = 'BASE TABLE'
ORDER BY table_name;

-- Jumlah baris tiap tabel (ANALYZE dulu supaya statistik akurat)
ANALYZE;

SELECT relname AS tabel, n_live_tup AS jumlah_baris
FROM pg_stat_user_tables
ORDER BY relname;

-- Contoh isi data salah satu tabel
SELECT * FROM pesanan LIMIT 5;
```
