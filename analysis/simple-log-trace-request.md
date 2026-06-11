# Analisis HTTP Traffic Endpoint `/api/products/`

## Ringkasan Pengujian

Pengujian dilakukan terhadap endpoint REST API `/api/products/` yang berjalan pada host lokal `127.0.0.1` port `8088`. Capture traffic dilakukan menggunakan `curl --trace-ascii` sehingga seluruh header dan payload HTTP dapat diamati secara detail.

---

# 1. Koneksi TCP

```text
== Info: Trying 127.0.0.1:8088...
== Info: Connected to 127.0.0.1 (127.0.0.1) port 8088 (#0)
```

### Analisis

Sebelum pertukaran data HTTP terjadi, client melakukan koneksi TCP ke server.

| Parameter          | Nilai     |
| ------------------ | --------- |
| Destination IP     | 127.0.0.1 |
| Destination Port   | 8088      |
| Transport Protocol | TCP       |
| Status             | Connected |

Karena komunikasi terjadi pada `localhost (127.0.0.1)`, paket tidak melewati jaringan eksternal dan langsung ditangani oleh network stack sistem operasi.

---

# 2. HTTP Request Analysis

## Raw Request Header

```http
GET /api/products/ HTTP/1.1
Host: 127.0.0.1:8088
User-Agent: curl/7.81.0
Accept: */*
Content-Type: application/json
```

## Header Breakdown

### Request Line

```http
GET /api/products/ HTTP/1.1
```

| Komponen | Nilai          |
| -------- | -------------- |
| Method   | GET            |
| Resource | /api/products/ |
| Protocol | HTTP/1.1       |

Method GET digunakan untuk meminta representasi resource tanpa mengubah data pada server.

### Host Header

```http
Host: 127.0.0.1:8088
```

Menentukan virtual host dan port tujuan request.

### User-Agent

```http
User-Agent: curl/7.81.0
```

Mengidentifikasi aplikasi client yang mengirim request.

### Accept

```http
Accept: */*
```

Client menerima seluruh tipe media yang dapat dikirim server.

### Content-Type

```http
Content-Type: application/json
```

Menunjukkan format data yang digunakan selama komunikasi adalah JSON.

---

# 3. HTTP Response Analysis

## Status Line

```http
HTTP/1.0 200 OK
```

### Analisis

| Parameter     | Nilai    |
| ------------- | -------- |
| Protocol      | HTTP/1.0 |
| Status Code   | 200      |
| Reason Phrase | OK       |

Menarik bahwa client menggunakan **HTTP/1.1**, sedangkan server merespons menggunakan **HTTP/1.0**. Hal ini menunjukkan implementasi server Python menggunakan handler HTTP sederhana yang masih mengirim response versi 1.0.

---

## Server Header

```http
Server: CommProtocolsDemo/1.0 Python/3.12.13
```

Informasi server menunjukkan:

| Field       | Value             |
| ----------- | ----------------- |
| Application | CommProtocolsDemo |
| Version     | 1.0               |
| Runtime     | Python 3.12.13    |

---

## Content-Type

```http
Content-Type: application/json; charset=utf-8
```

Response body dikirim dalam format JSON dengan encoding UTF-8.

UTF-8 dipilih karena:

* Mendukung karakter Unicode.
* Kompatibel dengan standar JSON modern.
* Efisien untuk data berbasis teks.

---

## Content-Length

```http
Content-Length: 986
```

Server mengirim payload sebesar:

```text
986 bytes
```

atau sekitar:

```text
0.96 KB
```

Nilai ini digunakan client untuk menentukan kapan seluruh body response telah diterima.

---

# 4. CORS Configuration Analysis

## Access-Control-Allow-Origin

```http
Access-Control-Allow-Origin: *
```

Mengizinkan request dari seluruh domain.

---

## Allowed Methods

```http
Access-Control-Allow-Methods:
GET, POST, PUT, PATCH, DELETE, OPTIONS
```

Endpoint mendukung operasi CRUD lengkap:

| Method  | Fungsi          |
| ------- | --------------- |
| GET     | Membaca data    |
| POST    | Menambah data   |
| PUT     | Update penuh    |
| PATCH   | Update sebagian |
| DELETE  | Menghapus data  |
| OPTIONS | CORS Preflight  |

---

## Allowed Headers

```http
Access-Control-Allow-Headers: Content-Type
```

Browser diperbolehkan mengirim header `Content-Type`.

---

# 5. Response Payload Analysis

## Metadata

```json
{
  "resource": "products",
  "case": "Product",
  "count": 5
}
```

### Interpretasi

| Field    | Nilai    |
| -------- | -------- |
| resource | products |
| case     | Product  |
| count    | 5        |

Server mengembalikan koleksi resource produk sebanyak 5 item.

---

## Sample Data Record

```json
{
  "id": 4,
  "name": "Smart watch",
  "category": "watch",
  "price": 132000,
  "stock": 20,
  "createdAt": "2026-06-11T14:22:06+0000"
}
```

### Struktur Data

| Field     | Type               |
| --------- | ------------------ |
| id        | Integer            |
| name      | String             |
| category  | String             |
| price     | Integer            |
| stock     | Integer            |
| createdAt | ISO-8601 Timestamp |

Format timestamp:

```text
2026-06-11T14:22:06+0000
```

mengikuti standar ISO-8601 sehingga mudah diproses oleh berbagai bahasa pemrograman dan database.

---

# 6. Encoding dan Data Transfer

Dari capture:

```text
<= Recv data, 986 bytes (0x3da)
```

Analisis:

| Parameter    | Nilai     |
| ------------ | --------- |
| Payload Size | 986 bytes |
| Hexadecimal  | 0x3DA     |
| Encoding     | UTF-8     |
| Format       | JSON      |

JSON dikirim dalam bentuk plain text sehingga dapat dibaca langsung tanpa proses decoding tambahan.

---

# 7. Connection Termination

```text
== Info: Closing connection 0
```

Karena server menggunakan HTTP/1.0, koneksi ditutup setelah response selesai dikirim.

Karakteristik ini berbeda dengan HTTP/1.1 yang umumnya menggunakan persistent connection (Keep-Alive) untuk mengurangi overhead pembukaan koneksi baru.

---

# Kesimpulan

Hasil capture menunjukkan komunikasi HTTP berjalan dengan sukses antara client `curl` dan server Python pada port 8088. Request menggunakan metode GET melalui HTTP/1.1, sedangkan server merespons menggunakan HTTP/1.0 dengan status `200 OK`. Response berisi payload JSON berukuran 986 byte yang memuat 5 data produk. Header CORS telah dikonfigurasi untuk mendukung akses lintas origin dan operasi CRUD lengkap. Dari sisi protokol, seluruh proses request, response, transfer payload, serta terminasi koneksi berhasil diamati melalui trace HTTP yang menyerupai analisis layer aplikasi pada Wireshark.
