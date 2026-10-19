# Architecture - Sistem Reservasi Ruang/Laboratorium (NIM 2440304024)

## Gaya arsitektur
Layered Architecture tiga lapis (Presentation, Business Logic, Data Access) dalam monolit modular, dengan Nginx sebagai reverse proxy, basis data relasional (PostgreSQL), dan cache in-memory.

## Komponen
- **Presentation:** ReservasiController, ApprovalController, RuangController, LaporanController
- **Business Logic:** ReservasiService, ApprovalService (Strategy Tk.1 dan Tk.2), ConflictChecker, AuthService (RBAC), ReportService
- **Data Access:** ReservasiRepo, RuangRepo, PenggunaRepo, PersetujuanRepo (ORM + connection pool maks. 25)

## Penanganan P = 124
- Beban puncak sekitar 12,4 permintaan/detik (1 permintaan per pengguna per 10 detik); target rancangan minimal 25 permintaan/detik.
- Server aplikasi stateless, dapat dijalankan 2 instance di belakang Nginx.
- Cache ketersediaan (TTL 30 detik, invalidasi saat tulis) dan indeks (ruang, tanggal, jam mulai).
- Waktu respons fungsi utama maksimal 3 detik pada persentil ke-95.

## Penanganan constraint persetujuan dua tingkat
- ApprovalService memakai Strategy: VerifikasiTingkat1Strategy dan PersetujuanTingkat2Strategy.
- RBAC memastikan hanya peran terkait yang dapat membuat persetujuan pada tingkatnya.
- Basis data menegakkan constraint unik (reservasi, tingkat) dan anti-bentrok jadwal.

## Trade-off
Monolit berlapis dipilih dibanding microservices: sederhana dan konsisten untuk skala P = 124, dengan konsekuensi komponen tidak dapat diskalakan terpisah.
