# PRD: AuthorSultra Paint V3 (CSP-Killer Edition)
## Product Requirements Document — Aplikasi Lukis Digital Kelas Profesional, Setara/Melampaui Clip Studio Paint

**Versi Dokumen:** 3.2 — Enhanced & Complete, Mode Eksekusi Otonom Penuh, Wajib Fungsional Utuh (Bukan Kerangka)
**Status:** MASTER SPEC — mengikat, tidak boleh direduksi tanpa persetujuan eksplisit
**Cara Pakai:** Tempelkan seluruh isi dokumen ini ke AI agent otonom, lalu beri satu perintah pemicu, misalnya: *"Bangun aplikasi ini sesuai PRD di atas dari Modul 1 sampai Modul 34, ikuti Section 0 (Build Governance Protocol) secara ketat — termasuk Section 0.8 (setiap fitur harus fungsional utuh, bukan sekadar kerangka UI) — dan jangan berhenti untuk minta konfirmasi di setiap modul kecuali sesuai Section 0.7 poin 4."* Setelah itu agent akan berjalan otonom membangun fitur yang benar-benar bisa dipakai, sampai selesai.
**Prinsip Utama:** *Zero Feature Left Behind, Zero Empty Shell* — setiap baris di dokumen ini adalah requirement wajib yang harus berfungsi nyata, bukan saran dan bukan tampilan kosong.

---

## 0. ATURAN WAJIB UNTUK AI AGENT OTONOM (BUILD GOVERNANCE PROTOCOL)

> Bagian ini WAJIB dibaca dan dipatuhi oleh setiap AI agent otonom (Claude Code, Antigravity, OpenCode, Cursor Agent, dsb.) sebelum menulis satu baris kode pun. Bagian ini adalah **kontrak kerja**, bukan rekomendasi.

### 0.1 Prinsip Dasar
1. **Sequential-Only Execution (Tidak Boleh Lompat-Lompat).** Agent WAJIB mengerjakan modul sesuai urutan nomor section pada dokumen ini (Section 1 → 2 → 3 → ... → 33). Modul berikutnya TIDAK BOLEH dimulai sebelum modul sebelumnya dinyatakan **DONE** (lihat 0.4).
2. **Single Source of Truth.** Dokumen PRD ini adalah satu-satunya rujukan kebenaran fitur. Jika ada ambiguitas, agent WAJIB memilih interpretasi yang **paling lengkap dan paling ketat** (bukan yang paling mudah dikerjakan), dan mencatat asumsi di `BUILD_LOG.md`.
3. **No Silent Skipping.** Dilarang keras melewati, menyederhanakan diam-diam, atau menghapus fitur apa pun dari daftar tanpa mencatat alasan eksplisit di `BUILD_LOG.md` dan mendapat persetujuan pengguna.
4. **No Fake Implementation.** Dilarang keras: kode placeholder yang berpura-pura berfungsi, `TODO: implement later` yang dibiarkan tanpa tiket lanjutan, fungsi yang mengembalikan data dummy/hardcoded agar terlihat "jalan", tombol UI yang tidak tersambung ke logika nyata, atau mock yang menggantikan implementasi asli secara permanen.
5. **Test-Before-Next.** Setiap modul HARUS punya bukti pengujian (lihat Section 32) yang lulus sebelum agent boleh pindah ke modul berikutnya.
6. **Regression Guard.** Sebelum menandai modul baru sebagai selesai, agent WAJIB menjalankan ulang seluruh checklist regresi modul-modul sebelumnya yang berkaitan (minimal smoke test) untuk memastikan tidak ada yang rusak.
7. **Definition of Done Ketat.** Fitur dianggap selesai HANYA jika: (a) berfungsi sesuai spesifikasi persis seperti tertulis, (b) teruji manual sesuai Section 32, (c) tidak menimbulkan regresi, (d) dicentang di `MASTER_CHECKLIST.md` (Section 33) dengan tanggal & catatan pengujian.

### 0.2 Struktur Kerja Wajib (Setiap Sesi Build)
1. **Baca dulu, jangan langsung ngoding.** Di awal setiap sesi, agent WAJIB membaca `BUILD_LOG.md` dan `MASTER_CHECKLIST.md` untuk mengetahui posisi terakhir sebelum melanjutkan.
2. **Rencanakan modul saat ini.** Agent menuliskan rencana singkat (daftar sub-tugas) untuk modul yang sedang dikerjakan sebelum menulis kode.
3. **Bangun incremental per sub-fitur**, bukan menulis seluruh modul sekaligus tanpa checkpoint. Setiap sub-fitur diuji sebelum lanjut ke sub-fitur berikutnya di modul yang sama.
4. **Update log setiap selesai sub-tugas** — bukan menumpuk update di akhir sesi.
5. **Berhenti dan laporkan** jika menemukan requirement yang secara teknis mustahil/kontradiktif — jangan diam-diam diubah atau dilewati.

### 0.3 File Wajib yang Harus Dipelihara Agent
- **`BUILD_LOG.md`** — jurnal kronologis: tanggal, modul, keputusan desain, asumsi, masalah ditemukan, cara diselesaikan.
- **`MASTER_CHECKLIST.md`** — salinan kerja dari Section 33, setiap baris punya checkbox `[ ]`/`[x]`, tanggal selesai, dan referensi test case.
- **`TEST_REPORT.md`** — hasil pengujian tiap modul (lihat format di Section 32.4).
- **`KNOWN_ISSUES.md`** — bug yang diketahui tapi belum diperbaiki, dengan severity dan rencana perbaikan (tidak boleh kosong-kosong disembunyikan).

### 0.4 Definisi Status Modul
| Status | Arti |
|---|---|
| `NOT_STARTED` | Belum dikerjakan sama sekali |
| `IN_PROGRESS` | Sedang dikerjakan, belum ada sub-fitur yang lulus test |
| `PARTIAL` | Sebagian sub-fitur lulus test, sisanya belum — TIDAK BOLEH lanjut ke modul berikutnya |
| `TESTED` | Semua sub-fitur lulus test individual |
| `DONE` | Lulus test individual + lulus regresi + dicentang di Master Checklist |

Agent hanya boleh pindah ke modul selanjutnya jika modul saat ini berstatus **DONE**.

### 0.5 Larangan Eksplisit (Anti-Pattern)
- ❌ Menulis "fitur ini akan disempurnakan nanti" lalu lanjut ke modul lain tanpa tiket di `KNOWN_ISSUES.md`.
- ❌ Menggabungkan banyak modul sekaligus dalam satu commit besar tanpa checkpoint pengujian di antaranya.
- ❌ Mengklaim "selesai lengkap" tanpa bukti pengujian tertulis di `TEST_REPORT.md`.
- ❌ Menghapus/menyederhanakan requirement karena "terlalu rumit" tanpa izin eksplisit pengguna.
- ❌ Mengabaikan error handling, edge case, atau validasi input demi kecepatan pengerjaan.
- ❌ Berasumsi user tidak akan menyadari fitur yang hilang atau tidak lengkap.
- ❌ Membangun tampilan/UI untuk banyak fitur sekaligus tanpa menyambungkan logika fungsionalnya (lihat larangan detail di Section 0.8).

### 0.6 Kriteria "PRD Ini Selesai Dibangun 100%"
Proyek dianggap selesai HANYA jika seluruh baris di `MASTER_CHECKLIST.md` (Section 33) tercentang `[x]`, seluruh modul berstatus `DONE`, `TEST_REPORT.md` lengkap untuk semua modul, dan hasil regresi penuh (full regression pass — seluruh fitur dites ulang berurutan dari Modul 1) tidak menunjukkan kegagalan apa pun.

### 0.7 Mode Eksekusi Otonom Penuh (Tanpa Konfirmasi Bertahap)

> Dokumen ini adalah otorisasi eksplisit dan final untuk seluruh cakupan pekerjaan yang tercantum. Agent TIDAK PERLU dan TIDAK BOLEH berhenti untuk meminta persetujuan/konfirmasi pengguna di setiap batas modul, sub-fitur, atau commit.

1. **Izin Berjalan Terus (Run-to-Completion).** Setelah pengguna memberi perintah "mulai build" (atau perintah setara), agent WAJIB berjalan dari Modul 1 hingga Modul 34 secara berkesinambungan dalam sesi/rangkaian sesi berturut-turut, mengikuti urutan Section 0.1–0.4, TANPA berhenti di tiap modul hanya untuk bertanya "lanjutkan?", "apakah ini sudah benar?", atau semacamnya.
2. **Keputusan Desain Kecil Diputuskan Sendiri.** Untuk detail implementasi yang tidak eksplisit di PRD (mis. nama variabel, struktur file, urutan sub-tugas dalam satu modul), agent mengambil keputusan sendiri berdasarkan praktik terbaik dan mencatatnya di `BUILD_LOG.md` — TIDAK perlu bertanya ke pengguna.
3. **Progres Dilaporkan, Bukan Diminta Izin.** Agent boleh (disarankan) memberi ringkasan progres singkat setiap beberapa modul selesai (untuk transparansi), tapi ini adalah **laporan status**, bukan **permintaan izin** — agent tetap lanjut ke modul berikutnya tanpa menunggu balasan pengguna.
4. **Kapan Agent BOLEH/HARUS Berhenti (Satu-satunya Pengecualian):**
   - Requirement yang secara teknis mustahil atau kontradiktif langsung (sudah diatur di 0.2.5) — berhenti, laporkan, tunggu arahan.
   - Kebutuhan kredensial/akses eksternal yang tidak tersedia (API key, akun pihak ketiga) yang memblokir kelanjutan build.
   - Batas kapasitas teknis platform/agent itu sendiri (mis. sesi terputus, limit konteks) — jika ini terjadi, agent WAJIB memastikan `BUILD_LOG.md`/`MASTER_CHECKLIST.md` sudah ter-update sebelum berhenti, sehingga sesi berikutnya (dari instruksi 0.2.1) bisa lanjut otomatis tanpa perlu penjelasan ulang dari pengguna.
   - Di luar dua kondisi di atas, TIDAK ADA alasan lain untuk berhenti dan meminta konfirmasi.
5. **Test & Regresi Tetap Wajib, Tapi Otomatis-Internal.** "Tanpa konfirmasi" TIDAK berarti melewati pengujian (Section 0.1.5–0.1.6, Section 33) — pengujian tetap wajib dan dilakukan sendiri oleh agent sebagai bagian dari alur kerja, bukan sebagai titik henti untuk menunggu approval pengguna. Kegagalan test ditangani sendiri oleh agent (perbaiki → uji ulang) sebelum lanjut, sesuai 0.1.7 & 33.3 — bukan dilempar ke pengguna sebagai pertanyaan "apakah boleh saya perbaiki?".
6. **Definition of Done Tidak Berubah.** Mode otonom ini mempercepat *proses* (tidak ada jeda menunggu konfirmasi antar-modul), tapi TIDAK mengurangi standar kelengkapan/kualitas di Section 0.6 — proyek tetap harus mencapai kriteria selesai 100% yang sama ketatnya.

### 0.8 LARANGAN KERANGKA/SKELETON KOSONG — WAJIB FUNGSIONAL UTUH DARI AWAL

> Ini adalah penegasan paling penting dalam seluruh dokumen: **PRD ini meminta aplikasi yang benar-benar bisa dipakai untuk menggambar sungguhan, bukan mockup UI, bukan prototipe tampilan, bukan kerangka yang "nanti disambung logikanya".** Setiap kali agent menyelesaikan sebuah modul, hasilnya WAJIB berupa fitur yang berfungsi nyata dan bisa langsung dicoba oleh pengguna saat itu juga.

1. **UI Tanpa Logika = Belum Selesai.** Tombol, slider, menu, atau panel yang tampil di layar tapi belum tersambung ke fungsi nyata TIDAK BOLEH ditandai selesai di `MASTER_CHECKLIST.md`, TIDAK BOLEH dilaporkan sebagai "modul X selesai", dan TIDAK dihitung lulus test manapun di Section 33. Tampilan visual saja (styling, layout) hanyalah tahap awal pengerjaan modul, bukan hasil akhirnya.
2. **Tidak Ada "Coming Soon" / "Fitur Belum Aktif".** Dilarang keras menyisakan teks placeholder seperti "Fitur ini akan segera hadir", tombol yang menampilkan toast "belum diimplementasi", atau menu yang di-disable permanen karena logikanya belum ditulis. Jika sebuah fitur ada di PRD ini, fitur itu WAJIB benar-benar bekerja sebelum modul terkait dianggap `DONE`.
3. **Build per Modul = Vertical Slice, Bukan Horizontal Layer.** Agent WAJIB membangun setiap modul sebagai potongan utuh dari atas ke bawah (UI + state + logika + rendering + penyimpanan sekaligus untuk fitur tersebut), BUKAN membangun seluruh UI aplikasi dulu (semua panel/tombol untuk 34 modul) baru kemudian mengisi logikanya belakangan. Urutan yang benar: selesaikan satu fitur secara utuh dan bisa dipakai → baru pindah ke fitur berikutnya (selaras dengan aturan sequential di Section 0.1).
4. **Uji "Bisa Dipakai Sungguhan" Sebagai Syarat Lulus.** Selain test case teknis di Section 33, setiap modul juga WAJIB lulus uji sederhana: *"Jika pengguna awam membuka aplikasi sekarang dan mencoba fitur ini, apakah benar-benar berfungsi seperti yang dijanjikan PRD, dari klik pertama sampai hasil akhir terlihat di kanvas/file?"* Jika jawabannya tidak, modul belum `DONE` — tidak peduli seberapa rapi tampilannya.
5. **Contoh Konkret Penerapan (Tidak Boleh Terjadi):**
   - Panel Brush tampil lengkap dengan semua slider (size, hardness, density, dst) tapi menggeser slider tidak mengubah goresan sungguhan di kanvas → SALAH, belum selesai.
   - Tombol "Export PNG" ada dan terlihat bagus tapi mengklik tidak menghasilkan file apa pun atau hanya menghasilkan kanvas kosong → SALAH, belum selesai.
   - Menu Filter berisi 25 item lengkap namanya tapi separuhnya hanya menutup dialog tanpa mengubah piksel → SALAH, belum selesai.
   - Tool Screentone terlihat di toolbar tapi mengklik area kanvas tidak menghasilkan pola halftone apa pun → SALAH, belum selesai.
6. **Prioritas Saat Waktu/Sumber Daya Terbatas.** Jika agent harus memilih antara mengerjakan **lebih sedikit fitur tapi masing-masing 100% fungsional**, versus **lebih banyak fitur tapi setengah jadi/sekadar kerangka**, agent WAJIB memilih opsi pertama, dan mencatat fitur yang belum sempat dikerjakan secara eksplisit di `KNOWN_ISSUES.md` (bukan disamarkan sebagai "selesai"). Namun ini hanya jalan keluar darurat — target akhir tetap seluruh 34 modul selesai fungsional penuh sesuai Section 0.6.

---

## 1. GAMBARAN UMUM PRODUK

**Nama Produk:** AuthorSultra Paint V3 (CSP-Killer Edition)
**Jenis Aplikasi:** Aplikasi Lukis & Komik Digital (Digital Painting + Manga/Comic Authoring) berbasis Web/Electron
**Target Platform:** Desktop (Windows via Electron/NW.js), Browser (Chrome/Edge modern), Neutralino, dioptimalkan agar tetap mulus di laptop/PC lawas (low-end hardware)
**Inspirasi Desain:** Paint Tool SAI v2 (UI/UX dasar) + Clip Studio Paint (kelengkapan brush engine, vector, manga tools, filter, dan perspective ruler) — dengan target **melampaui** kelengkapan fitur CSP di area brush engine, ruler, dan performa di hardware rendah
**Arsitektur:** Single-file/modular HTML5 + Canvas2D sebagai rendering utama, dengan lapisan akselerasi opsional WebGL2 untuk kompositing & stamping brush (lihat Section 30), Web Worker/OffscreenCanvas untuk tugas berat (filter, encode timelapse, tile compositing)
**Ukuran File:** Modular (dipecah per-engine untuk maintainability), total setara ±20,000+ baris kode
**Bahasa UI:** Bahasa Indonesia (ID), dengan struktur i18n siap untuk bahasa lain di masa depan (tidak wajib diimplementasi sekarang, tapi struktur kode harus tidak menghalangi)

### 1.1 Tolok Ukur Kompetitif (Wajib Dipenuhi)
Aplikasi ini WAJIB setara atau lebih unggul dari Clip Studio Paint pada seluruh aspek berikut:
1. **Kelengkapan Brush Engine** — pensil bertekstur asli, pena, kuas cat (bristle), airbrush, marker, watercolor dengan pigment-mixing, blur, smudge, effect pen, scatter brush, chalk/pastel, dip pen, glitter/pattern stamp — lihat Section 6.
2. **Dukungan Pen Tablet Penuh** — pressure, tilt, azimuth/barrel rotation, palm rejection, adjustable stabilizer — lihat Section 13.
3. **Ruler & Perspective System** — termasuk perspective 1/2/3-titik, curve ruler, radial ruler, symmetry — lihat Section 9.
4. **Manga/Comic Tools** — screentone (halftone), panel/frame border tool, speech balloon tool — fitur andalan CSP yang harus tersedia — lihat Section 31.
5. **Filter & Efek** — setara dengan filter CSP (Liquify, Tone Curve, Vibrance, Glow, Chromatic Aberration, Halftone, dll) — lihat Section 26 (perluasan menu Filter).
6. **Shortcut & Kustomisasi Total** — setiap tool, brush parameter, dan menu dapat di-remap — lihat Section 23.
7. **Performa Ultra Ringan** — tetap mulus (target minimum 30-60 FPS untuk stroke drawing) bahkan di laptop dengan CPU dual-core lama + integrated graphics + RAM 4GB — lihat Section 30.

---

## 2. ARSITEKTUR LAYOUT UTAMA (TOP-TO-BOTTOM)

### 2.1 Titlebar (`#titlebar`)
- Background: `#3a3a3a` (dark gray)
- Teks: `#eee`, bold, padding 3px 8px
- Terdiri dari:
  - **Logo:** Gambar PNG ter-embed base64 (22px tinggi), di kiri
  - **Judul:** "AuthorSultra Paint V1 - 22-07-2026"
  - **Sembunyi:** Otomatis disembunyikan (`display:none`) saat dijalankan dalam mode Electron/Neutralino (class `body.in-app`)
  - **Drag:** Tidak disebutkan explicit, native titlebar untuk window management

### 2.2 Menubar (`#menubar`)
- Background: `var(--panel)` = `#ececec`
- Border bawah: 1px solid `var(--bd)` = `#a8a8a8`
- **Menu Items:**
  1. **File:** New (Ctrl+N), Open (Ctrl+O), Open PSD (PSD multi-layer), Save (Ctrl+S), Save As (Ctrl+Shift+S), Export As Image (.png/.jpg/.webp), Export Timelapse (MP4/WebM/GIF/PNG Sequence), Stream/Live Mode, Recent Files, Close Tab (Ctrl+W), Exit
  2. **Edit:** Undo (Ctrl+Z), Redo (Ctrl+Shift+Z), Cut, Copy (Ctrl+C), Copy Merged (Ctrl+Shift+C), Paste (Ctrl+V), Paste as New Layer, Flip Horizontal, Flip Vertical, Clear Layer (Del), Fill Layer with FG Color, Select All (Ctrl+A), Deselect (Ctrl+D), Invert Selection, Selection from Alpha, Show/Hide Selection, Dilate Selection, Erode Selection, Select CPs/Strokes in Selection, Deselect All CPs
  3. **Image:** Resize Canvas (Ctrl+Alt+C), Resize Image (Ctrl+Alt+I), Crop to Selection, Rotate Canvas (CW/CCW/180), Flip Canvas (Horizontal/Vertical)
  4. **Layer:** New Raster Layer (Ctrl+Shift+N), New Vector Layer, New Folder, Import Image as Layer, Duplicate Layer, Delete Layer, Merge Down (Ctrl+E), Merge Folder, Merge Visible, Rasterize Layer, Convert to Vector Layer, Clear Layer, Fill Layer FG, Flip Layer (H/V), Transform (Ctrl+T), Alpha Lock, Clipping Group, Rename Layer, Layer Property, Lock/Unlock, Set as Private (Reference), Set as Live-Only
  5. **Filter:** Gaussian Blur, Motion Blur, Radial Zoom, Radial Spin, Mosaic/Pixelate, Sharpen, Unsharp Mask, Level Correction, Hue/Saturation/Lightness, Brightness/Contrast, Color Balance, Invert Color, Binarization, Posterize, Gradient Map, Tone Curve, Vibrance, Chromatic Aberration, Glow/Bloom, Vignette, Noise (Add/Reduce), Halftone/Screentone, Liquify, Perspective Warp (Mesh) — daftar lengkap & spesifikasi tiap filter ada di **Section 31**
  6. **View:** Zoom In (+), Zoom Out (-), Reset Zoom, Fit to Screen, Rotate, Flip View (H), Toggle Ruler/Symmetry, Reset Ruler, Toggle Paper, Toggle Grid
  7. **Selection:** Select All, Deselect, Invert, Selection from Alpha, Show/Hide Selection (Ctrl+H), Expand/Dilate, Contract/Erode, Feather, Transform Selection, Select CPs in Selection (vector), Select Strokes in Selection (vector)
  8. **Window:** Toggle Left Panel (F4), Toggle Right Panel (F5), Toggle Toolbar (F6), Toggle Status Bar, Fullscreen (F11)
  9. **Help:** About, Keyboard Shortcuts, Check for Updates, Performance Settings, Timelapse Quality Settings

- **Menu drop-down style:**
  - Background: `#f6f6f6`, border: 1px solid `#a8a8a8`
  - Shadow: `2px 3px 6px rgba(0,0,0,0.25)`, z-index: 100
  - Min-width: 230px, max-height: 70vh, overflow-y: auto
  - Item: padding 4px 22px 4px 12px, white-space nowrap
  - Hover: background `var(--sel)` = `#cfe0f5`
  - Disabled: color `#999`, no hover effect
  - Checked items: `✓ ` prefix + color `#1c3d78` + bold
  - Separator: `hr` border-top 1px solid `#c9c9c9`, margin 3px 0
  - Shortcut text: `.sc` class, color `#888`

### 2.3 Toolbar (`#toolbar`)
- Background: `var(--panel)`, padding 3px 8px
- Border bawah: 1px solid `var(--bd)`
- Layout: horizontal flex, gap 4px, wrap
- **Groups (`.grp`):**
  1. **Paper Texture Group:**
     - Checkbox "Paper Texture" (`#paperOn`)
     - Select (`#paperSel`): "Fine Paper" / "Canvas" / "Watercolor Rough" / "Custom…"
     - Range input opacity (`#paperOp`): 0-100, default 35
  2. **Ruler Group:**
     - Select (`#rulerSel`): Off / Straight (Mistar Lurus) / Ellipse/Circle / Parallel Lines / Concentric Ellipse / Vanishing Point (Titik Hilang) / Mirror Symmetry / Perspective 2-titik / Perspective 3-titik
     - Checkbox "Snap" (`#rulerSnap`): checked default
     - Label "Opac" + Range opacity (`#rulerOpac`): 10-100, default 85, display value (`#rulerOpacV`)
  3. **Symmetry Group (`#symGrp`):**
     - Checkbox "Symmetry" (`#symOn`): Dengan label `<b>`
     - Input number axis (`#symN`): min 1, max 16, default 2
     - Checkbox "Mirror" (`#symMirror`): checked default
     - Visual state: class `.on` = blue gradient background, `.off` = opacity 0.45 pada sub-elements
  4. **Timelapse Group:**
     - Checkbox "⏺ Timelapse" (`#tlToggle`): checked default
  5. **Live Stream Button:**
     - Button "🎥 Live" (`#btnStreamWin`): background `#2a4365`, color white, border `#4299e1`, font bold
     - Title: "Buka Pop-out Live View (Secret Layer Tersembunyi) (Ctrl+Shift+L)"
     - Berubah jadi 🔴 merah saat live active
  6. **Stabilizer Group:**
     - Button "-" (`#stabDn`): class `stabbtn` (20x20px, border 1px, gradient bg)
     - Range (`#stabRange`): 0-22, step 1, default 18, width 96px
     - Button "+" (`#stabUp`)
     - Label (`#stabLbl`): display nilai, min-width 30px, centered, bold, color `#1c3d78`
     - Mode: 0-15 = normal (angka), 16-22 = S-1 s/d S-7 (heavy pull-string)

### 2.4 Main Area (`#main`)
Flex container: flex:1, min-height:0. Di dalamnya ada:

#### 2.4.1 Left Column (`#colLeft`) — Width 224px
- Background: `var(--panel)`, overflow-y auto, scrollbar custom
- **Panel Navigator (`#pNav`):**
  - Header "Navigator" dengan tombol collapse (▾)
  - Canvas navigator (`#navCanvas`): 208x118px, background `#bbb`
  - Slider Zoom: label "Scale", range min -4 max 5 step 0.01, value display (`#zoomVal`)
  - Slider Rotasi: label "Rot", range min -180 max 180 step 1, value display (`#rotVal`)
  - Baris tombol mini:
    - Zoom Out ( − )
    - Zoom In ( + )
    - Reset Zoom ( ◎ )
    - Reset Rotasi ( ⟲ )
    - Flip View H ( ⇋ )
    - Fit to Screen ( ▣ )
    - Indikator FLIP (`#flipInd`): merah, tersembunyi

- **Panel Layer (`#pLayer`):** flex:1 (mengisi sisa tinggi)
  - Header "Layer" dengan collapse
  - **Layer Controls (`#layerCtrls`):** grid 2px gap
    - New Layer (Ctrl+Shift+N): SVG icon kertas + plus jingga
    - New Vector/Linework: SVG icon path hijau
    - New Folder/Grup: SVG icon folder kuning + plus
    - Import Layer Gambar: SVG icon gambar + foto
    - Add Layer Mask: SVG icon lingkaran setengah hitam
    - Duplicate Layer: SVG icon dua kertas tumpuk
    - Merge Down (Ctrl+E): SVG icon panah merge
    - Merge Folder: SVG icon folder + panah merge
    - Move Layer Up: SVG icon panah naik
    - Move Layer Down: SVG icon panah turun
    - Clipping Group: SVG icon panah lengkung clip
    - Lock/Unlock: SVG icon gembok
    - Reference ⇄ Normal: SVG icon konversi
    - Delete Layer: SVG icon tong sampah merah
  - **Mask Controls (`#maskCtrls`):** tersembunyi default, muncul (`.on` = flex) saat layer punya mask
    - Label "Mask:", button Invert, Reset, Hapus
    - Hint text warna `#b64a00`: "Editing Mask"
  - **Layer Properties (`#layerProps`):** grid 3 kolom (44px 1fr 40px)
    - Row 1: Mode blend (select 18 opsi: Normal, Multiply, Screen, Overlay, Add/Shine, Soft Light, Hard Light, Darken, Lighten, Dodge, Burn, Difference, Exclusion, Hue, Saturation, Color, Luminosity)
    - Row 2: Opacity (range 0-100), value display
    - Row 3: Alpha Lock checkbox (preserve opacity)
    - Row 4: Clipping Group checkbox
  - **Layer List (`#layerList`):** border 1px, background `#f2f2f2`, flex:1, min-height 120px, overflow-y auto
    - Setiap baris layer (`.layer-row`): flex, gap 5px, padding 3px 5px
    - **Status baris layer:**
      - `.active`: background `#cfe0f5`, shadow inset kiri jingga 3px
      - `.private`: background `#dbe4f0` (reference/secret)
      - `.liveonly`: background `#fff3d6` (hanya tampil di stream)
      - `.vector`: background `#e8f0e0` (hijau vector)
      - `.folder`: background `#faf3dd` (kuning folder)
      - `.clipped`: background `#eef3fb` (clipping child)
      - `.maskedit`: shadow inset 3px jingga + border jingga
      - `.drag-src`: opacity 0.45 (sedang di-drag)
      - `.drop-above`: shadow inset top 3px jingga
      - `.drop-below`: shadow inset bottom 3px jingga
      - `.drop-into`: background `#ffe9c2` + shadow inset 2px jingga (drop ke folder)
    - **Komponen baris layer:**
      - Leading icon (`.lay-lead`): 15x15px, SVG chevron untuk folder collapse, clip arrow jingga
      - Eye toggle (`.lay-eye`): 17x15px, SVG mata, cursor pointer
      - Thumbnail (`.lay-thumb`): 36x28px, border 1px, background kotak-kotak, canvas di dalamnya
      - Mask thumbnail (`.lay-mth`): 20x28px, border 1px abu, background putih
        - Hover: muncul tombol hapus lingkaran merah (13x13px, posisi absolute top-right)
        - `.editing`: outline 2px jingga
      - Layer name (`.lay-name`): flex:1, text-overflow ellipsis, bold (folder: warna `#7a5a10`)
      - Layer meta (`.lay-meta`): font 9px, color `#888`, text-overflow ellipsis
      - Right icons (`.lay-right`): absolute top-right
        - Lock icon (`.lay-lock`): font 9px
        - Type icon (`.lay-ic`): 15x15px, SVG untuk vector/raster/teks
  - **PRL Badge (`#prlBadge`):** margin-top 3px, warna `#365a9e`

#### 2.4.2 Splitter 1 (`#split1`)
- Width 5px, background panel, border kiri-kanan
- Cursor: col-resize, hover: warna `#cfe0f5`

#### 2.4.3 Right Column / Tools Column (`#colTools`) — Width 244px
- Background: `#f0f0f2` (SAI2 style)

- **Panel Color (`#pColor`):**
  - Tanpa header (hidden CSS), border none, border-bottom 1px `#d7d9dd`
  - **Color Toolbar (`#colorTb`):** flex wrap, gap 3px, margin-bottom 6px
    - Toggle Color Wheel (lingkaran)
    - Toggle RGB Sliders (tiga garis sejajar)
    - Toggle HSV Sliders (garis dengan segmen fill)
    - Toggle Mixer (tiga bar + titik-titik)
    - Toggle Swatch (9 titik grid)
    - Toggle Scratchpad (kotak + garis gelombang)
    - Tombol style (`.ctb-btn`): 26x22px, border radius 4px, gradient bg, `.on` = blue background + border + shadow inset
  - **Color Wheel (`#secWheel`):**
    - Canvas wheel (`#wheel`): 200x200px, wrapper (`#wheelWrap`) max-width 200px aspect-ratio 1:1
    - Canvas SV Box (`#svbox`): overlay di atas wheel, cursor crosshair
  - **RGB Sliders (`#secRGB`):**
    - 3 baris: R (red gradient), G (green gradient), B (blue gradient)
    - Masing-masing: label `<b>` (12px), range input (`.gslide`), value display (`.val`)
    - Range: 0-255
  - **HSV Sliders (`#secHSV`):**
    - 3 baris: H (0-360), S (0-100), V (0-100)
    - Format sama dengan RGB
  - **Color Mixer (`#secMixer`):**
    - 4 row mixer, masing-masing dengan chip warna (`.mixchip`: 18x18px, border 1px, border-radius 3px, cursor pointer)
    - Range input untuk mixing
  - **Swatches (`#secSwatch`):**
    - Grid 12 kolom, gap 1px
    - Setiap cell aspect-ratio 1:1, border 1px abu, cursor pointer
    - Hint text: "Ctrl+klik swatch = simpan warna", font 10px
  - **Scratchpad (`#secScratch`):**
    - Canvas (`#scratch`): 222x76px, background kotak-kotak, cursor crosshair
    - Hint: "Scratchpad (tidak terekam) — klik 2x untuk bersihkan", font 10px abu
  - **Foreground/Background (`#fgbg`):**
    - 2 color box (`.colbox`): 26x26px, border 2px abu
    - Button swap: "⇄" (X)
    - Di dalam `#toolsRow`: orientasi vertikal

- **Panel Tools (`#pTools`):**
  - **Group Tabs (`#groupTabs`):** flex wrap, gap 2px, border-bottom 1px abu
    - Tab (`.gtab`): padding 3px 10px, border radius 5px 5px 0 0, background `#ececef`
    - Tab active: `.on` = background `#d8dcf8`, border `#8a93d8`, color `#20255e`, bold
    - Klik kanan: menu New Group / Delete Group / Property
  - **Tools Row (`#toolsRow`):** flex, gap 6px
    - **Tool Grid (`#toolGrid`):** 6 kolom, gap 2px
    - Tombol tool (`.tool-btn`): aspect-ratio 1:1, border 1px `#c6c9ce`, bg `#fbfbfc`, border-radius 3px
    - SVG icon 17x17px
    - Hover: bg `#eef4ff`, border `#9db4dd`
    - Active (`.on`): bg `#d8e6fb`, border `#4a6ea9`, shadow inset 1px
    - **Tool list (28 tools):**
      1. Pen/Brush (gaya kuas biasa)
      2. Eraser (penghapus)
      3. Bucket (fill)
      4. Eyedropper/Picker
      5. Gradation (gradasi linear)
      6. Selection Rectangle
      7. Selection Ellipse
      8. Lasso (free/poly/polyfree)
      9. Magic Wand (similar/transparent/all)
      10. Selection Pen
      11. Selection Eraser
      12. Text (layer teks)
      13. Move (pindah layer)
      14. Zoom
      15. Rotate
      16. Hand/Pan
      17. Transform (Ctrl+T)
      18-19. Common tools (VLineC/StrC, VCurveC/CurveC)
      20-28. Vector tools (Pen, Line, Curve, Edit, Pressure, Eraser, Weight, Color, Scale, Symp)

- **Panel Brush (`#pBrush`):**
  - Header dinamis "Layer Tools" (berubah sesuai konteks)
  - **Brush Buttons (`#brushBtns`):**
    - ＋ Baru (buat brush kustom)
    - ✎ Edit (edit brush terpilih)
    - 🗑 Hapus (hapus brush kustom)
    - ⇩ Import (.abr Photoshop, .aspbrush, atau gambar tip)
    - ⇧ Simpan (export brush sebagai .aspbrush)
  - **Brush List (`#brushList`):** grid 4 kolom, gap 2px
    - Max height: 330px, min-height 96px, overflow-y auto, resize vertical
    - Item (`.brush-item`): 42px tinggi, border 1px `#c9ccd1`, bg putih, border-radius 2px
    - Nama brush (`.bn`): font 9px, bold, posisi kiri-atas, warna biru kalau special
    - Ikon brush (`.bic`): 18x18px SVG, posisi kanan-bawah
    - Active: bg `#d8e6fb`, border `#4a6ea9`
    - Link reference: nama biru (`.linkref .bn` color `#2b56c9`)
  - **Symp Settings (`#sympSettings`):** border 1px abu, bg `#f7f7f7`, padding 4px
    - Row tombol mode: "▼ Decrease" / "▲ Increase" (`.symp-mode`)
    - Slider Ukuran (px): range 0.1-100, default 4
    - Slider Radius: range 4-200, default 40
  - **Stroke Preview Canvas (`#strokePreview`):** 222x32px, border 1px, bg gelap, klik cycle bg
  - **Brush Mode Row (`#brushModeRow`):** grid 2 kolom
    - Select blend mode brush (independen dari blend layer)
    - AA Area (`#aaArea`): flex, button mode (ikon 5 tingkat / slider 0-100%), tombol AA 5 tingkat, range slider 0-100, value
  - **Brush Parameters (slider `.sbar` class):**
    1. **Brush Size:** 1-1000, default 12, fill gradient biru style SAI2
    2. **Min Size:** 0-100%, default 0 (persentase ukuran minimum oleh tekanan)
    3. **Density:** 1-100, default 100 (kepadatan tinta)
    4. **Min Density:** 0-100%, default 100
    5. **Hardness:** 0-100, default 80 (ketajaman tepi kuas, 100 = biner pixel-perfect)
    6. **Anti-Alias:** 0-100%, default 100% (tersembunyi default, 0% = pixel-perfect biner)
    7. **Spacing:** 1-1000%, default 1 (jarak antar stamp, 1% = paling rapat)
    8. **Scatter:** 0-200, default 0 (sebaran acak kuas)
    9. **Blending:** 0-100, default 50 (Water Color mode, seberapa kuat tinta campur)
    10. **Dilution:** 0-100, default 0 (Water Color, pengenceran air)
    11. **Persistence:** 0-100, default 50 (Water Color, persistensi warna dibawa sepanjang goresan)
    12. **Blur Width:** 10-100, default 50% (Blur mode, seberapa jauh piksel digeser)
  - **Brush Form Row (`#rowForm`):** select bentuk ujung kuas (Simple Circle atau kustom PNG/.abr)
    - Input "Mess.": keacakan rotasi 0-100
  - **Brush Texture Row (`#rowTex`):** select tekstur kertas (No Texture, Kertas Gambar, Kanvas, Watercolor Kasar, atau kustom)
    - Input "Intens.": intensitas 0-100, default 95
  - **Size Presets (`#sizePresets`):** grid 5 kolom, max-height 158px, overflow-y auto
    - Setiap tombol: tinggi 34px, isi dot lingkaran ukuran berbeda + teks
    - Active: bg `#d8e6fb`, border biru, shadow inset

- **Panel Vector (`#pVec`):** tersembunyi default, muncul saat layer vector aktif
  - Weight % slider (10-400, default 100)
  - Eraser mode: "Seluruh garis" / "Potong segmen"
  - Tombol: Simplify (RDP), Smooth, Warna→FG, Hapus garis
  - Info (`#vInfo`): jumlah garis dan titik

- **Panel Teks (`#textPanel`):** hanya muncul saat layer teks aktif
  - Textarea konten: 2 rows, resize vertical, font 12px sans-serif
  - Font select: Sans-serif, Serif, Monospace, Georgia, Times New Roman, Arial, Courier New, Impact, Comic Sans, Tahoma, Verdana
  - Ukuran slider: 6-400, default 64
  - Kemiringan slider: -60° s/d +60°, default 0
  - Spasi huruf: -20 s/d +80
  - Jarak baris: 60-300%, default 120%
  - Rata: Kiri/Tengah/Kanan
  - Tombol: B (bold), I (italic), color picker
  - Tombol: Ke Tengah, Warna→FG, Rasterize

#### 2.4.4 Splitter 2 (`#split2`)
- Sama dengan splitter 1, memisahkan colTools dari canvas area

#### 2.4.5 Canvas Area
- **Canvas Wrap (`#canvasWrap`):**
  - Flex:1, position relative, background `#909090`, overflow hidden
  - Cursor: crosshair
  - **Empty Workspace Placeholder (`#emptyWorkspace`):**
    - Muncul saat tidak ada kanvas terbuka
    - Background: `#232529`, flex column center
    - Icon 🎨 38px, teks "Tidak Ada Kanvas Terbuka"
    - Dua tombol: "＋ Kanvas Baru (Ctrl+Alt+N)" dan "📁 Buka File (Ctrl+O)"
  - **Viewport Canvas (`#viewport`):** absolute, top:0 left:0, touch-action none
  - **Overlay Canvas (`#overlay`):** absolute, top:0 left:0, pointer-events none, z-index 4
    - HiDPI scaled (dikalikan devicePixelRatio)
  - **Cursor Ring (`#cursorRing`):** absolute, border 1px hitam + outline 1px putih
    - Border-radius 50%, pointer-events none, tersembunyi default
    - Pseudo-element: titik tengah 3x3px hitam + shadow putih
    - `.symp`: border 1.5px biru `#3d63c0`, background transparan biru 20%
  - **Eyedropper Loupe (`#eyedropperLoupe`):**
    - Position fixed, 130x160px, z-index 9999, hidden default
    - Canvas kaca pembesar: 130x130px, border-radius 50%, border 3px putih
    - Badge: background gelap semi-transparan, teks hex warna, swatch bulat 12px
    - Grid 11x11 piksel diperbesar + crosshair center
  - **Scrollbars:**
    - Horizontal bar (`#hbar`): absolute bottom:0, height 13px, bg `#d6d6d6`
    - Vertical bar (`#vbar`): absolute right:0, width 13px, bg `#d6d6d6`
    - Thumb: bg `#adadad`, border 1px `#8a8a8a`, hover/drag jadi `#8faccf`
    - Corner (`#scrollCorner`): 13x13px, bg `#cfcfcf`

- **Document Tab Bar (`#docTabBar`):**
  - Background `#d8dadf`, border-top 1px `#a4a8af`, height 26px
  - Layout flex, gap 2px, padding 2px 6px, overflow-x auto
  - **Tabs List (`#docTabsList`):**
    - Tab (`.doc-tab`): flex inline, gap 5px, padding 2px 8px, border 1px, border-radius 3px
    - Background gradient `#f4f5f8` → `#d4d7dc`
    - Active: bg gradient `#e4ebfc` → `#c8d4f8`, border `#5c6bc0`, color `#1a237e`, bold
    - Komponen: icon 🖼️, title (max-width 95px ellipsis), dirty mark (* merah), zoom %, close button (✕)
    - Close hover: bg merah `#e57373`, teks putih
  - **New Tab Button (`#btnNewDocTab`):** 20x20px, "+", style sama seperti tab

### 2.5 Statusbar (`#statusbar`)
- Height: 24px, bg `var(--panel)`, border-top 1px abu
- Layout: flex, gap 12px, padding 0 10px, font 11px
- **Left items:**
  - `#stTool`: nama tool aktif (e.g. "Pen")
  - `#stZoom`: zoom level
  - `#stPos`: posisi kursor (x, y)
  - `#stSel`: status seleksi
- **Right items:**
  - `#recDot`: "⏺ REC [N]f" — indikator timelapse, merah saat recording, abu saat paused, cursor pointer
  - `#stMem`: estimasi penggunaan memori

### 2.6 Modal (`#modalBack` + `#modal`)
- Backdrop: fixed inset 0, bg rgba(0,0,0,0.35), z-index 200, flex center, hidden default
- Modal box: bg `#f4f4f4`, border 1px `#777`, shadow 3px 4px 10px
- Min-width 320px, max-width 560px, max-height 86vh, flex column
- Header (`.mh`): bg `#3a3a3a`, color `#eee`, padding 4px 10px, bold
- Body (`.mb`): padding 12px, overflow-y auto
- Footer (`.mf`): padding 8px 12px, flex gap 8px, justify-content flex-end, border-top 1px abu

### 2.7 Toast (`#toast`)
- Position fixed, bottom 36px, left 50%, translateX -50%
- Background `#333`, color white, padding 6px 16px, border-radius 2px
- Z-index 300, max-width 70%, hidden default

### 2.8 Context Menu (`#saiCtx`)
- Position fixed, z-index 400, bg white, border 1px `#9a9a9a`
- Shadow `2px 3px 8px rgba(0,0,0,0.35)`, min-width 170px
- Item (`.mi`): padding 6px 22px, hover bg `#dbe6fb`, white-space nowrap
- Separator: margin 3px 0, border-top 1px `#ddd`

---

## 3. SISTEM WARNA (CSS CUSTOM PROPERTIES)

```
--panel:     #ececec   (latar panel utama)
--panel2:    #e4e4e4   (latar panel sekunder)
--bd:        #a8a8a8   (border utama)
--bd2:       #c9c9c9   (border sekunder)
--txt:       #222       (teks utama)
--accent:    #e05a00   (aksen jingga)
--sel:       #cfe0f5   (warna seleksi biru muda)
--prl:       #dbe4f0   (warna private/reference layer)
--vec:       #e8f0e0   (warna vector layer hijau)
```

---

## 4. SISTEM DOKUMEN & MULTI-TAB (MULTI-CANVAS)

### 4.1 Document State
Setiap dokumen memiliki:
- `id`: unique ID
- `name`: nama tab ("NewCanvasN" atau dari file)
- `doc`: { w, h, dpi (default 300) }
- `layers[]`: array layer (flat, dengan parent pointer)
- `active`: indeks layer aktif
- `layerSeq`: counter ID layer global
- `view`: { zoom, x, y, rot, flip }
- `undoStack[]`: max 30 entries
- `redoStack[]`: max 30 entries
- `timelapse`: snapshot timelapse (frames, lazyFile reference)
- `paper`: { on, kind, opacity, tile, custom }
- `ruler`: { mode, snap, line, ellipse, parallel, concentric, vps[], sym }
- `filePath`: path file .asp (kalau dari disk)
- `dirty`: flag perubahan belum disimpan

### 4.2 Multi-Tab API
- `documents[]`: array semua dokumen terbuka
- `activeDocIdx`: indeks dokumen aktif
- Fungsi:
  - `addNewDocument(name, w, h)`: buat tab baru, reset timelapse
  - `switchDocument(idx)`: pindah tab (simpan state sebelumnya, load yang baru)
  - `closeDocumentTab(idx)`: tutup tab (konfirmasi kalau dirty)
  - `createDocSnapshot()`: simpan state dokumen ke snapshot
  - `loadDocSnapshot(snap)`: pulihkan dokumen dari snapshot
  - `isDocEmpty(doc)`: cek apakah kanvas kosong (hanya Background)
  - `openFileInSmartTab(fn)`: buka file di tab baru/reuse tab kosong

---

## 5. SISTEM LAYER

### 5.1 Tipe Layer
1. **Raster Layer (`type: "raster"`):**
   - Canvas ukuran dokumen
   - Operasi: draw pixel, stamp brush, filter, fill, clear
   - Properti: opacity, blend mode, alpha lock, clipping group, lock, visibility

2. **Vector Layer (`type: "vector"`):**
   - Array `strokes[]` di mana setiap stroke punya `pts[]` (x, y, w) + `color` + `opacity`
   - Operasi: draw vector, edit titik, pressure, eraser, line, curve
   - Bisa di-rasterize ke raster, atau raster di-trace balik ke vector

3. **Text Layer (`type: "text"`):**
   - Canvas hasil render teks, properti `textProps` berisi content, font, size, color, bold, italic, skew, letter spacing, line height, align, posisi x/y
   - Dapat diedit ulang (non-destructive)
   - Bisa di-rasterize

4. **Folder/Group (`type: "folder"`):**
   - Tidak punya canvas sendiri
   - Properti: collapsed (expand/collapse)
   - Fungsi: mengorganisir layer, render rekursif ke kompositor
   - Mask bisa diterapkan ke folder (memengaruhi seluruh isi)
   - Merge folder: flatten semua anak ke satu layer raster

### 5.2 Layer Properties
- `id`: integer unik (auto-increment `layerSeq`)
- `name`: string
- `visible`: boolean
- `opacity`: 0.0 - 1.0
- `blend`: string (18 mode: source-over, multiply, screen, overlay, lighter, soft-light, hard-light, darken, lighten, color-dodge, color-burn, difference, exclusion, hue, saturation, color, luminosity)
- `private`: boolean (layer referensi, tidak tampil di export/timelapse/public viewer)
- `liveOnly`: boolean (hanya tampil di live stream, tersembunyi di editor)
- `alphaLock`: boolean (preserve opacity — hanya mewarnai piksel existing)
- `clip`: boolean (clipping group — terpotong oleh alpha layer di bawahnya)
- `locked`: boolean (tidak bisa digambar/diedit)
- `parent`: integer|null (ID folder parent)
- `mask`: canvas (grayscale — putih=tampil, hitam=sembunyi)
- `maskEnabled`: boolean
- `maskLinked`: boolean (link/unlink mask ke layer)

### 5.3 Layer Tree Operations
- Flat array dengan `parent` pointer → tree dibangun via `ensureLayerTree()`
- Fungsi tree:
  - `childrenOf(parentId)`: dapatkan child langsung
  - `layerById(id)`: lookup by ID
  - `isDescendant(node, ancestorId)`: cek ancestry
  - `anyAncestorCollapsed(node)`: cek apakah ada folder parent yang collapsed
  - `depthOf(node)`: hitung kedalaman
  - `collectDescendants(node)`: semua child rekursif
  - `siblingIndex(node)`: posisi di antara siblings

### 5.4 Layer Drag & Drop
- Geser layer di list untuk reorder / memasukkan ke folder
- Visual feedback: drop indicator (above, below, into)
- Rule:
  - Tidak bisa drop folder ke dalam dirinya sendiri (circular)
  - Tidak bisa merge Secret/Live dengan layer normal (keamanan)
  - Folder collapse/expand otomatis saat drop

### 5.5 Layer Mask Editing
- Mode edit mask: gambar langsung ke canvas mask (putih=tampilkan, hitam=sembunyikan)
- Ruby overlay: area merah transparan di area tersembunyi oleh mask
- Operasi mask: Invert, Reset (tampil semua), Hapus
- Mask proxy: saat edit mask, brush stamp diarahkan ke canvas mask

### 5.6 Private/Secret Layer (PRL) & Live-Only
- **Private/Reference (PRL):** layer yang hanya tampil di editor (sebagai referensi), TIDAK tampil di:
  - Export gambar
  - Timelapse recording
  - Live stream viewer
  - Thumbnail publik
  - Tampilan layer publik lainnya
- **Live-Only:** layer yang HANYA tampil di live stream viewer, TIDAK tampil di editor
- Aturan kompositor: SATU kompositor, dua mode includePrivate (true = editor, false = publik)

---

## 6. BRUSH SYSTEM

### 6.1 Brush Object Structure
```javascript
{
  name: string,
  mode: "draw" | "air" | "water" | "blur" | "smudge",
  tip: Image|null,        // bentuk ujung kuas (custom)
  tipURL: string,          // data URL untuk persist
  tipName: string,
  spacing: 1-1000,         // % jarak antar stamp (1 = terapat)
  scatter: 0-200,          // sebaran acak
  hard: 0-100,             // kekerasan tepi (100 = biner)
  dens: 1-100,             // kepadatan
  minDens: 0-100,          // kepadatan minimum (tekanan ringan)
  min: 0-100,              // ukuran minimum % (pressure)
  size: 1-1000,            // ukuran kuas (diameter px)
  aa: 0-100,               // anti-alias level
  colorMode: "fg"|"fgbg"|"multi"|"multirnd",
  stops: [{t:0-1, c:[r,g,b]}],  // gradasi warna
  cycle: 40-1200,           // panjang siklus gradasi (px)
  angle: -180..180,         // sudut dasar tip
  rotJit: 0-180,            // jitter rotasi
  followStroke: boolean,    // arah tip ikut goresan
  sizeJit: 0-100,           // jitter ukuran %
  opacityJit: 0-100,        // jitter opasitas %
  scatterCount: 1-8,        // jumlah stempel per stamp
  texKind: string,          // jenis tekstur
  texInt: 0-100,            // intensitas tekstur
  texName: string,
  wcBlend: 0-100,           // Water Color blending
  wcDil: 0-100,             // Water Color dilution
  wcPers: 0-100,            // Water Color persistence
  keepOp: boolean,          // hanya mewarnai piksel existing
  blurW: 10-100,            // Blur width %
  blendMode: string,        // mode campuran brush (independen layer)
  custom: boolean           // flag brush kustom (bukan default)
}
```

### 6.2 Brush Modes
1. **Draw:** Stamp lingkaran solid/hardness gradient, mendukung tip custom
2. **Air:** Radial gradient transparan, density rendah (0.5x), semakin tebal di tengah
3. **Water:** Mencampur warna dari kanvas + warna FG. Dilution mengurangi opacity. Persistence menentukan seberapa jauh warna yang diserap dibawa. Blending seberapa kuat campuran
4. **Blur:** Menduplikasi piksel canvas di sekitar kuas (8 offset), efek blur/sebar
5. **Smudge:** Seperti Water tapi dengan mix rate 85%, menyeret warna kanvas

### 6.3 Brush Color Modes
1. **1 Warna (FG):** Warna foreground tunggal
2. **2 Warna (FG↔BG):** Interpolasi FG ke BG berdasarkan pressure
3. **Multi-warna (gradasi):** Cycle melalui color stops sepanjang goresan
4. **Multi-warna (acak):** Random pick dari color stops tiap stamp

### 6.4 Stamp Cache System
- Cache stamp per kombinasi (warna, hardness, size, anti-alias, tekstur) di `stampCacheMap`
- Key format: `"R,G,B_hardness_size_aaQuantized_texKey"`
- Dibersihkan (`clearStampCache()`) saat parameter brush berubah

### 6.5 Brush Tip Processing (Custom Shape)
- PNG transparent → digunakan langsung sebagai shape mask
- PNG opaque → dikonversi: alpha = 255 - luma (gelap = pekat), RGB dijadikan putih
- Corner/background detection: cek alpha 4 sudut, bedakan PNG transparan vs opaque
- Hasil: `_processedTipCanvas` di-cache
- Saat stamp: tip di-scale, di-tint dengan warna FG, diterapkan hardness radial mask, lalu tekstur

### 6.6 Pixel-Perfect Binary Rendering
- Hardness 100% + AA 0%: `fillHardDisc()` — sampling baris piksel utuh, ZERO piksel abu-abu
- Hardness 100% + AA >0%: `fillHardDiscAA()` — interior solid + edge fractional coverage
- Hardness <100%: radial gradient standar

### 6.7 Path Accumulator (Smooth Stroke)
- Saat brush standar (non-tip, non-scatter, non-pixel-perfect): titik dikumpulkan di `_pathBuf`
- Di-flush sebagai filled polygon (quad strip) dengan variable width
- Menghilangkan artefak "bumps" antar stamp → goresan mulus seperti SAI2
- Round joins antar segmen, ujung tidak pakai round cap (tip tajam)

### 6.8 Stroke Segment Rendering
- Quadratic Bézier antar titik-tengah → mulus, tidak patah-patah
- Spacing seragam sepanjang busur (berdasarkan `_spacingFor(pr)`)
- Pressure di-interpolasi mulus antar titik
- Saat stroke berakhir: ekor lancip mengecil ke 0 (taper)

### 6.9 Brush Texture System
- Canvas texture tile di-generate dari:
  - Paper texture (Perlin-noise 2D pada grid)
  - Canvas texture (gelombang sinus + noise)
  - Watercolor rough (noise halus + grain)
  - Custom image (dari user)
- Diaplikasikan via `destination-in` + pattern fill
- Cache tile per (kind, intensity quantized 5%)

### 6.10 Size Presets
- Grid 5 kolom, scrollable, max-height 158px
- Tombol preset: tinggi 34px, dot lingkaran ukuran sesuai preset + teks
- Default: berbagai ukuran kuas standar

### 6.11 PERLUASAN WAJIB — Engine Media Realistis per Kategori Brush

> Setiap kategori berikut WAJIB dirender dengan algoritma berbeda (bukan sekadar variasi opacity dari brush dasar). Tujuannya agar pengguna dapat **membedakan secara visual** pensil vs pena vs kuas vs airbrush vs marker vs watercolor hanya dari tampilan goresan.

**A. PENSIL (Pencil)**
- Rendering: kombinasi banyak titik noise mikro (grain map) yang mengikuti tekstur kertas aktif — grafit menumpuk lebih gelap di lekukan tekstur kertas (mirip "tooth" kertas asli).
- Parameter khusus: `graphiteGrain` (0-100, kepadatan butiran), `paperTooth` (mengikuti brush texture terpilih), `pressureToGrainDensity` (tekanan lebih besar → grain makin rapat/gelap, bukan hanya makin besar).
- Sensitif tekanan tinggi: ukuran, opacity, DAN kepadatan grain semua bereaksi terhadap pressure curve.
- Varian preset wajib: Pensil 2B (lembut, grain besar), Pensil HB (medium), Pensil 2H (keras, grain halus & terang).

**B. PENA / PEN (termasuk Dip Pen & Ballpoint)**
- Rendering: tepi tajam solid (hardness tinggi default), tanpa grain, garis presisi dengan taper otomatis di ujung goresan (start/end thinning).
- Varian **Dip Pen**: line-width sangat sensitif terhadap tekanan (min-size rendah), sedikit "wet ink pooling" di titik awal goresan (blob kecil saat pena pertama menyentuh kanvas, mensimulasikan tinta menggenang).
- Varian **Ballpoint/Pulpen**: line-width nyaris konstan (min-size tinggi ~85%, kurang sensitif tekanan), tepi solid keras, tidak ada pooling.
- Parameter khusus: `inkPooling` (0-100), `taperStart`/`taperEnd` (px).

**C. KUAS / BRUSH (Bristle Simulation)**
- Rendering: multi-bristle procedural — satu stroke terdiri dari beberapa (5-24, parameter `bristleCount`) sub-stroke tipis paralel dengan offset acak kecil, mensimulasikan helai bulu kuas nyata, sehingga tepi goresan terlihat sedikit "berserat" bukan garis vector sempurna.
- Parameter khusus: `bristleCount` (5-24), `bristleSpread` (0-20px), `bristleStiffness` (0-100, memengaruhi seberapa rapat bristle saat tekanan tinggi — tekanan besar bristle merapat/flatten seperti kuas asli ditekan).
- Opsional "wet edge" tipis untuk simulasi cat basah menumpuk di tepi (lihat Watercolor).

**D. AIRBRUSH**
- Rendering: BUKAN stamp diskrit — melainkan akumulasi kontinu berbasis waktu (`accumulateOverTime`), semakin lama kursor diam di satu titik (mouse-down tanpa gerak) semakin gelap/tebal, seperti airbrush fisik nyata.
- Falloff radial soft-edge sungguhan (Gaussian falloff, bukan linear gradient sederhana).
- Parameter khusus: `flowRate` (px opacity per 100ms diam), `fallOffRadius`, `nozzleSpread` (jitter posisi partikel semprot 0-30px), `particleDensity`.
- Harus tetap terasa halus walau kursor bergerak cepat (interpolasi flow rate sepanjang path, bukan hanya di titik stamp).

**E. MARKER / SPIDOL**
- Rendering: opacity build-up dengan **hard ceiling** (opacity tidak pernah melebihi nilai `maxOpacity` walau ditumpuk berkali-kali dalam satu stroke — mensimulasikan tinta marker yang jenuh), tepi solid flat, tidak ada gradient anti-alias lembut (tepi agak tegas seperti spidol asli).
- Overlap dalam satu goresan yang sama TIDAK menggelapkan lebih dari overlap antar goresan berbeda (single-stroke opacity cap), tapi overlap antar stroke berbeda tetap menumpuk normal — ini yang membedakan marker dari brush biasa.
- Parameter khusus: `maxOpacity` (default 85%), `edgeSharpness`.

**F. WATERCOLOR (Cat Air) — Wajib Setara CSP**
- Rendering pigment-mixing sungguhan: warna dari brush bercampur dengan warna EXISTING di kanvas berdasarkan `wcBlend`, bukan sekadar alpha-blend biasa — gunakan pencampuran subtraktif sederhana (approximate pigment mixing di ruang RGB dengan bobot) agar hasil campuran warna terasa "cat", bukan overlay digital datar.
- **Wet edges**: tepi goresan basah menumpuk pigmen lebih gelap (mensimulasikan air yang membawa pigmen ke pinggir saat mengering) — dirender sebagai cincin tipis lebih pekat di tepi luar stroke.
- **Paper texture absorption**: watercolor WAJIB menyerap tekstur kertas aktif lebih kuat dari brush lain (pigment "meresap" lebih banyak di lekukan kertas → variasi tone mengikuti noise kertas).
- Parameter (sudah ada di 6.1, dipertegas fungsinya): `wcBlend` (kekuatan campur), `wcDil` (pengenceran — makin tinggi makin transparan & makin menyebar), `wcPers` (persistence — seberapa jauh warna "terbawa" air sepanjang goresan berikutnya, mensimulasikan kuas basah yang membawa sisa pigmen).
- Preset wajib: Watercolor Flat, Watercolor Wet Edge, Watercolor Dry Brush (dilution rendah, grain tekstur kertas dominan).

**G. BLUR TOOL**
- Rendering: box/gaussian blur lokal pada area brush, kekuatan blur proporsional pressure × `blurWidth`.
- Tidak menambah warna baru — hanya memproses piksel existing di layer aktif dalam radius brush.

**H. SMUDGE TOOL**
- Rendering: menyeret piksel existing sepanjang arah goresan (drag-based sampling, mix rate tinggi ~85%) — beda dari Blur (yang mengaburkan di tempat) dan beda dari Water (yang mencampur dengan FG color) karena Smudge TIDAK memasukkan warna FG sama sekali, murni menyeret piksel kanvas.
- Parameter khusus: `smudgeStrength` (0-100), `smudgeLoadFalloff` (goresan makin panjang, "cat" yang terseret makin encer/pudar — mensimulasikan jari/kuas kering menyeret cat basah).

**I. EFFECT PEN**
- Kumpulan brush efek khusus non-realistis: Glitter (partikel berkilau acak posisi+ukuran), Confetti/Pattern Stamp (stempel bentuk berulang dengan rotasi acak), Halftone Pen (menggambar langsung dengan pola titik halftone — lihat juga Section 31 untuk versi layer-based), Neon Glow Pen (garis dengan outer glow blur), Chalk/Pastel (grain kasar + edge tersebar seperti kapur di kertas kasar).
- Setiap effect pen adalah brush mode terpisah (`mode: "effect"`, `effectType: "glitter"|"confetti"|"halftonePen"|"neonGlow"|"chalk"`), dengan parameter unik masing-masing yang disimpan di objek brush.

**J. SCATTER BRUSH**
- Rendering: setiap stamp memproduksi `scatterCount` (1-8) salinan tip dengan posisi acak dalam radius `scatter`, masing-masing dengan variasi independen: `sizeJit`, `opacityJit`, `rotJit`, dan (baru) `colorJit` (variasi hue/value acak kecil per instance agar tidak monoton — berguna untuk brush daun, bintang, tekstur alami).
- Parameter baru: `colorJit` (0-100, variasi warna acak per scatter instance).

### 6.12 Brush Dynamics — Pressure, Tilt, Velocity, Rotation
Setiap brush WAJIB dapat mengikat parameter berikut ke sumber dinamis apa pun (lihat Section 13 untuk detail input), dikonfigurasi lewat Brush Editor:
| Parameter Brush | Bisa Dikontrol Oleh |
|---|---|
| Ukuran (Size) | Pressure, Tilt, Velocity, Random, Fixed |
| Opacity | Pressure, Pen Wheel/Barrel Rotation, Random, Fixed |
| Hardness | Pressure, Tilt, Fixed |
| Rotasi tip | Tilt Direction, Stroke Direction, Barrel Rotation, Random, Fixed |
| Scatter | Velocity, Random, Fixed |
| Blending/Dilution (watercolor) | Pressure, Velocity, Fixed |

Setiap binding memiliki **kurva respons yang dapat diedit** (lihat 6.13), bukan hanya linear on/off.

### 6.13 Dynamics Curve Editor
- Editor kurva interaktif (mirip Pressure Curve CSP/SAI): grid 2D, sumbu X = input (0-100%), sumbu Y = output (0-100%), titik kontrol dapat ditambah/digeser/dihapus, interpolasi Catmull-Rom untuk transisi halus.
- Preset kurva: Linear, Soft Start (Ease-In), Soft End (Ease-Out), S-Curve, Custom.
- Preview live: goresan uji coba langsung di kanvas kecil dalam dialog, menampilkan efek kurva secara real-time.
- Kurva dapat disimpan sebagai bagian dari brush preset (tersimpan di objek brush, field `curves: { size: [...points], opacity: [...points], ... }`).

### 6.14 Brush Engine Preset Library (Wajib Tersedia Out-of-the-Box)
Minimum daftar brush preset bawaan agar kompetitif dengan CSP default set:
1. Pensil 2B, Pensil HB, Pensil 2H, Pensil Mekanik
2. Pena Presisi, Dip Pen (G-Pen style), Dip Pen (Maru-Pen/round style — cocok untuk manga), Ballpoint
3. Kuas Cat Air Datar, Kuas Cat Air Wet Edge, Kuas Cat Air Dry Brush
4. Kuas Cat Minyak (Oil, tekstur bristle kuat + blending tinggi)
5. Airbrush Halus, Airbrush Kasar (nozzle spread besar)
6. Marker Datar, Marker Highlighter (opacity cap rendah + blend mode multiply default)
7. Kuas Chalk/Pastel, Kuas Charcoal (arang, grain sangat kasar & gelap pekat)
8. Kuas Scatter Bintang, Kuas Scatter Daun (contoh kustomisasi scatter+colorJit)
9. Effect Pen: Glitter, Neon Glow, Halftone Pen
10. Kuas Blending Soft (untuk shading halus, hardness rendah + density tinggi)
11. Inking Brush (untuk manga lineart — hardness 100%, taper kuat, AA presisi)
12. Screentone Brush (aplikasikan pola halftone langsung sebagai brush — lihat Section 31)

### 6.15 Brush Editor Dialog (Lengkap)
- Tab: **Dasar** (size, hardness, density, opacity, blending), **Bentuk & Tekstur** (tip shape, paper texture, scatter), **Dinamika** (pressure/tilt/velocity binding + curve editor), **Watercolor** (blend/dilution/persistence — hanya aktif untuk mode Water), **Stroke** (spacing, taper start/end, jitter, rotasi), **Lanjutan** (blend mode brush, anti-alias, stamp cache behavior).
- Live preview stroke di bagian atas dialog, update real-time setiap parameter diubah.
- Tombol Reset ke Default, Simpan sebagai Preset Baru, Simpan Overwrite.

---

## 7. TOOL SYSTEM

### 7.1 Tool Categories

**Common Tools (selalu tampil di atas grid, tidak tergantung layer):**
- Selection Rectangle (`selrect`)
- Selection Ellipse (`selellipse`)
- Lasso (`sellasso`) — mode: Free / Poly / Polyfree
- Magic Wand (`selwand`) — mode: Similar Color / Transparent / All
- Text (`text`)
- Move (`move`)
- Zoom (`zoomt`)
- Rotate (`rotate`)
- Hand/Pan (`hand`)
- Eyedropper/Picker (`picker`)
- StrC / CurveC (raster line/curve)

**Normal Layer Tools (layer raster/reference/secret/folder):**
- Brush tools: Pencil, Pen, AirBrush, Brush, Water Color, Marker
- Eraser
- Selection Pen / Selection Eraser
- Bucket
- Gradation
- Blur
- Effect Pen
- Scatter
- Smudge
- VPen (brush untuk vector layer)

**Vector Layer Tools (layer vector):**
- Pen (V-Pen)
- Eraser (V-Eraser)
- Weight (V-Weight)
- Color (V-Color)
- Edit (V-Edit)
- Pressure (V-Pressure)
- Scale (V-Symp)
- Symp (V-Simplify)
- Curve (V-Curve)
- Line (V-Line)

### 7.2 Tool Grid Group System (SAI2-style)
- Grup tool horizontal di atas grid
- 4 template default: Basic, Binary, Artistic, Ver.1 Like
- Masing-masing grup punya slot tool (12-16 slot)
- Grup Basic tidak bisa dihapus
- Properti per-slot: nama tool, add name, shortcut key, stabilizer override, curve interpolation
- Klik kanan: New Group, Delete Group, Property
- Property dialog: nama, shortcut key, stabilizer mode (default/normal/v1), stabilizer level, curve interpolation checkbox
- Persist ke localStorage sebagai JSON

### 7.3 Selection Tools Detail

**Rectangle/Ellipse Selection:**
- Klik + drag → preview dashed rectangle/ellipse
- Shift = tambah ke seleksi existing
- Release = apply ke mask

**Lasso:**
- 3 mode: Free (drag bebas), Poly (klik-klik), Polyfree (combine poly + free segments)
- AA checkbox untuk antialiasing
- Klik ganda / Enter / klik titik awal = close
- Preview: garis biru putus-putus

**Magic Wand:**
- 3 mode: Similar Color (toleransi), Transparent (alpha < 30), All (semua warna sama)
- Source: Merged (composite) / Current Layer
- Tolerance: range 0-255, default 84
- Gap tolerance: 0-41 (morphological close untuk menghubungkan area)
- AA checkbox
- Ignore selection checkbox
- Shift = add to existing

**Selection Pen/Eraser:**
- Gambar langsung ke mask seleksi (putih = tambah, hitam = hapus)
- Size sesuai brush size

### 7.4 Gradation Tool
- Klik + drag: buat gradasi linear FG→transparan
- Shift: FG→BG (dua warna)
- Hormati seleksi (hanya area di dalam seleksi)
- Preview: garis hijau + handle start/end

### 7.5 Move Tool
- Geser konten layer (piksel digeser, bukan bounding box)
- Layer teks: ubah textProps.x / textProps.y
- Folder: tidak bisa dipindah
- Vector: gunakan V-Edit sebagai gantinya

### 7.6 Zoom Tool
- Klik: zoom in (1.5x) pada posisi klik
- Alt+klik: zoom out (1/1.5x)

### 7.7 Rotate Tool
- Drag: rotasi view di sekitar tengah viewport
- Update slider rotasi di panel Navigator

### 7.8 Hand/Pan Tool
- Drag: geser view (pan)
- Space + drag: temporary pan (dari tool apa pun)

### 7.9 Transform Tool (Ctrl+T)
- Aktif dari menu atau shortcut
- Crop konten ke bounding box (auto-detect dari alpha)
- Jika ada seleksi: crop ke batas seleksi
- 4 mode: Free, Resize, Rotate, Deform
- 4 corner handles + top-middle rotation handle
- Deform mode: 4 corner bebas (perspective/warp via affine triangle grid 16x16)
- Shift = constrain aspect ratio / snap angles
- Commit dengan Enter / klik di luar / switch tool
- Undo support (pre-transform canvas disimpan)

### 7.10 Eyedropper Tool
- Klik: ambil warna dari canvas
- Alt+klik (tool apa pun): eyedropper sementara (kecuali ruler ellipse/concentric active)
- Target: Merged (composite) / Current layer (radio button)
- Drag: continuous color sampling + loupe overlay
- Loupe magnifier: 11x11 piksel diperbesar 12x, grid, crosshair center, hex display

---

## 8. STABILIZER SYSTEM (SAI2-STYLE)

### 8.1 Stabilizer Modes
- **Normal Mode (0-15):** Multi-pole cascaded EMA (Exponential Moving Average) filter
  - Level 0: off (factor 1.0)
  - Level 1-15: factor 0.65 → 0.104 (semakin kecil = semakin smooth, semakin banyak lag)
- **S-Level (S-1 s/d S-7):** Heavy pull-string stabilizer
  - S-1: factor 0.120
  - S-7: factor 0.011 (ultra-smooth, ultra-precise)
  - Formula: `0.12 * pow(0.68, lvl-1)`

### 8.2 Stabilizer Implementation
- Spring-damper filter: posisi bergerak sebagian menuju target setiap frame
- TANPA zona-mati: garis tidak pernah patah/terputus
- Saat pen diangkat: brush "menyusul" (catch-up) mulus sampai titik akhir
- Pressure smoothing: `prS += (targetPr - prS) * 0.35`
- Rapid pressure decay: saat catch-up, pressure mengecil eksponensial (×0.35 per step, 4 steps max)

### 8.3 Tool-Level Stabilizer Override
- Setiap tool bisa punya stabilizer mode & level sendiri (override global)
- Disimpan di property tool per slot

### 8.4 Mode Stabilizer Tambahan (Wajib — Setara/Melebihi CSP "Stabilization")
- **Mode "Anticipate" (opsional, toggle):** Selain catch-up di akhir stroke, sistem dapat memprediksi arah goresan berikutnya berdasarkan velocity vector 3 titik terakhir, mengurangi lag terasa pada level stabilizer tinggi tanpa mengorbankan kehalusan.
- **Adjustable via Slider + Angka Presisi:** Selain slider 0-22, tambahkan input angka langsung (spinbox) agar pengguna power-user bisa mengatur nilai presisi tanpa drag slider.
- **Quick Toggle Shortcut:** Tombol pintas untuk cycle antar 3 level favorit (Off / Level tersimpan A / Level tersimpan B) — dapat di-assign user (lihat Section 23).
- **Per-Brush Default Stabilizer:** Setiap brush preset (Section 6.14) dapat menyimpan level stabilizer default-nya sendiri (override tool-level), auto-terapkan saat brush dipilih.
- **Visual Feedback:** Saat stabilizer level tinggi aktif, tampilkan garis tipis penunjuk jarak "tali" (lazy-nib line) dari titik kursor asli ke titik brush yang sedang digambar (opsional toggle, membantu pengguna memahami lag secara visual — fitur ini ADA di CSP dan wajib direplikasi).

---

## 9. RULER & SYMMETRY SYSTEM

### 9.1 Ruler Modes
1. **Straight / Mistar Lurus:** Garis lurus dengan handle center + rotate
2. **Ellipse / Circle:** Ellipse dengan quad deform (Alt) atau freeform handles (Ctrl)
3. **Parallel Lines:** Garis sejajar dengan angle control
4. **Concentric Ellipse:** Ellipse konsentris dengan ratio control
5. **Vanishing Point:** 1-4 titik hilang, semua goresan snap ke garis menuju VP terdekat
6. **Mirror / Symmetry:** N-axis radial symmetry (1-16) + mirror kiri-kanan opsional
7. **Perspective 2-titik:** 2 vanishing points
8. **Perspective 3-titik:** 3 vanishing points

### 9.2 Ruler Interaction
- **Straight:** Ctrl+drag untuk rotate, Ctrl+Shift untuk snap 15° (kompas). Geser tengah (Ctrl klik) untuk pindah.
- **Ellipse:** Default hanya center handle. Ctrl = 4 perimeter handles (freeform rx/ry + rotate). Ctrl+Shift = resize handles (skala seragam). Alt = quad deform mode (4 corner bebas + center, berbasis affine ellipse mapping).
- **Concentric:** Sama seperti ellipse, plus ratio control.
- **Parallel:** Ctrl+drag untuk rotate. Ctrl+Shift snap 15°.
- **Vanishing Point:** Kursor snap ke VP terdekat. Ctrl+klik untuk geser VP.
- **Mirror/Symmetry:** Ctrl+klik untuk geser center.

### 9.3 Constraint Engine
- `rulerConstrain(p, st)`: Proyeksikan titik ke ruler
- Straight: snap ke garis sepanjang angle
- Ellipse: snap ke perimeter ellipse terdekat (180+10 sample)
- Concentric: snap ke ellipse skala dari pusat
- Parallel: snap ke garis sejajar melewati titik origin
- Vanishing Point: snap ke garis dari origin menuju VP terdekat

### 9.4 Symmetry Rendering
- Setiap goresan di-transform radial (N axis) + mirror (opsional)
- `symPoints(x,y)`: menghasilkan array titik hasil transform simetri
- Toolbar Symmetry: checkbox on/off, input jumlah axis, checkbox mirror

### 9.5 Perspective Ellipse Engine
- Affine ellipse dari quad 4-titik (memetakan circle ke ellipse sejati via transformasi affine)
- Tidak distortion, tidak gepeng
- `affineEllipseFromQuad(quad, scale)`: menghasilkan center + 2 axis vector
- `snapToPerspectiveEllipse(p, quad, scale)`: proyeksi titik ke perimeter ellipse
- `drawPerspectiveEllipse(quad, scale)`: render ellipse mulus 360 segmen

### 9.6 Ruler Tambahan (Wajib — Melebihi Kelengkapan CSP)
9. **Curve Ruler (Mistar Kurva Bebas):** Pengguna menggambar kurva bebas (Catmull-Rom, sama seperti V-Curve) sebagai lintasan, goresan brush berikutnya snap mengikuti kurva tersebut. Berguna untuk garis lengkung organik (rambut, kain, alur tubuh) yang tidak bisa direpresentasikan oleh ellipse/parallel.
10. **Radial Ruler (Garis Radial dari Titik Pusat):** N garis lurus (2-36, dapat diatur) memancar dari satu titik pusat, goresan snap ke garis radial terdekat — berguna untuk efek kecepatan/ledakan/pola manga.
11. **Perspective 4-Titik & 5-Titik (opsional lanjutan):** Untuk sudut pandang ekstrem (fisheye/dome), tersedia sebagai mode lanjutan dari Perspective 3-titik.
12. **Ruler Layer (Multi-Ruler per Layer):** Setiap layer dapat menyimpan ruler-nya sendiri (bukan hanya satu ruler global per dokumen) — sehingga panel komik dengan sudut berbeda-beda dapat memiliki perspective ruler masing-masing. Ruler layer disimpan sebagai properti layer opsional (`layerRuler: {...}` sama struktur dengan `doc.ruler`).
13. **Snap Toggle Cepat:** Tombol pintas untuk mengaktif/nonaktifkan snap ruler sementara tanpa mengubah mode ruler (mirip menahan tombol tertentu sambil menggambar bebas melewati garis ruler).

---

## 10. UNDO / REDO SYSTEM

### 10.1 Undo Entry Types
1. **Raster full (`ras`):** ImageData seluruh layer
2. **Raster rect (`ras_rect`):** Hanya rectangle yang berubah (goresan brush, hemat memori)
3. **Vector (`vec`):** JSON stringify strokes array
4. **Selection (`sel`):** Active state + mask data

### 10.2 Stroke Undo (Rectangle Optimization)
- Sebelum stroke: backup seluruh layer ke `undoBackupCanvas`
- Selama stroke: `expandStrokeBox(x,y,r)` → bounding box goresan
- Setelah stroke: simpan hanya rectangle dari bounding box (ImageData + posisi)
- Hemat 10-100x dibanding simpan full layer

### 10.3 Limits
- Max 30 entries (UNDO_MAX)
- Oldest entry di-shift saat penuh
- Clear redo stack setiap push undo baru

---

## 11. COMPOSITOR SYSTEM

### 11.1 Recursive Compositor
- Layer tree dirender rekursif dari root (children of null)
- Fungsi `composeInto(ctx, nodes, includePrivate)`:
  - Iterasi nodes array (bottom-to-top)
  - Clipping group: render base, lalu clip child dengan `destination-in` ke alpha base
  - Folder: render semua child ke canvas temp, apply mask folder, lalu composite
  - Single layer: render dengan opacity + blend mode

### 11.2 Rendering Optimization
- **Folder cache:** Hasil render folder di-cache kecuali punya private/liveOnly descendant
- **Node cache:** Raster/vector output di-cache per node (`_renderCache`)
- **Cache invalidation:** `invalidateNodeCache(node)` → naik ke parent folder
- **Canvas pool:** `acquireCanvas()` / `releaseCanvas()` — max 24 canvas di pool
- **Flat draw cache:** Untuk layer tree flat (tanpa folder/clip/mask), pre-render below + above active layer

### 11.3 Mipmap Pyramid Rendering
- Untuk zoom-out (targetZoom < 0.85) dan kanvas > 128px:
  - Downsample bertahap (½ per level) sampai mendekati target zoom
  - Mencegah moiré, garis putus/bergerigi/kacau saat zoom jauh
  - Setiap level pakai `imageSmoothingEnabled = true` + `imageSmoothingQuality = "high"`
  - 2 buffer mipmap untuk ping-pong scaling

### 11.4 Mask Compositing
- Mask disimpan sebagai grayscale (putih=tampil, hitam=sembunyi)
- `maskBake(node)`: konversi RGB mask ke alpha = luminance
- Aplikasi: `destination-in` draw `_maskAlpha` ke canvas output
- Auto-bake saat mask dirty

### 11.5 Paper Texture in Compositor
- Diaplikasikan di compositor final (setelah semua layer) via `multiply` blend
- Pattern diulang (repeat) dari tile tekstur yang di-generate

---

## 12. VIEWPORT & NAVIGATION

### 12.1 View Transform
- `viewMatrix()`: DOMMatrix dengan translate(pan) → rotate → scale (zoom × flip)
- `screenToDoc(px, py)`: inverse matrix, untuk mapping klik ke koordinat dokumen
- `docToScreen(dx, dy)`: forward matrix, untuk rendering

### 12.2 Zoom
- Range: 0.05x (5%) s/d 32x (3200%)
- `setZoom(z, cx, cy)`: zoom terpusat pada posisi kursor/viewport
- Wheel: zoom dengan Ctrl, atau tanpa Ctrl (default zoom behavior)
- Ctrl+Alt+drag: resize brush size/radius langsung
- Slider Navigator: log2 scale (-4 s/d +5)

### 12.3 Pan
- Space+drag, Hand tool, Middle mouse button
- Scrollbar horizontal + vertical

### 12.4 Rotate
- Range: -180° s/d +180°
- Rotate tool: drag untuk rotasi
- Alt+Wheel: rotate step 5°
- Slider Navigator

### 12.5 Flip
- Flip horizontal view (mirror)
- Tombol ⇋ di panel Navigator
- Indikator "FLIP" merah

### 12.6 Fit to Screen
- Zoom agar seluruh kanvas terlihat (92% viewport)
- Reset rotasi

---

## 13. POINTER & INPUT PIPELINE

### 13.1 Event Handling
- `pointerdown` / `pointermove` / `pointerup` / `pointercancel` pada viewport
- Pointer capture (`setPointerCapture`) untuk tracking di luar viewport
- Coalesced events: `getCoalescedEvents()` untuk high-frequency pen input

### 13.2 Pressure Handling
- Pen pressure: `evPressure(e)` — nilai 0-1
  - Pen: `e.pressure` asli
  - Mouse: 0.5
- Pressure curve: `prCurve(pr)` — `Math.pow(clamp(pr,0,1), 1.5)` (SAI2-style)
- Pressure smoothing: low-pass filter dengan faktor 0.35

### 13.3 Cursor Ring
- Brush/Eraser/V-Pen/SelPen/SelEraser: lingkaran ukuran kuas + titik tengah
- Scale/Symp: lingkaran ukuran radius + titik tengah, class `.symp` border biru
- Tool lain: crosshair sistem
- Update real-time pada pointer move

### 13.4 Modifier Keys (Non-blocking by design)
- **Ctrl:** Rotate ruler / vertex edit / eyedropper / brush resize / deselect
- **Shift:** Snap angle (15°/45°) / add to selection / constrain aspect / straight line
- **Alt:** Eyedropper (pick color) / ruler quad deform mode
- **Space:** Pan sementara
- **Ctrl+Alt:** Brush resize drag
- **F1, F3, F5, F7:** Dicegah (prevent default)
- **Ctrl+S/P/O/D/R/J/H/U/G/F:** Dicegah di luar input field
- **Browser zoom (Ctrl+Wheel):** Dicegah

### 13.5 Context Menu
- Klik kanan: eyedropper + loupe magnifier (Ctrl untuk pick ke background)
- Semua context menu browser dicegah (`preventDefault()` pada `contextmenu` event)

### 13.6 Stream View Read-Only
- Class `body.stream-view`: semua input/select/button non-interactive (`pointer-events: none`)
- Kecuali viewport: tetap bisa pan/zoom (`pointer-events: auto`, cursor grab/grabbing)

### 13.7 DUKUNGAN PEN TABLET LENGKAP (Wajib — Setara/Melebihi CSP)

**A. Sumber Data Pointer Event yang WAJIB Dibaca**
- `pressure` (0-1): tekanan pena, sudah ada — dipertahankan.
- `tiltX`, `tiltY` (-90 s/d 90 derajat): kemiringan pena terhadap tablet, WAJIB dibaca dan dikonversi ke representasi `tiltAngle` (besar kemiringan 0-90°) dan `tiltDirection` (arah kemiringan 0-360°) via `atan2(tiltY, tiltX)`.
- `twist` / azimuth barrel rotation (jika didukung perangkat, mis. Wacom Art Pen): WAJIB dibaca jika tersedia, digunakan untuk brush kaligrafi yang berputar mengikuti rotasi badan pena.
- `pointerType`: `"pen"` vs `"mouse"` vs `"touch"` — WAJIB dibedakan agar mouse tidak salah menerapkan tilt/pressure palsu.
- `getCoalescedEvents()`: WAJIB digunakan untuk menangkap seluruh sample point berkecepatan tinggi (>200Hz pada tablet modern) agar goresan cepat tidak "melompat"/patah-patah.
- `getPredictedEvents()` (opsional, jika tersedia): dapat dipakai untuk mengurangi persepsi latency pada refresh rate rendah.

**B. Efek Tilt terhadap Brush**
- Brush WAJIB dapat dikonfigurasi agar `tiltAngle` memengaruhi: bentuk tip (elips memanjang seiring tilt membesar — mensimulasikan pena miring "menggores" lebih lebar, seperti pensil/kapur miring), opacity, atau texture grain intensity.
- `tiltDirection` WAJIB dapat memengaruhi rotasi tip brush (untuk brush kaligrafi/chisel-tip).
- Semua binding tilt dapat diatur lewat Dynamics Curve Editor yang sama (Section 6.13).

**C. Palm Rejection**
- Saat `pointerType === "pen"` terdeteksi aktif menggambar, event `pointerType === "touch"` yang datang bersamaan (multi-touch dari telapak tangan) WAJIB diabaikan untuk keperluan menggambar (kecuali gesture pan/zoom dua jari yang eksplisit diaktifkan lewat pengaturan).
- Pengaturan toggle: "Izinkan sentuhan untuk pan/zoom saat pena aktif" (default: on) vs "Nonaktifkan sentuhan total saat pena terdeteksi" (mode strict, untuk tablet layar sentuh seperti Wacom Movink/iPad-style device).

**D. Kompatibilitas Driver Tablet**
- WAJIB kompatibel dengan input Pointer Events standar dari driver: Wacom (Intuos/Cintiq/Movink), Huion, XP-Pen, Gaomon, dan tablet layar sentuh Windows (Surface Pen/N-trig).
- Tidak boleh bergantung pada API vendor-specific (mis. Wacom Web Plugin lama yang sudah deprecated) — seluruhnya via W3C Pointer Events standar agar portable lintas perangkat.
- Panel "Uji Tablet" (di Pengaturan): menampilkan grafik real-time pressure/tilt/posisi saat pengguna mencoret-coret area uji, untuk verifikasi driver terbaca dengan benar.

**E. Pengaturan Sensitivitas Tambahan**
- Kurva sensitivitas pressure global (terpisah dari kurva per-brush) — semacam "master calibration" agar pengguna dengan pena yang terlalu sensitif/kurang sensitif dapat mengkalibrasi sekali untuk semua brush.
- Opsi "Pressure Minimum Threshold" — mengabaikan noise tekanan sangat kecil di awal sentuhan pena (mencegah titik tak sengaja saat pena baru menyentuh permukaan).

---

## 14. VECTOR / LINEWORK ENGINE

### 14.1 Vector Stroke Structure
```javascript
{
  pts: [{x, y, w}],  // titik kontrol dengan lebar (weight)
  color: "#rrggbb",   // warna stroke
  opacity: 0-1        // opasitas stroke
}
```

### 14.2 Vector Tools Detail

**Pen (V-Pen):** Gambar bebas dengan stabilizer. Setiap goresan disimpan sebagai array titik. Quadratic Bézier via titik-tengah untuk rendering mulus.

**Line (V-Line):** Klik-klik untuk garis lurus multi-segmen. Shift untuk snap 45°. Enter/dblclick untuk commit.

**Curve (V-Curve):** Klik-klik untuk kurva Catmull-Rom multi-segmen (16 sample per segmen).

**StrC (V-LineC):** Click-hold-drag-release untuk satu garis lurus. Shift snap 45°.

**CurveC (V-CurveC):** Click-drag (definisikan ujung) → bend (sesuaikan kurva) → klik untuk commit. Quadratic Bézier.

**Edit (V-Edit):** Pilih titik (klik) atau segmen (klik pada garis). Geser titik. Alt+klik untuk hapus titik. Ctrl+drag untuk sisip titik + langsung geser.

**Pressure (V-Pressure):** Klik pada titik, drag atas/kanan = melebar, drag bawah/kiri = mengecil. Falloff 4 titik radius.

**Eraser (V-Eraser):** Dua mode: hapus seluruh stroke (klik pada garis) atau potong segmen (drag pada area).

**Weight (V-Weight):** Klik pada stroke: Alt+klik = tipiskan (×1/1.25), klik biasa = tebalkan (×1.25).

**Color (V-Color):** Klik pada stroke untuk ubah warna ke FG.

**Scale (V-Symp):** Sapu kuas di atas titik vector → ubah lebar garis (increase/decrease mode). Radius kuas 4-200px.

**Symp (V-Simplify):** Sapu kuas → buang titik berlebih dengan RDP tolerance 4.5 (bentuk garis tidak berubah).

### 14.3 Vector Preview
- Preview inkremental (tidak render ulang seluruh garis saat menggambar)
- Hanya segmen baru yang di-stamp ke overlay canvas
- Saat dilepas, garis final dirender identik dengan preview

### 14.4 Vector Rendering
- `renderStrokeToCtx(ctx, s)`: render stroke ke context
- Quadratic Bézier lewat titik-tengah (TIDAK overshoot)
- Opacity < 1: render solid ke temp canvas, lalu composite sekali (tanpa penumpukan alpha)
- RDP simplification dengan tolerance yang sama untuk preview & final

### 14.5 Vector Overlay
- Saat tool vector aktif / Ctrl ditekan: titik kontrol ditampilkan sebagai kotak kecil
- Stroke terpilih: skeleton garis biru + titik (selected point = jingga)
- Scale/Symp: area biru transparan yang tersapu kuas
- V-Pen: blit preview inkremental dari `_vecPreview`

### 14.6 Raster-to-Vector Conversion
- `traceRasterToVectorStrokes()`: telusuri piksel alpha → jalur kontinu
- Mark visited radius 3px, search radius 3-5px
- RDP simplify (tolerance 1.2) → 3-point Gaussian weight smoothing
- Hasil: vector stroke dengan lebar bervariasi natural

---

## 15. TEXT ENGINE

### 15.1 Text Layer Structure
```javascript
{
  type: "text",
  textProps: {
    content: "Teks",
    font: "sans-serif",
    size: 64,         // px
    color: "#000000",
    bold: false,
    italic: false,
    skew: 0,          // derajat (-60..60)
    letter: 0,        // spacing huruf (-20..80)
    line: 1.2,        // line height multiplier (0.6..3.0)
    align: "left"|"center"|"right",
    x, y              // posisi di kanvas
  },
  tDirty: boolean     // flag rebuild
}
```

### 15.2 Text Rendering
- `rebuildText(ly)`: render teks ke canvas layer
- Font style: `(italic?"italic ":"") + (bold?"700 ":"") + size+"px " + font`
- Skew: transformasi miring (`setTransform` dengan tan skew)
- Letter spacing: loop per karakter, ukur width, fill individual
- Multi-line: split by `\n`, increment Y per line

### 15.3 Text Panel Binding
- Two-way sync antara panel dan layer teks aktif
- Perubahan properti langsung update render (non-destructive)
- Tombol "Ke Tengah": set posisi ke tengah kanvas
- Tombol "Warna→FG": set warna teks ke foreground
- Tombol "Rasterize": konversi teks ke raster (tidak bisa diedit lagi)
- Klik dengan tool Text pada kanvas: buat layer teks baru di posisi klik

---

## 16. FLOOD FILL / BUCKET

### 16.1 Bucket Tool
- Scanline flood fill (bukan per-pixel = jauh lebih cepat)
- Source: Merged (composite) atau Current layer
- Tolerance: 0-255 (color difference)
- Warna: foreground
- Hormati seleksi (hanya fill area dalam seleksi)
- Stack-based, non-recursive

### 16.2 Magic Wand Selection
- Lihat bagian Selection Tools (section 7.3)

---

## 17. GRADATION

### 17.1 Gradation Tool
- Klik + drag: definisikan start (a) dan end (b)
- Shift: gradasi FG→BG (warna solid)
- Tanpa Shift: FG→transparan
- Hormati seleksi: pre-save area luar, bake selection setelah gradasi
- Preview: garis hijau dengan handle start + end

---

## 18. COLOR SYSTEM

### 18.1 Color Models
- Internal: HSV (h: 0-360, s: 0-100, v: 0-100)
- `hsv2rgb({h,s,v}) → [r,g,b]` (0-255)
- `rgb2hsv(r,g,b) → {h,s,v}`
- `fgCss()` / `bgCss()`: menghasilkan CSS string `rgb(r,g,b)`

### 18.2 Color Wheel
- Canvas 200x200px
- Draw HSV wheel (hue circle + saturation/brightness di dalamnya)
- Interactive: klik untuk pilih warna
- SV box overlay: square saturation-value selector

### 18.3 RGB/HSV Sliders
- Track: background gradient sesuai channel
- Thumb: 9x17px, bg putih semi-transparan, border gelap
- Value display: 3-digit untuk RGB, raw untuk HSV
- Two-way binding: ubah RGB → update HSV, dan sebaliknya

### 18.4 Color Mixer
- 4 baris mixer
- Setiap baris: chip warna (18x18px) + gradient slider
- Digunakan untuk mencampur warna custom

### 18.5 Swatches
- Grid 12 kolom
- Klik: pilih warna
- Ctrl+klik: simpan warna ke swatch
- Swatch tersimpan di localStorage

### 18.6 Foreground/Background
- Dua color box: FG (depan) dan BG (belakang)
- Swap button (⇄) atau shortcut X
- Klik box untuk color picker dialog

### 18.7 Eyedropper
- Lihat bagian Tool System (section 7.10)

---

## 19. LIVE STREAM / OBS BROADCAST ENGINE

### 19.1 Arsitektur 3 Lapis Transport
1. **Direct Call (same-origin):** `liveStreamWin.updateStreamDirect(canvas)`
2. **window.postMessage + ImageBitmap transfer (cross-origin/file://):** jalur utama
3. **BroadcastChannel API ("authorsultra_live_stream_v2"):** OBS integration

### 19.2 Mode Stream
- **Creator Mode (`?mode=stream&type=creator`):**
  - Studio lengkap disinkronkan (UI state + layer + brush + stabilizer + view)
  - Secret layer disamarkan (private layer tidak ditampilkan, live-only layer ditampilkan)
  - 20x/detik sync state (throttled)
- **Canvas Mode (`?mode=stream&type=canvas`):**
  - Kanvas bersih, hanya output gambar
  - Secret layer disembunyikan
  - UI minimal: zoom/fit/fullscreen controls

### 19.3 Stream Workflow
1. Buka popup window via `window.open()` atau `Neutralino.window.create()`
2. `broadcastLiveStreamFrame(force)`:
   - Render komposit publik (includePrivate=false) ke `streamCanvasBuf`
   - Scale sesuai `PERF.streamScale` (1 / 0.75 / 0.5)
   - Transfer via direct call / postMessage / BroadcastChannel
3. FPS: 10/15/20/25/30 (configurable `PERF.streamFps`)
4. Watchdog: hentikan stream saat viewer hilang (4 detik tanpa ping)

### 19.4 Stream Cache
- `streamBelowCacheCanvas` + `streamAboveCacheCanvas`: pre-render layer di bawah dan di atas layer aktif
- `streamCacheReady`: flag untuk menghindari compose ulang
- Flat draw cache untuk stream

### 19.5 Creator State Sync
- Sinkronisasi dua arah: toolbar state (tool, brush, color, size, stabilizer, ruler, view) dikirim ke viewer
- Viewer menampilkan UI yang sama (read-only)
- Throttled (max 20x/detik), JSON diff untuk efisiensi

---

## 20. TIMELAPSE ENGINE

### 20.1 Capture
- Throttle: 160ms antar frame (configurable `PERF.tlMs`: 120/160/400)
- Resolution: skala ke `TLQUAL.capRes` (1280/1920/2560/3840) — panjang sisi terpanjang
- Format blob: WebP (quality 0.92) atau PNG
- Hanya layer publik (non-private) yang di-capture
- Saat drawing: pakai stream cache untuk menghindari compose ganda
- Auto-throttle adaptif: saat frame > 100000, decimate setengah + naikkan throttle interval

### 20.2 Session
- `tlSession` counter: invalidasi callback toBlob dari dokumen lama
- Callback toBlob async → disimpan ke Map per sequence number
- Flush berurutan: gap diisi saat callback tiba

### 20.3 Memory Management
- Max frame di memori: `TL.MAX` (100000)
- Auto-decimate: filter setiap frame ke-2 + naikkan throttle 1.6x
- Lazy loading: timelapse disimpan sebagai file terpisah (newline-delimited)

### 20.4 Pause/Resume
- Klik REC dot di status bar
- Indikator: merah = recording, abu = paused

---

## 21. EXPORT SYSTEM

### 21.1 Export Image
- Format: PNG, JPEG (quality slider), WebP
- Resolusi: original / 0.5x / 0.25x
- Termasuk/tanpa paper texture
- Komposit publik (non-private layers only)

### 21.2 Export Timelapse (MP4 H.264)
- Encoder: WebCodecs VideoEncoder (H.264/AVC)
- Bitrate mode: Auto / Low / Medium / High / Max
- Hardware acceleration: prefer-hardware → prefer-software
- Codec candidates: avc1.640033, avc1.4d0033, avc1.420033, dsb.
- Microblock alignment: width/height kelipatan 16
- Keyframe interval: setiap 2 detik (fps×2)
- Parallel pre-decoding (PREFETCH=3 frame)
- Progress callback setiap 15 frame
- Batas H.264: max 2160p height
- Fallback: WebM VP9 untuk resolusi di atas 2160p

### 21.3 Export Timelapse (WebM)
- WebCodecs VideoEncoder (VP9)
- Quality: best (realtime)

### 21.4 Export Timelapse (GIF)
- Encode frame per frame ke canvas GIF
- Color quantization: 256 colors

### 21.5 Export Timelapse (PNG Sequence)
- Download tiap frame sebagai PNG terpisah
- Zip archive opsional

### 21.6 Export PSD
- Custom binary writer:
  - File header (26 bytes): signature "8BPS", version 1, channels 4, width, height, depth 8, color mode RGB
  - Layer records: RLE-compressed channel data (PackBits) untuk Alpha, Red, Green, Blue
  - Blend mode mapping: 4-char keys (norm, mul, scrn, over, dLit, etc.)
  - Merged image: full composite

### 21.7 Export Brush (.aspbrush)
- JSON format: `{format:"AuthorSultraPaintBrush", version:1, brush:{...}}`
- Tip disimpan sebagai data URL PNG

---

## 22. IMPORT / OPEN SYSTEM

### 22.1 ASP Project (.asp / .json)
- Format: JSON header line + newline-separated timelapse frames
- Fast streaming load: baca header saja, timelapse lazy dari disk
- Electron: IPC-based file slice reading
- Neutralino: readBinaryFile → Blob
- Browser: FileReader → parse

### 22.2 PSD Import
- Custom binary parser:
  - Parse color mode, width, height, depth, channels
  - Layer & mask section: extract nama layer, opacity, blend key, visible, clipping, bounds
  - Channel data: RAW atau RLE (PackBits) decompression
  - Map PSD blend keys ke Canvas2D globalCompositeOperation
  - Multi-layer: buat satu layer AuthorSultra per layer PSD
  - Fallback: composite image kalau tidak ada layer data

### 22.3 ABR Import (Photoshop Brush)
- Versi 1/2: parse brush records (spacing, bounds, depth, RLE mask)
- Versi 6/7/10: cari "8BIM samp" section, parse per brush
- Konversi mask ke tip image (grayscale)

### 22.4 Image Import
- PNG, JPG, JPEG, WebP, BMP
- Sebagai layer baru di dokumen yang sudah ada
- Atau sebagai kanvas baru seukuran gambar

### 22.5 PNG/BMP Brush Tip Import
- PNG transparent: langsung sebagai tip mask
- PNG opaque: alpha = 255 - luma (gelap = pekat)

### 22.6 Folder Brush Import
- Electron: baca folder "Brushes" di samping exe
- Chrome: File System Access API (`showDirectoryPicker`) + IndexedDB handle persistence
- Fallback: `<input webkitdirectory>`
- Struktur: `Brushes/*.png` = forms, `Brushes/NamaGrup/*.png` = forms grouped, `Brushes/Textures/**.png` = textures

### 22.7 Save/Export ASP
- JSON header line + newline + binary timelapse frames (newline-separated blobs)
- Untuk file besar: timelapse dipisah dari JSON

---

## 23. KEYBOARD SHORTCUTS (LENGKAP)

### 23.1 File Operations
- `Ctrl+N`: New canvas dialog
- `Ctrl+O`: Open file dialog
- `Ctrl+S`: Save
- `Ctrl+Shift+S`: Save As
- `Ctrl+W`: Close tab
- `Ctrl+Shift+E`: Export as image
- `Ctrl+Shift+T`: Export timelapse
- `Ctrl+Shift+L`: Live stream window

### 23.2 Edit Operations
- `Ctrl+Z`: Undo
- `Ctrl+Shift+Z` / `Ctrl+Y`: Redo
- `Ctrl+X`: Cut
- `Ctrl+C`: Copy
- `Ctrl+Shift+C`: Copy merged
- `Ctrl+V`: Paste
- `Ctrl+A`: Select all
- `Ctrl+D`: Deselect
- `Ctrl+Shift+I`: Invert selection
- `Ctrl+H`: Hide/show selection
- `Delete`: Clear layer
- `Shift+Delete`: Fill layer FG

### 23.3 Layer Operations
- `Ctrl+Shift+N`: New raster layer
- `Ctrl+E`: Merge down
- `Ctrl+T`: Transform
- `Ctrl+J`: Duplicate layer
- `Ctrl+Shift+J`: Duplicate to new document

### 23.4 View Operations
- `F`: Fit to screen
- `Ctrl++` / `Ctrl+=`: Zoom in
- `Ctrl+-`: Zoom out
- `Ctrl+0`: Reset zoom
- `R`: Rotate ruler/toggle ruler
- `F4`: Toggle left panel
- `F5`: Toggle right panel
- `F6`: Toggle toolbar
- `F11`: Fullscreen

### 23.5 Tools
- `B`: Brush/Pen
- `E`: Eraser
- `G`: Bucket
- `I`: Eyedropper
- `M`: Selection rectangle
- `L`: Lasso
- `W`: Magic wand
- `T`: Text
- `V`: Move
- `H`: Hand
- `Z`: Zoom
- `X`: Swap FG/BG

### 23.6 Stabilizer
- `S`: Toggle stabilizer level
- `Ctrl+Shift+S`: Save as (bukan stabilizer)

### 23.7 Tab Navigation
- `Ctrl+Tab`: Next document tab
- `Ctrl+Shift+Tab`: Previous document tab
- `Ctrl+1-9`: Switch to tab N

### 23.8 SISTEM KUSTOMISASI SHORTCUT TOTAL (Wajib — Tanpa Terkecuali)

> Daftar di 23.1–23.7 adalah shortcut **default**. WAJIB tersedia sistem agar SEMUA shortcut tersebut — dan setiap kontrol lain di aplikasi tanpa kecuali — dapat di-remap oleh pengguna.

**A. Cakupan Kustomisasi (100% Menu & Kontrol)**
- Semua item menu (File/Edit/Image/Layer/Filter/View/Selection/Window/Help).
- Semua tool (28+ tools di toolgrid, termasuk tool vector).
- Semua brush preset (shortcut cepat untuk memilih brush tertentu, mis. angka F1-F12 atau kombinasi custom).
- Semua brush parameter sebagai *incremental shortcut* (mis. `[` / `]` memperbesar/mengecilkan ukuran kuas, `{` / `}` mengurangi/menambah hardness, `,` / `.` opacity, dst — semua dapat di-remap, bukan hardcoded).
- Semua panel toggle (Navigator, Layer, Color, Tools, Brush, Vector, Text).
- Semua level stabilizer cepat (quick-switch presets).
- Semua mode ruler (cycle atau direct-select tiap mode ruler via shortcut).
- Blend mode layer (cycle next/prev blend mode via shortcut).
- Zoom preset (mis. tekan angka untuk jump ke 50%/100%/200%/400%).

**B. Dialog Pengaturan Shortcut**
- Layout: daftar scrollable dikelompokkan per kategori (sama seperti daftar di atas), search box untuk mencari command by name.
- Setiap baris: nama command, kolom shortcut aktif (klik untuk mulai rekam kombinasi tombol baru), tombol reset ke default per-item, indikator jika terjadi konflik (dua command dengan shortcut sama — ditandai warna merah + pesan "Bentrok dengan: [nama command lain]"; sistem TIDAK memblokir tapi memperingatkan sehingga pengguna sadar).
- Tombol global: "Reset Semua ke Default", "Ekspor Keymap (.json)", "Impor Keymap (.json)" — untuk memudahkan berbagi preset keymap antar pengguna atau migrasi dari software lain (mis. preset "Mirip CSP", "Mirip Photoshop", "Mirip SAI2" sebagai starting point bawaan).
- Preset keymap bawaan (dropdown pilihan cepat): "AuthorSultra Default", "Gaya Clip Studio Paint", "Gaya Photoshop", "Gaya Paint Tool SAI2".

**C. Radial Quick Menu (Pie Menu) — Fitur Tambahan Melebihi CSP**
- Tombol pintas yang dapat di-assign (default: tahan tombol tertentu / klik tengah mouse) memunculkan menu radial di posisi kursor berisi shortcut favorit yang dapat dikustomisasi pengguna (brush favorit, tool favorit, warna swatch favorit) — mempercepat workflow tanpa menjangkau panel.
- Konfigurasi radial menu: hingga 8 slot, drag-drop item dari brush list/tool list/swatch ke slot radial menu di dialog pengaturan.

**D. Penyimpanan**
- Keymap kustom disimpan di `localStorage` key `asp_keymap`, format JSON `{commandId: "Ctrl+Shift+X", ...}`.
- Radial menu config disimpan di `asp_radialmenu`.

---

## 24. PERFORMANCE SETTINGS

### 24.1 Stream Performance
- FPS: 15 / 30 / 60 (default 30)
- Scale: 1 (100%) / 0.75 / 0.5
- Disimpan di localStorage

### 24.2 Panel Throttle
- Panel refresh interval: 100ms / 150ms / 300ms (default 150ms)
- Mencegah navigator + thumbnail repaint tiap frame

### 24.3 Timelapse Throttle
- Capture interval: 120ms / 160ms / 400ms (default 160ms)

### 24.4 Mipmap Zoom
- Toggle on/off (default on)
- Saat off: pakai `drawImage` langsung dengan smoothing

### 24.5 Timelapse Quality
- Capture resolution: 1280 / 1920 / 2560 / 3840 (default 3840)
- Format: WebP / PNG
- WebP quality: 0.5-1.0 (default 0.92)
- Default export resolution: r720 / r1080 / r1440 / r4k / doc
- Bitrate mode: auto / max

### 24.6 ARSITEKTUR "ULTRA RINGAN" (Wajib — Target: Mulus di Laptop Jadul)

> Target benchmark eksplisit: pada hardware profil rendah (CPU dual-core ~2GHz generasi 2012-2015, integrated graphics tanpa dGPU, RAM 4GB, tanpa SSD/HDD 5400rpm), aplikasi WAJIB mempertahankan minimum **30 FPS terasa mulus saat menggambar stroke** pada kanvas kerja umum (2000×2000px @ 10-20 layer aktif), dan tidak boleh freeze/hang saat operasi berat (resize, filter, export).

**A. Tile-Based Canvas Rendering**
- Kanvas besar (>2048px sisi terpanjang) WAJIB dipecah menjadi tile (mis. 256×256px atau 512×512px, dikonfigurasi berdasar ukuran dokumen).
- Hanya tile yang bersinggungan dengan area dirty (area yang baru digambar/diubah) yang di-render ulang — bukan seluruh kanvas — meniru pendekatan Photoshop/CSP internal tiling untuk kanvas besar.
- Tile di luar viewport (tidak terlihat) tidak diproses sampai pengguna men-scroll/pan ke sana.

**B. Dirty-Rect Tracking**
- Setiap operasi gambar (stroke, fill, filter) WAJIB men-track bounding-box area yang berubah (`expandStrokeBox` sudah ada di 10.2 — perluas konsep ini ke seluruh pipeline render, bukan hanya undo).
- Compositor hanya me-render ulang region dirty, bukan full-canvas repaint per frame — ini WAJIB, bukan opsional, karena menjadi fondasi utama performa di hardware rendah.

**C. Worker Offloading (Web Worker + OffscreenCanvas)**
- Tugas berat yang TIDAK butuh interaksi real-time WAJIB dipindah ke Web Worker terpisah agar main thread (yang menangani input pointer) tetap responsif:
  - Filter berat (Gaussian Blur radius besar, Liquify, Unsharp Mask pada kanvas besar).
  - Encoding timelapse (WebCodecs sudah async, pastikan tidak memblokir input).
  - Penulisan/pembacaan file besar (.asp, PSD export/import, penulisan tile ke IndexedDB).
  - Auto-save berkala (lihat 24.6.E).
- Fallback: jika `OffscreenCanvas` tidak didukung browser, jalankan di main thread dengan chunking (`requestIdleCallback`/`setTimeout` batching) agar tetap tidak membekukan UI sepenuhnya.

**D. Adaptive Quality (Deteksi FPS Otomatis)**
- Sistem WAJIB memonitor FPS aktual secara berkala (moving average tiap 60 frame).
- Jika FPS turun di bawah ambang batas (default 24fps) selama periode berkelanjutan, sistem otomatis menurunkan kualitas render sementara: nonaktifkan mipmap smoothing sementara, turunkan resolusi preview brush cursor, kurangi frekuensi update panel Navigator/thumbnail, hingga FPS pulih — lalu kembalikan kualitas penuh secara bertahap.
- Indikator opsional di status bar (ikon kecil) menunjukkan saat mode "Performa Hemat" aktif, dengan toggle manual untuk memaksa mode ini on/off.

**E. Auto-Save Ringan**
- Auto-save berkala ke penyimpanan lokal (interval dikonfigurasi, default 3 menit) dijalankan di background/idle time (Worker/`requestIdleCallback`), tidak boleh menyebabkan frame drop terasa saat trigger.

**F. Akselerasi GPU Opsional (WebGL2 Compositing Layer)**
- Sebagai lapisan opsional (bukan pengganti Canvas2D, agar tetap kompatibel di perangkat tanpa GPU layak): kompositing layer akhir (blend semua layer menjadi satu output) dapat dijalankan via WebGL2 shader untuk blend mode yang mahal secara komputasi (mis. banyak layer dengan blend non-normal).
- Deteksi kapabilitas: jika WebGL2 tidak tersedia/GPU terlalu lemah (deteksi via `WEBGL_debug_renderer_info` heuristik atau uji benchmark cepat saat startup), otomatis fallback penuh ke Canvas2D tanpa penurunan fungsi (hanya performa).
- Toggle manual di Pengaturan Performa: "Gunakan Akselerasi GPU (jika tersedia)" — default ON dengan auto-fallback.

**G. Manajemen Memori**
- Canvas pool (sudah ada di 11.2) diperluas cakupannya ke seluruh sistem tile.
- Batas layer/tile tidak aktif dapat di-*evict* dari memori (disimpan terkompresi/dilepas) dan di-*rehydrate* saat dibutuhkan kembali, dengan indikator loading singkat jika terjadi (untuk dokumen sangat besar dengan puluhan layer).
- Peringatan memori: jika estimasi penggunaan RAM mendekati batas aman browser/OS, tampilkan toast peringatan dengan saran (merge layer, kurangi resolusi undo history, dsb).

**H. Startup & Load Time**
- Waktu buka aplikasi (cold start) ditarget di bawah 2 detik pada hardware rendah — dicapai dengan lazy-loading modul non-esensial (mis. modul export video, modul PSD parser) hanya saat pertama kali dipakai, bukan saat startup.

---

## 25. ASSET MANAGER (BRUSH FORM & TEXTURE)

### 25.1 Asset Storage
- `window.__assets`: { form[], tex[], formGroups[], texGroups[] }
- Persist ke localStorage (`asp_assets`)
- Ada batas ukuran localStorage (5-10MB)

### 25.2 Asset Types
- **Brush Forms:** Gambar bentuk ujung kuas (PNG/JPG/WebP/BMP)
- **Brush Textures:** Gambar tekstur kertas (PNG/JPG/WebP)

### 25.3 Asset Groups
- Form groups dan tex groups terpisah
- Grup: string nama
- Items bisa punya `group` field (string atau "" untuk unorganized)

### 25.4 Asset Picker Dialog
- Buka dari tombol `bFormBtn` atau `bTexBtn`
- Layout kiri-kanan: group list (115px) + item grid (flex)
- Pinned top: default items (Simple Circle / No Texture + Built-in textures)
- Scroll items: custom assets terfilter per group
- Footer: tombol Asset Manager

### 25.5 Asset Manager Dialog
- Kategori: Brush Forms / Brush Textures (tabs)
- Group list kiri
- Item grid kanan dengan canvas thumbnail 44x44px
- Tombol: 📁 Folder Brush, ⇩ Import PNG/BMP, ＋ Grup, ⇄ Pindah, 🗑 Hapus
- Klik kanan nama grup: Rename / Delete Group
- Import: `<input type="file">` → resize ke 128x128 → simpan sebagai data URL

### 25.6 Folder Brush Integration
- Lihat section 22.6
- Aset dari folder ditandai `dir: 1`
- Dipisahkan dari aset user saat clear
- Relink brush saat folder berubah

---

## 26. DIALOG & MODAL SYSTEM

### 26.1 Modal Functions
- `showModal(title, bodyHTML, buttons[])`: tampilkan modal
- `closeModal()`: tutup modal
- Button format: `{label, fn, primary?: boolean}`
- Primary button: style berbeda (warna biru/oranye)

### 26.2 Specific Dialogs
1. **New Canvas:** Width, Height, DPI, Background (White/Transparent/Custom), Name
2. **Resize Canvas:** Width, Height, Anchor point (9-position grid)
3. **Resize Image:** Width, Height, Resample method
4. **Export Image:** Format, Resolution scale, Quality, Paper checkbox
5. **Export Timelapse:** Format (MP4/WebM/GIF/PNG), FPS, Resolution, Bitrate, Hold last frame (seconds)
6. **Brush Editor:** Lihat section 6.11
7. **Filter Dialogs:** Generic parameter slider + preview
8. **Performance Settings:** Stream FPS, scale, panel throttle, tl throttle, mipmap toggle
9. **Timelapse Quality:** Resolution, format, quality, default export res
10. **About:** Versi, info GPU, credits

---

## 27. PERSISTENCE & STORAGE

### 27.1 localStorage Keys
- `asp_assets`: brush forms & textures (data URL + metadata)
- `asp_groups`: tool group configuration + brush presets
- `asp_perf`: performance settings (stream FPS, scale, throttle)
- `asp_tlqual`: timelapse quality settings

### 27.2 IndexedDB
- `asp_dir`: File System Access API handle untuk folder brush (Chrome)

### 27.3 File Save Format (.asp)
- Line 1: JSON header (metadata, layer structure, settings)
- Line 2+: Base64-encoded timelapse frames (newline-separated)
- Total: text-based, bisa di-load secara streaming

---

## 28. BROWSER / PLATFORM COMPATIBILITY

### 28.1 Desktop (Electron/NW.js)
- Full Node.js fs access untuk file save/load
- Native window management
- IPC untuk file slicing (lazy load)
- Folder brush auto-detect

### 28.2 Desktop (Neutralino)
- Native open/save dialogs
- Binary file reading
- Window creation untuk live stream

### 28.3 Browser (Chrome/Edge)
- File System Access API untuk folder brush
- IndexedDB untuk handle persistence
- BroadcastChannel untuk OBS live stream
- File input fallback untuk semua operasi file

---

## 29. RINGKASAN JUMLAH FITUR

| Kategori | Jumlah |
|----------|--------|
| Tools | 28+ |
| Kategori brush engine realistis | 10 (Pensil, Pena/Dip Pen, Kuas Bristle, Airbrush, Marker, Watercolor, Blur, Smudge, Effect Pen, Scatter) |
| Brush presets default | 20+ |
| Brush parameters | 17 dasar + Dynamics binding (pressure/tilt/velocity/rotation) per parameter |
| Layer blend modes | 18 |
| Ruler/symmetry modes | 13 (8 asli + curve, radial, 4/5-titik, ruler-per-layer, snap toggle) |
| Selection tools | 6 |
| Vector tools | 10+ |
| Filters/Effects | 25+ (termasuk Liquify, Tone Curve, Vibrance, Glow, Chromatic Aberration, Vignette, Halftone, Perspective Warp Mesh) |
| Manga/Comic tools | Screentone Layer Engine, Panel/Frame Tool, Speech Balloon Tool, Print Guide, Multi-Halaman |
| Export formats | 5+ (PNG, JPG, WebP, MP4, WebM, GIF, PSD) |
| Import formats | 7+ (ASP, PSD, ABR, PNG, JPG, WebP, BMP) |
| Keyboard shortcuts | 30+ default, 100% dapat dikustomisasi (semua tool/brush/menu/parameter) |
| Radial quick menu | 8 slot custom |
| Stabilizer levels | 23 (0-15 + S1-S7) + mode Anticipate + visual lazy-nib |
| Pen tablet input | Pressure, Tilt X/Y, Azimuth/Barrel Rotation, Palm Rejection, Kalibrasi |
| Undo levels | 30 |
| Color models | 2 (RGB + HSV) |
| Multi-canvas tabs | Unlimited |
| Layer types | 5 (Raster, Vector, Text, Folder, Screentone) |
| Stream/Viewer modes | 2 (Creator, Canvas) |
| Transport layers | 3 (Direct, postMessage, BroadcastChannel) |
| Timelapse quality presets | 4 (720p-4K) |
| Performance architecture | Tile rendering, dirty-rect, Web Worker offload, adaptive quality, GPU (WebGL2) opsional |
| Tool groups | 4 default (Basic, Binary, Artistic, Ver.1) |
| Dokumen governance AI-agent | BUILD_LOG.md, MASTER_CHECKLIST.md, TEST_REPORT.md, KNOWN_ISSUES.md |

---

## 30. SPESIFIKASI TEKNIS NON-FUNGSIONAL

### 30.1 Zero Dependencies
- Semua kode JavaScript vanilla (tidak ada npm, tidak ada CDN, tidak ada framework)
- Hanya 1 file HTML
- Canvas2D API sebagai rendering engine
- WebCodecs API untuk encoding video (optional, fallback ada)
- Tidak ada library eksternal

### 30.2 Content Security Policy
```
default-src 'self' data: blob: file:;
img-src 'self' data: blob: file:;
style-src 'self' 'unsafe-inline';
script-src 'self' 'unsafe-inline';
connect-src 'self';
worker-src 'self' blob:;
media-src 'self' blob: data:;
```

### 30.3 Browser Requirements
- `<canvas>` 2D dengan `willReadFrequently`
- `<input type="color">`
- `DOMMatrix` API
- Pointer Events API
- `getCoalescedEvents()` (opsional)
- `createImageBitmap()` (opsional)
- WebCodecs `VideoEncoder` (opsional, untuk MP4/WebM export)
- `BroadcastChannel` API (opsional, untuk OBS)
- `File System Access API` (opsional, untuk folder brush)
- `IndexedDB` (opsional, untuk handle persistence)

### 30.4 Performance
- HiDPI canvas scaling (devicePixelRatio)
- Canvas pool untuk menghindari alokasi berulang
- Mipmap pyramid untuk zoom-out mulus
- Compositor cache (folder, node)
- Flat draw cache untuk layer tree sederhana
- Stream cache terpisah dari compositor utama
- GUI throttle: panel update 150ms, bukan setiap frame render
- Lihat Section 24.6 untuk arsitektur performa "ultra ringan" secara lengkap (tile rendering, worker offload, adaptive quality, GPU opsional)

---

## 31. FILTER & EFFECTS ENGINE (LENGKAP)

> Setiap filter WAJIB memiliki dialog preview live (perubahan terlihat langsung di kanvas sebelum di-apply, dengan tombol Batal/Terapkan) dan WAJIB bekerja hormat terhadap seleksi aktif (hanya memengaruhi area terpilih jika ada seleksi).

### 31.1 Filter Blur & Distorsi
- **Gaussian Blur:** radius 0-100px, preview live.
- **Motion Blur:** sudut arah (0-360°) + jarak (0-100px).
- **Radial Zoom Blur:** titik pusat dapat digeser di preview, kekuatan 0-100.
- **Radial Spin Blur:** sama seperti zoom blur namun arah rotasi, kekuatan 0-100.
- **Liquify (Wajib Baru — setara CSP):** brush interaktif untuk mendorong/menarik/memutar/mengembang-kempiskan piksel secara lokal secara real-time (bukan dialog parameter statis) — mode: Dorong (Push), Putar Searah Jarum Jam, Putar Berlawanan, Kembang (Bloat), Kempis (Pucker), Ratakan Ulang (Reconstruct — mengembalikan sebagian area ke kondisi asli). Ukuran brush & kekuatan dapat diatur, undo per-stroke (bukan hanya undo seluruh sesi liquify).

### 31.2 Filter Ketajaman & Piksel
- **Sharpen / Unsharp Mask:** amount, radius, threshold.
- **Mosaic/Pixelate:** ukuran blok 2-100px.
- **Noise — Add:** jenis Gaussian/Uniform, intensitas 0-100 (berguna untuk tekstur film grain).
- **Noise — Reduce (Denoise):** berguna untuk membersihkan hasil scan/import foto referensi.

### 31.3 Filter Warna
- **Level Correction:** histogram RGB dengan slider input black/gamma/white + output black/white, per-channel (RGB/R/G/B).
- **Tone Curve (Wajib Baru — setara CSP):** editor kurva interaktif per-channel (RGB gabungan + R/G/B individual), titik kontrol dapat ditambah/digeser, interpolasi spline halus, preview live histogram di background kurva.
- **Hue/Saturation/Lightness:** slider per komponen, opsi "colorize" (satu hue seragam).
- **Vibrance (Wajib Baru):** menaikkan saturasi warna yang kurang jenuh lebih kuat dibanding warna yang sudah jenuh (berbeda dari Saturation biasa yang linear rata) — mencegah skin-tone over-saturate.
- **Brightness/Contrast:** slider standar -100 s/d 100.
- **Color Balance:** 3 range (Shadows/Midtones/Highlights) × 3 axis (Cyan-Red, Magenta-Green, Yellow-Blue).
- **Invert Color:** langsung tanpa dialog (Ctrl+I opsional shortcut).
- **Binarization (Threshold):** slider ambang 0-255, hasil hitam-putih murni (berguna untuk cleanup lineart).
- **Posterize:** jumlah level warna 2-32.
- **Gradient Map:** memetakan luminance ke gradasi warna kustom (color stops dapat diedit).

### 31.4 Filter Efek Artistik & Cahaya
- **Glow/Bloom (Wajib Baru):** area terang di-highlight dengan cahaya menyebar lembut — intensitas, threshold, radius.
- **Chromatic Aberration (Wajib Baru):** pergeseran channel R/G/B ke arah berlawanan dari tepi kanvas, intensitas dapat diatur — efek gaya lensa kamera/glitch.
- **Vignette (Wajib Baru):** penggelapan/pencerahan tepi kanvas radial, intensitas + radius + softness.
- **Halftone/Screentone Filter:** menerapkan pola titik/garis halftone ke seluruh layer/seleksi berdasarkan luminance — lihat detail penuh & versi manga-specific (layer-based, bukan hanya filter) di **Section 32.1**.

### 31.5 Filter Transformasi Lanjutan
- **Perspective Warp (Mesh Transform):** grid mesh (default 3×3, dapat ditambah subdivisi) di atas layer, setiap titik grid dapat digeser bebas untuk deformasi non-linear (melebihi kemampuan Transform 4-corner biasa di Section 7.9) — berguna untuk menyesuaikan tekstur pada permukaan lengkung/perspektif kompleks.

### 31.6 Filter Dialog Umum
- Setiap filter parameter memakai komponen `.sbar` yang sama dengan Brush Parameters (konsistensi UI).
- Preview real-time di kanvas utama (bukan thumbnail kecil di dialog) — perubahan terlihat langsung, murah secara komputasi via downsampled preview saat dragging slider (kualitas penuh saat slider dilepas, memanfaatkan arsitektur adaptive quality di 24.6.D).
- Tombol: Reset Parameter, Batal, Terapkan (OK).

---

## 32. MANGA & COMIC AUTHORING TOOLS (Fitur Andalan CSP — Wajib Ada)

> Ini adalah kategori fitur yang membuat CSP dominan di kalangan komikus/mangaka. Karena Author Sultra berfokus pada produksi komik, kategori ini WAJIB diimplementasi lengkap, bukan opsional.

### 32.1 Screentone / Halftone Layer Engine
- **Layer Tipe Baru: Screentone Layer** — layer khusus yang menyimpan parameter pola (bukan raster piksel statis), sehingga skala/densitas dapat diubah non-destruktif kapan saja setelah diterapkan.
- Parameter: `dotShape` (Bulat/Garis/Silang/Diamond), `linesPerInch` (LPI — kerapatan pola, 10-85), `angle` (sudut rotasi pola 0-180°), `density` (0-100%, seberapa besar dot mengisi grid — mengontrol persepsi "gelap-terang" tone).
- Aplikasi: pengguna membuat seleksi/mask (misal area bayangan pada karakter), lalu "Konversi Seleksi ke Screentone" — area terpilih menjadi layer screentone dengan mask mengikuti bentuk seleksi.
- Bisa juga digambar langsung menggunakan **Screentone Brush** (lihat Section 6.14 poin 12) untuk aplikasi manual bebas bentuk.
- Preset pola bawaan minimum: Dot 10-60-85 LPI, Line Screen horizontal/vertikal/diagonal, Cross-hatch, Gradient Tone (dot yang density-nya berubah gradasi otomatis mengikuti arah tertentu — untuk efek bayangan gradasi manga klasik).
- Rasterize opsional: layer screentone dapat di-rasterize ke raster biasa jika diperlukan untuk performa/export final.

### 32.2 Panel / Frame (Border Komik) Tool
- **Panel Layer/Ruler:** alat untuk menggambar bingkai panel komik — grid otomatis (mis. 2×3, 3×3, custom) atau digambar manual per-panel dengan bentuk bebas (rectangle, polygon bebas untuk panel non-standar/pecah).
- **Gutter (jarak antar panel):** dapat diatur seragam (px/mm) di seluruh halaman.
- **Panel sebagai Clipping Mask otomatis:** setiap panel secara otomatis membuat clipping boundary — konten/layer yang digambar "di dalam" panel otomatis terpotong rapi sesuai bentuk panel (tidak perlu manual clipping tiap kali).
- **Border Style:** ketebalan garis panel, warna, sudut membulat (rounded corner) opsional.
- **Page Template:** simpan/muat template tata-letak panel yang sering dipakai sebagai preset (untuk kecepatan produksi halaman berseri).

### 32.3 Speech Balloon / Text Balloon Tool
- Tool khusus untuk membuat balon dialog: bentuk dasar (Oval, Kotak, Awan/Pikiran, Meledak/Teriak, Kotak Narasi).
- Ekor balon (tail/pointer) dapat digambar dan diarahkan ke posisi karakter yang bicara, dengan handle untuk mengatur posisi & lebar pangkal ekor.
- Balon terikat dengan Text Layer di dalamnya (teks otomatis mengikuti ukuran balon, dengan opsi auto-resize balon mengikuti panjang teks atau sebaliknya).
- Style balon: ketebalan outline, warna isi (default putih), warna outline (default hitam) — dapat disimpan sebagai preset.

### 32.4 Manga-Specific Ruler & Guide
- **Guide Halaman Cetak (Bleed/Trim/Safe Margin):** garis panduan non-print untuk area cetak aman, trim line, dan bleed area — sesuai standar cetak komik (dapat diatur mm/inch sesuai target penerbit).
- **Page Numbering & Multi-Page Project:** dokumen dapat berisi banyak "halaman" dalam satu proyek (mirip Story/Multi-page management CSP) dengan navigasi antar halaman, bukan hanya multi-tab dokumen terpisah (lihat integrasi dengan Section 4 Multi-Tab — halaman komik adalah unit di atas tab dokumen, satu "proyek komik" dapat berisi N halaman).

---

## 33. SISTEM PENGUJIAN (QA / TESTING PROTOCOL) — WAJIB DIIKUTI AI AGENT

> Bagian ini adalah pelengkap teknis dari aturan governance di **Section 0**. Section 0 menjelaskan *aturan perilaku* agent; section ini menjelaskan *cara teknis* melakukan pengujian.

### 33.1 Prinsip Pengujian
- Setiap sub-fitur WAJIB memiliki minimal 1 **test case manual terskrip** (langkah-langkah eksplisit + hasil yang diharapkan) sebelum ditandai selesai.
- Pengujian dilakukan secara fungsional (menjalankan aplikasi sungguhan/browser automation, bukan hanya membaca kode) — jika lingkungan agent mendukung automation (mis. Playwright/Puppeteer), gunakan untuk regression test otomatis pada alur kritikal (buka file, gambar stroke, undo/redo, export).
- Untuk aspek visual yang sulit diverifikasi otomatis (kehalusan goresan, akurasi warna brush, hasil filter), agent WAJIB mengambil screenshot bukti dan mendeskripsikan hasil observasi secara tertulis di `TEST_REPORT.md`.

### 33.2 Kategori Test Wajib per Modul
1. **Functional Test:** fitur melakukan apa yang dispesifikasikan (mis. "Stabilizer level 10 menghasilkan lag kurang lebih X px dari posisi kursor asli").
2. **Edge Case Test:** kondisi batas (kanvas 1×1px, brush size 1000, undo di stack kosong, dokumen tanpa layer, tekanan pressure = 0, dsb).
3. **Regression Test:** memastikan modul-modul sebelumnya yang berkaitan langsung tetap berfungsi setelah perubahan modul baru.
4. **Performance Test (untuk modul terkait rendering/brush/compositor):** ukur FPS/latency pada skenario representatif, bandingkan dengan target di Section 24.6/30.4.
5. **Cross-Platform Test (minimal sampling):** verifikasi berjalan di target platform utama (Electron desktop + Browser Chrome/Edge) — tidak perlu menguji di semua kombinasi tablet fisik, tapi WAJIB menguji jalur kode Pointer Events dengan simulasi pressure/tilt.

### 33.3 Alur Kerja Pengujian per Modul
1. Selesaikan implementasi sub-fitur.
2. Tulis/jalankan test case sesuai kategori di 33.2 yang relevan.
3. Catat hasil (lulus/gagal + detail) di `TEST_REPORT.md`.
4. Jika gagal: perbaiki, uji ulang — TIDAK boleh lanjut dengan status gagal dibiarkan tanpa catatan di `KNOWN_ISSUES.md`.
5. Setelah seluruh sub-fitur modul lulus: jalankan regression smoke test terhadap modul-modul sebelumnya yang relevan.
6. Update status modul di `MASTER_CHECKLIST.md` menjadi `DONE` hanya setelah langkah 1-5 tuntas.

### 33.4 Format `TEST_REPORT.md` (Wajib Konsisten)
```
## Modul [Nomor Section]: [Nama Modul]
Tanggal: YYYY-MM-DD
Status: TESTED / DONE

### Test Case 1: [Nama Fitur]
Langkah: 1) ... 2) ... 3) ...
Hasil Diharapkan: ...
Hasil Aktual: ...
Status: LULUS / GAGAL
Catatan: ...

### Regression Check
Modul terkait diuji ulang: [daftar]
Hasil: Tidak ada regresi ditemukan / [detail regresi]
```

---

## 34. MASTER CHECKLIST — DEFINITION OF DONE (RANGKUMAN SELURUH MODUL)

> Agent WAJIB menyalin daftar ini menjadi `MASTER_CHECKLIST.md` yang hidup (checkbox dicentang seiring progres, dengan tanggal & referensi test case). Daftar ini adalah ringkasan navigasi — rincian lengkap tiap butir tetap merujuk ke section terkait di atas.

- [ ] **Modul 0 — Governance:** File `BUILD_LOG.md`, `MASTER_CHECKLIST.md`, `TEST_REPORT.md`, `KNOWN_ISSUES.md` dibuat dan dipelihara aktif.
- [ ] **Modul 2 — Layout UI Utama:** Titlebar, Menubar (semua item), Toolbar (semua grup), Left Column (Navigator+Layer), Tools Column (Color+Tools+Brush+Vector+Teks), Canvas Area, Statusbar, Modal, Toast, Context Menu.
- [ ] **Modul 4 — Multi-Dokumen/Tab:** Semua fungsi document state & tab API.
- [ ] **Modul 5 — Sistem Layer:** 4 tipe layer, properti layer, tree ops, drag-drop, mask editing, private/live-only.
- [ ] **Modul 6 — Brush Engine (LENGKAP):** Struktur brush, 5 mode dasar + perluasan A-J (pensil, pena, kuas bristle, airbrush, marker, watercolor, blur, smudge, effect pen, scatter), Dynamics (pressure/tilt/velocity/rotation) + Curve Editor, 12+ preset wajib, Brush Editor dialog lengkap.
- [ ] **Modul 7 — Tool System:** Semua 28+ tools + grup tab sistem.
- [ ] **Modul 8 — Stabilizer:** Normal 0-15 + S1-S7 + mode Anticipate + visual feedback lazy-nib.
- [ ] **Modul 9 — Ruler & Symmetry:** 8 mode asli + 5 mode tambahan (curve, radial, 4/5-titik, ruler-per-layer, snap toggle cepat).
- [ ] **Modul 10 — Undo/Redo:** Semua tipe entry + optimasi rect.
- [ ] **Modul 11 — Compositor:** Recursive compose, cache, mipmap, mask, paper texture.
- [ ] **Modul 12 — Viewport/Navigasi:** Zoom/pan/rotate/flip/fit.
- [ ] **Modul 13 — Input Pipeline:** Pointer events + **dukungan tablet lengkap (pressure/tilt/azimuth/palm rejection/kalibrasi)**.
- [ ] **Modul 14 — Vector Engine:** Semua 10 tool vector + preview + raster-to-vector.
- [ ] **Modul 15 — Text Engine:** Struktur, rendering, panel binding.
- [ ] **Modul 16 — Bucket/Flood Fill:** Scanline fill + magic wand.
- [ ] **Modul 17 — Gradation:** Linear FG→transparan/FG→BG dengan seleksi.
- [ ] **Modul 18 — Color System:** Wheel, RGB/HSV slider, mixer, swatch, FG/BG, eyedropper.
- [ ] **Modul 19 — Live Stream Engine:** 3 lapis transport, mode creator/canvas.
- [ ] **Modul 20 — Timelapse Engine:** Capture, session, memori, pause/resume.
- [ ] **Modul 21 — Export System:** PNG/JPG/WebP, MP4/WebM/GIF/PNG-seq, PSD, brush export.
- [ ] **Modul 22 — Import System:** ASP, PSD, ABR, image, brush tip, folder brush.
- [ ] **Modul 23 — Shortcut System:** Semua shortcut default **+ sistem kustomisasi total (remap semua kontrol) + radial quick menu**.
- [ ] **Modul 24 — Performance:** Stream/panel/timelapse throttle + **arsitektur ultra ringan (tile rendering, dirty-rect, worker offload, adaptive quality, GPU opsional, manajemen memori, startup time)**.
- [ ] **Modul 25 — Asset Manager:** Brush form & texture storage, groups, picker, manager dialog, folder brush.
- [ ] **Modul 26 — Dialog/Modal System:** Semua dialog spesifik.
- [ ] **Modul 27 — Persistence:** localStorage keys, IndexedDB, format .asp.
- [ ] **Modul 28 — Kompatibilitas Platform:** Electron/NW.js, Neutralino, Browser.
- [ ] **Modul 30 — Spesifikasi Non-Fungsional:** Zero dependencies, CSP, browser requirements, performance.
- [ ] **Modul 31 — Filter & Effects (LENGKAP):** Blur/distorsi + Liquify, ketajaman/piksel + noise, warna + Tone Curve + Vibrance, artistik/cahaya (Glow/Chromatic Aberration/Vignette), Halftone filter, Perspective Warp mesh.
- [ ] **Modul 32 — Manga & Comic Tools:** Screentone layer engine, Panel/Frame tool, Speech Balloon tool, guide cetak & multi-halaman.
- [ ] **Modul 33 — QA Protocol:** Seluruh modul di atas memiliki `TEST_REPORT.md` lengkap tanpa status GAGAL yang dibiarkan.
- [ ] **Regresi Penuh Akhir:** Setelah seluruh modul DONE, jalankan satu putaran regresi penuh dari Modul 2 sampai Modul 32 berurutan sebelum proyek dinyatakan selesai 100%.

---
*Dokumen ini mendeskripsikan rancangan lengkap aplikasi AuthorSultra Paint V3 (CSP-Killer Edition) tanpa menyertakan kode. Setiap fitur, fungsi, UI, dan interaksi yang tercantum — termasuk seluruh perluasan di Section 0 dan Section 6.11–34 — WAJIB diimplementasikan 100% tanpa ada yang dilewati, disederhanakan, atau ditunda tanpa persetujuan eksplisit. AI agent otonom yang membangun aplikasi ini WAJIB mematuhi Build Governance Protocol (Section 0) dan QA Protocol (Section 33) secara ketat dan berurutan.*
