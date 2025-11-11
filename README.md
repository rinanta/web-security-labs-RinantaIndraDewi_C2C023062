# web-security-labs-RinantaIndraDewi_C2C023062
# SQLInjection-XSS-KerentananUpload-BrokenAccessControl
# 1. SQL INJECTION
# 🔴 Versi VULNERABLE (Rentan)
# $query = "SELECT * FROM users WHERE username='$username' AND password='$password'";
# $result = mysqli_query($conn, $query);
# Karakteristik:
# Input langsung digabung ke query SQL (string concatenation)
# Tidak ada validasi atau sanitasi input
