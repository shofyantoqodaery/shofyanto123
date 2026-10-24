# D6. Test Case Design

**Skenario:** Perhitungan denda keterlambatan pembayaran sewa menggunakan fungsi `calculate_late_fee(due_date, paid_date, monthly_rent)`.
**Aturan Bisnis:** Denda dikenakan sebesar Rp 5.000 per hari keterlambatan. Total batas maksimal denda tidak boleh melebihi harga sewa bulanan (`monthly_rent`).

## Tabel Pengujian

| ID | Title | Type | Preconditions | Input / Steps | Expected Result |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-01 | Bayar tepat waktu | Common | Tanggal valid | `due_date`: 10-Okt, `paid_date`: 10-Okt, `rent`: 500000 | Denda Rp 0 |
| TC-02 | Bayar lebih awal | Common | Tanggal valid | `due_date`: 10-Okt, `paid_date`: 08-Okt, `rent`: 500000 | Denda Rp 0 |
| TC-03 | Terlambat 1 hari | Common | Tanggal valid | `due_date`: 10-Okt, `paid_date`: 11-Okt, `rent`: 500000 | Denda Rp 5000 |
| TC-04 | Terlambat 5 hari | Common | Tanggal valid | `due_date`: 10-Okt, `paid_date`: 15-Okt, `rent`: 500000 | Denda Rp 25000 |
| TC-05 | Terlambat 30 hari | Common | Tanggal valid | `due_date`: 10-Okt, `paid_date`: 09-Nov, `rent`: 500000 | Denda Rp 150000 |
| TC-06 | Format tanggal kosong | Edge | Sistem menerima input *null* | `due_date`: "", `paid_date`: 11-Okt, `rent`: 500000 | Pesan Error: "Format tanggal tidak valid" |
| TC-07 | Sewa bernilai negatif | Edge | Input harga salah | `due_date`: 10-Okt, `paid_date`: 15-Okt, `rent`: -500000 | Pesan Error: "Harga sewa tidak bisa negatif" |
| TC-08 | Keterlambatan ekstrem (Melebihi batas) | Edge | Telat sangat lama (>100 hari) | `due_date`: 10-Okt, `paid_date`: 10-Mar (Telat 151 hari), `rent`: 500000 | Denda dibatasi maksimal Rp 500.000 |
| TC-09 | Lewat tahun kabisat (Feb 29) | Edge | Pembayaran melewati 29 Feb | `due_date`: 28-Feb-2024, `paid_date`: 01-Mar-2024, `rent`: 500000 | Terlambat 2 hari, denda Rp 10000 |
| TC-10 | Tanggal bayar mundur | Edge | *Error timezone* / manipulasi | `due_date`: 10-Okt-2026, `paid_date`: 10-Okt-2025, `rent`: 500000 | Denda Rp 0 |

## Penjelasan Pemilihan Edge Cases
Pemilihan kasus ekstrem (edge cases) difokuskan pada antisipasi *input* yang tidak valid seperti format tanggal kosong dan nominal uang sewa negatif yang dapat merusak kalkulasi matematis pada fungsi. Kasus ekstrem seperti tahun kabisat (TC-09) dan manipulasi tanggal bayar yang mundur (TC-10) dipilih untuk memastikan modul waktu bekerja secara akurat terlepas dari anomali kalender atau sistem. Selain itu, TC-08 diuji secara khusus untuk memastikan bahwa aturan bisnis (batas maksimal denda tidak melebihi harga sewa) berjalan dengan baik sehingga sistem tidak membebankan biaya yang tidak logis kepada penyewa.
