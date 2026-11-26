# Deck Presentasi &amp; Live Demo: Booket
### PT DJS (Deploy Jumat Sore) — Solusi Tender Rusuh Booking Ticket &amp; Proposal PoC
**Karakter Dokumen:** Dual Pitch (Client Solution Showcase, Deep Technical Assessment Defense, &amp; PoC Cloud Budgeting)

> **ARSIP:** Naskah ini mencerminkan deck dan asumsi budget 23 November 2026. Gunakan [presentasi HTML terbaru](index.html) untuk susunan slide yang berlaku dan [ringkasan budget terbaru](budget/BOOKET_BUDGET_RINGKAS.md) untuk angka terkini. Pagu PoC US$7.200 di bawah sudah disupersede.

---

```carousel
### Slide 1: Cover & Proposal PoC
PT DJS mempersembahkan Booket (`booket-512519`): Solusi War Tiket Konser Skala Masif + Rancangan PoC US$7.200 / 3 Bulan.
<!-- slide -->
### Slide 2: Analisis Masalah & Temuan Worker Idle
4 kerentanan utama war tiket termasuk risiko background outbox/consumer Cloud Run kekurangan CPU saat HTTP idle.
<!-- slide -->
### Slide 3: Pilar Solusi PT DJS
Admission Control (`service-waiting-room` + 3 core workers CPU kontinu), Confluent Kafka event streaming, & Cloud SQL PG 18 conditional update.
<!-- slide -->
### Slide 4: Inventaris Infrastruktur Aktual (`booket-512519`)
17 Cloud Run services (Go, Rust, Keycloak, Order, Outbox, Waiting Room, dukcapil-mock), 2 VM, PG 18 Enterprise Plus, Cloudflare, Brevo, & Kafka.
<!-- slide -->
### Slide 5: Waiting Room Internals & Adaptive Polling
FCFS Queue & Lua Script admission control, polling adaptif (`pollAfterSeconds` + jitter), serta `@booketid/core` 0.11.1 (`etaSeconds`).
<!-- slide -->
### Slide 6: Database Integrity & Aturan NIK
Cloud SQL PG 18 single-transaction conditional decrement, `MAX_TICKETS_PER_ORDER = 3`, `1 NIK = 1 Tiket Peserta`, & kepatuhan UU PDP.
<!-- slide -->
### Slide 7: Two-Tier TTL & Rekonsiliasi Anti-PHP
Hold 5 menit + Payment 10 menit server-authoritative, Midtrans snapshot persistence (`orderSnapshot.ts`), & idempotensi callback/outbox.
<!-- slide -->
### Slide 8: Clean Architecture & Self-Healing Fallback
Domain pure rules (`rules.ts`), Facade store pattern, serta auto-recovery JSON legacy 200 OK di `waitingRoomFallback.ts`.
<!-- slide -->
### Slide 9: Midtrans, Brevo & Notched E-Ticket Pass
Pembayaran Midtrans sandbox/live, pengiriman e-ticket transaksional via Brevo (US$499/bln), & Dynamic QR berlekuk.
<!-- slide -->
### Slide 10: 6-Stage GitLab CI/CD & 666 Tests
`npm audit`, Gitleaks, 666 Vitest tests (91 files), SonarQube, Vite build, Artifact Registry JKT, & Cloud Run deploy.
<!-- slide -->
### Slide 11: Live Demo Runbook (4 Skenario)
Happy Path Keycloak-Waiting Room-Checkout, Guard NIK Unik, Session Restore Antrean Anda, & Midtrans Pass Konser.
<!-- slide -->
### Slide 12: Audit Biaya Existing vs Target PoC
Optimasi Cloud SQL N2 8vCPU/64GiB (US$1.059/bln) → 4vCPU/32GiB (~US$503,64/bln) & alokasi CPU kontinu untuk 3 worker inti.
<!-- slide -->
### Slide 13: 3 Skenario Proyeksi Budget (3 Bulan & 1 Tahun)
Usulan PoC US$7.140,74 (~US$7.200) | Fallback Existing+Kafka US$8.364,11 (~US$8.400) | Uji Beban 100x + 2 eCKU US$9.766,19 (~US$9.800).
<!-- slide -->
### Slide 14: Roadmap PoC 12 Minggu, Kriteria Uji & Batas Cakupan
Timeline 5 fase (1 Des 2026 – 28 Feb 2027), SLA target uji, & transparansi batas cakupan non-pajak/non-gateway-fee.
<!-- slide -->
### Slide 15: Kesimpulan & Tanya Jawab
Kesiapan teknis, demo langsung, & rekomendasi keputusan PoC US$7.200 (fallback US$8.400).
```

---

## Ringkasan Materi Baru dari Paket Budgeting Backend (`BOOKET_BUDGET_PACKAGE`)

### Slide 12: Audit Biaya Cloud Existing (`booket-512519`) vs Target PoC
- **Kondisi Existing (Baseline 16–22 Nov 2026 WIB)**:
  - Run rate GCP + Cloudflare Free + Brevo + alokasi domain sebelum tambahan Kafka & cadangan: **US$1.786,31/bulan**.
  - Ditambah usulan **Confluent Cloud Standard** (1 eCKU 24/7 Jakarta **US$554,90/bln** + GCP outbound egress **US$1,90/bln**): total sebelum cadangan **US$2.343,11/bulan** (**US$2.811,73/bulan** dengan cadangan 20%).
  - Driver terbesar eksisting: **Cloud SQL Enterprise Plus PG 18 N2 (8 vCPU, 64 GiB RAM, 375 GiB cache)** sebesar **US$1.059,03/bulan**, padahal sampel 7 hari menunjukkan pemakaian CPU rata-rata hanya **0,94%** dan RAM **38,39%** (~24,57 GiB).
- **Optimasi Target PoC Stabil (Sebelum Cadangan: US$1.936,46/bulan | +20%: US$2.323,75/bulan)**:
  - **Cloud SQL Resize Bersyarat**: N2 **4 vCPU, 32 GiB RAM**, SSD tetap 100 GiB, cache dimatikan pasca-uji + aktifkan scheduled backup (20 GiB) $\rightarrow$ **~US$503,64/bulan**.
  - **Core Workers Reliabel**: `service-order`, `service-outbox`, dan `service-waiting-room` diubah menjadi **min 1 instance dengan instance-based billing (CPU kontinu)** sehingga biaya Cloud Run naik terukur ke **US$239,02/bulan** agar proses async tidak terhenti saat HTTP idle.
  - **Cleanup Jaringan**: Usulan melepas unused external IP `booket-id-gitlab-runner` (US$8,03/bln) dan VPC connector (US$16,44/bln) setelah dependensi terverifikasi Direct VPC.

### Slide 13: 3 Skenario Anggaran 3 Bulan (PoC) & Proyeksi 1 Tahun
*(Mata uang USD sebelum pajak; periode PoC 1 Des 2026 – 28 Feb 2027 = 90 hari kalender; 1 tahun = 1 Des 2026 – 30 Nov 2027)*

| Skenario | Bulanan Standar (pra-cadangan) | Kas 3 Bulan (incl. 1 renewal .id $15,15 + cadangan 20%) | Pembulatan Usulan 3 Bulan | Proyeksi 1 Tahun (incl. cadangan 20%) |
|---|---|---|---|---|
| **PoC dengan Transisi (Rekomendasi)** | **US$1.936,46** *(pasca-transisi)* | **US$7.140,74** *(incl. 14 hari transisi Des)* | **US$7.200,00** | **US$28.108,88** *(~US$28.200)* |
| **Existing + Kafka (Fallback)** | **US$2.343,11** | **US$8.364,11** | **US$8.400,00** | **US$33.740,73** *(~US$33.800)* |
| **Uji Beban (100x + Kafka 2 eCKU)** | **US$2.736,92** | **US$9.766,19** | **US$9.800,00** | **US$39.411,67** *(bila tiap bulan)* |

- **Estimasi Penghematan Tahunan**: **US$5.631,85 (~16,7%)** dengan memilih Target PoC vs mempertahankan konfigurasi existing + Kafka sepanjang tahun.
- **Tata Kelola Kontrol Biaya**: Monitoring per SKU bulanan + budget alert **50%, 80%, 100% actual & forecast** (memperhatikan pula aturan Google Observability alerting pricing per **1 September 2027**).

### Slide 14: Roadmap PoC 12 Minggu, Kriteria Keberhasilan & Batas Cakupan
- **Tahapan 12 Minggu**:
  1. **Minggu 1–2**: Validasi invoice/kontrak vendor (GCP, registrar, quote Confluent Jakarta, kuota Brevo), baseline kapasitas, & uji backup/restore.
  2. **Minggu 3–4**: Terapkan konfigurasi PoC, integrasi alur order, Kafka outbox, & email Brevo.
  3. **Minggu 5–8**: UAT terbatas, antrean, kuota NIK, pembayaran sandbox, retry event, & **uji worker idle 30 menit**.
  4. **Minggu 9–10**: Uji beban terkontrol bertahap & simulasi pemulihan.
  5. **Minggu 11–12**: Rekonsiliasi biaya riil vs estimasi & keputusan rekomendasi produksi.
- **Batas Cakupan (Transparansi)**:
  - Bukan tagihan aktual (dataset BigQuery billing export belum aktif; Cloud Asset API belum aktif; Firebase Hosting 403).
  - Belum mencakup pajak, biaya development/migrasi, tenaga operasional, fee payment gateway, overage Brevo/SMS/WhatsApp, atau resource lingkungan kedua yang terpisah paralel.
  - Cadangan 20% merupakan buffer perencanaan.

---

Untuk file anggaran rinci per SKU dan per bulan, lihat folder `budget/` (`BOOKET_PROPOSAL_POC.md`, `BOOKET_BUDGET_EXISTING.md`, `BOOKET_BUDGET_3_BULAN.md`, dan `BOOKET_BUDGET_1_TAHUN.md`).
