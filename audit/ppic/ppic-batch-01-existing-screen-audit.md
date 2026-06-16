# PPIC Existing Screen Audit - Batch 01

## Ringkasan Batch
Batch ini berisi observasi awal beberapa screen existing yang digunakan oleh role **PPIC / PIC** pada sistem existing. Audit dilakukan berdasarkan screenshot layar yang terlihat, tanpa akses source code, sehingga temuan dibedakan antara **observasi objektif** dan **asumsi yang masih perlu validasi**.

## Screen yang Terbaca dalam Batch Ini
1. Dashboard PPIC
2. Supplier Raw Material
3. Supplier Packaging
4. Raw Material - Data Raw Material
5. Raw Material - Expired Raw Material

---

# 1. Screen Audit - Dashboard PPIC

## Identitas Screen
- Nama screen: Dashboard
- Module / area: PPIC Dashboard
- Role pengguna: PPIC / PIC
- Tipe screen: dashboard / summary monitoring
- Tujuan screen: memberi ringkasan cepat status sales order, aktivitas, customer utama, dan order terakhir

## Komponen Layar
- Sidebar menu utama
- Search bar di header
- Summary cards: Draft SO, On Processing SO, Processed SO, Total SO
- Grafik status sales order per bulan
- Activity evaluation panel
- Top customers panel
- Last 5 sales orders panel

## Observasi Objektif
- Dashboard menampilkan metrik sales order, bukan hanya material planning.
- Ada keterkaitan kuat antara PPIC dan proses sales order monitoring.
- Sidebar menunjukkan menu yang cukup luas: Dashboard, Supplier, Raw Material, Packaging, Customer, Product, Sales Order, Monitoring Sales Order, Sample, Production Schedule, Report.
- Role yang login tertulis sebagai **Superadmin / Managing - Owner / Login PPIC**.
- Last 5 sales orders menampilkan nomor PO, nama customer, tanggal PO, item produk, dan pembuat.

## Dugaan Logic Bisnis
- PPIC existing kemungkinan memonitor order dari sisi status komersial/operasional sebelum masuk ke perencanaan atau schedule.
- Dashboard dipakai untuk prioritisasi order dan pemantauan aktivitas customer / sample / product.

## Pertanyaan Terbuka
- Apakah angka Draft SO, On Processing SO, dan Processed SO murni status sales order atau juga dipakai sebagai trigger planning PPIC?
- Apakah dashboard ini hanya informatif atau ada klik-through ke detail transaksi?

## Kandidat Requirement Rebuild
- Dashboard PPIC perlu memisahkan metrik komersial dan metrik planning agar lebih fokus.
- Perlu dashboard readiness yang lebih spesifik: material, sample approval, production schedule, shortage, urgent order.

---

# 2. Screen Audit - Supplier Raw Material

## Identitas Screen
- Nama screen: Supplier Raw Material
- Module / area: Supplier
- Role pengguna: PPIC / PIC
- Tipe screen: master / list view
- Tujuan screen: melihat daftar supplier bahan baku

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel daftar supplier
- Kolom terlihat: nomor, nama, alamat, email, no telepon, detail
- Tombol aksi `Detail`

## Observasi Objektif
- Supplier raw material dipisahkan dari supplier packaging.
- Screen ini tampak hanya list + search + detail, belum terlihat tombol create/edit/delete pada screenshot.
- Data supplier berisi identitas dasar dan kontak.

## Dugaan Logic Bisnis
- Supplier master kemungkinan dipakai sebagai referensi pada raw material, purchase request, atau planning comparison.
- PPIC diberi visibilitas ke supplier master walaupun belum tentu sebagai owner perubahan data.

## Pertanyaan Terbuka
- Apakah PPIC hanya bisa view supplier atau juga maintain supplier?
- Apakah data supplier ini dipakai untuk analisa harga, lead time, dan shortage?

## Kandidat Requirement Rebuild
- Master supplier sebaiknya punya klasifikasi supplier type, status aktif, lead time, MOQ, dan item mapping.
- View list supplier existing masih terlalu basic jika dipakai untuk planning decision.

---

# 3. Screen Audit - Supplier Packaging

## Identitas Screen
- Nama screen: Supplier Packaging
- Module / area: Supplier
- Role pengguna: PPIC / PIC
- Tipe screen: master / list view
- Tujuan screen: melihat daftar supplier packaging

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel supplier packaging
- Kolom terlihat: nomor, nama, alamat, email, no telepon, detail
- Tombol aksi `Detail`

## Observasi Objektif
- Supplier packaging dipisah dari supplier raw material secara eksplisit di menu.
- Data yang tampil pada screenshot terlihat lebih sedikit dibanding raw material supplier.
- Struktur list serupa dengan supplier raw material.

## Dugaan Logic Bisnis
- Sistem existing membedakan secara tegas kebutuhan sourcing packaging dan bahan baku.
- Ini mengarah ke pemisahan domain item dalam master data existing.

## Pertanyaan Terbuka
- Apakah supplier packaging punya logic approval berbeda?
- Apakah packaging supplier terhubung dengan desain / artwork / material spec?

## Kandidat Requirement Rebuild
- Packaging supplier sebaiknya terhubung dengan approval spec/desain dan histori supplier per item packaging.
- Jika tetap dipisah, perlu alasan bisnis yang jelas; kalau tidak, cukup satu master supplier dengan klasifikasi type.

---

# 4. Screen Audit - Raw Material / Data Raw Material

## Identitas Screen
- Nama screen: Data Raw Material
- Module / area: Raw Material
- Role pengguna: PPIC / PIC
- Tipe screen: transaction-monitoring / master-list hybrid
- Tujuan screen: memantau stok dan data bahan baku

## Komponen Layar
- Search field
- Tombol `Find`
- Tombol `Stock`
- Tombol `Export to Excel`
- Tombol `Export Excel Per Batch`
- Tombol `Export Excel Warehouse`
- Tabel raw material
- Kolom terlihat: nomor, kode alias, nama raw material, supplier, min. stok (kg), stok berjalan (kg), aksi, detail
- Tombol aksi terlihat: `Update Min. Stok`, `Stock`, `Outstanding`, `Harga`

## Observasi Objektif
- Screen ini bukan sekadar master bahan baku, tapi juga titik monitoring stok dan analisa operasional.
- Ada fitur export dengan beberapa varian, artinya kebutuhan report existing cukup tinggi.
- Ada pembedaan antara **min stock** dan **stok berjalan**.
- Ada tombol `Outstanding`, yang mengindikasikan kebutuhan melihat item yang masih outstanding / belum terpenuhi.
- Ada tombol `Harga`, yang mengindikasikan adanya histori atau referensi harga per item.
- Ada tombol `Update Min. Stok`, berarti min stock bisa di-maintain dari layar ini.

## Dugaan Logic Bisnis
- PPIC likely menggunakan screen ini untuk review readiness material, safety stock, dan kebutuhan pembelian.
- Screen ini bisa menjadi salah satu pusat keputusan shortage / purchase request / monitoring inventory.

## Pertanyaan Terbuka
- Apakah `Stock` membuka kartu stok / batch detail?
- Apakah `Outstanding` menunjukkan PO open, PR, atau material shortage?
- Apakah `Harga` hanya history harga supplier atau juga dipakai untuk cost comparison?
- Siapa yang boleh ubah min stock?

## Kandidat Requirement Rebuild
- Fungsi ini perlu dipecah jelas antara master material, stock view, batch view, shortage view, dan pricing reference.
- Di sistem baru, raw material dashboard sebaiknya support stock on hand, reserved, incoming, expiring, shortage, dan supplier lead time.

---

# 5. Screen Audit - Raw Material / Expired Raw Material

## Identitas Screen
- Nama screen: Expired Raw Material
- Module / area: Raw Material
- Role pengguna: PPIC / PIC
- Tipe screen: monitoring list / exception report
- Tujuan screen: memantau bahan baku expired atau mendekati expired

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel expired raw material
- Kolom terlihat: nomor, kode, nama raw material, supplier, batch, expired date, stok berjalan (kg), aksi
- Tombol aksi `Detail`

## Observasi Objektif
- Existing system sudah memisahkan monitoring expired sebagai screen khusus.
- Data yang dipakai sudah batch-level, karena ada kolom batch dan expired date.
- PPIC diberi visibilitas ke stok yang berisiko expired.
- Menu raw material memiliki sub menu lain yang terlihat: Data Raw Material, Expired Raw Material, Out Of Stock Raw Material, Perbandingan Harga, Purchase Request, Analisa Raw Material.

## Dugaan Logic Bisnis
- Existing PPIC flow kemungkinan banyak bergantung ke monitoring inventory exception berbasis layar-layar terpisah.
- Batch tracking tampaknya sudah ada, setidaknya dari sisi monitoring expired.

## Pertanyaan Terbuka
- Apakah expired report ini purely monitoring, atau ada action lanjutan seperti block / quarantine / disposal?
- Apakah PPIC bisa create purchase request dari shortage/expired directly?

## Kandidat Requirement Rebuild
- Di sistem baru, monitoring expiry sebaiknya terhubung dengan stock status, FEFO, blocked stock, dan replenishment suggestion.
- Expired dan near-expired lebih baik dipisah statusnya, bukan hanya satu layar umum.

---

# Kesimpulan Awal Batch PPIC-01

## Observasi Global
- Role PPIC existing tidak hanya fokus pada production schedule, tetapi juga punya visibilitas besar ke sales order, supplier, raw material, sample, dan report.
- Banyak fungsi existing dibangun sebagai **list monitoring per topik**, misalnya raw material, expired material, packaging supplier, dan dashboard order.
- Sistem terlihat cukup operasional dan manual-report oriented, terbukti dari banyak tombol export dan layar monitoring terpisah.

## Arah Audit Selanjutnya
Batch berikutnya yang paling penting untuk PPIC:
1. Production Schedule
2. Purchase Request
3. Analisa Raw Material
4. Out Of Stock Raw Material
5. Monitoring Sales Order
6. Sales Order detail bila memang dipakai PPIC sebagai trigger planning

## Catatan Objektivitas
Temuan di atas masih berbasis observasi layar. Beberapa dugaan logic bisnis masih perlu divalidasi saat lebih banyak screen tersedia atau saat user menjelaskan urutan kerja aktual.
