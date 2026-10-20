# Midnight Love Photobooth — versi publik

Website photobooth berbahasa Indonesia, siap untuk GitHub Pages. Ada beberapa gaya strip, pilihan tata letak, filter, upload foto/kamera, hasil PNG, serta alur sesi foto LDR menggunakan tautan.

## Fitur
- Bahasa Indonesia dan desain responsif.
- 12 desain strip yang benar-benar berbeda: Midnight Classic, Korean 4-Cut, Polaroid Stack, Film Contact Sheet, Heart Windows, Love Magazine, Memory Tickets, Scrapbook, Split Portrait, Comic Pop, Minimal Editorial, dan Neon Afterdark.
- Layout strip 4/3/2 frame, foto berdampingan, kolase 2x2.
- Upload foto dan sesi kamera langsung untuk setiap frame; hasil jepretan otomatis masuk ke frame yang dipilih.
- Jumlah foto per orang mengikuti tata letak: 2, 3, atau 4 foto. Kedua orang wajib melengkapi jumlah frame yang sama.
- Setiap foto orang pertama dan kedua disimpan sebagai kumpulan foto pada sesi, sehingga pasangan bisa melengkapi frame melalui tautan lintas perangkat.
- Filter natural, hangat, dingin, hitam putih, dreamy.
- Unduh hasil PNG.
- Tautan sesi lintas perangkat memakai Supabase (perlu konfigurasi satu kali).

## A. Publikasikan di GitHub Pages
1. Buat repository GitHub baru.
2. Unggah `index.html` ke root repository.
3. Masuk **Settings → Pages**.
4. Pilih **Deploy from a branch**, branch `main`, folder `/(root)`, lalu Save.

## B. Aktifkan tautan sesi lintas perangkat (Supabase gratis)
GitHub Pages hanya menyajikan file statis. Agar foto yang kamu ambil bisa dilihat pacar melalui link, perlu database online. Website ini sudah disiapkan untuk Supabase. Foto tiap frame dikirim sebagai kumpulan data foto dalam kolom `photo_one` dan `photo_two`; kolom `layout` menyimpan jumlah frame yang dipilih.

1. Buat project di https://supabase.com/
2. Di project, buka **SQL Editor → New query**, lalu jalankan SQL berikut untuk prototipe:

```sql
create table if not exists public.photobooth_sessions (
  id uuid primary key default gen_random_uuid(),
  photo_one text,
  name_one text default '',
  photo_two text,
  name_two text default '',
  caption text default 'Jarak boleh jauh, kenangan tetap dekat.',
  style text default 'midnight',
  layout text default 'four',
  created_at timestamptz not null default now()
);

alter table public.photobooth_sessions add column if not exists layout text default 'four';

alter table public.photobooth_sessions enable row level security;

create policy "public can create photobooth sessions"
on public.photobooth_sessions for insert to anon
with check (true);

create policy "public can view photobooth sessions"
on public.photobooth_sessions for select to anon
using (true);

create policy "public can update photobooth sessions"
on public.photobooth_sessions for update to anon
using (true) with check (true);

grant usage on schema public to anon;
grant select, insert, update on public.photobooth_sessions to anon;
```

3. Buka **Project Settings → API** (atau bagian API Keys pada dashboard project) dan salin **Project URL** serta **anon/public key**.
4. Buka `index.html`, cari dua baris berikut dekat awal script:

```js
const SUPABASE_URL = "GANTI_DENGAN_SUPABASE_PROJECT_URL";
const SUPABASE_ANON_KEY = "GANTI_DENGAN_SUPABASE_ANON_KEY";
```

Ganti nilainya dengan URL dan anon/public key milik project Supabase. **Jangan pernah gunakan `service_role` key di website publik.**
5. Simpan lalu unggah ulang `index.html` ke GitHub. Tunggu deploy selesai.

### Cara menggunakan link LDR
1. Kamu dan pasangan membuka website.
2. Kamu unggah/ambil foto pertama.
3. Tekan **Buat tautan sesi LDR**, lalu salin dan kirim link ke pasangan.
4. Pasangan membuka link tersebut, mengambil/mengunggah foto kedua. Foto tersimpan ke sesi yang sama.
5. Buat strip foto lalu unduh PNG.

## Penting tentang keamanan
SQL di atas membuat prototipe publik yang sederhana: siapa pun yang mengetahui ID sesi dapat membaca/mengubah data sesi, dan data foto disimpan sebagai data URL di tabel. Gunakan hanya untuk uji coba dengan foto yang tidak sensitif. Untuk peluncuran publik yang serius, sebaiknya gunakan Supabase Storage, masa kedaluwarsa sesi, batas ukuran file, dan kebijakan akses berbasis token/owner. Jangan memasukkan data pribadi sensitif.


## Alur website terbaru

Website menggunakan menu bertahap: **Masuk → Pilih strip (12 desain) → Foto orang pertama → Bagikan tautan → Foto orang kedua di perangkat pasangan → Preview hasil → Galeri**. Tautan pasangan otomatis membuka menu foto kedua. Galeri disimpan secara lokal di browser/perangkat yang digunakan; galeri tidak otomatis tersinkron antarperangkat.

Tombol berbagi WhatsApp dan tautan sesi lintas perangkat memerlukan konfigurasi Supabase sesuai petunjuk di atas. Jangan gunakan Supabase `service_role` key di website publik.
