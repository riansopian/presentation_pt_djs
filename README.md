# PT DJS (Deploy Jumat Sore) — Booket Presentation, Architecture &amp; PoC Budget

Repository ini berisi materi presentasi interaktif, arsitektur teknis end-to-end, serta rincian anggaran *Proof of Concept (PoC)* &amp; proyeksi infrastruktur cloud oleh **PT DJS (Deploy Jumat Sore)** untuk **Tender Rusuh Booking Ticket**.

---

## 🌐 Live Presentation Deck (GitHub Pages)
Akses slide presentasi interaktif langsung melalui browser:
👉 **[https://riansopian.github.io/presentation_pt_djs/](https://riansopian.github.io/presentation_pt_djs/)**

[Unduh PowerPoint Booket (30 slide)](Booket-PT-DJS-Pengalaman-Pembeli.pptx)

### Kontrol Navigasi Presentasi:
- **Panah Kanan (`→`) / Spasi / PageDown**: Slide Berikutnya
- **Panah Kiri (`←`) / PageUp**: Slide Sebelumnya
- **Tombol `N`**: Munculkan / Sembunyikan **Speaker Notes** (naskah presenter)
- **Tombol `F`**: Mode Fullscreen

---

## 📂 Struktur Dokumen

1. **[`index.html`](index.html)**:
   - Presentasi HTML interaktif berisi **29 slide** dengan tema Booket dan lima tangkapan layar aplikasi dalam mode demo.
   - Bab Frontend membahas routing, state SDK, pemulihan sesi, dan validasi checkout. Bab Backend membahas layanan, stok, pembayaran, dan outbox. Bagian budget memuat estimasi November 2026, forecast operasi 12 bulan terbaru, perbandingan scope dengan budget lama, serta hal yang perlu ditetapkan untuk PoC.
   - Sebelas diagram merangkum rancangan Booket dan ulasan lima layanan backend. Bab infrastruktur memuat inventaris 17 deployment Cloud Run dan membahas DNS dan CDN Cloudflare serta email transaksional Brevo. Materi produk mencakup FairQueue, pembayaran Snap, dan hasil uji yang dilaporkan. Ikon menggunakan [Phosphor](assets/architecture/PHOSPHOR-LICENSE.txt) dan [Simple Icons](https://github.com/simple-icons/simple-icons).
2. **[`presentation_deck_pt_djs.md`](presentation_deck_pt_djs.md)**:
   - Naskah versi 23 November yang disimpan sebagai arsip; untuk deck dan budget terbaru, gunakan `index.html` dan ringkasan budget di atas.
3. **[`technical_assessment_deep_dive_pt_djs.md`](technical_assessment_deep_dive_pt_djs.md)**:
   - Whitepaper arsitektur teknis untuk panel assessor.
4. **Ringkasan budget terbaru ([`budget/BOOKET_BUDGET_RINGKAS.md`](budget/BOOKET_BUDGET_RINGKAS.md))** — November US$2.024,62 termasuk cadangan; forecast 12 bulan US$15.346,78. Cloud SQL dihapus dan tidak masuk forecast. Perbandingan dengan budget 23 November tidak setara scope.
5. **Dokumen budget 23 November** di [`budget/`](budget/) disimpan sebagai arsip dan tidak lagi menjadi pagu terkini.

---

&copy; 2026 PT DJS (Deploy Jumat Sore). All rights reserved.
