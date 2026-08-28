# Product Requirement Document (PRD)

## DompetKu Pro - Cloud Database & AI Financial Assistant

---

## 1. Executive Summary (Ringkasan Eksekutif)

**DompetKu Pro** adalah aplikasi manajemen keuangan pribadi berbasis web (_Single Page Application_) yang dirancang untuk mencatat, melacak, dan menganalisis arus kas masuk, pengeluaran, serta investasi secara _real-time_.

Aplikasi ini mengintegrasikan **Cloud Database (Firebase Firestore)** untuk penyimpanan data permanen yang aman dan dapat diakses lintas perangkat, serta **Asisten AI Chat Bot** yang mendukung pencatatan transaksi otomatis menggunakan perintah teks alami (_natural language commands_).

---

## 2. Product Goals (Tujuan Produk)

- **Pencatatan Tanpa Hambatan:** Mempercepat input data keuangan melalui perintah teks instan pada _chat bot_ tanpa harus mengisi formulir manual yang kaku.
- **Keamanan & Konsistensi Data:** Menghilangkan risiko kehilangan data akibat _refresh_ browser atau pergantian perangkat dengan memanfaatkan penyimpanan _cloud_ berbasis NoSQL.
- **Pelaporan Cepat:** Memfasilitasi rekapitulasi keuangan otomatis yang terhubung langsung ke platform perpesanan WhatsApp (Nomor Tujuan: `083161854892`).

---

## 3. Target User & Use Cases (Pengguna & Kasus Penggunaan)

- **Freelancer / Software Developer / Profesional Mandiri:** Membutuhkan pencatatan keuangan yang cepat di tengah kesibukan kerja tanpa antarmuka yang rumit.
- **Kasus Penggunaan Utama:**
  - Pengguna ingin mencatat pendapatan proyek secara instan melalui chat.
  - Pengguna ingin memantau sisa saldo bersih (_Net Balance_) secara _real-time_.
  - Pengguna ingin mengirimkan laporan rekapitulasi keuangan ke WhatsApp partner kerja atau arsip pribadi.

---

## 4. Key Features & Functional Requirements (Fitur Utama)

### 4.1. Dashboard & Ringkasan Statistik

- Menampilkan kartu total secara terpisah untuk:
  - **Total Pemasukan (Masuk)**
  - **Total Pengeluaran (Keluar)**
  - **Total Investasi (Invest)**
- Menghitung **Saldo Bersih (Sisa Kas)** secara otomatis menggunakan rumus:
  $$\text{Saldo Bersih} = \text{Total Masuk} - (\text{Total Keluar} + \text{Total Invest})$$

### 4.2. Cloud Database Integration (Firebase Firestore)

- Sinkronisasi data secara _real-time_ menggunakan **Firebase SDK Modular (v10)**.
- Setiap penambahan atau penghapusan data langsung memperbarui koleksi `transactions` di _cloud_ tanpa memerlukan _page reload_.

### 4.3. Manual Transaction Form

- Formulir input tradisional yang mencakup:
  - Dropdown Tipe Transaksi (_Masuk, Keluar, Invest_).
  - Input teks untuk Keterangan/Nama Transaksi.
  - Input angka untuk Jumlah nominal Rupiah (min. > 0).

### 4.4. AI Smart Auto-Command Bot

- Fitur _chat assistant_ di sisi kanan antarmuka yang mampu mendeteksi pola kalimat perintah pengguna.
- Mengekstrak kata kunci tipe transaksi dan nominal angka (mendukung satuan _ribu_ dan _juta_) untuk langsung dieksekusi dan disimpan ke database cloud.

### 4.5. WhatsApp Direct Reporting

- Tombol aksi instan untuk menyusun rekapitulasi total keuangan ke dalam format teks rapi berformat Markdown WhatsApp, lalu membuka URL _direct message_ ke nomor **`6283161854892`**.

---

## 5. Technical Requirements & Stack (Kebutuhan Teknis)

- **Frontend Framework:** HTML5, Modern JavaScript (ES6+), Tailwind CSS v4 (via CDN).
- **UI Components & Icons:** FontAwesome 6.4.0.
- **Database & Backend:** Google Cloud Firestore (NoSQL).
- **Security Rules (Firestore):**
  ```javascript
  rules_version = '2';
  service cloud.firestore {
    match /databases/{database}/documents {
      match /transactions/{document=**} {
        allow read, write: if true;
      }
    }
  }
  ```
