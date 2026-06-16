# ERP Existing Audit Workspace

Repository ini dipakai sebagai workspace audit **ERP existing tanpa source code**.

## Tujuan
Workspace ini difokuskan untuk membantu proses:
- inventory screen by screen
- mapping fungsi bisnis existing
- mapping field, aksi, dan status
- identifikasi flow antar layar
- penyusunan requirement rebuild yang objektif

## Prinsip Kerja
Pendekatan repo ini **bukan untuk cloning source code**, tetapi untuk:
- memahami perilaku sistem existing
- mendokumentasikan proses dan data yang terlihat
- membangun requirement yang bisa dipakai untuk rebuild ERP baru

## Deliverable Utama
- catalog screen existing
- audit per screen
- field mapping
- use case mapping
- business rule observation
- gap / improvement note
- requirement rebuild candidates

## Struktur Folder
- `audit/` ? dokumen audit utama
- `screenshots/` ? screenshot / capture layar existing
- `references/` ? dokumen pendukung lain

## Cara Kerja
1. Kumpulkan screenshot per screen
2. Buat audit objektif per screen
3. Mapping relasi antar layar
4. Tarik use case, business rules, dan pain point
5. Susun requirement rebuild berdasarkan temuan
