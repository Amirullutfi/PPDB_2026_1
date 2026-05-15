# Product Requirements Document (PRD) & Struktur Proyek WEB-PPDB-KEREN

Dokumen ini berisi penjelasan lengkap mengenai struktur direktori (folder) dan *Product Requirements Document* (PRD) untuk proyek **WEB-PPDB-KEREN**.

---

## 📂 1. Struktur Folder Proyek

Proyek ini menggunakan arsitektur modern berbasis **React** (Frontend) dengan **Google Apps Script & Google Sheets** (Backend/Database). Berikut adalah penjelasan struktur foldernya:

```text
WEB-PPDB-KEREN/
│
├── gas/                     # Backend Google Apps Script
│   └── code.gs              # Script utama yang menghubungkan React dengan Google Sheets (berjalan sebagai REST API).
│
├── src/                     # Source code utama Frontend (React)
│   ├── components/          # Komponen UI yang dapat digunakan kembali (reusable)
│   │   ├── Footer.tsx       # Bagian bawah (footer) website
│   │   ├── MapPicker.tsx    # Komponen peta untuk memilih lokasi (menggunakan Leaflet)
│   │   └── Navbar.tsx       # Navigasi utama (header) website
│   │
│   ├── context/             # React Context untuk manajemen state global (misal: Autentikasi Admin)
│   ├── lib/                 # Konfigurasi atau inisialisasi library eksternal
│   ├── pages/               # Halaman-halaman utama (Routing)
│   │   ├── AdminDashboard.tsx # Halaman panel kontrol Admin (mengelola data, export PDF/Excel)
│   │   ├── AdminLogin.tsx     # Halaman login untuk Admin
│   │   ├── CheckStatus.tsx    # Halaman bagi pendaftar untuk mengecek status kelulusan
│   │   ├── Guide.tsx          # Halaman panduan tata cara pendaftaran
│   │   ├── Home.tsx           # Halaman utama (Landing Page)
│   │   └── RegistrationForm.tsx # Halaman formulir pendaftaran siswa baru
│   │
│   ├── services/            # Modul untuk komunikasi API (mengirim/menerima data dari Google Apps Script)
│   ├── utils/               # Fungsi-fungsi bantuan (helper functions)
│   ├── App.tsx              # Komponen utama yang mengatur Routing (React Router)
│   ├── main.tsx             # Entry point aplikasi React
│   └── index.css            # File CSS utama (Konfigurasi Tailwind CSS)
│
├── GAS_SETUP.md             # Panduan cara setup database Google Sheets dan Apps Script
├── package.json             # Daftar dependensi library (React, Tailwind, jspdf, leaflet, dll) dan script build
└── vite.config.ts           # Konfigurasi Vite (Module Bundler & Development Server)
```

---

## 📑 2. Product Requirements Document (PRD)

### 2.1. Tujuan Produk (Product Objective)
Membangun platform sistem **Penerimaan Peserta Didik Baru (PPDB) Online** yang interaktif, modern, dan gratis dalam hal infrastruktur database (karena memanfaatkan ekosistem Google Sheets). Sistem ini dirancang untuk memudahkan calon siswa mendaftar secara online dan memudahkan pihak sekolah (Admin) dalam mengelola data pendaftar tanpa perlu menyewa server database (MySQL/PostgreSQL) yang mahal.

### 2.2. Target Pengguna (User Personas)
1. **Calon Siswa / Orang Tua Siswa**
   - Ingin mendapatkan informasi mengenai sekolah dan prosedur PPDB.
   - Ingin mengisi formulir pendaftaran secara online.
   - Ingin mengetahui status pendaftaran (Diterima / Ditolak / Diproses).
2. **Admin Sekolah / Panitia PPDB**
   - Ingin memantau data pendaftar secara real-time.
   - Ingin mengubah status pendaftar (memvalidasi kelulusan).
   - Ingin mengunduh laporan pendaftaran dalam bentuk Excel atau PDF.
   - Ingin mengkonfigurasi sistem PPDB (seperti membuka/menutup jalur pendaftaran).

### 2.3. Fitur Utama (Core Features)

**A. Fitur Publik (Calon Siswa)**
- **Landing Page (Beranda)**: Menampilkan informasi profil singkat sekolah dan tombol akses cepat.
- **Panduan Pendaftaran**: Menjelaskan langkah-langkah, syarat, dan ketentuan mendaftar.
- **Formulir Pendaftaran Interaktif**:
  - Input data diri, asal sekolah, dan orang tua.
  - Integrasi **MapPicker** untuk memilih lokasi alamat pendaftar menggunakan peta interaktif (Leaflet).
  - Sistem akan otomatis men-generate **Nomor Pendaftaran** (misal: PPDB-2024-001) setelah berhasil disubmit.
- **Cek Status Kelulusan**: Pendaftar dapat memasukkan Nomor Pendaftaran untuk melihat status saat ini (Proses, Diterima, atau Ditolak beserta alasannya).

**B. Fitur Admin (Panitia)**
- **Autentikasi**: Login khusus admin.
- **Dashboard Manajemen Pendaftar**:
  - Menampilkan seluruh data pendaftar dari Google Sheets.
  - Mengubah status pendaftar menjadi Diterima atau Ditolak (bisa melampirkan alasan penolakan).
- **Export Data**: Kemampuan mengekspor data pendaftar ke file format **.xlsx** (Excel) dan **.pdf** untuk laporan.
- **Pengaturan Sistem (Settings)**:
  - Mengubah Nama Sekolah.
  - Mengubah Tahun Pendaftaran.
  - Mengontrol Status Pendaftaran (Buka / Tutup).

### 2.4. Arsitektur & Technology Stack
- **Frontend Framework**: React.js (v19) dengan TypeScript.
- **Build Tool**: Vite.js (untuk performa pengembangan yang sangat cepat).
- **Styling**: Tailwind CSS (v4) untuk UI/UX yang modern, responsif, dan *clean*.
- **Animasi & Interaksi**: Framer Motion (untuk animasi transisi UI yang halus).
- **Routing**: React Router DOM (v7).
- **Maps / Geolocation**: Leaflet & React-Leaflet.
- **Export Utilities**: `jspdf`, `jspdf-autotable` (PDF) dan `xlsx` (Excel).
- **Backend / Database**: 
  - **Google Apps Script** (`doPost` dan `doGet`) berfungsi sebagai REST API.
  - **Google Sheets** berfungsi sebagai Database (memiliki *sheet* `Pendaftar` dan `Pengaturan`).

### 2.5. Batasan dan Asumsi (Constraints & Assumptions)
- Aplikasi bergantung pada layanan Google (Apps Script & Sheets). Kuota eksekusi mengikuti batas limit standar akun Google gratis.
- Foto atau berkas pendaftar (jika ada) diasumsikan dikelola dengan link Google Drive atau layanan cloud lainnya jika fitur ini dikembangkan lebih lanjut.
- Tidak ada database relasional tradisional (SQL), melainkan berbasis flat-file tabel (Spreadsheet).
