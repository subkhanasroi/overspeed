# JARVIS Overspeed Dashboard

## Menjalankan
Ekstrak seluruh ZIP ke satu folder, lalu buka `index.html` di Chrome atau Edge. Simpan folder `vendor` di samping file HTML. Semua dependensi sudah disertakan; tidak perlu internet atau npm untuk menjalankan.

Jika browser membatasi penyimpanan pada file lokal, jalankan `python -m http.server 8000` di folder hasil ekstraksi lalu buka http://localhost:8000. Alternatif: gunakan VS Code Live Server atau letakkan folder pada web server.

Upload file Excel melalui tombol Upload Excel. Dashboard pertama kali dibuka kosong. Upload baru menggantikan data sebelumnya. Data tersimpan hanya pada browser/perangkat yang sama (IndexedDB), sehingga dapat dibaca halaman `detail.html`. Reset Data menghapus data upload dari browser ini.

## Aturan pengolahan
- Pilih otomatis sheet data mentah yang memiliki kolom Device Name, Fleet, dan Start Speed. Sheet pivot dan sheet hasil olahan dilewati.
- Hanya Fleet yang diawali HD atau DT yang dihitung. Contoh: `HD JALAN FALCON` dan `DT OUTPIT`. Nama perangkat SRT, DA, DB, dan H57 tetap diterima jika Fleet-nya HD/DT. Unit lain dikeluarkan.
- Speed Correction = Start Speed − 3 km/jam, dibulatkan dua desimal; Start Speed asli tetap ditampilkan.
- Overspeed menggunakan perbandingan ketat >30 atau >40, bukan >=. Nilai tepat 30/40 tidak termasuk masing-masing ambang.
- Baris Start Speed kosong/tidak valid dikeluarkan dan jumlahnya ditampilkan. Angka desimal titik/koma dan akhiran km/h didukung.
- Total data, maksimum, komposisi jenis unit, dan persentase menggunakan seluruh geofence HD/DT valid. Top 10 persimpangan hanya mencakup nama geofence mengandung `Simpang`.
- Persentase = jumlah record overspeed / jumlah record HD/DT valid. Ini bukan persentase semua perjalanan atau tingkat kepatuhan kendaraan.
- Record tidak dideduplikasi. Re-upload dipertahankan seperti sumber. Source Row dan nama sheet tersedia untuk penelusuran.
- Begin Time ditampilkan mengikuti sumber tanpa konversi zona waktu.
- Detail hanya menampilkan record melebihi ambang aktif, 50 baris per halaman, dengan pencarian unit/fleet/geofence.
- Upload maksimum 50 MB. Data mentah tidak disertakan dalam paket aplikasi.

## Struktur
index.html — dashboard
 detail.html — halaman detail terpisah
styles.css — CSS Tailwind hasil kompilasi + tema
input.css — sumber Tailwind
app.js — interaksi, grafik, tabel, penyimpanan lokal
core.js — pembacaan dan penghitungan data
vendor/ — Chart.js dan SheetJS untuk penggunaan offline

## Mengubah CSS
Untuk pengembangan, install dependensi `npm install`, lalu:
`npx @tailwindcss/cli -i input.css -o styles.css --minify`

## Hasil validasi dengan lampiran
File Alarm_20261006230558.xlsx: 8.381 record sumber, 8.364 HD/DT valid, 17 unit lain dikeluarkan, 0 Start Speed tidak valid.
>30: 8.364 record. >40: 4.596 record (HD 3.794, DT 802). Maksimum Speed Correction: 76,14 km/jam.
File ini memiliki jumlah record berbeda dari workbook overview contoh; hasil dihitung dari upload, tidak mengambil angka contoh.
# overspeed
