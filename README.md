# PT DJS (Deploy Jumat Sore) — Booket.id Presentation & Technical Assessment

Repository ini berisi materi presentasi, live slide deck interaktif, dan lembar pertahanan teknis (*technical assessment defense*) yang diajukan oleh **PT DJS (Deploy Jumat Sore)** untuk **Tender Rusuh Booking Ticket**.

---

## 🌐 Live Presentation Deck (GitHub Pages)
Akses slide presentasi interaktif langsung melalui browser:
👉 **[https://riansopian.github.io/presentation_pt_djs/](https://riansopian.github.io/presentation_pt_djs/)**

### Kontrol Navigasi Presentasi:
- **Panah Kanan (`→`) / Spasi / PageDown**: Slide Berikutnya
- **Panah Kiri (`←`) / PageUp**: Slide Sebelumnya
- **Tombol `N`**: Munculkan / Sembunyikan **Speaker Notes** (naskah kata-kata pembicara)
- **Tombol `F`**: Mode Fullscreen

---

## 📂 Struktur Dokumen

1. **[`index.html`](index.html)**:
   - Slide deck interaktif 13 slide selaras dengan tema aplikasi web **Booket** (*light app shell*, crimson accent `#e11d48`, hero `#18191e`, *shimmer progress bar*, & *notched ticket pass*).
   - Siap dipresentasikan dalam mode fullscreen dengan responsivitas tinggi.
2. **[`presentation_deck_pt_djs.md`](presentation_deck_pt_djs.md)**:
   - Naskah presentasi lengkap slide-by-slide, speaker notes, dan **Live Demo Runbook (4 Skenario)**.
3. **[`technical_assessment_deep_dive_pt_djs.md`](technical_assessment_deep_dive_pt_djs.md)**:
   - Whitepaper arsitektur teknis mendalam untuk panel penilai (assessor).
   - Rincian algoritma antrean Redis $O(\log N)$, evaluasi Lua Script atomik, transaksi conditional update PostgreSQL, state machine Two-Tier TTL, kepatuhan UU PDP, dan verifikasi 666 automated tests ($\ge 80\%$ coverage).

---

## 🚀 Ringkasan Solusi: Booket

- **Virtual Waiting Room & Admission Control**: Mengisolasi hot-path transaksi menggunakan Redis Sorted Sets FCFS, mencegah server crash saat *flash crowd* 20.000+ RPS.
- **Self-Healing Fallback**: Penanganan payload legacy 200 OK secara otomatis di frontend (`waitingRoomFallback.ts`) tanpa membuat antarmuka pengguna crash.
- **Two-Tier Server-Authoritative TTL**: Batas isi form 5 menit dan batas pembayaran 10 menit divalidasi mutlak dari waktu UTC server.
- **Conditional SQL Decrement & Guard NIK**: Maksimal 3 tiket per order dengan aturan ketat `1 NIK = 1 Tiket Peserta` yang dikunci dalam satu transaksi atomik PostgreSQL.
- **Midtrans & Dynamic E-Ticket Pass**: Integrasi pembayaran Midtrans dan e-ticket pass berlekuk dengan barcode dinamis yang diperbarui periodik.
- **Engineering Excellence**: Teruji dengan **91 test files, 666 automated unit tests**, 6-stage GitLab CI/CD (Gitleaks, `npm audit`, Vitest, SonarQube, Vite build, Artifact Registry), dan deploy otomatis ke Google Cloud Run (`asia-southeast2`).

---

&copy; 2026 PT DJS (Deploy Jumat Sore). All rights reserved.
