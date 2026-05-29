# Roadmap: AstraPay API Docs Scraping

Dokumen ini berisi peta jalan (roadmap) untuk proses scraping dan konversi dokumentasi AstraPay API ke format Markdown untuk kebutuhan Hackathon.

## Fase 1: Inisialisasi & Perencanaan
- [x] Inisialisasi `ROADMAP.md` & `implementation_plan.md`
- [ ] Diskusi alur program dengan USER dan konfirmasi metode file (single vs multi-file)
- [ ] Verifikasi ketersediaan library Python (`requests`, `beautifulsoup4`)

## Fase 2: Pengembangan Script Scraper (Python)
- [ ] Pembuatan parser HTML ke Markdown terstruktur
- [ ] Integrasi pengunduh aset otomatis ke folder `assets/`
- [ ] Penanganan link relatif agar merujuk ke aset lokal

## Fase 3: Eksekusi & Validasi
- [ ] Eksekusi script scraping pada file HTML sumber
- [ ] Validasi format tabel, blockquote, link, dan tag code di Markdown
- [ ] Validasi rendering gambar lokal
- [ ] Penyusunan hasil akhir di folder `astrapay_api_docs/`
