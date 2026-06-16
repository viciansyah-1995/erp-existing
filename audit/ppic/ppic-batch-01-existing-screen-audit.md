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
