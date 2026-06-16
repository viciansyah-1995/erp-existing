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


---

# 9. Screen Audit - Raw Material / Analisa Raw Material

## Identitas Screen
- Nama screen: Analisa Raw Material
- Module / area: Raw Material
- Role pengguna: PPIC / PIC
- Tipe screen: analysis / evaluation screen
- Tujuan screen: mengevaluasi hasil analisa bahan baku sample atau kandidat material sebelum dipakai

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel analisa raw material
- Kolom terlihat: tanggal datang sample, supplier, nama raw material, batch, expire date, result, COA tersedia/tidak, bisa dipakai/tidak, moisture, approval head BoD
- Badge status berwarna untuk beberapa kolom evaluasi

## Observasi Objektif
- Existing system memiliki layar analisa raw material yang cukup spesifik, bukan sekadar stock atau master material.
- Tabel menyimpan data batch-level dan expire date, sehingga analisa dikaitkan ke sampel / lot tertentu.
- Ada kolom narasi `result` yang cukup panjang, berisi penilaian atau kesimpulan penggunaan material.
- Ada indikator `COA tersedia/tidak`, sehingga certificate of analysis menjadi bagian penting dalam evaluasi material.
- Ada indikator `bisa dipakai/tidak`, sehingga layar ini tampak menjadi salah satu titik keputusan pemakaian bahan.
- Ada status `moisture`, yang menunjukkan ada parameter kualitas tertentu yang dipantau secara eksplisit.
- Ada kolom `approval head BoD`, berarti existing system melibatkan approval level tinggi setidaknya untuk hasil tertentu atau keputusan material tertentu.
- Menu ini berada di bawah modul Raw Material, berdampingan dengan data raw material, expired, out of stock, perbandingan harga, dan purchase request.

## Dugaan Logic Bisnis
- Existing PPIC tidak hanya melakukan planning material, tetapi juga memiliki visibilitas terhadap hasil evaluasi sample material dari supplier.
- Screen ini kemungkinan dipakai sebagai referensi keputusan apakah material supplier layak dipakai untuk produk tertentu, atau perlu reject / hold / compare dengan material lain.
- Narasi result yang panjang mengindikasikan proses evaluasi existing masih cukup manual dan deskriptif, belum sepenuhnya berbasis parameter terstruktur.

## Pertanyaan Terbuka
- Siapa yang input hasil analisa raw material: PPIC, QC, R&D, atau gabungan?
- Apakah `bisa dipakai/tidak` adalah keputusan final, atau hanya rekomendasi awal?
- Approval Head BoD dipakai untuk semua analisa atau hanya untuk kondisi tertentu?
- Moisture status di sini apakah hasil lab, QC, atau evaluasi supplier?
- Apakah screen ini terkait langsung ke purchase decision atau hanya referensi teknis?

## Kandidat Requirement Rebuild
- Di sistem baru, analisa raw material sebaiknya dipisah jelas antara evaluasi teknis, evaluasi kualitas, evaluasi dokumen, dan keputusan bisnis pemakaian.
- Parameter seperti COA, moisture, pass/fail, intended use, replacement note, dan approval sebaiknya dibuat lebih terstruktur, tidak hanya narasi result panjang.
- Jika approval level tinggi memang dibutuhkan, trigger approval harus jelas: misalnya material baru, material alternatif, deviasi mutu, atau supplier substitution.

---

# Pembaruan Kesimpulan PPIC Setelah Analisa Raw Material

## Observasi Global Tambahan
- Existing PPIC module tidak hanya berperan pada planning dan shortage, tetapi juga menyentuh evaluasi teknis material.
- Modul raw material existing tampaknya menjadi campuran antara:
  - master data
  - stock monitoring
  - expiry monitoring
  - shortage monitoring
  - purchase request
  - price comparison
  - material analysis
- Ini menunjukkan desain existing cukup sentralistik di satu domain menu, tetapi berpotensi membingungkan ownership proses antar PPIC, Purchasing, QC, dan R&D.

## Implikasi ke Rebuild
- Dalam sistem baru, perlu dipisahkan dengan jelas:
  - **material planning** (PPIC)
  - **supplier/commercial sourcing** (Purchasing)
  - **quality / test result** (QC)
  - **technical/formulation suitability** (R&D bila relevan)
- PPIC tetap perlu visibilitas, tetapi belum tentu menjadi owner input untuk seluruh aktivitas analisa material.


---

# 10. Screen Audit - Monitoring Sales Order

## Identitas Screen
- Nama screen: Monitoring Sales Order
- Module / area: Sales Order / Monitoring
- Role pengguna: PPIC / PIC
- Tipe screen: transaction monitoring list
- Tujuan screen: memantau status sales order yang relevan untuk proses PPIC

## Komponen Layar
- Search field
- Tombol `Find`
- Tombol `Export to Excel`
- Tabel monitoring sales order
- Kolom terlihat: aksi, status, no PO, no SO, customer, produk, kode sample, no PO PIC, no item, tanggal PO, due date PO, tanggal pemesanan, tanggal persetujuan marketing, dibuat oleh, gramasi, qty, isi box, box
- Tombol aksi: `Detail`, `History`, `Detail Pending`, `Close PO`
- Badge status: terlihat label seperti `Processing MKT`, `Pending`, dan badge lain terkait proses order
- Badge kecil pada salah satu kolom yang tampak seperti nomor item / line status

## Observasi Objektif
- PPIC memiliki akses langsung ke layar monitoring sales order, bukan hanya dashboard summary.
- Layar ini memuat detail order yang cukup operasional: due date, tanggal pemesanan, approval marketing, gramasi, qty, isi box, dan box.
- Ada tombol `Close PO`, berarti PPIC atau role ini diberi ability untuk menutup order / PO dari layar monitoring.
- Ada tombol `Detail Pending`, menunjukkan existing system memisahkan kondisi pending tertentu yang perlu ditindaklanjuti.
- Status order sangat terkait dengan proses marketing, terlihat dari badge `Processing MKT` dan kolom tanggal persetujuan marketing.

## Dugaan Logic Bisnis
- Monitoring sales order tampaknya menjadi titik awal penting bagi PPIC untuk menentukan order mana yang siap diproses lebih lanjut.
- Existing PPIC flow kemungkinan bergantung pada status komersial / marketing sebelum lanjut ke readiness material atau schedule.
- Layar ini bisa menjadi jembatan antara fungsi commercial dan planning.

## Pertanyaan Terbuka
- Siapa yang berwenang menekan `Close PO`?
- Apakah `Close PO` menutup order secara bisnis, atau hanya menandai selesai secara operasional?
- `Detail Pending` mengarah ke jenis pending apa: DP, sample, dokumen, material, atau approval?
- Apakah due date PO menjadi trigger utama schedule PPIC?

## Kandidat Requirement Rebuild
- Sistem baru perlu memisahkan dengan jelas antara status commercial, status planning, dan status fulfillment.
- Monitoring sales order untuk PPIC sebaiknya menyorot readiness, shortage, hold reason, dan next action, bukan hanya status umum.
- Hak untuk `Close PO` perlu governance jelas agar tidak tercampur dengan fungsi commercial.

---

# 11. Screen Audit - Customer / Data Customer

## Identitas Screen
- Nama screen: Data Customer
- Module / area: Customer
- Role pengguna: PPIC / PIC
- Tipe screen: master list / reference view
- Tujuan screen: melihat daftar customer dan mengakses relasi ke produk serta sales order

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel customer
- Kolom terlihat: nomor, name, personal name, alamat, email, no telepon, detail
- Tombol aksi: `Produk`, `Sales Order`

## Observasi Objektif
- PPIC punya akses langsung ke master customer.
- Dari list customer, user bisa langsung lompat ke `Produk` dan `Sales Order`, sehingga customer dipakai sebagai anchor navigasi ke data lain.
- Data customer memuat alamat, email, dan nomor telepon, yang berarti visibilitas PPIC ke data customer cukup luas.

## Dugaan Logic Bisnis
- Existing system memungkinkan PPIC menelusuri produk dan sales order berdasarkan customer tertentu.
- Ini menunjukkan struktur data existing cukup customer-centric untuk navigasi operasional.

## Pertanyaan Terbuka
- Apakah customer master ini hanya view-only untuk PPIC?
- Apakah `Produk` berarti daftar produk yang pernah dipesan customer atau produk yang terdaftar untuk customer tersebut?
- Apakah ada constraint khusus per customer seperti MOQ, packaging spec, atau SLA?

## Kandidat Requirement Rebuild
- Di sistem baru, customer reference untuk PPIC sebaiknya fokus pada informasi yang relevan untuk planning: active products, current orders, service level, dan packaging/profile requirements.
- Data kontak customer tidak harus tampil penuh jika tidak dibutuhkan PPIC untuk operasional sehari-hari.

---

# 12. Screen Audit - Packaging / Data Packaging

## Identitas Screen
- Nama screen: Data Packaging
- Module / area: Packaging
- Role pengguna: PPIC / PIC
- Tipe screen: inventory monitoring / master list hybrid
- Tujuan screen: memantau data dan stok packaging

## Komponen Layar
- Search packaging field
- Search packaging by kategori field
- Tombol `Find`
- Tombol `Reset`
- Tombol `Export to Excel`
- Tabel packaging
- Kolom terlihat: nomor, kode, nama packaging, kategori produk, unit, stok berjalan, min. stok, detail
- Tombol aksi: `Stok`, `Outstanding`

## Observasi Objektif
- Struktur layar packaging sangat mirip dengan raw material data screen.
- Packaging memiliki stok berjalan dan min stock, menunjukkan packaging diperlakukan sebagai inventory planning object yang setara pentingnya dengan raw material.
- Ada search by kategori, sehingga packaging memiliki klasifikasi per kategori produk.
- Ada tombol `Outstanding`, sama seperti raw material, menandakan packaging shortage/open requirement juga dimonitor tersendiri.
- Unit packaging terlihat bervariasi (roll, pieces), jadi UoM sudah relevan pada layar existing.

## Dugaan Logic Bisnis
- Existing PPIC process memperlakukan packaging sebagai domain inventory dan replenishment tersendiri.
- Monitoring packaging likely dipakai untuk memastikan readiness sebelum produksi atau release order.

## Pertanyaan Terbuka
- Apakah packaging punya purchase request screen terpisah? (menu menunjukkan ada `Purchase Request` dan `Pembatalan Purchase Request`).
- Apakah `Outstanding` untuk packaging berarti open procurement / shortage / pending fulfillment?
- Apakah packaging terkait langsung dengan customer-specific artwork atau hanya item generik?

## Kandidat Requirement Rebuild
- Di sistem baru, packaging planning sebaiknya disejajarkan dengan raw material planning tetapi tetap dibedakan karena approval spec/artwork bisa berbeda.
- View packaging perlu mendukung stock on hand, reserved, incoming, outstanding procurement, dan customer/product linkage.

---

# 13. Screen Audit - Product / Data Produk

## Identitas Screen
- Nama screen: Data Produk
- Module / area: Product
- Role pengguna: PPIC / PIC
- Tipe screen: master product list
- Tujuan screen: melihat daftar produk yang terdaftar di sistem

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel data produk
- Kolom terlihat: aksi, nama produk, nama BPOM, size per sachet (gr), customer, dibuat tanggal
- Tombol aksi: `Detail`, `Edit`

## Observasi Objektif
- PPIC punya akses ke product master dan bahkan terlihat ada tombol `Edit` pada layar list.
- Kolom `nama BPOM` menunjukkan product master sangat terkait dengan data regulatory / compliance.
- Product master juga dikaitkan ke customer, artinya product list existing kemungkinan customer-specific atau minimal customer-referenced.
- `Size per sachet (gr)` tampil sebagai field utama, yang menunjukkan gramasi menjadi informasi inti di master produk.

## Dugaan Logic Bisnis
- Existing product master berfungsi sebagai titik temu antara commercial product, regulatory naming, dan production sizing.
- Jika PPIC bisa edit product, maka ownership master data existing tampaknya belum dipisah ketat.

## Pertanyaan Terbuka
- Apakah `Edit` benar-benar aktif untuk role PPIC atau hanya terlihat karena login superadmin?
- Apakah product master di-create dari sample approval, sales, atau R&D?
- Seberapa erat hubungan produk dengan customer-specific formula atau packaging?

## Kandidat Requirement Rebuild
- Di sistem baru, product master sebaiknya punya ownership lebih jelas antara commercial, regulatory, dan production data.
- Field penting seperti gramasi, regulatory name, customer mapping, active status, dan packaging profile perlu dipisah per fungsi tetapi tetap sinkron.

---

# Kemungkinan Reverse Engineering Lanjutan dari Screen PPIC Existing

## Yang Berpotensi Bisa Diambil dari Existing System
Berdasarkan screen yang sudah terlihat, beberapa area reverse engineering non-source-code yang realistis untuk dilakukan nanti:

### 1. Reverse menu map dan navigasi modul
Bisa dilakukan dengan:
- inventory semua menu sidebar
- klik-through per menu / submenu
- catat pola relasi antar layar

Potensi hasil:
- module map existing
- relasi customer -> produk -> sales order
- relasi raw material / packaging -> stock -> outstanding -> PR

### 2. Reverse field dan status workflow
Bisa dilakukan dengan:
- inventory kolom tabel
- inventory field form detail
- catat status badge dan perubahan status
- catat tombol aksi yang muncul pada kondisi tertentu

Potensi hasil:
- kamus status existing
- trigger transisi status
- kandidat business rules per screen

### 3. Reverse data model permukaan
Bisa dilakukan dari observasi UI dengan menandai entity yang berulang:
- customer
- product
- sales order
- raw material
- packaging
- supplier
- purchase request
- batch / expired

Potensi hasil:
- conceptual entity relationship awal
- candidate master vs transaction mapping

### 4. Reverse report/export behavior
Bisa dilakukan dengan:
- identifikasi layar yang punya export Excel
- lihat variasi export yang tersedia
- nanti jika memungkinkan, uji file hasil export

Potensi hasil:
- kebutuhan report existing
- kebutuhan kolom output yang dianggap penting user
- indikasi tabel data di belakang layar

### 5. Reverse hidden technical clues bila akses browser tersedia
Jika nanti audit dilakukan langsung di browser hidup dan legal aksesnya tersedia, bisa dilanjut ke:
- inspect request URL
- network payload
- parameter filter
- pattern ID / code / status
- nama endpoint dan response structure

Potensi hasil:
- API behavior observation
- parameter naming existing
- petunjuk entity dan struktur data

## Risiko Reverse Engineering yang Perlu Dijaga

### Risiko 1 - Salah simpulan ownership proses
Banyak screen existing mencampur visibilitas dan ownership. Karena itu, melihat sebuah menu di role PPIC tidak otomatis berarti PPIC adalah owner prosesnya.

Mitigasi:
- pisahkan antara `bisa lihat`, `bisa input`, `bisa approve`, dan `benar-benar owner`.

### Risiko 2 - Salah baca status bisnis
Badge seperti `Processing MKT`, `Pending`, `READY`, atau status lainnya bisa tampak jelas di UI, tapi artinya belum tentu sama dengan dugaan kita.

Mitigasi:
- setiap status harus dicatat sebagai observasi dulu, baru diberi hipotesis dan pertanyaan validasi.

### Risiko 3 - Layar monitoring dianggap sebagai flow penuh
Banyak screen existing tampak seperti monitoring/reporting layer, bukan sumber transaksi utama.

Mitigasi:
- cari form create/edit/detail yang menjadi source of truth sebelum menyimpulkan alur bisnis final.

### Risiko 4 - Role saat screenshot terlalu tinggi
Login yang terlihat adalah `Superadmin / Managing - Owner / Login PPIC`, jadi screen yang muncul belum tentu mewakili hak akses user PPIC murni.

Mitigasi:
- tandai semua kemampuan edit/close/approve sebagai `perlu validasi hak akses role nyata`.

### Risiko 5 - Legal dan etika reverse engineering
Jika nanti masuk ke level network capture, endpoint inspection, atau DB observation, pastikan aksesnya sah dan memang diizinkan oleh owner sistem.

Mitigasi:
- fokus dulu ke observasi bisnis dari layar
- jika masuk technical inspection, lakukan hanya dengan izin yang jelas.

---

# Pembaruan Kesimpulan PPIC Setelah Batch Ini

## Observasi Global Tambahan
- PPIC existing terlihat sangat terhubung dengan customer, product, sales order, raw material, dan packaging sekaligus.
- Existing UI menunjukkan domain PPIC sangat lebar, tetapi ini belum tentu berarti ownership PPIC juga selebar itu.
- Modul-modul yang muncul memperlihatkan bahwa existing system kemungkinan dibangun berdasarkan kebutuhan operasional harian dan monitoring cepat, bukan pemisahan domain yang bersih.

## Arah Audit Selanjutnya yang Direkomendasikan
1. Detail / form screen dari Sales Order
2. Production Schedule
3. Detail / form screen dari Purchase Request Raw Material
4. Packaging Purchase Request / Pembatalan Purchase Request
5. Sample module yang terkait product creation
6. Screen report PPIC untuk melihat output final yang dianggap penting user


---

# 14. Screen Audit - Sales Order / Data Sales Order (SO)

## Identitas Screen
- Nama screen: Data Sales Order (SO)
- Module / area: Sales Order
- Role pengguna: PPIC / PIC
- Tipe screen: transaction list / operational order list
- Tujuan screen: melihat daftar sales order yang menjadi basis tindak lanjut operasional PPIC

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel data sales order
- Kolom terlihat: aksi, no PO, no SO, customer, tanggal PO, due date PO, NC/JE, tanggal pembuatan, tanggal persetujuan marketing, dibuat oleh, produk, kode sample, satuan/unit, jumlah/qty, jumlah/qty produksi, hasil produksi, status
- Tombol aksi `Detail`
- Badge status `Processing PPIC`
- Kolom jumlah produksi dan hasil produksi menunjukkan nilai numerik per line order
- Product row tampak bisa multi-line di bawah customer/order tertentu

## Observasi Objektif
- Screen ini berbeda dari `Monitoring Sales Order` karena terasa lebih seperti list operasional utama, bukan hanya monitoring summary.
- Status yang terlihat pada layar ini adalah `Processing PPIC`, yang mengindikasikan order sudah masuk ke domain kerja PPIC.
- Ada pemisahan antara `jumlah/qty`, `jumlah/qty produksi`, dan `hasil produksi`, artinya existing system mencoba melacak transisi order dari permintaan ke realisasi produksi.
- Ada kolom `kode sample`, menunjukkan hubungan langsung antara sales order dengan referensi sample tertentu.
- Ada kolom `NC/JE` yang belum jelas artinya, tapi tampak sebagai field operasional penting pada order.
- Tabel ini tampak menampilkan multiple product line dalam satu customer/order context, sehingga satu PO bisa berisi beberapa item.

## Dugaan Logic Bisnis
- Ini kemungkinan salah satu list utama yang digunakan PPIC untuk memutuskan order mana yang harus disiapkan / diproses.
- Status `Processing PPIC` bisa menjadi handoff dari Marketing ke PPIC.
- Kolom qty produksi dan hasil produksi mengindikasikan ada tracking kuantitas per order line menuju realisasi.

## Pertanyaan Terbuka
- Apa arti field `NC/JE` dalam konteks order existing?
- Apakah `jumlah/qty produksi` diisi manual oleh PPIC atau otomatis dari production schedule / production execution?
- `hasil produksi` mengacu ke output aktual, output good, atau delivered quantity?
- Apakah screen ini source utama order processing PPIC, sementara monitoring sales order hanya layar bantu?

## Kandidat Requirement Rebuild
- Sistem baru perlu memisahkan dengan jelas order commercial list, order planning queue, production allocation, dan fulfillment progress.
- Handoff dari Marketing ke PPIC harus memiliki status transisi yang jelas, misalnya `Commercial Clear`, `Ready for PPIC Review`, `PPIC Planning`, `Released to Production`.
- Relasi order line, sample code, product, qty requested, qty planned, qty produced, dan output final harus tetap dipertahankan secara eksplisit.

---

# Pembaruan Kesimpulan PPIC Setelah Screen Data Sales Order

## Observasi Global Tambahan
- Existing PPIC benar-benar berada di tengah hubungan antara sales order dan production readiness.
- Screen `Data Sales Order (SO)` menunjukkan bahwa PPIC existing bukan hanya memonitor, tetapi sudah menjadi tahap status tersendiri dalam lifecycle order.
- Relasi sample code dengan sales order menandakan pentingnya linkage sample-to-order pada sistem existing.

## Implikasi ke Rebuild
- Rebuild ERP baru harus menjaga chain berikut tetap eksplisit:
  - customer
  - product
  - sample reference
  - sales order line
  - qty request
  - PPIC processing status
  - qty produksi / hasil produksi
- Ini penting supaya sistem baru tidak hanya meniru tampilan list, tetapi benar-benar menangkap business state yang bergerak di balik order.


---

# 15. Screen Audit - Sample / Data Sample

## Identitas Screen
- Nama screen: Data Sample
- Module / area: Sample
- Role pengguna: PPIC / PIC
- Tipe screen: reference / monitoring list
- Tujuan screen: melihat daftar sample yang sudah terdaftar dan keterkaitannya dengan produk

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel data sample
- Kolom terlihat: nomor, nama produk, kode sample, total cost per sachet (Rp), total cost per kg (Rp), tipe sample, dibuat tanggal, aksi
- Tombol aksi `Detail`
- Badge tipe sample, terlihat label seperti `Draft Sample`

## Observasi Objektif
- PPIC punya visibilitas langsung ke modul sample.
- Sample ditautkan ke nama produk dan kode sample secara eksplisit.
- Ada informasi costing sample (`cost per sachet` dan `cost per kg`), sehingga modul sample existing juga memuat informasi cost reference.
- Badge `Draft Sample` menunjukkan sample memiliki lifecycle status sendiri.
- Kode sample tampak sangat konsisten digunakan di banyak layar lain (sales order, product, monitoring), menandakan sample code adalah entity penting di existing system.

## Dugaan Logic Bisnis
- Sample kemungkinan menjadi titik referensi utama sebelum produk masuk ke sales order dan planning.
- PPIC mungkin menggunakan sample screen untuk memastikan referensi sample yang dipakai order sudah sesuai dan terdaftar.

## Pertanyaan Terbuka
- Apakah PPIC hanya bisa view sample atau juga memicu penggunaan sample untuk order tertentu?
- Apakah sample cost dipakai untuk planning/costing atau sekadar referensi komersial?
- Apa perbedaan lifecycle antara Draft Sample dan sample approved/final?

## Kandidat Requirement Rebuild
- Sistem baru perlu menjaga relasi sample -> product -> sales order secara eksplisit.
- Status sample harus jelas: draft, under review, approved, locked, obsolete.
- Jika PPIC memang perlu visibilitas, cukup tampilkan field yang relevan untuk planning dan readiness, bukan seluruh costing kalau tidak dibutuhkan.

---

# 16. Screen Audit - Production Schedule / Jadwal Timbang & Mixing

## Identitas Screen
- Nama screen: Jadwal Timbang & Mixing
- Module / area: Production Schedule
- Role pengguna: PPIC / PIC
- Tipe screen: scheduling dashboard / calendar view
- Tujuan screen: mengatur dan memantau jadwal timbang, mixing, dan produksi mingguan

## Komponen Layar
- Kalender mini bulanan di panel kiri
- Event list: `Jadwal Timbang`, `Jadwal Mixing`, `Jadwal Produksi Mingguan`
- Kalender besar di panel kanan
- Toggle tampilan: Month / Week / Day / List
- Tombol navigasi kalender

## Observasi Objektif
- Ini adalah screen schedule inti yang paling dekat dengan fungsi planning PPIC.
- Existing system memisahkan event schedule ke minimal tiga jenis: timbang, mixing, dan produksi mingguan.
- Tersedia beberapa mode tampilan kalender, artinya schedule bukan sekadar list sederhana tetapi sudah dipetakan secara waktu.
- Dari screenshot belum terlihat detail event card atau form create/edit schedule.

## Dugaan Logic Bisnis
- PPIC existing kemungkinan menyusun schedule berbasis aktivitas proses, bukan hanya manufacturing order global.
- Pemisahan timbang dan mixing menunjukkan tahap proses produksi dianggap penting sebagai unit penjadwalan tersendiri.

## Pertanyaan Terbuka
- Apakah schedule dibuat manual drag-drop atau dari form terstruktur?
- Apakah jadwal produksi mingguan diturunkan dari sales order, PR material, atau keputusan manual PIC?
- Bagaimana relasi schedule dengan kapasitas line, manpower, dan material readiness?

## Kandidat Requirement Rebuild
- Di sistem baru, production scheduling perlu menghubungkan order demand, readiness material, kapasitas line, dan aktivitas proses kunci.
- Jika timbang dan mixing memang perlu dijadwalkan terpisah, sistem baru harus mendukung operation-level scheduling, bukan hanya MO header.

---

# 17. Screen Audit - Report / Laporan Sales Order (SO)

## Identitas Screen
- Nama screen: Laporan Sales Order (SO)
- Module / area: Report
- Role pengguna: PPIC / PIC
- Tipe screen: report list / export-oriented report
- Tujuan screen: menyediakan laporan sales order yang lebih lengkap untuk monitoring dan export

## Komponen Layar
- Search field
- Tombol `Find`
- Tombol `Export to Excel`
- Tabel laporan sales order
- Kolom terlihat: no PO, no SO, customer, produk, kode sample, no PPIC, no PR, tanggal PO, due date PO, tanggal pembuatan, tanggal persetujuan marketing, dibuat oleh, gramasi, qty, isi box, box, roll, botol, karton, total batch, total kirim, dan kolom lain yang terpotong
- Badge di kolom packaging seperti `Custom`
- Badge lain seperti `Cari Printing`

## Observasi Objektif
- Laporan sales order di report module jauh lebih detail dibanding monitoring list biasa.
- Ada field `no PPIC` dan `no PR`, sehingga laporan ini sudah menghubungkan order ke proses internal PPIC dan request pengadaan / proses terkait.
- Ada field packaging detail (`box`, `roll`, `botol`, `karton`) yang menunjukkan order report juga dipakai untuk memantau kebutuhan atau realisasi packaging.
- Badge `Custom` dan `Cari Printing` menunjukkan adanya kebutuhan packaging/printing spesifik pada order tertentu.

## Dugaan Logic Bisnis
- Report ini kemungkinan dipakai sebagai alat monitoring lintas fungsi, bukan hanya laporan pasif.
- Sales order report existing tampak menjadi salah satu sumber untuk melihat dampak order terhadap packaging, PPIC process, dan pengiriman.

## Pertanyaan Terbuka
- Apa arti `no PPIC` dan bagaimana nomor itu dihasilkan?
- Apakah `no PR` mengacu ke purchase request material/packaging yang terkait order?
- Badge `Cari Printing` mengindikasikan status apa: perlu desain, perlu vendor printing, atau material belum siap?

## Kandidat Requirement Rebuild
- Report order di sistem baru sebaiknya tidak menjadi satu layar superpadat; lebih baik dibagi antara operational dashboard dan detailed export report.
- Namun, linkage order -> PPIC -> PR -> packaging requirement harus tetap dipertahankan.

---

# 18. Screen Audit - Packaging / Purchase Request Packaging

## Identitas Screen
- Nama screen: Purchase Request Packaging
- Module / area: Packaging
- Role pengguna: PPIC / PIC
- Tipe screen: transaction list / request monitoring
- Tujuan screen: mencatat dan memantau purchase request packaging

## Komponen Layar
- Search field
- Filter tanggal awal dan tanggal akhir
- Tombol `Find`
- Tombol `+ Tambah Purchase Request Packaging`
- Tombol `Export to Excel`
- Dropdown filter status
- Tabel PR packaging
- Kolom terlihat: aksi, no sales order, no PO, tanggal PO, nama, tanggal permintaan, tanggal approve, keterangan, supplier/printing, quantity (piece/roll), harga satuan, total harga, status PO, tanggal kirim, jumlah kirim (piece/roll)
- Tombol aksi yang terlihat: `Edit`, `Cancel`, `Approve`, `Set Detail`
- Badge status PO terlihat seperti `Close`, `Draft`, `Pending`
- Ada baris total / kekurangan pengiriman

## Observasi Objektif
- Existing system memisahkan PR packaging dari PR raw material.
- PPIC atau PIC tampak bisa create dan set detail PR packaging langsung dari layar ini.
- Aksi `Approve` muncul di layar list, berarti approval bisa terjadi langsung di daftar transaksi.
- Aksi `Set Detail` menunjukkan PR packaging punya detail lanjutan yang belum terlihat di batch ini.
- `Supplier/Printing` berada di level penting, menandakan packaging procurement sangat terkait dengan vendor printing.
- Ada penghitungan `kekurangan pengiriman`, menunjukkan screen ini juga dipakai memonitor fulfillment / shortage dari sisi supply packaging.

## Dugaan Logic Bisnis
- PR packaging existing adalah gabungan antara request, approval, supplier/printing assignment, dan shipment tracking.
- Packaging procurement kemungkinan lebih kompleks daripada raw material karena terkait printing/vendor spesifik.

## Pertanyaan Terbuka
- Apakah `Approve` di list screen dilakukan PPIC sendiri atau role lain yang kebetulan terlihat karena login superadmin?
- Apa fungsi detail dari `Set Detail`?
- Status `Close`, `Draft`, dan `Pending` mengacu ke PR atau PO actual?
- Apakah packaging PR dipicu dari sales order directly atau dari readiness check terpisah?

## Kandidat Requirement Rebuild
- Sistem baru perlu memisahkan PR packaging, approval, vendor assignment, artwork/printing readiness, dan receiving/fulfillment tracking dengan lebih jelas.
- Namun, satu summary view tetap dibutuhkan karena existing user tampak sangat bergantung pada monitoring dari satu layar.

---

# 19. Screen Audit - Packaging / Pembatalan Permintaan Packaging

## Identitas Screen
- Nama screen: Pembatalan Permintaan Packaging
- Module / area: Packaging
- Role pengguna: PPIC / PIC
- Tipe screen: exception / cancellation monitoring
- Tujuan screen: memantau dan mencatat pembatalan purchase request packaging

## Komponen Layar
- Search field
- Tombol `Find`
- Tabel pembatalan permintaan packaging
- Kolom terlihat: no sales order, no PO, nama, tanggal permintaan, keterangan, alasan pembatalan, supplier/printing, quantity (piece/roll), harga satuan, total harga, status PO, tanggal kirim, jumlah kirim (piece/roll), keterangan
- Ada summary `Total kekurangan pengiriman`
- Badge status `Close`

## Observasi Objektif
- Existing system memberi layar khusus untuk pembatalan PR packaging, bukan hanya status di layar utama.
- Ada kolom `alasan pembatalan`, artinya cancellation reason menjadi data yang dicatat.
- Shipment shortfall/kekurangan kirim tetap ditampilkan, sehingga pembatalan tetap dikaitkan dengan fulfillment reality.
- Status PO masih ditampilkan walaupun permintaan dibatalkan, menandakan relasi antara PR/PO/cancellation tetap dipertahankan.

## Dugaan Logic Bisnis
- Pembatalan packaging request tampaknya merupakan exception yang cukup sering atau cukup penting sampai diberi layar khusus.
- Existing process mungkin rawan perubahan order, perubahan desain, atau perubahan kebutuhan packaging sehingga cancellation perlu dimonitor tersendiri.

## Pertanyaan Terbuka
- Siapa yang berwenang membatalkan PR packaging?
- Apakah cancellation ini membatalkan PR, membatalkan PO, atau hanya membatalkan sebagian kebutuhan?
- Apakah alasan pembatalan distandardkan atau hanya free text?

## Kandidat Requirement Rebuild
- Sistem baru perlu cancellation workflow yang jelas untuk packaging karena dampaknya bisa ke vendor, cost, timeline, dan stock.
- Cancellation reason perlu dikategorikan: perubahan order, desain belum approve, supplier issue, over-request, atau perubahan planning.

---

# Pembaruan Kesimpulan PPIC Setelah Batch Ini

## Observasi Global Tambahan
- Role PPIC existing terlihat sangat kuat pada area koordinasi order-to-planning, bukan hanya material monitoring.
- Sample, production schedule, sales order report, packaging PR, dan cancellation menunjukkan PPIC menjadi titik kontrol operasional lintas domain.
- Existing system cenderung memusatkan banyak keputusan dan monitoring dalam role ini, meskipun ownership riil masih perlu divalidasi.

## Implikasi Rebuild Tambahan
- Rebuild ERP baru kemungkinan perlu mempertahankan **visibility breadth** untuk PPIC, tetapi membatasi **edit/approve ownership** secara lebih disiplin.
- Modul schedule, sample linkage, packaging request, dan sales order operational report harus dianggap sebagai komponen inti desain PPIC, bukan tambahan kecil.

## Arah Audit Selanjutnya yang Direkomendasikan
1. Detail / form create Purchase Request Packaging
2. Detail / form create Purchase Request Raw Material
3. Detail Sales Order
4. Detail event di Production Schedule
5. Report lain di module report (raw material, packaging, shipment)
6. Jika ada, approval-specific screen untuk PPIC workflow
