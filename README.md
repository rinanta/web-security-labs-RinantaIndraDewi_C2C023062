# web-security-labs-RinantaIndraDewi_C2C023062
# 1. SQL INJECTION
Versi VULNERABLE (Rentan)
$query = "SELECT * FROM users WHERE username='$username' AND password='$password'";
$result = mysqli_query($conn, $query);
Karakteristik:
Input langsung digabung ke query SQL (string concatenation)
Tidak ada validasi atau sanitasi input
Password disimpan dalam plaintext
Karakter khusus (', --, OR) tidak di-filter
Contoh Eksploitasi:
Username: admin' OR '1'='1
Password: ' OR '1'='1
**Versi SAFE (Aman)**
Kode:$stmt = $pdo->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->execute([$username, $password]);
Karakteristik:
Menggunakan Prepared Statements (PDO)
Parameter Binding - input sebagai data, bukan kode
Password di-hash dengan password_hash() (bcrypt/argon2)
Escape otomatis untuk karakter khusus

# 2. CROSS-SITE SCRIPTING (XSS)
Versi VULNERABLE (Rentan)
Kode: // Simpan komentar
$stmt->execute([$username, $comment]);
// Tampilkan langsung tanpa encoding
echo $row['comment']; // VULNERABLE!
Karakteristik:
Output ditampilkan langsung ke HTML
Tidak ada encoding karakter khusus
Script berbahaya dieksekusi di browser
**Save**
Output di-encode dengan htmlspecialchars()
Konversi karakter khusus ke HTML entities
Script tidak dieksekusi, ditampilkan sebagai teks
<script>alert('XSS')</script>
Hasil:
Script TIDAK dieksekusi
Tampil sebagai: &lt;script&gt;alert('XSS')&lt;/script&gt;
Browser menampilkan sebagai teks biasa

# 3. FILE UPLOAD VULNERABILITY
Versi VULNERABLE (Rentan)
Karakteristik:
Tidak ada validasi ekstensi file
Nama file asli digunakan langsung
File disimpan di dalam webroot (bisa diakses langsung)
Tidak ada pengecekan MIME type
// File: shell.php
<?php system($_GET['cmd']); ?>
File PHP BERHASIL di-upload
Akses: http://localhost/uploads/shell.php?cmd=whoami
Remote Code Execution (RCE) BERHASIL
Penyerang kontrol penuh atas server

**Versi SAFE (Aman)**
Karakteristik:

Whitelist validation - hanya ekstensi tertentu
File size limit - maksimal 5MB
MIME type check - validasi tipe file
Filename randomization - nama file diacak dengan UUID
Storage outside webroot atau disable execution
// File: shell.php
<?php system($_GET['cmd']); ?>
Hasil:
File PHP DITOLAK
Error: "File type not allowed"
File tidak tersimpan
Tidak ada RCE

# 4. BROKEN ACCESS CONTROL (IDOR)
 Versi VULNERABLE (Rentan)
 Karakteristik:

Tidak ada pengecekan kepemilikan (ownership)
ID dari URL langsung digunakan
Tidak ada validasi session vs requested ID
Semua data bisa diakses dengan ubah ID
URL Normal  : profile.php?id=101  (data sendiri)
URL Exploit : profile.php?id=102  (data user lain)
Data user lain BISA DIAKSES
Tampil: Nama, Email, Nilai, Program Studi
Horizontal Privilege Escalation berhasil

**ersi SAFE (Aman)**
Karakteristik:

Server-side authorization check
Validasi: requested_id == session_user_id
Double validation (PHP + SQL level)
Session management yang proper
Error message tidak bocorkan info
Session User ID : 101
URL Exploit     : profile.php?id=102
Hasil:
Akses DITOLAK
Tampil: "Access Denied"
Data user lain TERLINDUNGI
Log dapat diaktifkan untuk monitoring
