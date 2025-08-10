# 📊 Hotel Booking Demand Prediction

Proyek ini bertujuan untuk memprediksi kemungkinan pembatalan pemesanan hotel berdasarkan data historis booking.  
Model **Logistic Regression** digunakan untuk membantu pihak hotel dalam mengantisipasi pembatalan dan mengambil langkah strategis.

---

## 1. 🏨 Business Problem & Data Understanding

Sekitar **30%** pemesanan hotel dibatalkan, yang mengakibatkan:
- Kerugian pendapatan.
- Biaya operasional tambahan.
- Penurunan tingkat okupansi.

**Stakeholder pengguna model:**
- **Tim Reservasi** → Mengantisipasi pembatalan sejak awal booking.
- **Tim Marketing** → Menyesuaikan promosi bagi pelanggan berisiko tinggi membatalkan.
- **Manajemen** → Menyusun kebijakan deposit yang lebih efektif.

**Target Prediksi:**  
`is_canceled` → 1 (batal) atau 0 (tidak batal).

**Fitur dataset yang digunakan:**
| Kolom | Tipe Data | Deskripsi |
| -- | -- | -- |
| country | object | Negara asal booking |
| market_segment | object | Segmen pasar |
| previous_cancellations | integer | Riwayat pembatalan sebelumnya |
| booking_changes | integer | Jumlah perubahan booking |
| deposit_type | object | Jenis deposit |
| days_in_waiting_list | integer | Lama booking di waiting list |
| customer_type | object | Tipe pelanggan |
| reserved_room_type | object | Kode tipe kamar |
| required_car_parking_space | integer | Kapasitas parkir |
| total_of_special_request | integer | Jumlah permintaan khusus |
| is_canceled | integer | Label: 1 (batal) / 0 (tidak batal) |

---

## 2. 🧹 Data Cleaning, Feature Selection & Feature Engineering

### Langkah-langkah:
1. **Melihat distribusi pembatalan booking** dengan visualisasi `countplot`.
2. **Data Cleaning**:
   - Menghapus data dengan `market_segment = 'Undefined'`.
3. **Feature Selection**:
   - Menggunakan kolom: `market_segment`, `previous_cancellations`, `booking_changes`, `deposit_type`, `is_canceled`.
4. **Feature Engineering**:
   - Mengelompokkan `market_segment` → **Online** (1) atau **Offline** (0).
   - Menggabungkan `previous_cancellations` & `booking_changes` → `booking_modifications`.
   - Mapping `deposit_type`: `No Deposit`=0, `Non Refund`=1, `Refundable`=2.

**Hasil Korelasi:**
- `market_segment` vs `is_canceled` → hampir 0 (tidak signifikan).
- `deposit_type` vs `is_canceled` → 0.39 (positif moderat).
- `booking_modifications` vs `is_canceled` → 0.06 (sangat rendah).

---

## 3. 🤖 Analytics (Algorithm & Evaluation Metrics)

**Algoritma:** Logistic Regression  
**Fitur input:** `market_segment`, `deposit_type`  
**Label:** `is_canceled`

### Evaluasi Model:
- **Accuracy:** 75%
- **Precision:**
  - Kelas 0 (tidak batal): 72%
  - Kelas 1 (batal): 98%
- **Recall:**
  - Kelas 0: 1.00 (sempurna)
  - Kelas 1: 0.33 (rendah)
- **F1-score:**
  - Kelas 0: 0.84
  - Kelas 1: 0.49

**Kesimpulan Evaluasi:**
- Model **sangat baik** mendeteksi booking **tidak batal**.
- Model **kurang baik** mendeteksi booking **batal** (recall rendah).
- Prediksi batal memiliki tingkat kepastian tinggi (precision 98%).

---

## 4. 📌 Conclusion & Recommendation

### Insight:
- Recall tinggi di kelas 0 → hampir semua booking yang tidak batal terdeteksi dengan benar.
- Recall rendah di kelas 1 → banyak booking batal tidak terdeteksi.
- Tingkat kepastian prediksi batal sangat tinggi (precision 98%).

### Rekomendasi:
1. **Gunakan prediksi model untuk strategi marketing**:  
   Kirim penawaran atau follow-up ke pelanggan yang diprediksi akan batal.
2. **Pertimbangkan penyesuaian kebijakan deposit**:  
   Tingkatkan jumlah deposit pada pelanggan yang berisiko tinggi membatalkan.
3. **Perbaikan model**:
   - Tambah fitur lain seperti `lead_time`, `total_of_special_request`.
   - Gunakan algoritma yang lebih kompleks seperti Random Forest atau XGBoost.
   - Atasi imbalance class dengan **oversampling** atau **SMOTE**.


