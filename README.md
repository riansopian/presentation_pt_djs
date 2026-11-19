# 🎤 PT DJS (Deploy Jumat Sore) — Booket.id Presentation & Technical Assessment

Repository ini berisi materi presentasi, live slide deck interaktif, dan lembar pertahanan teknis (*technical assessment defense*) yang diajukan oleh **PT DJS (Deploy Jumat Sore)** untuk **Tender Rusuh Booking Ticket Telkomsel**.

---

## 🌐 Live Presentation Deck (GitHub Pages)
Akses slide presentasi interaktif langsung melalui browser:
👉 **[https://riansopian.github.io/presentation_pt_djs/](https://riansopian.github.io/presentation_pt_djs/)**

### 🎮 Kontrol Navigasi Presentasi:
- **Panah Kanan (`→`) / Spasi / PageDown**: Slide Berikutnya
- **Panah Kiri (`←`) / PageUp**: Slide Sebelumnya
- **Tombol `N`**: Munculkan / Sembunyikan **Speaker Notes** (naskah kata-kata pembicara)

---

## 📂 Struktur Dokumen

1. **[`index.html`](index.html)**:
   - Slide deck interaktif 14 slide berbasis Tailwind CSS.
   - Siap dipresentasikan dalam mode fullscreen dengan responsivitas tinggi.
2. **[`presentation_deck_pt_djs_tsel.md`](presentation_deck_pt_djs_tsel.md)**:
   - Naskah presentasi lengkap slide-by-slide.
   - Konsep visual, poin pembicaraan utama, naskah speaker notes, dan **Live Demo Runbook (4 Skenario)**.
3. **[`technical_assessment_deep_dive_pt_djs.md`](technical_assessment_deep_dive_pt_djs.md)**:
   - Whitepaper arsitektur teknis mendalam untuk panel penilai (assessor).
   - Rincian algoritma antrean Redis $O(\log N)$, evaluasi Lua Script atomik, transaksi conditional update PostgreSQL bebas race condition, state machine Two-Tier TTL, kepatuhan UU PDP, dan verifikasi 666 automated tests (>80% coverage).

---

## 🚀 Ringkasan Solusi: Booket.id

Booket.id adalah platform penjualan tiket konser berdaya tampung tinggi dengan prinsip keadilan (*Fairness*) dan jaminan zero-overselling:
- **Virtual Waiting Room & Admission Control**: Mengisolasi hot-path transaksi menggunakan Redis Sorted Sets FCFS, mencegah server crash saat *flash crowd* 20.000+ RPS.
- **Two-Tier Server-Authoritative TTL**: Batas isi form 5 menit dan batas pembayaran 10 menit divalidasi mutlak dari waktu UTC server.
- **Conditional SQL Decrement**: Pengurangan kuota stok dan pembatasan 3 tiket per NIK dijalankan dalam satu transaksi atomik PostgreSQL.
- **Anti-Fraud Dynamic E-Ticket**: QR code dinamis yang berotasi tiap 30 detik untuk mencegah manipulasi tangkapan layar oleh calo.
- **Reusable WebView**: Satu endpoint `/pay/:bookingId?embed=1` yang fleksibel untuk Web dan aplikasi mobile MyTelkomsel.
- **Engineering Excellence**: Teruji dengan **91 test suites, 666 automated unit tests**, dan pipeline otomatis GitLab CI/CD.

---

&copy; 2026 PT DJS (Deploy Jumat Sore). All rights reserved.
