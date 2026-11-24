# Panduan Praktikum 11 PBD: Export Schema (No. 4) & Restore Database (No. 5)

Panduan langkah demi langkah untuk mengerjakan **Nomor 4 (Export Schema)** dan **Nomor 5 (Restore Database)** dengan `pg_dump` dan `pg_restore` lewat **Command Prompt (CMD)** di Windows. Disusun supaya bisa diikuti siapa saja di kelompok, apa pun versi PostgreSQL dan lokasi instalasinya.

> Disusun oleh Irawan Prasetyo (L0125016). Contoh di panduan ini memakai database `ojek_online`, PostgreSQL versi 18 di drive `D:`, dan folder backup `D:\Backup`. **Semua itu bisa berbeda di laptopmu, jadi sesuaikan dulu lewat tabel di bawah.**

---

## 0. Sesuaikan dulu dengan laptopmu

Ganti nilai contoh dengan nilaimu sendiri di setiap perintah.

| Yang perlu disesuaikan | Contoh di panduan ini | Cara mengeceknya |
| :--- | :--- | :--- |
| Folder `bin` PostgreSQL | `D:\Program Files\PostgreSQL\18\bin` | Lihat bagian **2** |
| Nama database studi kasus | `ojek_online` | `psql -U postgres -h localhost -l` |
| User PostgreSQL | `postgres` | Biasanya `postgres` |
| Port | `5432` | Default `5432` |
| Folder penyimpanan backup | `D:\Backup` | Bebas, atau ikuti yang ditentukan di soal |
| Nama file schema | `PBD11_schema_ojek_online.sql` | Ikuti penamaan dari soal atau dosen |
| Nama file full backup (hasil No. 3) | `ojek_online_full.backup` | Ikuti file dari anggota yang mengerjakan No. 3 |
| Nama database tujuan restore | `ojek_online_restore` | Bebas, **harus beda** dari database asal |

---

## 1. Persiapan awal

### 1.1 Pastikan service PostgreSQL berjalan
1. Tekan tombol **Windows**, ketik `services.msc`, lalu **Enter**.
2. Cari `postgresql-x64-<versi>`. Kalau statusnya belum *Running*, klik kanan lalu pilih **Start**.

### 1.2 Buka CMD
Pakai **Command Prompt (CMD)**, jangan PowerShell, karena soal meminta CMD dan sintaks perintahnya sedikit berbeda.

> Setiap perintah dijalankan **satu per satu**: paste satu baris, tekan **Enter**, tunggu selesai, baru lanjut. Kalau beberapa baris di-paste sekaligus, Enter-nya bisa tidak terkirim dan perintahnya jadi menyambung.

---

## 2. Cari lokasi folder `bin` PostgreSQL

Perintah `pg_dump` dan `pg_restore` ada di folder `bin` milik PostgreSQL. Di banyak laptop folder ini belum masuk PATH, jadi muncul error:

```
'pg_dump' is not recognized as an internal or external command
```

Berikut cara menemukannya, urut dari yang paling cepat. Berhenti di cara yang berhasil.

### Cara A: cek apakah sudah ada di PATH
```cmd
where pg_dump
```
Kalau muncul path, berarti sudah terdaftar dan kamu bisa langsung ke bagian 3 tanpa `cd`. Kalau muncul `Could not find files`, lanjut ke cara berikutnya.

### Cara B: tebak lokasi umum (cek drive C dan D)
```cmd
dir "C:\Program Files\PostgreSQL"
dir "D:\Program Files\PostgreSQL"
```
Kalau ada folder angka (misalnya `16`, `17`, `18`), itu versi PostgreSQL yang terpasang. Folder `bin` ada di dalamnya, misalnya `D:\Program Files\PostgreSQL\18\bin`.

> Contoh kasus nyata: tebakan `C:\Program Files\PostgreSQL\16\bin` gagal dengan *The system cannot find the path specified*. Setelah dicek, ternyata PostgreSQL terpasang di **drive D versi 18**. Jadi jangan menebak versi dan drive; **cek dulu**.

### Cara C: lewat service Windows (paling akurat)
```cmd
sc query state= all | findstr /i postgres
```
Catat nama service-nya (misalnya `postgresql-x64-18`), lalu:
```cmd
sc qc postgresql-x64-18
```
Lihat baris `BINARY_PATH_NAME`. Lokasi `pg_ctl.exe` di situ berada di folder `bin` yang kita cari.

### Cara D: tanya PostgreSQL sendiri (lewat pgAdmin)
Buka **pgAdmin**, klik kanan database lalu **Query Tool**, kemudian jalankan:
```sql
SHOW data_directory;
```
Hasilnya misalnya `D:/Program Files/PostgreSQL/18/data`. Folder `bin` sejajar dengan folder `data`, jadi path-nya `D:\Program Files\PostgreSQL\18\bin`.

### Cara E: cari file langsung (paling lama, pakai kalau cara lain gagal)
```cmd
dir /s /b C:\pg_dump.exe
dir /s /b D:\pg_dump.exe
```

### Setelah path ketemu: masuk ke folder `bin`
Pakai `/d` supaya CMD ikut pindah drive.
```cmd
cd /d "D:\Program Files\PostgreSQL\18\bin"
```

Pastikan tool-nya terbaca:
```cmd
pg_dump --version
```
Kalau muncul versinya, misalnya `pg_dump (PostgreSQL) 18.6`, persiapan berhasil.

**Alternatif:** daripada `cd` setiap kali buka CMD baru, tambahkan folder `bin` ke PATH untuk sesi CMD ini saja:
```cmd
set PATH=%PATH%;D:\Program Files\PostgreSQL\18\bin
```
Setelah itu `pg_dump` dan `pg_restore` bisa dipanggil dari folder mana pun. Pengaturan ini hilang saat CMD ditutup.

---

## 3. Cek koneksi dan nama database

```cmd
psql -U postgres -h localhost -p 5432 -l
```
Masukkan password user `postgres` saat diminta. Perhatikan: **layar tidak menampilkan apa pun saat kamu mengetik password** (tidak ada bintang), itu normal.

Pastikan database studi kasusmu ada di daftar. Kalau namanya beda dari `ojek_online`, pakai nama yang sebenarnya di semua perintah berikutnya.

Kalau muncul `password authentication failed`, lihat **bagian 9 (lupa password)**.

---

## 4. Siapkan folder backup

`pg_dump` tidak membuat folder otomatis. Kalau foldernya belum ada, akan muncul error `could not open output file ... No such file or directory`.
```cmd
mkdir D:\Backup
```
Kalau laptopmu tidak punya drive `D:`, ganti semua `D:\Backup` jadi `C:\Backup`.

---

## 5. Nomor 4: Export Schema (tanpa data)

Mengekspor **struktur database saja** (tabel, kolom, constraint, trigger, view, function) ke file SQL teks biasa.

### 5.1 Jalankan export
```cmd
pg_dump -U postgres -h localhost -p 5432 -d ojek_online -s -F p -f "D:\Backup\PBD11_schema_ojek_online.sql"
```
Masukkan password saat diminta. Kalau sukses, **tidak ada output** dan kamu langsung kembali ke prompt. Itu normal.

| Opsi | Arti |
| :--- | :--- |
| `-U postgres` | User PostgreSQL yang dipakai |
| `-h localhost -p 5432` | Alamat server dan port |
| `-d ojek_online` | Database yang diekspor |
| `-s` | *schema-only*: hanya struktur, **tanpa isi data** |
| `-F p` | Format *plain*: hasilnya file SQL teks biasa |
| `-f "..."` | Lokasi dan nama file hasil |

### 5.2 Verifikasi hasil
```cmd
dir "D:\Backup\PBD11_schema_ojek_online.sql"
```
Memastikan file terbuat dan ukurannya lebih dari 0 byte.

```cmd
findstr /C:"CREATE TABLE" "D:\Backup\PBD11_schema_ojek_online.sql"
```
Menampilkan daftar `CREATE TABLE`, bukti struktur berhasil diekspor.

```cmd
findstr /C:"COPY" "D:\Backup\PBD11_schema_ojek_online.sql"
```
Hasilnya **harus kosong**. Dump yang berisi data pasti punya blok `COPY ... FROM stdin`, jadi kosong berarti tidak ada data tabel di file.

Terakhir, buka file `D:\Backup\PBD11_schema_ojek_online.sql` di **Notepad** dan screenshot bagian `CREATE TABLE` dan constraint-nya.

---

## 6. Nomor 5: Restore Database

Memulihkan struktur **dan** data dari file *full backup* (format custom, hasil Nomor 3) ke database baru.

### 6.0 Siapkan file full backup
Pakai file `.backup` dari anggota yang mengerjakan Nomor 3. Kalau belum ada, buat dulu untuk uji coba (ini perintah yang sama dengan Nomor 3):
```cmd
pg_dump -U postgres -h localhost -p 5432 -d ojek_online -F c -f "D:\Backup\ojek_online_full.backup"
```

### 6.1 Buat database tujuan baru
Namanya **harus berbeda** dari database asal.
```cmd
psql -U postgres -h localhost -p 5432 -c "CREATE DATABASE ojek_online_restore;"
```
Output yang diharapkan: `CREATE DATABASE`

### 6.2 Jalankan restore
```cmd
pg_restore -U postgres -h localhost -p 5432 -d ojek_online_restore -v "D:\Backup\ojek_online_full.backup"
```
`-v` (*verbose*) menampilkan prosesnya di layar: `creating TABLE ...`, `processing data for table ...`, `creating CONSTRAINT ...`, `creating FK CONSTRAINT ...`.

**Restore berhasil kalau:**
- Tidak ada baris yang diawali `pg_restore: error:`.
- Tidak ada pesan akhir `pg_restore: warning: errors ignored on restore: N`.

### 6.3 Verifikasi struktur dan data (langsung dari CMD)

Semua perintah di bawah dijalankan **langsung di CMD**, tanpa perlu masuk ke `psql` dulu. Tiap perintah akan meminta password lagi, itu normal. Jalankan **satu per satu**.

**a. Cek daftar tabel di database hasil restore**
```cmd
psql -U postgres -h localhost -p 5432 -d ojek_online_restore -c "\dt"
```

**b. Hitung jumlah baris tiap tabel di database hasil restore**
```cmd
psql -U postgres -h localhost -p 5432 -d ojek_online_restore -c "ANALYZE;" -c "SELECT relname AS tabel, n_live_tup AS jumlah_baris FROM pg_stat_user_tables ORDER BY relname;"
```
Hasilnya daftar semua tabel beserta jumlah barisnya.

**c. Hitung dengan cara yang sama di database asal**
```cmd
psql -U postgres -h localhost -p 5432 -d ojek_online -c "ANALYZE;" -c "SELECT relname AS tabel, n_live_tup AS jumlah_baris FROM pg_stat_user_tables ORDER BY relname;"
```
Bandingkan hasil (b) dan (c). Kalau jumlah baris semua tabel **sama**, restore berhasil.

**d. Lihat contoh isi tabel** (ganti `pesanan` dengan salah satu tabel milik studi kasusmu)
```cmd
psql -U postgres -h localhost -p 5432 -d ojek_online_restore -c "SELECT * FROM pesanan LIMIT 5;"
```

**Penjelasan singkat query hitung baris:**
- `ANALYZE;` memperbarui statistik tabel. Database yang baru saja di-restore belum punya statistik, jadi tanpa langkah ini angkanya bisa tampil `0`.
- `pg_stat_user_tables` adalah tabel bawaan PostgreSQL yang berisi satu baris per tabel pengguna.
- `relname` adalah nama tabel dan `n_live_tup` adalah perkiraan jumlah baris yang masih hidup. Untuk tabel berukuran kecil seperti di tugas ini, angkanya sama dengan jumlah baris sebenarnya.

**Mau bukti yang pasti untuk satu tabel?** Pakai `COUNT(*)`, lalu jalankan di kedua database:
```cmd
psql -U postgres -h localhost -p 5432 -d ojek_online_restore -c "SELECT COUNT(*) FROM pesanan;"
psql -U postgres -h localhost -p 5432 -d ojek_online -c "SELECT COUNT(*) FROM pesanan;"
```

**Opsional: perbandingan otomatis dengan `fc`.** Simpan hasil kedua database ke file, lalu bandingkan:
```cmd
psql -U postgres -h localhost -p 5432 -d ojek_online -c "ANALYZE;" -c "SELECT relname AS tabel, n_live_tup AS jumlah_baris FROM pg_stat_user_tables ORDER BY relname;" > D:\Backup\baris_asal.txt
psql -U postgres -h localhost -p 5432 -d ojek_online_restore -c "ANALYZE;" -c "SELECT relname AS tabel, n_live_tup AS jumlah_baris FROM pg_stat_user_tables ORDER BY relname;" > D:\Backup\baris_restore.txt
fc D:\Backup\baris_asal.txt D:\Backup\baris_restore.txt
```
Kalau hasilnya `FC: no differences encountered`, isi kedua database sama persis.

> **Alternatif interaktif:** kamu juga bisa masuk dulu dengan `psql -U postgres -h localhost -p 5432 -d ojek_online_restore`, lalu mengetik query SQL-nya (tanpa bagian `-c "..."`) di prompt `ojek_online_restore=#`. Keluar dengan `\q`.

---

## 7. Checklist screenshot untuk laporan PDF

### Nomor 4 (Export Schema)
- [ ] Perintah `pg_dump -s -F p ...` yang berjalan tanpa error
- [ ] Output `dir` yang menunjukkan file `.sql` sudah terbuat
- [ ] Output `findstr /C:"CREATE TABLE"` yang menampilkan daftar tabel
- [ ] Output `findstr /C:"COPY"` yang kosong (bukti tanpa data)
- [ ] Isi file `.sql` saat dibuka di Notepad

### Nomor 5 (Restore Database)
- [ ] Perintah dan output `CREATE DATABASE ojek_online_restore;`
- [ ] Perintah `pg_restore` beserta log `-v` yang berhasil
- [ ] Output `\dt` di database hasil restore
- [ ] Hasil hitung jumlah baris di database asal **dan** hasil restore (atau hasil `fc`)
- [ ] Output `SELECT * FROM ... LIMIT 5;` sebagai bukti data pulih

> **Tips screenshot:** jangan sampai password terlihat. Password yang diketik memang tidak tampil di layar, jadi aman selama kamu tidak memakai `set PGPASSWORD=...` di layar yang di-screenshot.

---

## 8. Snippet untuk digabung ke file SQL kelompok (`PBD11_KelompokX.sql`)

Ganti `[Nama Lengkap]`, `[NIM]`, nama database, dan path sesuai milikmu. Tulis namamu di komentar sesuai ketentuan tugas.

```sql
-- ============================================================
-- NOMOR 4: Export Schema Database
-- Dikerjakan oleh: [Nama Lengkap] ([NIM])
-- ============================================================
-- Perintah CMD:
-- pg_dump -U postgres -h localhost -p 5432 -d ojek_online -s -F p -f "D:\Backup\PBD11_schema_ojek_online.sql"
--
-- Verifikasi (CMD):
-- dir "D:\Backup\PBD11_schema_ojek_online.sql"
-- findstr /C:"CREATE TABLE" "D:\Backup\PBD11_schema_ojek_online.sql"
-- findstr /C:"COPY" "D:\Backup\PBD11_schema_ojek_online.sql"  -- hasil kosong = tanpa data

-- ============================================================
-- NOMOR 5: Restore Database
-- Dikerjakan oleh: [Nama Lengkap] ([NIM])
-- ============================================================
CREATE DATABASE ojek_online_restore;

-- Perintah CMD:
-- pg_restore -U postgres -h localhost -p 5432 -d ojek_online_restore -v "D:\Backup\ojek_online_full.backup"

-- Verifikasi struktur dan jumlah baris (jalankan di ojek_online_restore dan ojek_online):
ANALYZE;
SELECT relname AS tabel, n_live_tup AS jumlah_baris
FROM pg_stat_user_tables
ORDER BY relname;

-- Verifikasi sampel isi data:
SELECT * FROM pesanan LIMIT 5;
```

---

## 9. Troubleshooting

| Error / kendala | Penyebab | Solusi |
| :--- | :--- | :--- |
| `'pg_dump' is not recognized` | Folder `bin` belum masuk PATH | Ikuti **bagian 2**, lalu `cd /d "<path bin>"` atau `set PATH=...` |
| `The system cannot find the path specified` saat `cd` | Versi atau drive PostgreSQL salah tebak | Cek dengan `dir "C:\Program Files\PostgreSQL"` dan `dir "D:\Program Files\PostgreSQL"` |
| `cd` tidak pindah drive | Lupa `/d` | Pakai `cd /d "D:\..."` |
| `could not open output file ... No such file or directory` | Folder tujuan belum dibuat | `mkdir D:\Backup` |
| `password authentication failed for user "postgres"` | Password salah atau lupa | Lihat **bagian 10** |
| `connection ... failed: Connection refused` | Service PostgreSQL mati | Buka `services.msc` lalu **Start** service `postgresql-x64-<versi>` |
| `database "ojek_online" does not exist` | Nama database beda | Cek dengan `psql -U postgres -h localhost -l` |
| `database "ojek_online_restore" already exists` | Database tujuan sudah pernah dibuat | Pakai nama lain atau `DROP DATABASE ojek_online_restore;` (hati-hati, ini menghapus database itu) |
| `role "..." does not exist` / error owner saat `pg_restore` | Role asal backup tidak ada di laptopmu | Tambah `--no-owner --no-privileges` pada perintah `pg_restore` |
| `unsupported version (1.xx) in file header` | `pg_restore` lebih lama daripada `pg_dump` pembuat backup | Pakai `pg_restore` dengan versi sama atau lebih baru |
| Perintah menyambung jadi satu baris | Beberapa baris di-paste sekaligus | Jalankan satu per satu |

---

## 10. Lupa password `postgres` (reset di PostgreSQL lokal)

Lakukan **hanya** di PostgreSQL milik laptopmu sendiri. Intinya: matikan pengecekan password sementara, ganti password, lalu nyalakan lagi.

> **Penting:** jangan lupa langkah 6. Selama `pg_hba.conf` masih `trust`, siapa pun yang bisa konek ke komputer ini bisa masuk tanpa password.

1. **Buka CMD sebagai Administrator** (klik kanan CMD lalu *Run as administrator*).

2. **Cari nama service** dan lokasi folder `data` (lihat cara A–D di bagian 2):
   ```cmd
   sc query state= all | findstr /i postgres
   ```

3. **Backup lalu edit `pg_hba.conf`** (sesuaikan path):
   ```cmd
   copy "D:\Program Files\PostgreSQL\18\data\pg_hba.conf" "D:\Program Files\PostgreSQL\18\data\pg_hba.conf.bak"
   notepad "D:\Program Files\PostgreSQL\18\data\pg_hba.conf"
   ```
   Di bagian bawah file, ubah `scram-sha-256` menjadi `trust` pada dua baris ini, lalu simpan:
   ```
   host    all    all    127.0.0.1/32    trust
   host    all    all    ::1/128         trust
   ```
   Pastikan file tersimpan sebagai `pg_hba.conf`, bukan `pg_hba.conf.txt`.

4. **Restart service** (sesuaikan nama service):
   ```cmd
   net stop postgresql-x64-18
   net start postgresql-x64-18
   ```

5. **Ganti password.** Kalau `trust` aktif, kamu masuk **tanpa ditanya password**:
   ```cmd
   cd /d "D:\Program Files\PostgreSQL\18\bin"
   psql -U postgres -h localhost -c "ALTER USER postgres WITH PASSWORD 'ISI_PASSWORD_BARU';"
   ```
   Ganti `ISI_PASSWORD_BARU` dengan password pilihanmu dan **catat**. Output sukses: `ALTER ROLE`.
   Kalau perintah ini masih meminta password, berarti `trust` belum aktif. Periksa lagi langkah 3 dan 4.

6. **Kembalikan `pg_hba.conf`**: ubah dua baris tadi kembali ke `scram-sha-256`, simpan, lalu restart service lagi (perintah `net stop` dan `net start` seperti langkah 4).

7. **Tes password baru:**
   ```cmd
   psql -U postgres -h localhost -l
   ```
   Kalau daftar database muncul, berhasil.

**Catatan:**
- `net stop` yang gagal dengan *Access is denied* berarti CMD belum dibuka sebagai Administrator.
- Kalau pgAdmin sudah menyimpan koneksi lama, perbarui password tersimpannya lewat *Properties* di server tersebut.
