# UiPath REFramework (Modern) – Otomatisasi Purchase Order

## Ringkasan
Proyek ini membangun otomatisasi end-to-end menggunakan UiPath REFramework (Modern) dengan Cloud Orchestrator.  
Workflow memproses Purchase Order (PO) dari Orchestrator Queue, melakukan validasi data, mengirim email ke vendor, memperbarui status PO di aplikasi web, serta mencatat hasil ke file ringkasan.  

Fokus utama:
- Struktur REFramework yang robust
- Exception handling (Business vs System)
- Reliabilitas transaksi

---

## Skenario
Perusahaan memiliki daftar Purchase Order (PO) yang diunggah ke Orchestrator Queue: `Queue-PO`.  
Untuk setiap transaksi:
1. Validasi data PO (email vendor, jumlah, tanggal jatuh tempo).
2. Kirim email ke vendor menggunakan template HTML.
3. Update status PO di aplikasi web mock.
4. Catat hasil ke file ringkasan.

---

## Struktur Proyek
Automation mengikuti REFramework (Modern):
- Init → Load `Config.xlsx`, Orchestrator Assets, inisialisasi aplikasi.
- GetTransactionData → Ambil item dari `Queue-PO` (Unique Reference aktif).
- Process → Jalankan langkah transaksi (validasi, email, update web).
- End → Tutup aplikasi, hasilkan file output.

---

## File Input
- `Config.xlsx` → Settings & Orchestrator assets  
- `Queue-PO_Sample.csv` → Data sampel untuk queue  
- `WorkItems_Mock.html` → Web mock modern  
- `email_template.html` → Template email HTML  
- `RiskMatrix.xlsx` → Skeleton dokumen risiko  

---

## Detail Transaksi
- Validasi  
  - Email vendor sesuai regex  
  - Amount > 0  
  - Due Date valid  

- Proses  
  - Kirim email via Outlook menggunakan `email_template.html`  
  - Update status PO di `WorkItems_Mock.html` (browser Edge)  
  - Lewati item yang sudah berstatus Processed  

---

## Exception Handling
- BusinessException → Data tidak valid → Tidak retry, tandai gagal  
- SystemException → Error teknis (mis. selector gagal) → Retry maksimal 2 kali  
- Screenshot → Diambil hanya saat SystemException jika flag aktif di `Config.xlsx`  

---

## Output
Disimpan di folder Output:
- `RunSummary.csv` → Jumlah transaksi sukses vs gagal  
- `ErrorDetails.csv` → Detail error  

---

## Deliverables
- Proyek UiPath (`.xaml`, `project.json`)  
- File hasil (`RunSummary.csv`, `ErrorDetails.csv`)  
- Dokumentasi:  
  - `README.md` (file ini)  
  - `ChangeLog.md`  
  - `RiskMatrix.xlsx`  
  - `TestPlan.docx`  
- Opsional: Video demo ≤ 5 menit  

---

## Cara Menjalankan
1. Upload data sampel (`Queue-PO_Sample.csv`) ke Orchestrator Queue `Queue-PO`.  
2. Konfigurasi aset dan setting di `Config.xlsx` serta Orchestrator Assets.  
3. Jalankan `Main.xaml` di UiPath Studio/Assistant.  
4. Periksa folder Output untuk hasil eksekusi.  

---

## Catatan
- Pastikan Outlook sudah terkonfigurasi untuk pengiriman email.  
- Gunakan browser Edge untuk membuka `WorkItems_Mock.html`.  
- Jangan ubah pengaturan Unique Reference pada Queue `Queue-PO`.  