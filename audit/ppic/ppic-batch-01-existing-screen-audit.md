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


---

# 6. Screen Audit - Raw Material / Out Of Stock Raw Material

## Identitas Screen
- Nama screen: Out Of Stock Raw Material
- Module / area: Raw Material
- Role pengguna: PPIC / PIC
- Tipe screen: monitoring list / exception report
- Tujuan screen: memantau bahan baku dengan stok habis atau nol

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel out of stock raw material
- Kolom terlihat: nomor, kode, nama raw material, supplier, stok berjalan (kg), min. stok (kg), aksi
- Tombol aksi `Detail`

## Observasi Objektif
- Existing system memiliki layar exception khusus untuk material yang out of stock, terpisah dari layar raw material umum.
- Nilai stok berjalan di screenshot terlihat `0.00`, sehingga layar ini tampaknya fokus pada item yang benar-benar kosong.
- Min. stok tetap ditampilkan berdampingan dengan stok berjalan, artinya threshold stock dipakai sebagai referensi penting dalam monitoring.
- Menu existing memecah monitoring material menjadi beberapa layar berbeda: data raw material, expired raw material, out of stock raw material, perbandingan harga, purchase request, dan analisa raw material.

## Dugaan Logic Bisnis
- PPIC existing kemungkinan menggunakan layar ini untuk identifikasi shortage yang butuh tindakan pembelian atau reschedule.
- Out of stock screen tampaknya bersifat monitoring dan exception-driven, bukan sekadar master list.

## Pertanyaan Terbuka
- Apakah item di layar ini otomatis menjadi kandidat purchase request?
- Apakah out of stock dihitung hanya dari on hand, atau sudah mempertimbangkan reserved/incoming stock?
- Apakah ada alert otomatis atau hanya passive monitoring?

## Kandidat Requirement Rebuild
- Sistem baru perlu shortage view yang lebih informatif: on hand, reserved, incoming, open PR/PO, dan suggested replenishment.
- Out of stock sebaiknya dibedakan dari below min stock agar prioritas keputusan lebih jelas.

---

# 7. Screen Audit - Raw Material / Perbandingan Harga

## Identitas Screen
- Nama screen: Perbandingan Harga
- Module / area: Raw Material
- Role pengguna: PPIC / PIC
- Tipe screen: report / search screen
- Tujuan screen: membandingkan harga bahan baku

## Komponen Layar
- Search field dengan placeholder pencarian kata kunci raw material
- Tombol `Find`
- Area hasil di bawah search

## Observasi Objektif
- Screen ini disiapkan untuk membandingkan harga raw material, tetapi screenshot tidak menampilkan hasil pencarian atau tabel hasil.
- Nama menu dan layout menunjukkan bahwa pricing comparison sudah menjadi kebutuhan eksplisit dalam sistem existing.
- PPIC diberi akses ke informasi perbandingan harga, bukan hanya purchasing atau finance.

## Dugaan Logic Bisnis
- Existing system kemungkinan mendukung keputusan pembelian atau evaluasi supplier dari sisi harga material.
- Jika hasil comparison muncul setelah pencarian, berarti per item raw material bisa memiliki multi-supplier / multi-price history.

## Pertanyaan Terbuka
- Apakah perbandingan harga membandingkan supplier aktif sekaligus, atau history harga dalam periode waktu tertentu?
- Apakah data ini hanya referensi visual atau bisa dipakai generate PR / keputusan supplier?
- Apakah ada dimensi kuantitas, satuan, batch, atau effective date dalam comparison ini?

## Kandidat Requirement Rebuild
- Di sistem baru, comparison view sebaiknya support item, supplier, last price, current quote, lead time, MOQ, payment term, dan tanggal efektif harga.
- Jika PPIC memang memakai data harga untuk planning, perlu batasan yang jelas antara visibilitas PPIC dan ownership purchasing.

---

# 8. Screen Audit - Raw Material / Purchase Request Raw Material

## Identitas Screen
- Nama screen: Purchase Request Raw Material
- Module / area: Raw Material / Purchase Request
- Role pengguna: PPIC / PIC
- Tipe screen: transaction list / request monitoring
- Tujuan screen: mencatat dan memantau purchase request bahan baku

## Komponen Layar
- Search field
- Filter tanggal awal dan tanggal akhir
- Tombol `Find`
- Tombol `+ Tambah Purchase Request Raw Material`
- Tombol `Export to Excel`
- Dropdown filter status / tampilan di kanan atas
- Tabel purchase request
- Kolom terlihat: aksi, no. PO/nomor dokumen, kode raw material, raw material, nama bahan baku original, quantity, availability, tanggal permintaan, tanggal approve, catatan, status PR, tanggal kirim, jumlah kirim (kg), batch/expired date
- Badge status availability `READY`
- Badge status PR terlihat berwarna kuning (label belum terbaca jelas dari screenshot)

## Observasi Objektif
- Purchase request raw material di existing system sudah menjadi transaksi terpisah, bukan sekadar catatan shortage.
- Ada tombol create `Tambah Purchase Request Raw Material`, berarti PPIC atau PIC bisa membuat request langsung dari modul ini.
- Tabel PR cukup kaya informasi: tidak hanya quantity dan item, tetapi juga availability, approval date, shipment date, delivered qty, dan batch/expired date.
- Label `availability` menunjukkan sistem menampilkan status kesiapan item pada layar PR.
- Kolom `jumlah kirim (kg)` dan `batch/expired date` mengindikasikan bahwa existing system juga memonitor realisasi kirim material terhadap PR.
- Nama kolom `No. PO` pada layar PR raw material masih perlu divalidasi: bisa jadi itu nomor referensi internal, bukan purchase order final ke vendor.

## Dugaan Logic Bisnis
- Existing PR flow tampaknya menggabungkan request, monitoring availability, dan sebagian status fulfillment pada satu layar.
- PPIC kemungkinan membuat PR berdasarkan shortage / schedule need, lalu memonitor progres realisasi material dari layar ini.
- Kehadiran field batch/expired date menunjukkan material receipt atau shipment mungkin sudah ditautkan ke PR level.

## Pertanyaan Terbuka
- Apakah `No. PO` di sini benar-benar PO vendor, atau hanya nomor request / referensi dokumen?
- Status PR kuning itu artinya apa: pending, open, partial, atau approved?
- Availability `READY` mengacu ke stok internal, vendor readiness, atau status pemenuhan request?
- Siapa yang approve PR: purchasing, manager, atau owner?
- Apakah PPIC bisa edit PR setelah approval atau setelah shipment terjadi?

## Kandidat Requirement Rebuild
- Sistem baru sebaiknya memisahkan jelas antara PR, RFQ/PO, receipt, dan fulfillment status, walaupun tetap saling terhubung.
- View PR perlu support shortage reason, related production/schedule, requested date, approved date, supplier assignment, incoming quantity, dan remaining quantity.
- Jika batch/expiry memang dipakai di layar PR, perlu dipastikan apakah itu business need valid atau efek desain existing yang terlalu campur antara request dan receiving.

---

# Pembaruan Kesimpulan PPIC Setelah Batch Tambahan

## Observasi Global Tambahan
- Existing PPIC sangat bergantung pada **monitoring by exception screen**, misalnya expired, out of stock, dan PR monitoring.
- Modul raw material existing tampak menjadi pusat keputusan penting untuk shortage, replenishment, supplier visibility, dan tracking request.
- Existing system masih terlihat report-heavy dan transactional-monitoring-heavy, dengan banyak layar terpisah untuk topik yang saling berhubungan.
- Dari sisi requirement rebuild, kemungkinan besar modul baru perlu menyederhanakan layar-layar ini menjadi dashboard + actionable views, bukan banyak screen terpecah tanpa relasi yang jelas.

## Arah Audit Selanjutnya yang Direkomendasikan
1. Analisa Raw Material
2. Detail / form create Purchase Request Raw Material
3. Monitoring Sales Order
4. Production Schedule
5. Sales Order detail yang menjadi trigger kebutuhan material
6. Packaging module untuk melihat apakah polanya mirip dengan raw material
