# QA — Institut STTS Campus Studio v2.0

Tanggal: 30 September 2026. Ini laporan pengujian aplikasi nyata, bukan penilaian dari gambar konsep.

## Hasil terukur

| Kelompok | Hasil |
|---|---:|
| Uji unit/kontrak Node (core, motion, welcome, storage, model campus) | **101 lulus, 0 gagal** |
| Browser: alur edit foto, titik, panah, motion, ekspor/download | **35 pemeriksaan lulus** |
| Browser: CRUD, impor, panorama CPU, validasi media, mobile | **37 pemeriksaan lulus** |
| Total pemeriksaan browser | **72 lulus** |
| Uncaught JavaScript errors pada dua suite | **0** |

Runtime yang diuji: Chromium 144.0.7559.96 pada Linux, Playwright Python 1.57.0, Node.js 22.16.0. Tampilan desktop diuji pada 1440×950; pengunjung desktop 1440×900; mobile 390×844. Renderer panorama fallback diuji dengan WebGL dinonaktifkan secara eksplisit dan instance renderer aktual bertipe `software`.

Model baru diuji dengan template **5 gedung / 18 lantai / 26 entri area / 0 foto**. Fixture sintetis terpisah digunakan untuk pengujian foto, panorama dan migrasi, dan tidak dimuat pada template atau hasil pengunjung yang diserahkan.

## Hal penting yang benar-benar diuji

Foto diunggah melalui file input UI, bukan hanya memasukkan state palsu. Titik ditempatkan dan digeser dengan event pointer. Panah dan panah balik diklik pada viewer; label ruangan dan breadcrumb harus ikut berubah. Kamera diuji lewat perubahan nilai zoom dari waktu ke waktu, bukan sekadar keberadaan tombol. Opening welcome memiliki animasi browser; kamera menunggu proses masuk selesai. Tombol pause/play kecil, reduced motion, panel informasi, dan navigasi ke area tanpa foto diuji.

HTML pengunjung didapat dari event download, dimuat ulang sebagai halaman independen, lalu Start dan panahnya digunakan. Backup JSON dan CSV juga diperiksa dari hasil download. JSON v2 serta fixture AruTour v1 diimpor melalui file input, validasi gambar, dan dialog konfirmasi.

CRUD struktur memeriksa induk lantai/ruang, kode/nomor duplikat, perpindahan ruang, konfirmasi hapus, pembersihan data terkait, dan Undo/Redo. Model publik menghilangkan ruang tersembunyi, catatan internal, dan aset tidak terpakai; panah ke target tersembunyi memblokir ekspor. Payload HTML/URL/prototype dan rasio panorama tidak valid memiliki pengujian kontrak.

Utilitas server lokal Python juga menjalani smoke test HTTP loopback: server terikat ke 127.0.0.1, `/index.html` mengembalikan HTTP 200 dengan 234.864 byte. Ini bukan pengujian persistensi browser pada origin HTTP.

## Batas verifikasi — jangan dibaca sebagai certification production

**Navigasi file:// dan URL HTTP(S) diblokir oleh kebijakan browser lingkungan runner.** Rendering menggunakan Playwright `set_content`. IndexedDB pada origin kosong ditolak secara nyata; aplikasi menampilkan kebutuhan Backup JSON, bukan berpura-pura berhasil menyimpan. Kontrol transaksi, konflik revisi, dan kuota storage mendapat tes kontrak dengan adapter tiruan. **Persistensi native setelah browser ditutup, browser multi-tab aktual, file:// di komputer pengguna, dan server-origin autosave belum diverifikasi.**

Jalur GPU WebGL asli, performa pada batas 300 foto/100 MB, Safari/Firefox, perangkat ponsel fisik, sentuhan/pinch fisik, host publik, dan aksesibilitas penuh belum mendapatkan validasi menyeluruh. Tampilan mobile adalah viewport Chromium, bukan klaim uji perangkat nyata. Launcher Windows disertakan sebagai utilitas opsional tetapi tidak dijalankan pada Windows di lingkungan ini.

Tidak ada pengujian maupun klaim bahwa foto 2D membentuk geometri 3D, bahwa direktori sama dengan denah, atau bahwa panah menyatakan jalur berjalan yang benar. Belum ada foto asli ruangan, logo resmi, atau persetujuan konten kampus. Kesiapan isi foto tetap **0/26**, terpisah dari hasil uji kode.

## Perbaikan yang ditemukan saat pengujian

Keterkaitan heading ruangan diperbaiki agar runtime baru tidak menulis ke judul tersembunyi mesin lama. Undo tidak lagi mendapat entri duplikat dari event field tanpa perubahan. Shortcut Escape pada dialog pencarian dibuat deterministik. Preview tidak ditutupi toast autosave editor. Transisi masuk memiliki zoom nyata selain fade, motion tidak berjalan di belakang welcome, dan navigasi selama transisi sibuk dijaga.

## Checkpoint artefak

- `index.html` SHA-256: `652fd0fdbdc24ba5ee741bd1b3a3c5abb479f7a59f35d0c7ea941ce9831c967a`
- `ISTTS-Campus-Tour.html` SHA-256: `83ef870dff83a35fd5408d6b9fccdebdd8e77b49a1d23ed6824e98838ee7f748`
- `ISTTS-Template.campus.json` SHA-256: `0c99636c982f919f3fc22db50d3348049c7f84b2233beca7117c9ef7f3d93887`

Screenshot pada `docs/previews` dirender langsung dari checkpoint aplikasi. File hasil fixture pengujian tidak dipakai sebagai demo kampus publik.

## Daftar 72 pemeriksaan browser

1. Seed shows 5 buildings, 18 floors and 26 areas
2. Storage denial is surfaced without claiming saved
3. 2D upload creates photo in selected room
4. Uploaded raster re-encoded to WebP
5. Click-to-place info point and text editor are wired
6. Numeric position updates rendered anchor
7. Drag updates stored point coordinates
8. Second room gets independent photo
9. Arrow target resolves to another room by stable scene ID
10. Reverse arrow is actually created
11. Duplicate reverse arrow prevented
12. Motion settings persist without running editor camera
13. Preview starts on welcome before initializing motion
14. Welcome entry has actual browser animation
15. Start enters configured first room
16. Camera zoom changes over time after entry
17. No large legacy motion chip exists
18. Compact pause stops camera
19. Compact play resumes motion
20. Information dot opens its description
21. Opening point information pauses motion
22. Clicking navigation arrow updates scene, room and breadcrumb
23. Reverse arrow returns to origin
24. Directory searches cross-building area
25. Missing photo destination has explicit state
26. Home returns to welcome and prevents hidden motion
27. Preview closes without destroying draft
28. Publication contains no blocking errors after valid arrows
29. HTML export triggers a genuine download
30. Public export excludes Studio class and private notes
31. Downloaded public HTML works independently and follows arrows
32. Campus copyright and Arustudio appear in standalone tour
33. Backup JSON preserves hierarchy, points and welcome
34. Inventory checklist CSV downloads all 26 areas
35. No uncaught JS errors in integration flow
36. Add building via form updates model and structure cards
37. Add floor links to correct parent building
38. Duplicate floor form is rejected without altering structure
39. Add room links to selected floor
40. Room rename keeps stable hierarchy ID
41. Undo restores room label
42. Redo restores latest room label
43. Move room updates breadcrumb without changing ID
44. Corrupt media rejected atomically
45. Non-2:1 panorama rejected atomically
46. 360 upload creates a panorama scene
47. 360 CPU renderer works when WebGL unavailable
48. Drag rotates panorama camera
49. Panorama hotspot stores spherical coordinates
50. Reduced-motion preference disables automatic panorama rotation
51. Welcome editor live preview uses saved heading
52. Custom cover upload does not create an extra room or tour photo
53. Logo upload maps brand media independently
54. Dangerous URL edit rejected
55. Actual JSON reimport preserves media and room location
56. Delete cascade requires confirmation
57. Deleting building removes its rooms and scene
58. Undo restores deleted hierarchy and photograph
59. Actual v1 file import produces unmapped building and preserves photos
60. Actual legacy motion survives migration
61. Mobile welcome has no horizontal overflow
62. Mobile building card filters directory to correct building
63. Mobile entry starts without overlaid large navigation panel
64. Mobile building selector changes selected building
65. Mobile floor control reaches third floor
66. Search empty state is explicit
67. Escape dismisses directory dialog
68. Mobile editor has no horizontal overflow
69. Mobile location tab shows searchable hierarchy
70. Mobile edit tab exposes room controls
71. Mobile canvas tab shows photo upload area
72. No uncaught errors across CRUD, imports, mobile and CPU panorama