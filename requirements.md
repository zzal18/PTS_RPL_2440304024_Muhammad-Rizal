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

