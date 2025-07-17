# UAS Keamanan Komputer

## Identitas
- Nama: Muhammad Faizi Munir
- NIM: 241080200115
- Kelas:
- Repo GitHub: [https://github.com/faizimunir/tugas-UAS]

---

## Bagian A – Bug Fixing JWT REST API

### Bug 1: Token tetap aktif setelah logout
**Penjelasan:**  
1.  Tidak ada pembatasan waktu token dibuat, sehingga membuat token tetap valid. 

2. tidak ada pembatasan akses endpoint membuat semua user bisa mengakses siapa saja.

3. data dari role yang lain, misal user biasa, dapat mengakses data yang dimiliki oleh admin.

**Solusi:**  
1. perlu adanya pembatasan token agar request token, token yang diberikan memiliki batasan waktu. mislanya membuat batasan waktu token valid selama 1 jam seperti :

function encodeJWT($payload)
{
    $key = getJWTKey();
    $issuedAt = time();
    $expirationTime = $issuedAt + 3600; // token berlaku 1 jam atau 3600 detik

    $payload = array_merge($payload, [
        'iat' => $issuedAt,
        'exp' => $expirationTime,
    ]);

    return JWT::encode($payload, $key, 'HS256');
}

2. perlu dilakukan autentikasi JWT di setiap jalannya perinta atau api, sehingga perlu selalu dilakukan pengecekan apakah user tersebut memang memiliki hak akses dalam data tersebut.

$token = $this->request->getHeaderLine('Authorization');
        if (!empty($token) && preg_match('/Bearer\s(\S+)/', $token, $matches)) {
            $token = $matches[1];
            $decodedToken = \decodeJWT($token);

3. Setelah token JWT berhasil diverifikasi dan identitas pengguna (misalnya, `user1`) berhasil diekstrak dari payload token, akan menambahkan pemeriksaan otorisasi di logika controller untuk setiap permintaan yang melibatkan data pengguna. karena hanya user, jadi hanya diberikan akses data untuk user.

## Bagian B – Simulasi Serangan dan Solusi

### Jenis Serangan: Broken Access Control  
**Simulasi Postman:**  
...

**Solusi Implementasi:**  
...

---

## Bagian C – Refleksi Teori & Etika

### 1. CIA Triad dalam Keamanan Informasi  
...

### 2. UU ITE yang relevan 
Pasal 30: Akses Ilegal (Unauthorized Access)
Pasal 32: Perubahan, Perusakan, dan Pemindahan Informasi/Dokumen Elektronik Secara Ilegal

### 3. Pandangan Al-Qur'an  
- Surah Al-Baqarah: 205  
orang - orang yang merusak, mencuri data atau melakukan perubahan data tidak sesuai merupakan "mengadakan kerusakan" di ranah digital. Allah tidak menyukai perbuatan merusak, baik di dunia fisik maupun digital, karena ia mengganggu ketertiban dan keharmonisan yang diciptakan-Nya. Oleh karena itu, cybercrime dan tindakan merusak sistem adalah haram dan bertentangan dengan ajaran Islam.

### 4. Etika Cyber dan Kejujuran  
Jika mereka menemukan celah cyber dari sebuah website sebaiknya dia memberikan informasi kepada pihak yang berwenang dari website tersebut, sehingga mereka bisa memperbaikinya. 
Jujur melakukan segala aktifitas pengolahan data user sesuai ketentuan yang ada.
dan amanah menjalankan pekerjaan pengolahan data sesuai ketentuan, tidak menggunakan data orang lain untuk kepentingan sendiri, atau merusaknya.

