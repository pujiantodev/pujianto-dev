---
pubDatetime: 2025-01-28T06:52:57.000+07:00
modDatetime:
title: Optimalkan Pencarian dengan Laravel Retensi Query Parameters Menggunakan `withQueryString()`
featured: false
draft: false
tags:
  - laravel
description: optimalkan pencarian dengan laravel dengan withquerystring
---

## Table of contents

![withQueryString](../../assets/images/withQueryString.png)

## Optimalkan Pencarian dengan Laravel: Retensi Query Parameters Menggunakan `withQueryString()`

## Masalah

Bayangkan Anda sedang mengembangkan sebuah aplikasi web berbasis Laravel yang memiliki fitur daftar produk lengkap dengan pencarian, filter, dan sorting. Ketika pengguna melakukan pencarian dengan kata kunci tertentu, atau memilih filter dan sorting, semuanya berjalan dengan baik. Namun, ada masalah ketika pengguna berpindah ke halaman berikutnya dalam hasil paginasi.

Saat pengguna mengklik halaman berikutnya, parameter pencarian, filter, atau sorting yang mereka pilih hilang. Alhasil, daftar produk kembali menampilkan data default, dan pengguna harus mengulang proses pencarian atau filter mereka. Masalah ini tentu mengganggu pengalaman pengguna dan dapat menurunkan tingkat kepuasan mereka terhadap aplikasi Anda.

Masalah ini sering muncul karena query parameters seperti `?search=keyword&sort=asc` tidak secara otomatis disertakan oleh sistem paginasi Laravel saat berpindah halaman. Lalu, bagaimana cara mengatasinya?

## Solusi

Laravel menyediakan metode praktis bernama `withQueryString()` yang dapat digunakan bersama paginasi untuk memastikan bahwa semua query parameters yang ada di URL akan tetap dipertahankan saat navigasi antar halaman.

Dengan menggunakan `withQueryString()`, Anda tidak perlu lagi khawatir kehilangan parameter seperti pencarian, filter, atau sorting. Fitur ini sangat membantu untuk meningkatkan pengalaman pengguna dalam aplikasi berbasis data.

## Contoh Implementasi

Berikut adalah contoh implementasi dari `withQueryString()` dalam sebuah controller Laravel:

### Langkah 1: Buat Controller dengan Fitur Pencarian dan Paginasi

Misalkan kita sedang membangun sebuah halaman daftar produk dengan pencarian dan sorting.

```php
use App\Models\Product;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    public function index(Request $request)
    {
        // Inisialisasi query untuk produk
        $products = Product::query();

        // Filter berdasarkan pencarian
        if ($request->has('search')) {
            $products->where('name', 'like', '%' . $request->query('search') . '%');
        }

        // Tambahkan sorting jika ada
        if ($request->has('sort')) {
            $products->orderBy('name', $request->query('sort'));
        }

        // Paginate dengan withQueryString()
        $products = $products->paginate(10)->withQueryString();

        return view('products.index', compact('products'));
    }
}
```

### Langkah 2: Buat Tampilan (Blade) untuk Menampilkan Daftar Produk

Selanjutnya, kita membuat tampilan sederhana untuk menampilkan daftar produk beserta fitur pencarian dan paginasi:

```blade
<!-- resources/views/products/index.blade.php -->
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Daftar Produk</h1>

    <!-- Form Pencarian -->
    <form method="GET" action="{{ route('products.index') }}" class="mb-4">
        <input type="text" name="search" placeholder="Cari produk..." value="{{ request('search') }}">
        <select name="sort">
            <option value="asc" {{ request('sort') === 'asc' ? 'selected' : '' }}>A-Z</option>
            <option value="desc" {{ request('sort') === 'desc' ? 'selected' : '' }}>Z-A</option>
        </select>
        <button type="submit">Cari</button>
    </form>

    <!-- Daftar Produk -->
    <ul>
        @foreach ($products as $product)
            <li>{{ $product->name }}</li>
        @endforeach
    </ul>

    <!-- Paginasi -->
    <div class="pagination">
        {{ $products->links() }}
    </div>
</div>
@endsection
```

### Langkah 3: Uji Coba

1. Jalankan aplikasi Anda dan buka halaman daftar produk.
2. Lakukan pencarian, pilih filter, dan coba navigasi ke halaman berikutnya.
3. Perhatikan bahwa query parameters seperti `?search=keyword&sort=asc` tetap ada di URL selama navigasi antar halaman.

## Kesimpulan

Dengan menggunakan metode `withQueryString()` dari Laravel, Anda dapat mempertahankan parameter pencarian, filter, dan sorting di URL selama navigasi antar halaman pada hasil paginasi. Hal ini meningkatkan pengalaman pengguna dengan memastikan data yang relevan tetap tersedia tanpa perlu mengulang proses pencarian.

Fitur sederhana ini sangat membantu dalam pengembangan aplikasi berbasis data, seperti marketplace, sistem manajemen produk, atau aplikasi katalog. Jadi, jika Anda sedang membangun aplikasi dengan fitur serupa, pastikan untuk memanfaatkan `withQueryString()`! Siap mencoba? Selamat mengembangkan! 🚀
