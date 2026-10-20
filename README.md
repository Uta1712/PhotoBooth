# Midnight Love — Couple Photobooth

Website photobooth romantis bertema Midnight Love, dibuat dalam satu file HTML agar mudah dipasang di GitHub Pages.

## Fitur
- Upload foto untuk dua orang.
- Ambil foto langsung melalui kamera HP/laptop (memerlukan izin kamera).
- Hasil photostrip PNG yang bisa diunduh.
- 3 tata letak, 4 gaya frame, caption, nama, dan sparkle.
- Koneksi browser-to-browser (WebRTC) untuk mengirim foto antardua perangkat dengan menyalin kode offer/answer.
- Responsif untuk HP, tablet, dan desktop.

## Cara pakai di GitHub Pages
1. Ekstrak ZIP ini.
2. Buat repository baru di GitHub, misalnya `midnight-love-photobooth`.
3. Upload `index.html` ke root repository (jangan dimasukkan ke subfolder).
4. Buka **Settings → Pages**.
5. Pada **Build and deployment**, pilih **Deploy from a branch**.
6. Pilih branch `main` dan folder `/(root)`, lalu tekan **Save**.
7. Tunggu beberapa menit dan buka URL Pages yang diberikan GitHub.

## Cara membuat photostrip
1. Isi foto untuk Person One dan Person Two melalui Upload photo atau Use camera.
2. Sesuaikan nama, caption, gaya frame, dan layout.
3. Tekan **Generate photostrip**.
4. Tekan **Download photostrip** untuk menyimpan PNG.

## Mengirim foto antarperangkat (LDR)
1. Kedua orang membuka website yang sudah terpasang di GitHub Pages.
2. Perangkat pertama tekan **Create connection code**, lalu kirim isi Offer Code lewat chat.
3. Perangkat kedua pilih **I have a session code**, tempel offer, lalu tekan **Create answer code**.
4. Perangkat kedua kirim Answer Code kembali.
5. Perangkat pertama tempel Answer Code lalu tekan **Connect**.
6. Setelah status Connected, tombol **Send my photo** mengirim foto Person One dari perangkat tersebut. Tombol **Send partner photo to my device** mengirim foto Person Two dari perangkat tersebut; nama tombol membantu memilih foto yang akan dikirim, tetapi foto diterima pada slot foto lawan di perangkat penerima.

## Catatan
- Kamera memerlukan izin dan secure context; GitHub Pages menggunakan HTTPS.
- Koneksi WebRTC memakai STUN publik untuk membantu koneksi. Jaringan tertentu mungkin memblokir koneksi peer-to-peer; fitur ini tidak dijamin bekerja di semua jaringan.
- Kode koneksi adalah informasi sesi sementara. Jangan unggah atau bagikan ke tempat publik.
- Foto tidak diunggah ke server oleh fitur photostrip; proses gambar berjalan di browser. Untuk koneksi LDR, foto dikirim langsung antarbrowser ketika koneksi berhasil.
- Nama dan foto tidak disimpan permanen oleh website. Unduh hasil jika ingin menyimpannya.
