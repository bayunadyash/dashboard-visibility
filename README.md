# Skintific — Visibility Compliance Dashboard

Dashboard laporan **visibility compliance** (Planogram · OSA · POSM) per region, dibuat dari laporan Excel bulanan, lengkap dengan perbandingan *month-over-month*.

## 📊 Isi Dashboard

- **5 KPI Card** — Stores Submitted, Overall Visibility, Planogram Compliance, OSA (Availability), POSM Take-Out Compliance — masing-masing dengan ring gauge dan **delta vs bulan sebelumnya**
- **4 Filter** — Reporting Period, Region, AOM, dan **TL (Team Leader)**; filter TL menyisir semua KPI/chart/tabel ke 1 TL (region otomatis terkunci, OSA jadi N/A karena data OSA hanya per region)
- **Compliance by Region** — bar chart 14 region (bisa ganti metrik: Overall / Planogram / OSA / POSM)
- **Issue Breakdown** — donut: Stores OOS, Incorrect POSM, Incorrect NPD Zone, Incorrect Variant/Product Arrangement, No Foto/Foto Blur
- **Compliance Trend** — line chart Overall · Planogram · OSA · POSM per bulan (otomatis nambah saat bulan baru ditambahkan)
- **Region Performance Scorecard** — tabel per region + Grand Total + kolom Δ vs bulan lalu
- **TL Performance Overview** — ranking semua TL (stores, submission, planogram, POSM take-out, overall, Δ vs bulan lalu) + strip Best TL 🏆 / Bottom TL ⚠️ / Average / Total Stores; baris terbaik (hijau) & terbawah (merah)
- **Planogram vs OSA vs POSM by Region** — grouped bar
- **Regions Needing Attention** — 5 region terbawah + main issue
- **Key Insight & Next Action** — dihasilkan otomatis dari data (termasuk saran coaching untuk bottom TL)

## 🗂 Struktur Data

Semua data ada di dalam `index.html`:

**1. Data region** — objek `RAW`:

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

**2. Data per TL** — objek `TL_RAW` (dari sheet Visibility per TL):

```js
const TL_RAW = {
  "2026-09": [ T("NAMA TL","REGION","AOM", stores, sub, TRUE, FALSE, npdZone, variant, posmBad, noFoto, posmSub, takeout), ... ],
  "2026-10": [ ... ],
};
```

**3. No Foto per region** — objek `NOFOTO` (rollup dari kolom No Foto/Foto Blur di sheet TL):

```js
const NOFOTO = { "2026-09": {"BALI NUSA TENGGARA":1, ...}, "2026-10": {...} };
```

Catatan: OSA hanya ada di laporan regional (tidak per TL), jadi saat filter TL aktif metrik Overall dihitung dari **Planogram + POSM** saja.

### Update bulan berikutnya

1. Tambah satu blok baru di `RAW` (contoh `"2026-11"`) dengan angka dari Excel regional.
2. Tambah satu blok baru di `TL_RAW` + `NOFOTO` dengan angka dari sheet Visibility per TL.
3. Blok terbaru otomatis jadi periode default, bulan sebelumnya otomatis jadi pembanding (delta), dan **Compliance Trend otomatis menambah titik bulan baru**.
4. Dropdown **Reporting Period**, semua KPI, chart, tabel, TL ranking, dan insight ikut terupdate otomatis.

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
