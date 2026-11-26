# PT DJS (Deploy Jumat Sore) — Booket Presentation, Architecture &amp; PoC Budget

Repository ini berisi materi presentasi interaktif, arsitektur teknis end-to-end, serta rincian anggaran *Proof of Concept (PoC)* &amp; proyeksi infrastruktur cloud oleh **PT DJS (Deploy Jumat Sore)** untuk **Tender Rusuh Booking Ticket**.

---

## 🌐 Live Presentation Deck (GitHub Pages)
Akses slide presentasi interaktif langsung melalui browser:
👉 **[https://riansopian.github.io/presentation_pt_djs/](https://riansopian.github.io/presentation_pt_djs/)**

### Kontrol Navigasi Presentasi:
- **Panah Kanan (`→`) / Spasi / PageDown**: Slide Berikutnya
- **Panah Kiri (`←`) / PageUp**: Slide Sebelumnya
- **Tombol `N`**: Munculkan / Sembunyikan **Speaker Notes** (naskah presenter)
- **Tombol `F`**: Mode Fullscreen

---

## 📂 Struktur Dokumen

1. **[`index.html`](index.html)**:
   - Presentasi HTML interaktif berisi **29 slide** dengan tema Booket dan lima tangkapan layar aplikasi dalam mode demo.
   - Bab Frontend membahas routing, state SDK, pemulihan sesi, dan validasi checkout. Bab Backend membahas layanan, stok, pembayaran, dan outbox. Presentasi juga memuat demo, biaya bulanan infrastruktur, anggaran PoC tiga bulan, proyeksi anggaran satu tahun, dan rencana PoC 12 minggu.
   - Sebelas diagram merangkum rancangan Booket dan ulasan lima layanan backend. Bab infrastruktur memuat inventaris 17 deployment Cloud Run dan membahas DNS dan CDN Cloudflare serta email transaksional Brevo. Materi produk mencakup FairQueue, pembayaran Snap, dan hasil uji yang dilaporkan. Ikon menggunakan [Phosphor](assets/architecture/PHOSPHOR-LICENSE.txt) dan [Simple Icons](https://github.com/simple-icons/simple-icons).
2. **[`presentation_deck_pt_djs.md`](presentation_deck_pt_djs.md)**:
   - Naskah presentasi slide-by-slide, speaker notes, dan panduan demo.
3. **[`technical_assessment_deep_dive_pt_djs.md`](technical_assessment_deep_dive_pt_djs.md)**:
   - Whitepaper arsitektur teknis untuk panel assessor.
4. **Paket Anggaran &amp; Proposal PoC Backend ([`budget/`](budget/))**:
   - [`budget/BOOKET_PROPOSAL_POC.md`](budget/BOOKET_PROPOSAL_POC.md) — Proposal PoC 3 Bulan (1 Des 2026 – 28 Feb 2027), tahapan 12 minggu, &amp; kriteria uji.
   - [`budget/BOOKET_BUDGET_EXISTING.md`](budget/BOOKET_BUDGET_EXISTING.md) — Audit konfigurasi live `booket-512519` (17 Cloud Run, PG 18, VM, Cloudflare, Brevo, &amp; usulan Confluent Kafka).
   - [`budget/BOOKET_BUDGET_3_BULAN.md`](budget/BOOKET_BUDGET_3_BULAN.md) — Proyeksi 3 bulan: PoC **US$7.140,74 (~US$7.200)** vs Fallback **US$8.364,11 (~US$8.400)** vs Uji Beban **US$9.766,19 (~US$9.800)**.
   - [`budget/BOOKET_BUDGET_1_TAHUN.md`](budget/BOOKET_BUDGET_1_TAHUN.md) — Proyeksi 12 bulan (**US$28.108,88 / ~US$28.200**, estimasi hemat ~16,7% vs existing + Kafka).

---

&copy; 2026 PT DJS (Deploy Jumat Sore). All rights reserved.
