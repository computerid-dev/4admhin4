# EchoNote — Admin Dashboard (privat)

Ini project **terpisah** dari EchoNote (sosial medianya) — deploy ke Vercel project sendiri,
domain sendiri, **jangan dibagikan ke publik**. Pakai Firebase project yang SAMA (`echonoteein`)
supaya bisa lihat/kelola data yang sama.

## Yang bisa dilakukan admin di sini

- **Antrean** — accept/reject pendaftaran & percobaan login manual, toggle Mode Auto
- **Kelola Pengguna** — cari, blokir/buka blokir akun
- **Moderasi Post** — lihat semua post (teks/foto/video), hapus yang melanggar (otomatis bersihkan komentar & file medianya)
- **Trending** — post dengan skor tertinggi (like + komentar×2)

## Yang TIDAK bisa dilakukan admin di sini (disengaja)

- Tidak bisa like, komentar, atau bikin post sendiri — cuma lihat & moderasi
- **Tidak ada akses chat sama sekali** — dashboard ini tidak punya fitur untuk membaca pesan chat siapa pun

**Catatan jujur soal chat:** pesan chat tersimpan di Firebase Realtime Database project yang sama.
Dashboard admin ini sengaja **tidak dibuatkan fitur** untuk membacanya, jadi lewat aplikasi ini
chat sepenuhnya privat. Tapi ini bukan enkripsi end-to-end yang sesungguhnya (di mana bahkan
pemilik server pun tidak bisa membaca isinya) — siapa pun yang punya akses ke Firebase Console
project ini secara teknis masih bisa membuka Realtime Database dan melihat isinya langsung dari
sana. Kalau kamu butuh privasi chat yang benar-benar tidak bisa dibaca siapa pun termasuk kamu
sendiri (true end-to-end encryption), itu perlu enkripsi di sisi client dengan kunci yang cuma
dipegang 2 orang yang chat-an — belum dibangun, kasih tahu saya kalau mau ditambahkan.

## Cara deploy

1. Import folder ini sebagai project Vercel **terpisah** dari EchoNote (repo/folder beda).
2. Set environment variables (sama seperti project EchoNote, service account Firebase yang sama):

   | Key | Value |
   |---|---|
   | `FIREBASE_PROJECT_ID` | `project_id` dari service account JSON |
   | `FIREBASE_CLIENT_EMAIL` | `client_email` dari service account JSON |
   | `FIREBASE_PRIVATE_KEY` | `private_key` dari service account JSON |
   | `FIREBASE_STORAGE_BUCKET` | `echonoteein.firebasestorage.app` (buat hapus media saat moderasi) |
   | `ADMIN_SECRET` | Password admin — **boleh beda** dari yang dipakai kalau kamu sempat set di project lama, tapi harus sama persis dengan yang kamu masukkan tiap login di sini |

3. Deploy. Dapat URL privat (mis. `admin-echonote.vercel.app`) — jangan disebar, cukup kamu yang tahu. Kalau mau, bisa juga di-set domain custom yang tidak gampang ditebak.

## Struktur

```
/api          → accept, reject, pending, settings, users, ban, posts, delete-post, trending, login
/lib          → firebaseAdmin.js, helpers.js (sama seperti project EchoNote)
/public       → index.html (dashboard-nya, ini satu-satunya halaman)
middleware.js → anti-scrape dasar (sama seperti EchoNote), robots.txt disallow all
```
