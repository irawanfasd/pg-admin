C:\Users\Asus>cd D
The system cannot find the path specified.

C:\Users\Asus>cd "D:\Program Files\PostgreSQL\18\bin"

C:\Users\Asus>cd /d "D:\Program Files\PostgreSQL\18\bin"

D:\Program Files\PostgreSQL\18\bin>pg_dump -U postgres -h localhost -p 5432 -d ojek_online -s -F p -f "D:\Backup\PBD11_schema_ojek_online.sql"
Password:

pg_dump: error: could not open output file "D:\Backup\PBD11_schema_ojek_online.sql": No such file or directory

D:\Program Files\PostgreSQL\18\bin>mkdir D:\Backup

D:\Program Files\PostgreSQL\18\bin>pg_dump -U postgres -h localhost -p 5432 -d ojek_online -s -F p -f "D:\Backup\PBD11_schema_ojek_online.sql"
Password:


D:\Program Files\PostgreSQL\18\bin>dir "D:\Backup\PBD11_schema_ojek_online.sql"
 Volume in drive D is DATA
 Volume Serial Number is 3C09-91F3

 Directory of D:\Backup

24/11/2026  08:10            53.079 PBD11_schema_ojek_online.sql
               1 File(s)         53.079 bytes
               0 Dir(s)  120.509.636.608 bytes free

D:\Program Files\PostgreSQL\18\bin>findstr /C:"CREATE TABLE" "D:\Backup\PBD11_schema_ojek_online.sql"
CREATE TABLE public.actor (
CREATE TABLE public.address (
CREATE TABLE public.anggota (
CREATE TABLE public.buku (
CREATE TABLE public.category (
CREATE TABLE public.city (
CREATE TABLE public.country (
CREATE TABLE public.customer (
CREATE TABLE public.detail_peminjaman (
CREATE TABLE public.film (
CREATE TABLE public.film_actor (
CREATE TABLE public.film_category (
CREATE TABLE public.inventory (
CREATE TABLE public.jam_operasional (
CREATE TABLE public.kategori (
CREATE TABLE public.kendaraan (
CREATE TABLE public.klaim_promo (
CREATE TABLE public.language (
CREATE TABLE public.lokasi_favorit (
CREATE TABLE public.mobil (
CREATE TABLE public.motor (
CREATE TABLE public.payment (
CREATE TABLE public.peminjaman (
CREATE TABLE public.pengemudi (
CREATE TABLE public.pengguna (
CREATE TABLE public.penumpang (
CREATE TABLE public.penumpang_no_hp (
CREATE TABLE public.pesanan (
CREATE TABLE public.pesanan_food (
CREATE TABLE public.pesanan_ride (
CREATE TABLE public.pesanan_send (
CREATE TABLE public.promo (
CREATE TABLE public.rental (
CREATE TABLE public.staff (
CREATE TABLE public.store (

D:\Program Files\PostgreSQL\18\bin>findstr /C:"COPY" "D:\Backup\PBD11_schema_ojek_online.sql"

D:\Program Files\PostgreSQL\18\bin>pg_dump -U postgres -h localhost -p 5432 -d ojek_online -F c -f "D:\Backup\ojek_online_full.backup"
Password:


D:\Program Files\PostgreSQL\18\bin>psql -U postgres -h localhost -p 5432 -c "CREATE DATABASE ojek_online_restore;"
Password for user postgres:

CREATE DATABASE

D:\Program Files\PostgreSQL\18\bin>pg_restore -U postgres -h localhost -p 5432 -d ojek_online_restore -v "D:\Backup\ojek_online_full.backup"
pg_restore: connecting to database for restore
Password:

pg_restore: executing SELECT pg_catalog.set_config('search_path', '', false);
pg_restore: creating TYPE "public.aktor_pembatal"
pg_restore: creating TYPE "public.status_pengemudi"
pg_restore: creating TYPE "public.status_pesanan"
pg_restore: creating PROCEDURE "public.add_klaim_promo(character varying, character varying)"
pg_restore: creating PROCEDURE "public.add_penumpang(character varying, character varying, character varying)"
pg_restore: creating FUNCTION "public.buatderetangka(integer)"
pg_restore: creating FUNCTION "public.caripesanan(character varying)"
pg_restore: creating FUNCTION "public.check_duplicate_pengemudi()"
pg_restore: creating FUNCTION "public.hitungtotalpesanan(character varying)"
pg_restore: creating FUNCTION "public.prevent_delete_pesanan()"
pg_restore: creating FUNCTION "public.update_performa_otomatis()"
pg_restore: creating FUNCTION "public.validate_tarif_pesanan()"
pg_restore: creating TABLE "public.actor"
pg_restore: creating SEQUENCE "public.actor_actor_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.actor_actor_id_seq"
pg_restore: creating TABLE "public.address"
pg_restore: creating SEQUENCE "public.address_address_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.address_address_id_seq"
pg_restore: creating TABLE "public.anggota"
pg_restore: creating SEQUENCE "public.anggota_id_anggota_seq"
pg_restore: creating SEQUENCE OWNED BY "public.anggota_id_anggota_seq"
pg_restore: creating TABLE "public.buku"
pg_restore: creating SEQUENCE "public.buku_id_buku_seq"
pg_restore: creating SEQUENCE OWNED BY "public.buku_id_buku_seq"
pg_restore: creating TABLE "public.category"
pg_restore: creating SEQUENCE "public.category_category_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.category_category_id_seq"
pg_restore: creating TABLE "public.city"
pg_restore: creating SEQUENCE "public.city_city_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.city_city_id_seq"
pg_restore: creating TABLE "public.country"
pg_restore: creating SEQUENCE "public.country_country_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.country_country_id_seq"
pg_restore: creating TABLE "public.customer"
pg_restore: creating SEQUENCE "public.customer_customer_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.customer_customer_id_seq"
pg_restore: creating TABLE "public.detail_peminjaman"
pg_restore: creating TABLE "public.film"
pg_restore: creating TABLE "public.film_actor"
pg_restore: creating TABLE "public.film_category"
pg_restore: creating SEQUENCE "public.film_film_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.film_film_id_seq"
pg_restore: creating TABLE "public.inventory"
pg_restore: creating SEQUENCE "public.inventory_inventory_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.inventory_inventory_id_seq"
pg_restore: creating TABLE "public.jam_operasional"
pg_restore: creating SEQUENCE "public.jam_operasional_id_jam_seq"
pg_restore: creating SEQUENCE OWNED BY "public.jam_operasional_id_jam_seq"
pg_restore: creating TABLE "public.kategori"
pg_restore: creating SEQUENCE "public.kategori_id_kategori_seq"
pg_restore: creating SEQUENCE OWNED BY "public.kategori_id_kategori_seq"
pg_restore: creating TABLE "public.kendaraan"
pg_restore: creating TABLE "public.klaim_promo"
pg_restore: creating TABLE "public.language"
pg_restore: creating SEQUENCE "public.language_language_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.language_language_id_seq"
pg_restore: creating TABLE "public.lokasi_favorit"
pg_restore: creating TABLE "public.mobil"
pg_restore: creating TABLE "public.motor"
pg_restore: creating TABLE "public.payment"
pg_restore: creating SEQUENCE "public.payment_payment_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.payment_payment_id_seq"
pg_restore: creating TABLE "public.peminjaman"
pg_restore: creating SEQUENCE "public.peminjaman_id_peminjaman_seq"
pg_restore: creating SEQUENCE OWNED BY "public.peminjaman_id_peminjaman_seq"
pg_restore: creating TABLE "public.pengemudi"
pg_restore: creating TABLE "public.pengguna"
pg_restore: creating TABLE "public.penumpang"
pg_restore: creating TABLE "public.penumpang_no_hp"
pg_restore: creating TABLE "public.pesanan"
pg_restore: creating TABLE "public.pesanan_food"
pg_restore: creating TABLE "public.pesanan_ride"
pg_restore: creating TABLE "public.pesanan_send"
pg_restore: creating TABLE "public.promo"
pg_restore: creating TABLE "public.rental"
pg_restore: creating SEQUENCE "public.rental_rental_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.rental_rental_id_seq"
pg_restore: creating TABLE "public.staff"
pg_restore: creating SEQUENCE "public.staff_staff_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.staff_staff_id_seq"
pg_restore: creating TABLE "public.store"
pg_restore: creating SEQUENCE "public.store_store_id_seq"
pg_restore: creating SEQUENCE OWNED BY "public.store_store_id_seq"
pg_restore: creating VIEW "public.view_riwayat_pesanan"
pg_restore: creating DEFAULT "public.actor actor_id"
pg_restore: creating DEFAULT "public.address address_id"
pg_restore: creating DEFAULT "public.anggota id_anggota"
pg_restore: creating DEFAULT "public.buku id_buku"
pg_restore: creating DEFAULT "public.category category_id"
pg_restore: creating DEFAULT "public.city city_id"
pg_restore: creating DEFAULT "public.country country_id"
pg_restore: creating DEFAULT "public.customer customer_id"
pg_restore: creating DEFAULT "public.film film_id"
pg_restore: creating DEFAULT "public.inventory inventory_id"
pg_restore: creating DEFAULT "public.jam_operasional id_jam"
pg_restore: creating DEFAULT "public.kategori id_kategori"
pg_restore: creating DEFAULT "public.language language_id"
pg_restore: creating DEFAULT "public.payment payment_id"
pg_restore: creating DEFAULT "public.peminjaman id_peminjaman"
pg_restore: creating DEFAULT "public.rental rental_id"
pg_restore: creating DEFAULT "public.staff staff_id"
pg_restore: creating DEFAULT "public.store store_id"
pg_restore: processing data for table "public.actor"
pg_restore: processing data for table "public.address"
pg_restore: processing data for table "public.anggota"
pg_restore: processing data for table "public.buku"
pg_restore: processing data for table "public.category"
pg_restore: processing data for table "public.city"
pg_restore: processing data for table "public.country"
pg_restore: processing data for table "public.customer"
pg_restore: processing data for table "public.detail_peminjaman"
pg_restore: processing data for table "public.film"
pg_restore: processing data for table "public.film_actor"
pg_restore: processing data for table "public.film_category"
pg_restore: processing data for table "public.inventory"
pg_restore: processing data for table "public.jam_operasional"
pg_restore: processing data for table "public.kategori"
pg_restore: processing data for table "public.kendaraan"
pg_restore: processing data for table "public.klaim_promo"
pg_restore: processing data for table "public.language"
pg_restore: processing data for table "public.lokasi_favorit"
pg_restore: processing data for table "public.mobil"
pg_restore: processing data for table "public.motor"
pg_restore: processing data for table "public.payment"
pg_restore: processing data for table "public.peminjaman"
pg_restore: processing data for table "public.pengemudi"
pg_restore: processing data for table "public.pengguna"
pg_restore: processing data for table "public.penumpang"
pg_restore: processing data for table "public.penumpang_no_hp"
pg_restore: processing data for table "public.pesanan"
pg_restore: processing data for table "public.pesanan_food"
pg_restore: processing data for table "public.pesanan_ride"
pg_restore: processing data for table "public.pesanan_send"
pg_restore: processing data for table "public.promo"
pg_restore: processing data for table "public.rental"
pg_restore: processing data for table "public.staff"
pg_restore: processing data for table "public.store"
pg_restore: executing SEQUENCE SET actor_actor_id_seq
pg_restore: executing SEQUENCE SET address_address_id_seq
pg_restore: executing SEQUENCE SET anggota_id_anggota_seq
pg_restore: executing SEQUENCE SET buku_id_buku_seq
pg_restore: executing SEQUENCE SET category_category_id_seq
pg_restore: executing SEQUENCE SET city_city_id_seq
pg_restore: executing SEQUENCE SET country_country_id_seq
pg_restore: executing SEQUENCE SET customer_customer_id_seq
pg_restore: executing SEQUENCE SET film_film_id_seq
pg_restore: executing SEQUENCE SET inventory_inventory_id_seq
pg_restore: executing SEQUENCE SET jam_operasional_id_jam_seq
pg_restore: executing SEQUENCE SET kategori_id_kategori_seq
pg_restore: executing SEQUENCE SET language_language_id_seq
pg_restore: executing SEQUENCE SET payment_payment_id_seq
pg_restore: executing SEQUENCE SET peminjaman_id_peminjaman_seq
pg_restore: executing SEQUENCE SET rental_rental_id_seq
pg_restore: executing SEQUENCE SET staff_staff_id_seq
pg_restore: executing SEQUENCE SET store_store_id_seq
pg_restore: creating CONSTRAINT "public.actor actor_pkey"
pg_restore: creating CONSTRAINT "public.address address_pkey"
pg_restore: creating CONSTRAINT "public.anggota anggota_email_key"
pg_restore: creating CONSTRAINT "public.anggota anggota_pkey"
pg_restore: creating CONSTRAINT "public.buku buku_pkey"
pg_restore: creating CONSTRAINT "public.category category_name_key"
pg_restore: creating CONSTRAINT "public.category category_pkey"
pg_restore: creating CONSTRAINT "public.city city_pkey"
pg_restore: creating CONSTRAINT "public.country country_pkey"
pg_restore: creating CONSTRAINT "public.customer customer_pkey"
pg_restore: creating CONSTRAINT "public.detail_peminjaman detail_peminjaman_pkey"
pg_restore: creating CONSTRAINT "public.film_actor film_actor_pkey"
pg_restore: creating CONSTRAINT "public.film_category film_category_pkey"
pg_restore: creating CONSTRAINT "public.film film_pkey"
pg_restore: creating CONSTRAINT "public.inventory inventory_pkey"
pg_restore: creating CONSTRAINT "public.jam_operasional jam_operasional_pkey"
pg_restore: creating CONSTRAINT "public.kategori kategori_pkey"
pg_restore: creating CONSTRAINT "public.kendaraan kendaraan_id_pengemudi_key"
pg_restore: creating CONSTRAINT "public.kendaraan kendaraan_pkey"
pg_restore: creating CONSTRAINT "public.klaim_promo klaim_promo_pkey"
pg_restore: creating CONSTRAINT "public.language language_pkey"
pg_restore: creating CONSTRAINT "public.lokasi_favorit lokasi_favorit_pkey"
pg_restore: creating CONSTRAINT "public.mobil mobil_pkey"
pg_restore: creating CONSTRAINT "public.motor motor_pkey"
pg_restore: creating CONSTRAINT "public.payment payment_pkey"
pg_restore: creating CONSTRAINT "public.peminjaman peminjaman_pkey"
pg_restore: creating CONSTRAINT "public.pengemudi pengemudi_pkey"
pg_restore: creating CONSTRAINT "public.pengguna pengguna_pkey"
pg_restore: creating CONSTRAINT "public.penumpang penumpang_email_key"
pg_restore: creating CONSTRAINT "public.penumpang_no_hp penumpang_no_hp_pkey"
pg_restore: creating CONSTRAINT "public.penumpang penumpang_pkey"
pg_restore: creating CONSTRAINT "public.pesanan_food pesanan_food_pkey"
pg_restore: creating CONSTRAINT "public.pesanan pesanan_pkey"
pg_restore: creating CONSTRAINT "public.pesanan_ride pesanan_ride_pkey"
pg_restore: creating CONSTRAINT "public.pesanan_send pesanan_send_pkey"
pg_restore: creating CONSTRAINT "public.promo promo_pkey"
pg_restore: creating CONSTRAINT "public.rental rental_pkey"
pg_restore: creating CONSTRAINT "public.staff staff_pkey"
pg_restore: creating CONSTRAINT "public.staff staff_username_key"
pg_restore: creating CONSTRAINT "public.store store_pkey"
pg_restore: creating TRIGGER "public.pengemudi trg_check_duplicate_pengemudi"
pg_restore: creating TRIGGER "public.pesanan trg_hitung_performa"
pg_restore: creating TRIGGER "public.pesanan trg_prevent_delete_pesanan"
pg_restore: creating TRIGGER "public.pesanan trg_validate_tarif_pesanan"
pg_restore: creating FK CONSTRAINT "public.address fk_address_city"
pg_restore: creating FK CONSTRAINT "public.buku fk_buku_kategori"
pg_restore: creating FK CONSTRAINT "public.city fk_city_country"
pg_restore: creating FK CONSTRAINT "public.customer fk_customer_address"
pg_restore: creating FK CONSTRAINT "public.customer fk_customer_store"
pg_restore: creating FK CONSTRAINT "public.detail_peminjaman fk_detail_buku"
pg_restore: creating FK CONSTRAINT "public.detail_peminjaman fk_detail_peminjaman"
pg_restore: creating FK CONSTRAINT "public.film_actor fk_film_actor_actor"
pg_restore: creating FK CONSTRAINT "public.film_actor fk_film_actor_film"
pg_restore: creating FK CONSTRAINT "public.film_category fk_film_category_category"
pg_restore: creating FK CONSTRAINT "public.film_category fk_film_category_film"
pg_restore: creating FK CONSTRAINT "public.film fk_film_language"
pg_restore: creating FK CONSTRAINT "public.film fk_film_original_language"
pg_restore: creating FK CONSTRAINT "public.pesanan_food fk_food_pesanan"
pg_restore: creating FK CONSTRAINT "public.inventory fk_inventory_film"
pg_restore: creating FK CONSTRAINT "public.inventory fk_inventory_store"
pg_restore: creating FK CONSTRAINT "public.klaim_promo fk_klaim_penumpang"
pg_restore: creating FK CONSTRAINT "public.klaim_promo fk_klaim_promo"
pg_restore: creating FK CONSTRAINT "public.mobil fk_mobil_kendaraan"
pg_restore: creating FK CONSTRAINT "public.motor fk_motor_kendaraan"
pg_restore: creating FK CONSTRAINT "public.payment fk_payment_customer"
pg_restore: creating FK CONSTRAINT "public.payment fk_payment_rental"
pg_restore: creating FK CONSTRAINT "public.payment fk_payment_staff"
pg_restore: creating FK CONSTRAINT "public.peminjaman fk_peminjaman_anggota"
pg_restore: creating FK CONSTRAINT "public.kendaraan fk_pengemudi_kendaraan"
pg_restore: creating FK CONSTRAINT "public.pengemudi fk_pengguna_pengemudi"
pg_restore: creating FK CONSTRAINT "public.penumpang fk_pengguna_penumpang"
pg_restore: creating FK CONSTRAINT "public.lokasi_favorit fk_penumpang_lokasi"
pg_restore: creating FK CONSTRAINT "public.penumpang_no_hp fk_penumpang_nohp"
pg_restore: creating FK CONSTRAINT "public.pesanan fk_pesanan_pengemudi"
pg_restore: creating FK CONSTRAINT "public.pesanan fk_pesanan_penumpang"
pg_restore: creating FK CONSTRAINT "public.rental fk_rental_customer"
pg_restore: creating FK CONSTRAINT "public.rental fk_rental_inventory"
pg_restore: creating FK CONSTRAINT "public.rental fk_rental_staff"
pg_restore: creating FK CONSTRAINT "public.pesanan_ride fk_ride_pesanan"
pg_restore: creating FK CONSTRAINT "public.pesanan_send fk_send_pesanan"
pg_restore: creating FK CONSTRAINT "public.staff fk_staff_address"
pg_restore: creating FK CONSTRAINT "public.staff fk_staff_store"
pg_restore: creating FK CONSTRAINT "public.store fk_store_manager"

D:\Program Files\PostgreSQL\18\bin>psql -U postgres -h localhost -p 5432 -d ojek_online_restore
Password for user postgres:

psql (18.6)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

ojek_online_restore=# a^Apsql -U postgres -h localhost -p 5432 -d ojek_online_restore -c "ANALYZE;" -c "SELECT relname AS tabel, n_live_tup AS jumlah_baris FROM pg_stat_user_tables ORDER BY relname;"
ojek_online_restore-# psql -U postgres -h localhost -p 5432 -d ojek_online_restore -c "ANALYZE;" -c "SELECT relname AS tabel, n_live_tup AS jumlah_baris FROM pg_stat_user_tables ORDER BY relname;"
ojek_online_restore-# psql -U postgres -h localhost -p 5432 -d ojek_online_restore -c "\dt"
ojek_online_restore-# psql -U postgres -h localhost -p 5432 -l
ojek_online_restore-# \q

D:\Program Files\PostgreSQL\18\bin>\dt
'\dt' is not recognized as an internal or external command,
operable program or batch file.

D:\Program Files\PostgreSQL\18\bin>pg_dump --version
pg_dump (PostgreSQL) 18.6

D:\Program Files\PostgreSQL\18\bi
