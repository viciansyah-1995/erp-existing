# Objective - ERP Existing Audit

## Latar Belakang
ERP existing perlu dipahami dan didokumentasikan walaupun source code tidak tersedia. Fokus utamanya adalah menangkap perilaku sistem dari sisi bisnis dan operasional agar bisa dipakai sebagai dasar rebuild, replacement, atau improvement system.

## Objective Utama
1. Mengidentifikasi screen dan fungsi utama ERP existing
2. Mendokumentasikan input, output, field, aksi, dan status per screen
3. Memetakan hubungan antar screen dan alur bisnis yang terlihat
4. Menarik use case dan business rules yang bisa diobservasi
5. Menyusun dasar requirement yang objektif untuk proses rebuild

## Batasan
- Audit dilakukan berdasarkan observasi layar, flow user, dan dokumen pendukung
- Audit tidak bergantung pada source code
- Temuan harus dipisahkan antara **fakta yang terlihat** dan **asumsi yang perlu validasi**

## Prinsip Objektivitas
Saat menulis audit:
- catat hanya yang benar-benar terlihat di layar atau flow
- bedakan observasi dengan asumsi
- hindari menebak logic teknis bila belum ada bukti
- tulis pertanyaan terbuka jika ada hal yang belum jelas
