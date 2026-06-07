# Encoding Analysis - Product Inventory API

## Overview

Pada praktikum ini dilakukan analisis terhadap Product Inventory API yang berjalan pada Communication Protocol Demo App. API menggunakan protokol HTTP dan format data JSON untuk komunikasi antara client dan server.

Pengujian dilakukan menggunakan beberapa endpoint yang mencakup operasi GET, POST, dan PUT untuk melihat proses pengambilan, penambahan, serta pembaruan data produk.

---

# Data Encoding Format

API menggunakan format **JSON (JavaScript Object Notation)** sebagai media pertukaran data.

Contoh response:

```json
{
  "resource": "products",
  "count": 3,
  "data": [
    {
      "id": 1,
      "name": "Laptop Pro 14",
      "category": "electronics",
      "price": 14500000,
      "stock": 12
    }
  ]
}
```

JSON dipilih karena:

* Mudah dibaca manusia.
* Ringan dibandingkan XML.
* Didukung oleh hampir semua bahasa pemrograman.
* Cocok digunakan untuk REST API.

---

# HTTP Headers Analysis

Contoh response header:

```http
Content-Type: application/json; charset=utf-8
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, PATCH, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type
```

### Content-Type

Menunjukkan bahwa data yang dikirim oleh server menggunakan format JSON dengan encoding UTF-8.

### Access-Control-Allow-Origin

Mengizinkan resource diakses dari berbagai origin sehingga mendukung komunikasi lintas domain (CORS).

### Access-Control-Allow-Methods

Menunjukkan metode HTTP yang didukung oleh API yaitu GET, POST, PUT, PATCH, DELETE, dan OPTIONS.

---

# Request Analysis

## GET Request

Endpoint:

```http
GET /api/products
```

Tujuan:

Mengambil seluruh data produk yang tersedia pada server.

Karakteristik:

* Tidak mengubah data.
* Tidak memiliki request body.
* Digunakan untuk membaca informasi.

---

## POST Request

Endpoint:

```http
POST /api/products
```

Request Body:

```json
{
  "name": "Smart watch",
  "category": "watch",
  "price": 132000,
  "stock": 22
}
```

Tujuan:

Menambahkan produk baru ke dalam sistem.

Karakteristik:

* Mengirim data dalam format JSON.
* Membuat resource baru pada server.

---

## PUT Request

Endpoint:

```http
PUT /api/products/5
```

Request Body:

```json
{
  "name": "Smart watch",
  "category": "watch",
  "price": 132000,
  "stock": 20
}
```

Tujuan:

Memperbarui data produk yang sudah ada.

Karakteristik:

* Mengirim seluruh data produk yang diperbarui.
* Digunakan untuk update resource.

---

# JSON Structure Analysis

Struktur data produk:

```json
{
  "id": 5,
  "name": "Smart watch",
  "category": "watch",
  "price": 132000,
  "stock": 20
}
```

| Field    | Tipe Data | Deskripsi             |
| -------- | --------- | --------------------- |
| id       | Integer   | Identitas unik produk |
| name     | String    | Nama produk           |
| category | String    | Kategori produk       |
| price    | Integer   | Harga produk          |
| stock    | Integer   | Jumlah stok tersedia  |

---

# Status Code Analysis

## 200 OK

Menunjukkan request berhasil diproses dan server mengembalikan data yang diminta.

## 201 Created

Menunjukkan resource baru berhasil dibuat melalui request POST.

## 400 Bad Request

Menunjukkan request tidak sesuai dengan aturan API atau data yang dikirim tidak valid.

Contoh error:

```json
{
  "error": "Use POST /api/products to create a new resource",
  "resource": "products"
}
```

Pesan tersebut membantu client memahami kesalahan penggunaan endpoint.

## 404 Not Found

Menunjukkan resource yang diminta tidak ditemukan pada server.

---

# Communication Flow

1. Client mengirim HTTP Request ke endpoint API.
2. Server menerima dan memproses request.
3. Server mengakses data produk.
4. Server mengembalikan HTTP Response.
5. Data dikirim dalam format JSON.
6. Client membaca dan menampilkan hasil response.

---

# Conclusion

Berdasarkan hasil pengujian, Product Inventory API menggunakan protokol HTTP dengan format data JSON sebagai mekanisme komunikasi utama. Format JSON memberikan struktur data yang sederhana, ringan, dan mudah dipahami. Penggunaan method GET, POST, dan PUT memungkinkan proses pengambilan, penambahan, serta pembaruan data produk berjalan secara terstruktur sesuai prinsip REST API.

Analisis menunjukkan bahwa status code dan header yang diberikan server membantu client memahami hasil request serta menangani kesalahan dengan lebih baik.
