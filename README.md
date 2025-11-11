#web-security-labs-RinantaIndraDewi_C2C023062
1. SQL INJECTION
🔴 Versi VULNERABLE (Rentan)
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
✅ Login BERHASIL tanpa password valid
Query menjadi: WHERE username='admin' OR '1'='1' AND password='' OR '1'='1'
Kondisi selalu TRUE → Bypass authentication

🟢 Versi SAFE (Aman)
Kode:$stmt = $pdo->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->execute([$username, $password]);
Karakteristik:
Menggunakan Prepared Statements (PDO)
Parameter Binding - input sebagai data, bukan kode
Password di-hash dengan password_hash() (bcrypt/argon2)
Escape otomatis untuk karakter khusus
