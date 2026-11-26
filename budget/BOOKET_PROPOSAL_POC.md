> **STATUS: ARSIP (23 November 2026).** Dokumen ini telah disupersede oleh [ringkasan budget terbaru](BOOKET_BUDGET_RINGKAS.md), yang memakai scope backend 26 November. Angka di bawah tidak berlaku sebagai pagu terkini; perbandingan dengan forecast baru tidak setara karena Cloud SQL dihapus dan metode cadangan berubah.

# Proposal Proof of Concept Platform Booket

**Versi:** 1.0, 23 November 2026.  
**Untuk:** Tim Booket.  
**Periode usulan:** 1 Desember 2026–28 Februari 2027.  
**Status:** Usulan ruang lingkup dan anggaran untuk ditinjau. Bukan bukti pengadaan atau perubahan infrastruktur.

## Ringkasan usulan

PoC ini bertujuan memvalidasi alur pemesanan Booket, antrean checkout, pembayaran sandbox, pengiriman email, dan pemrosesan event Kafka pada lingkungan yang sudah tersedia. Hasil yang dicari adalah bukti bahwa alur bisnis dan proses background bekerja, kapasitas dapat diukur, serta biaya operasi dapat dipantau sebelum menentukan konfigurasi produksi.

Usulan dana cloud dan platform selama tiga bulan adalah **US$7.200,00 sebelum pajak**, dengan hasil perhitungan **US$7.140,74**. Nilai tersebut sudah mencakup Brevo US$499/bulan, Confluent Cloud Standard 1 eCKU aktif 24/7, Cloudflare Free untuk dua site, satu renewal domain `.id`, dua minggu transisi, dan cadangan biaya 20%.

Dana ini menggunakan ulang resource existing. Biaya tenaga kerja, pengembangan fitur, migrasi berbayar, fee transaksi payment gateway, dan lingkungan kedua yang sepenuhnya terpisah belum dihitung. Penawaran pelaksanaan komersial memerlukan estimasi pekerjaan tersendiri.

## Latar belakang dan kondisi saat ini

Snapshot project `booket-512519` menemukan 17 Cloud Run service, dua VM aktif, satu Cloud SQL Enterprise Plus PostgreSQL 18, satu Serverless VPC connector, tiga bucket dan lima repository Artifact Registry. Cloudflare masih Free dengan dua site Workers/Pages. Brevo direncanakan US$499/bulan berdasarkan informasi pengguna. Anggaran Confluent Standard ditambahkan sesuai permintaan; akun dan cluster existing belum diperiksa.

Cloud SQL sekarang memiliki 8 vCPU, RAM 64 GiB, SSD 100 GiB dan data cache 375 GiB. Sampel tujuh hari menunjukkan CPU rata-rata 0,94% dan RAM rata-rata 38,39%. Data ini mendukung pengujian konfigurasi lebih kecil, tetapi belum mewakili kondisi peak penjualan. Backup terjadwal dan PITR disabled pada snapshot.

Enam Cloud Run memiliki minimum instance aktif, sedangkan seluruh konfigurasi yang dicek memakai request-based billing. Repositori menyebut admission worker, outbox, consumer dan rekonsiliasi berjalan di background. PoC perlu menguji pemrosesan saat tidak ada traffic HTTP dan menganggarkan CPU kontinu untuk tiga worker inti. Minimum instance saja tidak dianggap bukti pekerjaan background akan terus mendapat CPU.

## Tujuan dan ruang lingkup

PoC mencakup alur login Keycloak, akses katalog produk, antrean waiting room, admission checkout, pembuatan order, pembayaran sandbox dan perubahan status order. Event diterbitkan dan dikonsumsi melalui Confluent Kafka. Email transaksional menggunakan Brevo, dengan data kontak CRM minimum untuk kebutuhan uji yang telah disepakati.

Dua site tetap dilayani melalui Cloudflare Workers/Pages dan domain `.id` yang tersedia. Inventory mencakup Cloud Run frontend yang masih ada; anggaran tidak menghapus biaya frontend GCP hanya karena dua site juga ada di Pages. Pemetaan site dan penghentian layanan lama harus dibuktikan terpisah.

PoC tidak menjanjikan sertifikasi production, target pengunjung tanpa load test, high availability menyeluruh, private networking Confluent, pengiriman SMS/WhatsApp, atau feature tambahan di luar alur tersebut. Angka uji beban adalah anggaran aktivitas, bukan jaminan kapasitas.

## Rancangan teknis yang diusulkan

Permintaan pengguna masuk melalui Cloudflare ke site frontend dan gateway/API yang terpakai. Cloud Run menangani identitas dan API bisnis. PostgreSQL Cloud SQL menyimpan state durable. Koneksi internal yang telah memakai Direct VPC dipertahankan. VM gateway dan datastore tetap dianggarkan karena fungsi proses aktual di dalam VM belum diaudit.

Target database memakai N2 4 vCPU dan RAM 32 GiB dengan SSD tetap 100 GiB. Data cache dinonaktifkan hanya setelah pengujian memperlihatkan latency dan pemakaian disk yang dapat diterima. Scheduled backup ditambahkan dalam rancangan dengan allowance storage terpakai 20 GiB dan satu uji pemulihan. Tidak mengasumsikan disk existing bisa diperkecil.

`service-order`, `service-outbox`, dan `service-waiting-room` dianggarkan masing-masing min 1 instance dengan instance-based billing, 1 vCPU dan RAM 512 MiB. Consumer pada service lain tetap perlu pengujian saat idle; jika juga memerlukan CPU kontinu, anggaran dan konfigurasi diperbarui. Connector dan alamat IPv4 yang tidak dipakai hanya boleh dilepas setelah pengecekan dependensi dan rollback plan.

Kafka menggunakan **Confluent Cloud Standard** sebagai interpretasi tier profesional terendah untuk production. Standard tersedia di GCP Jakarta dan memakai public endpoint terenkripsi TLS/SASL. Target utama adalah 1 eCKU aktif 24/7, dengan allowance storage logical rata-rata 20 GiB dan fanout traffic 10 GiB ingress/30 GiB egress per bulan. Biaya storage dihitung 3x sesudah replikasi. Tarif regional harus dikonfirmasi sebelum subscription. Paket support premium, managed connectors dan produk stream processing tidak termasuk. Standard 1 eCKU tidak dinyatakan memiliki SLA 99,99%.

Brevo mengikuti harga pengguna US$499/bulan. Pada awal PoC, verifikasi fitur dan kuota paket, autentikasi domain pengirim, template email uji dan sinkronisasi kontak yang dibutuhkan. Batas overage dan biaya channel lain harus dikonfirmasi sebelum mengirim traffic riil dalam jumlah besar.

## Anggaran

| Skenario | Per bulan standar sebelum cadangan | Kas 3 bulan termasuk cadangan |
| --- | --- | --- |
| Existing + Kafka | US$2.343,11 | US$8.364,11 |
| PoC dengan transisi | US$1.936,46 | US$7.140,74 |
| Uji beban | US$2.736,92 | US$9.766,19 |

Angka per bulan standar memakai 730 jam. Kas tiga bulan memakai kalender 90 hari dan satu renewal domain penuh. Kolom PoC memasukkan transisi 14 hari di Desember. Seluruh angka menggunakan USD dan belum memasukkan pajak. Cadangan 20% diterapkan pada total kas tiga bulan; kolom bulanan disajikan sebelum cadangan. Detail rekonsiliasi terdapat pada [budget tiga bulan](BOOKET_BUDGET_3_BULAN.md).

Run rate target PoC setelah transisi adalah **US$1.936,46/bulan**, atau **US$2.323,75 dengan cadangan**. Cloud SQL sekitar US$503,64, Cloud Run US$239,02, Brevo US$499 dan Confluent US$554,90 per bulan standar adalah komponen utamanya. GCP egress ke Confluent dihitung terpisah pada Network, sehingga biaya vendor dan biaya sumber tidak tertukar.

Anggaran satu tahun bila target diteruskan, termasuk transisi dan cadangan, adalah **US$28.108,88**, dibulatkan US$28.200,00 sebelum pajak. Ini proyeksi operasional dan tidak mengharuskan kontrak setahun.

## Tahapan pelaksanaan

| Waktu | Kegiatan | Bukti keluaran |
| --- | --- | --- |
| Minggu 1–2 | Validasi invoice/kontrak dan dependensi, baseline kapasitas, uji backup restore, verifikasi Brevo dan konfigurasi Confluent | Daftar resource/owner, baseline latency/usage, hasil restore dan biaya vendor terkonfirmasi |
| Minggu 3–4 | Terapkan perubahan PoC setelah peninjauan, integrasi alur order dan event, pengaturan email uji | Lingkungan PoC, hasil pengujian alur dan daftar perubahan/rollback |
| Minggu 5–8 | Uji pengguna terbatas, antrian dan kuota, pembayaran sandbox, retry event, observasi proses idle | Hasil UAT, sampel event/email, data lag dan latency, biaya mingguan |
| Minggu 9–10 | Uji beban terkontrol bertahap sesuai envelope yang dipilih dan uji pemulihan | Laporan kapasitas, error dan biaya per aktivitas |
| Minggu 11–12 | Perbaikan prioritas, rekonsiliasi biaya dan evaluasi kelanjutan | Laporan hasil PoC, konfigurasi rekomendasi dan keputusan lanjut/tunda |

Jadwal mulai bergantung kesiapan akses, data uji, akun vendor dan penanggung jawab. Rancangan tidak otomatis memberikan izin mengganti resource. Pelaksanaan dan pengeluaran mengikuti peninjauan rencana perubahan yang disepakati tim.

## Kriteria keberhasilan yang diusulkan

Kriteria berikut adalah target uji untuk disepakati sebelum pelaksanaan, bukan hasil yang sudah dicapai.

| Area | Kriteria penerimaan | Cara membuktikan |
| --- | --- | --- |
| Alur transaksi | Login, antre, admission, checkout dan status pembayaran sandbox selesai pada seluruh test case yang disepakati | UAT dengan ID transaksi dan catatan status |
| Konsistensi stok | Tidak ada oversell pada dataset uji; duplikasi event/callback tidak membuat pencatatan ganda | Rekonsiliasi order, claim dan stok setelah pengujian concurrent |
| Worker dan Kafka | Outbox, admission dan rekonsiliasi berjalan ketika tidak ada request pengguna; event dapat diproses ulang | Uji idle 30 menit, consumer lag, retry serta pemeriksaan state akhir |
| Email dan CRM | Email uji terkirim dan statusnya dapat dilacak; kontak CRM yang disepakati tersimpan tanpa duplikasi tak diinginkan | Log Brevo dan daftar kontak uji terkontrol |
| Kinerja | Usulan awal error rate <1% dan p95 API inti <2 detik pada profil uji yang disepakati | Laporan load test yang menyebut throughput, concurrency, durasi dan pengecualian provider |
| Pemulihan | Backup dapat dipulihkan dan data uji dapat diverifikasi | Satu simulasi restore pada lingkungan sementara terkontrol |
| Biaya | Pemakaian vendor direkonsiliasi setiap minggu dan perubahan scope tidak melewati envelope tanpa review | Laporan GCP, Confluent, Brevo dan catatan keputusan biaya |

Waktu tunggu antrean yang memang disengaja dan latency penyelesaian pembayaran eksternal tidak disamakan dengan latency API inti. Target RPO/RTO dan kapasitas concurrent final ditetapkan setelah baseline, agar proposal tidak menjanjikan nilai tanpa pengukuran.

## Risiko dan keputusan yang perlu diselesaikan

| Risiko atau ketergantungan | Tindakan dalam PoC |
| --- | --- |
| SQL 32 GiB tidak cukup pada peak | Load test bertahap, monitor RAM/connections/latency, pertahankan fallback konfigurasi existing |
| Background consumer kehilangan CPU | Uji idle dan tetapkan instance billing atau pola eksekusi terpicu yang sesuai, perbarui budget consumer tambahan |
| Tarif vendor dan kuota belum terkonfirmasi | Validasi quote regional Confluent, package Brevo dan invoice registrar pada minggu pertama |
| Confluent Standard memakai public networking | Pastikan TLS/SASL dan ACL. Jika private-only wajib, hitung ulang Enterprise dan konektivitasnya |
| Email atau CRM melampaui kuota | Batasi data/volume uji dan konfirmasi overage serta fitur paket sebelum kampanye |
| PoC harus terisolasi penuh | Buat estimasi resource duplikat, jangan gunakan budget reuse existing tanpa revisi |
| Lonjakan beban menaikkan eCKU dan log | Pantau pemakaian dan tetapkan batas kapasitas yang sudah diuji; budget alert bukan pembatas tagihan |
| Pajak dan biaya pengerjaan belum masuk | Tambahkan berdasarkan invoice/ketentuan billing serta estimasi kerja yang disetujui |

## Rekomendasi tindak lanjut

Tinjau usulan PoC dengan envelope **US$7.200,00 sebelum pajak** untuk resource yang dipakai ulang. Pertahankan fallback **US$8.400,00** bila database harus tetap pada kapasitas existing sepanjang tiga bulan. Envelope uji beban penuh **US$9.800,00** hanya digunakan jika pengujian pada skenario tersebut dijalankan. Nilai ini tidak boleh dijumlahkan sebagai tiga anggaran terpisah untuk satu lingkungan yang sama.

Pada minggu pertama, lengkapi invoice GCP dan registrar, quote Confluent regional, rincian kuota Brevo, serta penanggung jawab cloud/backend, QA dan pemilik bisnis. Sesudah validasi baseline, putuskan konfigurasi PoC dan rencana perubahan yang akan dijalankan. Pada akhir PoC, keputusan kelanjutan didasarkan pada hasil uji, biaya nyata dan kebutuhan production yang terukur.

## Referensi anggaran

- [Budget existing dan daftar sumber tarif](BOOKET_BUDGET_EXISTING.md)
- [Budget tiga bulan dan transisi](BOOKET_BUDGET_3_BULAN.md)
- [Proyeksi satu tahun](BOOKET_BUDGET_1_TAHUN.md)

Seluruh estimasi memiliki batas cakupan pada dokumen budget existing. Penyusunan proposal ini tidak mengaktifkan subscription, mengirim email, membuat Kafka cluster, atau mengubah konfigurasi cloud.
