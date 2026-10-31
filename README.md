# Skintific — Visibility Compliance Dashboard

Dashboard laporan **visibility compliance** (Planogram · OSA · POSM) per region, dibuat dari laporan Excel bulanan, lengkap dengan perbandingan *month-over-month*.

## 📊 Isi Dashboard

- **5 KPI Card** — Stores Submitted, Overall Visibility, Planogram Compliance, OSA (Availability), POSM Take-Out Compliance — masing-masing dengan ring gauge dan **delta vs bulan sebelumnya**
- **Compliance by Region** — bar chart 14 region (bisa ganti metrik: Overall / Planogram / OSA / POSM)
- **Issue Breakdown** — donut: Stores OOS, Incorrect POSM, Incorrect NPD Zone, Incorrect Product Arrangement
- **Region Performance Scorecard** — tabel per region + Grand Total + kolom Δ vs bulan lalu
- **Planogram vs OSA vs POSM by Region** — grouped bar
- **Regions Needing Attention** — 5 region terbawah + main issue
- **Key Insight & Next Action** — dihasilkan otomatis dari data

## 🗂 Struktur Data

Semua data ada di dalam `index.html` pada objek `RAW`:

```js
const RAW = {
  "2026-09": { label:"September 2026", short:"Sep", plano:[...], osa:[...], posm:[...] },
  "2026-10": { label:"October 2026",   short:"Oct", plano:[...], osa:[...], posm:[...] },
};
```

Helper per baris:

- `P(region, aom, stores, sub, ok, bad, npdZone, prodArr, posmBad)` — planogram
- `O(region, alloc, good, oos)` — OSA
- `M(region, alloc, sub, takeout)` — POSM

### Update bulan berikutnya

1. Tambah satu blok baru di `RAW` (contoh `"2026-11"`) dengan angka dari Excel bulan tersebut.
2. Blok terbaru di `RAW` otomatis jadi periode default, dan bulan sebelumnya otomatis jadi pembanding (delta).
3. Dropdown **Reporting Period**, semua KPI, chart, tabel, dan insight ikut terupdate otomatis.

## 🚀 Deploy di Vercel

1. Import repo ini di [vercel.com/new](https://vercel.com/new)
2. Framework preset: **Other**, Root Directory: `/` (default)
3. Deploy — `index.html` langsung diserve sebagai halaman utama

Catatan: dashboard memuat Chart.js dari CDN (`cdn.jsdelivr.net`), jadi butuh koneksi internet saat dibuka.

## 🖥 Menjalankan Lokal

Buka `index.html` langsung di browser, atau jalankan server statis:

```bash
python3 -m http.server 8000   # atau: ruby -run -e httpd . -p 8000
```
