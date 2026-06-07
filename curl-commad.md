# uts-comp2-1

# Communication Protocol UTS - Product Inventory API

## Identitas

* Nama: [Mohammad Raifandi Baiqi]
* NIM: [25120500002]
* Kelas: [Comp2]
* Case: Product / Inventory API
* Repository: []

---

# Deskripsi Case

Pada praktikum ini dilakukan pengujian terhadap Product Inventory API yang tersedia pada Communication Protocol Demo App. Pengujian dilakukan menggunakan metode HTTP GET, POST, dan PUT untuk melihat proses pengambilan data produk, penambahan produk baru, serta pembaruan data produk.

Format data yang digunakan oleh API adalah JSON (JavaScript Object Notation). Analisis dilakukan terhadap request, response, status code, header, dan struktur data yang dikirimkan oleh server.

---

# Base URL

```text
http://127.0.0.1:8088
```

---

# Curl Commands

## 1. GET All Products

Mengambil seluruh daftar produk yang tersedia.

```bash
curl -i http://127.0.0.1:8088/api/products/
```

### Tujuan

Menampilkan seluruh data produk yang tersimpan pada server.

---

## 2. GET Product Detail

Mengambil detail produk berdasarkan ID.

```bash
curl -i http://127.0.0.1:8088/api/products/5
```

### Tujuan

Menampilkan informasi lengkap produk dengan ID tertentu.

---

## 3. POST Product 1

Menambahkan produk baru Smart Watch.

```bash
curl -i -X POST http://127.0.0.1:8088/api/products \
-H "Content-Type: application/json" \
-d '{"name":"Smart watch","category":"watch","price":132000,"stock":22}'
```

### Request Body

```json
{
  "name": "Smart watch",
  "category": "watch",
  "price": 132000,
  "stock": 22
}
```

---

## 4. POST Product 2

Menambahkan produk baru Monitor.

```bash
curl -i -X POST http://127.0.0.1:8088/api/products \
-H "Content-Type: application/json" \
-d '{"name":"monitor","category":"tech","price":112000,"stock":10}'
```

### Request Body

```json
{
  "name": "monitor",
  "category": "tech",
  "price": 112000,
  "stock": 10
}
```

---

## 5. PUT Update Product

Memperbarui data produk dengan ID 5.

```bash
curl -i -X PUT http://127.0.0.1:8088/api/products/5 \
-H "Content-Type: application/json" \
-d '{"name":"Smart watch","category":"watch","price":132000,"stock":20}'
```

### Request Body

```json
{
  "name": "Smart watch",
  "category": "watch",
  "price": 132000,
  "stock": 20
}
```

---

# Analisis Response

## Status Code

### 200 OK

Menunjukkan request berhasil diproses oleh server dan data berhasil dikirimkan.

### 201 Created

Menunjukkan resource baru berhasil dibuat melalui request POST.

### 400 Bad Request

Menunjukkan format request tidak sesuai atau endpoint yang digunakan tidak valid.

### 404 Not Found

Menunjukkan data dengan ID tertentu tidak ditemukan.

---

# Analisis Header

Contoh header response:

```http
Content-Type: application/json; charset=utf-8
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, PATCH, DELETE, OPTIONS
```

Keterangan:

* Content-Type menunjukkan format data yang digunakan adalah JSON.
* Access-Control-Allow-Origin memungkinkan akses dari berbagai origin.
* Access-Control-Allow-Methods menunjukkan method HTTP yang didukung API.

---

# Analisis Encoding JSON

API menggunakan format JSON sebagai media pertukaran data.

Contoh struktur data:

```json
{
  "id": 5,
  "name": "Smart watch",
  "category": "watch",
  "price": 132000,
  "stock": 20
}
```

Penjelasan:

* id : Integer
* name : String
* category : String
* price : Integer
* stock : Integer

JSON dipilih karena ringan, mudah dibaca manusia, dan didukung hampir seluruh bahasa pemrograman modern.

---

# Kesimpulan

Berdasarkan hasil pengujian, Product Inventory API berhasil menjalankan operasi pengambilan data produk, penambahan produk baru, dan pembaruan data produk menggunakan protokol HTTP.

API menggunakan format JSON sebagai media pertukaran data dengan struktur yang sederhana dan mudah dipahami. Melalui praktikum ini dapat dipahami penggunaan method GET, POST, dan PUT, pemanfaatan status code HTTP, serta proses komunikasi antara client dan server menggunakan REST API.
