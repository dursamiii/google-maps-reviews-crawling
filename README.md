# google-maps-reviews-crawling

Crawl ulasan (reviews) dari Google Maps pakai Playwright, lalu simpan ke CSV. Source utama ada di `crawling-gmaps-main.ipynb`.

## Requirements
- Python 3.9+ (disarankan)
- Jupyter Notebook / JupyterLab
- Browser Chromium untuk Playwright

## Install
Jalankan cell pertama di notebook:
```bash
%pip install playwright pandas
!playwright install chromium
```

## Cara pakai (via notebook)
Buka dan jalankan `crawling-gmaps-main.ipynb` dari atas ke bawah. Konfigurasi ada di cell variabel:

```python
GMAPS_URL = "https://www.google.com/maps/place/..."
TARGET_REVIEWS = 5000
CSV_PATH = "mercure.csv"
HEADLESS = False

# 0 = Paling relevan
# 1 = Terbaru
# 2 = Rating tertinggi
# 3 = Rating terendah
SORT_MODE = 2
```

Lalu jalankan crawling dan simpan ke CSV (contoh yang dipakai di notebook):
```python
new_reviews = await crawl_reviews(GMAPS_URL, TARGET_REVIEWS, existing_ids, sort_mode=SORT_MODE)
```

## Output
CSV akan tersimpan di `CSV_PATH` dengan kolom:
- `review_id` (unik per review)
- `rating` (int 1–5, bisa `None` kalau gagal kebaca)
- `review` (teks ulasan)

## Resume mode (lanjut dari CSV)
Notebook otomatis “resume” kalau file `CSV_PATH` sudah ada:
- membaca `review_id` lama → skip yang sudah pernah diambil
- append review baru ke CSV yang sama

## Notes
- `HEADLESS=False` memudahkan debugging (browser kebuka). Kalau mau jalan di background, set `True`.
- Google Maps kadang menampilkan UI Bahasa Indonesia/Inggris; selector di notebook sudah coba handle keduanya.
- Kalau kena CAPTCHA / diblok, coba kurangi `TARGET_REVIEWS`, jalankan headful (`HEADLESS=False`), atau ulang beberapa saat kemudian.
