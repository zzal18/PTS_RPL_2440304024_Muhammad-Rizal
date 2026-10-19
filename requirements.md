# Requirements - Sistem Reservasi Ruang/Laboratorium (NIM 2440304024)

## Functional Requirements

### FR-01 - Melihat Ketersediaan Ruang/Lab
- **Aktor:** Pemohon (Mahasiswa/Dosen)
- **Deskripsi:** Sistem menampilkan daftar ruang/lab beserta slot tanggal dan jam yang kosong atau terisi. Pemohon dapat memfilter berdasarkan jenis ruang, kapasitas, dan tanggal.
- **Prioritas:** High
- **Alasan:** Pemohon harus tahu slot kosong sebelum mengajukan; mengatasi akar masalah data jadwal yang tidak terpusat.

### FR-02 - Mengajukan Permintaan Reservasi
- **Aktor:** Pemohon (Mahasiswa/Dosen)
- **Deskripsi:** Pemohon mengisi ruang, tanggal, jam mulai dan selesai, keperluan, dan jumlah peserta. Sistem memvalidasi isian, menolak slot yang bentrok, lalu menyimpan reservasi berstatus Menunggu Verifikasi.
- **Prioritas:** High
- **Alasan:** Fungsi inti sistem dan titik awal seluruh alur persetujuan.

### FR-03 - Verifikasi Tingkat 1
- **Aktor:** Koordinator Lab
- **Deskripsi:** Koordinator Lab meninjau kelengkapan data dan kesesuaian ruang (kapasitas dan jenis kegiatan), lalu meloloskan reservasi ke Tk.2 atau menolaknya dengan catatan.
- **Prioritas:** High
- **Alasan:** Mewujudkan constraint persetujuan dua tingkat dan menyaring permohonan sebelum sampai ke Kaprodi.

### FR-04 - Persetujuan Tingkat 2
- **Aktor:** Ketua Program Studi (Kaprodi)
- **Deskripsi:** Kaprodi hanya dapat memproses reservasi yang sudah lolos Tk.1. Kaprodi menyetujui atau menolak (catatan wajib saat menolak). Persetujuan mengunci slot jadwal.
- **Prioritas:** High
- **Alasan:** Keputusan akhir yang sah; tidak boleh ada reservasi Disetujui tanpa verifikasi Tk.1.

### FR-05 - Pemantauan Status dan Pembatalan
- **Aktor:** Pemohon (Mahasiswa/Dosen)
- **Deskripsi:** Pemohon melihat status reservasi beserta riwayat keputusan, dan dapat membatalkan reservasi yang masih berstatus Menunggu Verifikasi atau Menunggu Persetujuan.
- **Prioritas:** Medium
- **Alasan:** Transparansi status menjawab masalah permohonan yang tidak diketahui nasibnya.

### FR-06 - Penjadwalan dan Pencegahan Bentrok
- **Aktor:** Pemohon, Koordinator Lab (melihat); Sistem (memproses)
- **Deskripsi:** Jadwal ruang hanya memuat reservasi berstatus Disetujui. Sistem memeriksa konflik saat pengajuan dan sekali lagi saat persetujuan Tk.2, sehingga satu slot tidak dapat dipesan ganda.
- **Prioritas:** High
- **Alasan:** Bentrok jadwal adalah masalah utama kasus ini.

### FR-07 - Pengelolaan Data Ruang/Lab
- **Aktor:** Admin
- **Deskripsi:** Admin menambah, mengubah, dan menonaktifkan data ruang/lab (nama, jenis, kapasitas, lokasi, status aktif).
- **Prioritas:** Medium
- **Alasan:** Data ruang yang benar adalah dasar ketersediaan dan validasi kapasitas.

### FR-08 - Rekap Reservasi
- **Aktor:** Koordinator Lab, Kaprodi
- **Deskripsi:** Sistem menyajikan rekap reservasi per periode, per ruang, dan per status (diajukan, disetujui, ditolak, dibatalkan).
- **Prioritas:** Medium
- **Alasan:** Dibutuhkan untuk evaluasi pemanfaatan ruang dan laporan ke pimpinan.

## Nonfunctional Requirements (P = 124, X = 3 detik)

| ID | Kategori | Pernyataan terukur | Alasan |
|---|---|---|---|
| NFR-01 | Performance | Pada beban 124 pengguna aktif bersamaan, fungsi utama (lihat ketersediaan, ajukan reservasi, verifikasi, persetujuan) memiliki waktu respons maksimal 3 detik pada persentil ke-95 dalam kondisi operasional normal. | X = 3 detik dipilih karena pengguna web yang menunggu lebih dari 3 detik cenderung mengulang aksi, sehingga berisiko menimbulkan pengajuan ganda. |
| NFR-02 | Capacity / Scalability | Sistem mampu melayani minimal 124 pengguna aktif bersamaan. Dengan asumsi satu permintaan per pengguna setiap 10 detik, beban puncak sekitar 12,4 permintaan/detik; sistem dirancang untuk minimal 25 permintaan/detik (cadangan 2x). | Cadangan 2x mengantisipasi lonjakan pada awal semester dan masa praktikum. |
| NFR-03 | Security | Akses diatur berdasarkan 4 peran (Pemohon, Koordinator Lab, Kaprodi, Admin). Kata sandi disimpan sebagai hash (bcrypt), seluruh lalu lintas memakai HTTPS (TLS 1.2 atau lebih baru), dan sesi kedaluwarsa setelah 30 menit tidak aktif. Endpoint verifikasi Tk.1 dan persetujuan Tk.2 hanya dapat diakses peran terkait (100% permintaan dari peran lain ditolak). | Keputusan persetujuan harus tidak dapat dipalsukan oleh pengguna biasa. |
| NFR-04 | Usability | Pemohon baru dapat mengajukan reservasi dalam maksimal 5 langkah dan 2 menit tanpa pelatihan, dan antarmuka dapat digunakan pada layar ponsel (lebar minimal 360 px). | Mayoritas pemohon adalah mahasiswa yang mengakses melalui ponsel. |
| NFR-05 | Reliability / Availability | Ketersediaan sistem minimal 99% pada jam layanan 07.00-21.00 WITA (downtime maksimal sekitar 4 jam per bulan) dan basis data dibackup harian. | Jam layanan 14 jam x 30 hari = 420 jam; 1% setara kurang lebih 4,2 jam. |
| NFR-06 | Constraint NIM: persetujuan dua tingkat | Sebanyak 100% reservasi berstatus Disetujui harus memiliki tepat dua catatan persetujuan berurutan (Tk.1 lalu Tk.2) yang dibuat oleh pengguna dengan peran berbeda. Sistem menolak setiap upaya melompati tingkat atau persetujuan Tk.2 oleh peran selain Kaprodi. | Menjamin aturan bisnis inti terpenuhi tanpa pengecualian. |
| NFR-07 | Data integrity | Pada pengujian 124 permintaan yang menarget slot yang sama secara bersamaan, hanya satu reservasi yang berhasil disetujui (0 kasus double booking). | Bentrok jadwal adalah akar masalah utama; harus dijaga di level basis data, bukan hanya di aplikasi. |

