# Audit Meta Ads — Grup Elharamain

- **Tanggal audit:** 25 Sep 2026
- **Periode data:** 30 hari terakhir (`last_30d`), mata uang IDR
- **Sumber:** Meta Ads MCP (entity, dataset/pixel, opportunity score)
- **Blueprint / DNA akun:** DATA TIDAK TERSEDIA. Repo ini belum punya Blueprint, jadi audit ini menjadi baseline pertama.
- **Perubahan yang dieksekusi:** tidak ada. Audit ini hanya membaca data.

> **Koreksi (25 Sep 2026):** KPI akun ini adalah **CPR = biaya per percakapan WA**, bukan purchase. Purchase memang tidak diharapkan tercatat di Meta. Karena itu, T1 dan tindakan #1–#2 di bawah **dicabut**. Rekap versi terbaru ada di `2026-09-25-rekap.md`.

---

## 0. Cakupan akun

| Status | Jumlah | Catatan |
|---|---|---|
| Total akun terhubung | 53 | 50 di halaman 1, 3 di halaman 2 |
| Bisa dianalisis (MCP aktif + queryable) | 37 | |
| **Punya spend 30 hari terakhir** | **15** | Fokus audit |
| MCP belum aktif (rollout Meta) | 8 | Elharamainclose, PAUSE Elharamain Wisata, Fifi, Umroh Plus, 4× "Elharamain Wisata (Read-Only)". **DATA TIDAK TERSEDIA** |
| Tidak bisa di-query | 8 | 4 CLOSED ("Unknown error"), 4 DISABLED (ditandai Meta karena "unusual activity") |

---

## 1. Data Quality Gate

| Item | Nilai |
|---|---|
| Total spend (15 akun) | **Rp294.184.284** |
| Konversi utama: *Messaging conversations started* | **5.605** (12 dari 15 akun; 3 akun hasilnya "mixed"/reach) |
| Lead (event `lead`) | 634 |
| **Purchase** | **7** ❗ |
| Attribution window | `1d_view_7d_click`. Dicek pada 10 adset akun Fikri; akun lain **belum diverifikasi** |
| Pixel health | EMQ **6,1–6,4** (dataset "Haji" & "Admin"). Match key hanya IP/UA/fbp/fbc; email/nama ≤16%, **tanpa nomor telepon**. Dataset "elharamain Balaschat" hampir mati (17 PageView/7 hari) |

**Aturan gate:**
- Purchase = 7 (< 30). Karena itu **semua temuan terkait purchase/ROAS otomatis berstatus ? HIPOTESIS.**
- Messaging conversation = 5.605 (≥ 30), jadi temuan di level percakapan boleh diberi label lebih tinggi.

---

## 2. Scorecard per akun (30 hari)

| Akun | Spend | Share | Percakapan | Biaya/percakapan | Lead | Biaya/lead | Purchase | CPM | CTR | Freq |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Elharamain Haji 2 | 67,6 jt | 23,0% | 1.578 | 42.871 | 122 | 554.506 | 0 | 17.874 | 1,55% | 1,78 |
| Fikri | 41,7 jt | 14,2% | 971 | 42.919 | 151 | 275.988 | 2 | 44.797 | 1,69% | 1,63 |
| Elharamain Haji | 40,6 jt | 13,8% | 1.176 | **34.546** | 47 | 864.379 | 0 | 14.839 | 1,53% | 1,55 |
| Tira | 37,8 jt | 12,9% | 862 | 43.907 | 137 | 276.261 | 1 | 41.620 | 1,78% | 1,65 |
| Nida | 29,7 jt | 10,1% | *mixed* | — | 79 | 375.809 | 2 | 8.724 | **0,53%** | 1,63 |
| Elharamain Haji 4 | 23,6 jt | 8,0% | *mixed* | — | 27 | 874.081 | 1 | 18.086 | 2,40% | 1,73 |
| AIni | 15,6 jt | 5,3% | 253 | **61.715** | 17 | 918.463 | 0 | 20.945 | 0,90% | 1,22 |
| Alif | 11,3 jt | 3,8% | 209 | 54.024 | 19 | 594.260 | 1 | 61.607 | 1,17% | 1,42 |
| Elharamain Haji 3 | 6,4 jt | 2,2% | 137 | 46.638 | 8 | 798.670 | 0 | 32.990 | 1,70% | 1,38 |
| Hanif | 5,7 jt | 1,9% | 131 | 43.242 | 8 | 708.080 | 0 | 28.905 | 2,03% | 1,36 |
| Diana | 4,0 jt | 1,4% | 89 | 44.965 | 1 | 4,0 jt | 0 | 23.756 | 1,05% | 1,41 |
| Elharamain | 3,9 jt | 1,3% | 84 | 45.933 | 0 | — | 0 | 27.030 | 1,00% | 1,32 |
| Irfan | 3,4 jt | 1,2% | 88 | 38.940 | 17 | **201.574** | 0 | 45.190 | 1,78% | 1,52 |
| Elharamain Wisata | 1,5 jt | 0,5% | reach 655rb | — | 0 | — | 0 | 1.444 | 0,05% | 1,55 |
| Elharamain Fifi | 1,4 jt | 0,5% | 27 | 51.083 | 1 | 1,4 jt | 0 | 118.441 | 2,15% | 1,42 |

**Rata-rata biaya per percakapan (blended): Rp42.716.** Median Rp44.436 (n=12 akun). Rentang Rp34.546–61.715.

---

## 3. Tiga Hal Mengejutkan (NOTICE)

1. **Rp294 jt dibelanjakan, hanya 7 purchase yang tercatat.** Kampanye di akun terbesar (Haji 2, Nida, Haji 4) memakai objective *Sales* dengan indikator hasil `actions:purchase`, dan hasilnya "Not available".
2. **CPM tidak memprediksi biaya per percakapan.** CPM Haji 2 Rp17,9 rb dan CPM Fikri Rp44,8 rb, tetapi biaya per percakapan keduanya sama-sama ±Rp42,9 rb. Rentang CPM 8× (14,8 rb–118 rb) hanya menghasilkan rentang biaya/percakapan 1,8×.
3. **Nama CS yang sama muncul di banyak akun sekaligus.** Kampanye "Fikri", "Nida", "Tira", "Aini", "Hanif", "Irfan", "Diana" berjalan paralel di akun Fikri, Tira, Haji 2, Haji 4, Nida. Di Haji 2 saja ada **≥50 kampanye ARCHIVED** yang masih menyerap spend dalam 30 hari ini, sementara **9 kampanye ACTIVE di sana spend-nya Rp0**.

---

## 4. Temuan

### T1. Tracking purchase tidak menangkap penjualan
- **Label:** ✓ TERBUKTI (untuk fakta "purchase tidak tercatat"). Apakah ini masalah NYATA atau PELAPORAN masih ? HIPOTESIS.
- **Data point:** 15 akun, Rp294 jt, 7 purchase. Semua kampanye ber-indikator `actions:purchase` yang muncul di sampel (≈60 kampanye di Haji 2, Nida, Haji 4) menunjukkan "Not available".
- **Bukti pendukung:** dataset "Haji" memang punya event Purchase, tetapi match key-nya hanya IP/UA/fbp. Closing umroh/haji terjadi di WhatsApp dan tidak dikirim balik ke Meta.
- **Bukti pembantah:** 7 purchase tetap tercatat (Fikri 2, Nida 2, Tira 1, Haji 4 1, Alif 1). Artinya jalur purchase bisa berfungsi, hanya volumenya nyaris nol.
- **Implikasi:** kampanye yang dioptimasi ke *purchase* tidak punya sinyal belajar. Algoritma berjalan "buta".

### T2. Biaya per percakapan stabil di Rp35–62 rb
- **Label:** ✓ TERBUKTI
- **Data point:** 12 akun, 5.605 percakapan. 10 dari 12 akun (83%) berada di Rp38–55 rb.
- **Bukti pendukung:** median Rp44,4 rb, blended Rp42,7 rb.
- **Bukti pembantah:** outlier AIni (Rp61,7 rb) dan Elharamain Haji (Rp34,5 rb).
- **Implikasi:** **Rp42–45 rb** bisa dipakai sebagai *baseline* internal. Kampanye percakapan di atas ±Rp55 rb masuk kandidat evaluasi.

### T3. CPM rendah ≠ percakapan murah
- **Label:** ~ PROBABLE
- **Data point:** 12 akun.
- **Bukti pendukung:** Haji 2 (CPM 17,9 rb → 42,9 rb/percakapan) ≈ Fikri (CPM 44,8 rb → 42,9 rb) ≈ Tira (CPM 41,6 rb → 43,9 rb).
- **Bukti pembantah:** Elharamain Haji punya CPM terendah (14,8 rb) sekaligus biaya/percakapan termurah (34,5 rb).
- **Implikasi:** jangan menilai akun/kampanye dari CPM atau CTR. Nilailah dari biaya per percakapan dan, lebih jauh lagi, dari kualitas lead.

### T4. Kualitas percakapan (rasio lead/percakapan) sangat bervariasi
- **Label:** ? HIPOTESIS (lead di beberapa akun < 10; definisi event `lead` belum dikonfirmasi: apakah dari CS/Balas Otomatis?)
- **Data point:** 634 lead, 5.605 percakapan.
- **Bukti:** rasionya Irfan 19%, Tira 16%, Fikri 16%, Haji 2 8%, AIni 7%, Haji **4%**, Diana 1%.
- **Implikasi:** akun Haji yang "termurah per percakapan" justru punya biaya per lead **Rp864 rb**, 3× lebih mahal dari Fikri/Tira (Rp276 rb). **Jika event `lead` = prospek valid, peringkat akun terbalik.**

### T5. Campaign churn tinggi: kampanye terus dibuat ulang lalu di-archive
- **Label:** ✓ TERBUKTI (untuk struktur). Dampaknya ke CPR masih ? HIPOTESIS.
- **Data point:** Haji 2 memiliki ≥50 kampanye ARCHIVED dengan spend 30 hari (penamaan "Fikri 1/2/3", "Nida 1/2/5", "Tira 1/2/3"). Pola serupa ada di Nida dan Haji 4.
- **Bukti pendukung:** mayoritas kampanye hanya menyerap Rp0,3–3,7 jt sebelum di-archive. Angka itu terlalu kecil untuk keluar dari *learning phase*.
- **Bukti pembantah:** DATA TIDAK TERSEDIA. Tren sebelum/sesudah tidak bisa dibandingkan karena tool trend mengembalikan "No performance trend data".

### T6. Satu CS beriklan di banyak akun sekaligus → risiko kanibalisasi
- **Label:** ? HIPOTESIS (overlap audiens belum diukur)
- **Data point:** 7 nama CS masing-masing muncul di 3–5 akun. Targeting-nya serupa (LLA 1%, Interest, "Umroh").
- **Implikasi:** akun-akun milik grup yang sama bisa saling berebut lelang untuk audiens yang sama.

### T7. ±10% budget lari ke objective non-percakapan
- **Label:** ~ PROBABLE
- **Data point:** akun Nida (Rp29,7 jt, CTR 0,53%). Di 15 kampanye teratasnya: Engagement ±Rp14,5 jt, Traffic/profile visit ±Rp4,6 jt, Awareness ±Rp1,3 jt. Ditambah akun Elharamain Wisata (Rp1,5 jt, reach, CTR 0,05%).
- **Bukti pembantah:** kampanye engagement "Tira" di akun Nida tetap menghasilkan 39 lead.
- **Implikasi:** ±Rp21 jt/bulan tidak menghasilkan percakapan yang bisa diukur.

### T8. Tidak ada kelelahan audiens
- **Label:** ✓ TERBUKTI
- **Data point:** 15/15 akun memiliki frequency 1,22–1,78.
- **Implikasi:** masalah akun ini **bukan** fatigue. Refresh kreatif bukan prioritas #1.

### T9. Kualitas sinyal pixel sedang (EMQ ±6)
- **Label:** ✓ TERBUKTI (2 dari 2 dataset yang dicek)
- **Data point:** dataset "Haji" (1313353029656348) dan "Admin" (2017127175571851).
- **Bukti:** email/nama ≤16%, tanpa nomor telepon. Pada dataset "Admin", AddToCart selalu muncul dalam kelipatan 2 per jam. **? HIPOTESIS: ada double-fire.**
- **Implikasi:** kampanye "haji" (Haji 4) yang dioptimasi ke AddToCart menunjukkan 510 ATC @Rp4,2 rb dengan CTR 8,94%. Angka ini kemungkinan besar **menggelembung** dan bukan sinyal niat beli.

### T10. Opportunity Score Meta (akun Haji 2): 94/100
- Rekomendasi tunggal: "Optimalkan iklan Anda secara otomatis dengan 3 penyempurnaan materi iklan Advantage+ lagi" (+6 poin).
- [Terapkan di Ads Manager](https://adsmanager.facebook.com/adsmanager/manage/accounts?act=1032468828178224&breakdown_regrouping=1&recommendation_hash_string=0e1b7e5745590db2091652e3bd1d1745&recommendation_type=aplusc_standard_enhancements_bundle&is_mfr_model_shown_by_default=1&referral_source=marketing_api&nav_source=ads_mcp)
- Ini benchmark Meta, **bukan** acuan utama.

---

## 5. Keputusan / Tindakan (hanya dari TERBUKTI / PROBABLE)

> Tidak ada yang dieksekusi. Semua tindakan menunggu konfirmasi.

| # | Tindakan | Dasar | Prioritas |
|---|---|---|---|
| 1 | **Berhenti mengoptimasi ke `purchase`** di kampanye yang tidak punya sinyal purchase (Haji 2, Nida, Haji 4). Pakai optimasi *Conversations* sampai sinyal purchase ada | T1 ✓ | 🔴 |
| 2 | **Kirim purchase offline/CAPI dari CRM/WhatsApp** (DP umroh/haji) ke dataset "Haji", termasuk nomor telepon ter-hash. Target ≥30 purchase/bulan agar bisa dipakai optimasi | T1 ✓, T9 ✓ | 🔴 |
| 3 | **Tetapkan baseline biaya per percakapan Rp42–45 rb.** Kampanye > Rp55 rb setelah ≥Rp1 jt spend masuk review | T2 ✓ | 🟠 |
| 4 | **Hentikan pola buat-ulang/archive kampanye.** Konsolidasikan per CS menjadi 1 kampanye percakapan yang dibiarkan belajar ≥7 hari / ≥50 percakapan | T5 ✓ | 🟠 |
| 5 | **Alihkan ±Rp21 jt/bulan** dari Engagement/Traffic/Awareness (akun Nida, Elharamain Wisata) ke kampanye percakapan | T7 ~ | 🟠 |
| 6 | Jangan menilai kinerja dari CPM/CTR. KPI utama: biaya per percakapan, lalu biaya per lead valid | T3 ~ | 🟡 |
| 7 | Tunda refresh kreatif besar-besaran karena frequency masih rendah | T8 ✓ | 🟡 |

## 6. Rencana Tes (dari HIPOTESIS)

| Hipotesis | Tes | Metrik lulus |
|---|---|---|
| T4: rasio lead/percakapan = kualitas | Konfirmasi definisi event `lead`. Hitung manual lead valid per akun selama 14 hari | Peringkat akun berdasarkan biaya/lead valid |
| T6: kanibalisasi antar-akun | Gabungkan kampanye 1 CS ke 1 akun selama 14 hari, bandingkan CPM & biaya/percakapan | CPM turun ≥10% tanpa kehilangan volume |
| T9: AddToCart double-fire | Cek Events Manager → Test Events untuk pixel "Admin" & "Haji" | 1 klik = 1 ATC |
| T1: NYATA vs PELAPORAN | Rekonsiliasi jumlah jamaah daftar/DP bulan ini vs 7 purchase Meta | Selisih besar → PELAPORAN |

## 7. DATA TIDAK TERSEDIA (perlu dilengkapi)

- Blueprint / DNA akun (belum ada di repo)
- Revenue, margin, dan penjualan aktual (DP/pelunasan) untuk menghitung ROAS dan profit riil
- Definisi event `lead` (sumber: Balas Otomatis / CS / form?)
- Attribution setting untuk 14 akun lain (baru 1 akun yang dicek)
- Tren periode-ke-periode (tool trend: "No performance trend data")
- 8 akun dengan MCP belum aktif dan 8 akun CLOSED/DISABLED
- Breakdown per kreatif/ad (belum ditarik; lanjutkan dengan skill `meta-ads-creative-audience-discovery`)
