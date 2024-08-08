---
title: "Tạo một ứng dụng CRUD đơn giản với Laravel là cách tuyệt vời để làm quen với framework này"
meta_title: ""
description: "Tạo một ứng dụng CRUD đơn giản với Laravel là cách tuyệt vời để làm quen với framework này"
date: 2024-07-29T05:00:00Z
image: "/images/trung-crud-operation-laravel-min.png"
categories: ["Information"]
author: "Lê Huỳnh Trung"
tags: ["Laravel"]
draft: false
---

Tạo một ứng dụng CRUD đơn giản với Laravel là cách tuyệt vời để làm quen với framework này. CRUD là viết tắt của Create, Read, Update, Delete - bốn thao tác cơ bản của một ứng dụng web. Dưới đây là hướng dẫn từng bước để tạo một ứng dụng CRUD đơn giản trong Laravel.

## 1. Cài đặt Larave`l

Đầu tiên, bạn cần cài đặt một dự án Laravel mới. Mở terminal hoặc command prompt và chạy các lệnh sau:

```
bash
Sao chép mã
composer create-project --prefer-dist laravel/laravel crud-app
cd crud-app
php artisan serve
```

Điều này sẽ khởi động một server phát triển tại http://localhost:8000.

## 2. Cấu hình cơ sở dữ liệu

Mở tệp .env và cập nhật cấu hình cơ sở dữ liệu của bạn:

```
dotenv
Sao chép mã
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=crud_db
DB_USERNAME=root
DB_PASSWORD=
```

Tạo cơ sở dữ liệu crud_db trong MySQL bằng công cụ quản lý cơ sở dữ liệu bạn ưa thích (phpMyAdmin, HeidiSQL, ...).

## 3. Tạo Migration và Model

Tiếp theo, tạo một model và migration cho Post bằng lệnh Artisan:

```
bash
Sao chép mã
php artisan make:model Post -m
```

Mở tệp migration trong database/migrations/ và cập nhật như sau:

```
php
Sao chép mã
public function up()
{
Schema::create('posts', function (Blueprint $table) {
$table->id();
$table->string('title');
$table->text('content');
$table->timestamps();
});
}
```

Chạy migration để tạo bảng trong cơ sở dữ liệu:

```
bash
Sao chép mã
php artisan migrate
```

## 4. Tạo Controller và Routes

Tạo một controller để quản lý các thao tác CRUD:

```
bash
Sao chép mã
php artisan make:controller PostController --resource
```

Mở tệp routes/web.php và thêm các routes cho Post:

```
php
Sao chép mã
use App\Http\Controllers\PostController;

Route::resource('posts', PostController::class);
```

## 5. Tạo Views

Tạo các view để hiển thị và thao tác với dữ liệu. Trong thư mục resources/views/, tạo thư mục posts và các tệp sau:

- index.blade.php
- create.blade.php
- edit.blade.php
- show.blade.php

**index.blade.php:**

```
  blade
  Sao chép mã
  @extends('layout')

@section('content')

<h1>Posts</h1>
<a href="{{ route('posts.create') }}">Create Post</a>
<ul>
@foreach($posts as $post)
<li>
<a href="{{ route('posts.show', $post->id) }}">{{ $post->title }}</a>
<a href="{{ route('posts.edit', $post->id) }}">Edit</a>
<form action="{{ route('posts.destroy', $post->id) }}" method="POST">
@csrf
@method('DELETE')
<button type="submit">Delete</button>
</form>
</li>
@endforeach
</ul>
@endsection
```

**create.blade.php:**

```
blade
Sao chép mã
@extends('layout')

@section('content')

<h1>Create Post</h1>
<form action="{{ route('posts.store') }}" method="POST">
@csrf
<input type="text" name="title" placeholder="Title">
<textarea name="content" placeholder="Content"></textarea>
<button type="submit">Create</button>
</form>
@endsection
```

**edit.blade.php:**

```
blade
Sao chép mã
@extends('layout')

@section('content')

<h1>Edit Post</h1>
<form action="{{ route('posts.update', $post->id) }}" method="POST">
@csrf
@method('PUT')
<input type="text" name="title" value="{{ $post->title }}">
<textarea name="content">{{ $post->content }}</textarea>
<button type="submit">Update</button>
</form>
@endsection
```

**show.blade.php:**

```
blade
Sao chép mã
@extends('layout')

@section('content')

<h1>{{ $post->title }}</h1>
<p>{{ $post->content }}</p>
<a href="{{ route('posts.index') }}">Back to posts</a>
@endsection
```

**layout.blade.php:**

```
blade
Sao chép mã

<!DOCTYPE html>
<html>
<head>
    <title>Laravel CRUD</title>
</head>
<body>
    @yield('content')
</body>
</html>
```

## 6. Cập nhật Controller

Mở app/Http/Controllers/PostController.php và cập nhật như sau:

```
php
Sao chép mã
namespace App\Http\Controllers;

use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
public function index()
{
$posts = Post::all();
return view('posts.index', compact('posts'));
}

    public function create()
    {
        return view('posts.create');
    }

    public function store(Request $request)
    {
        Post::create($request->all());
        return redirect()->route('posts.index');
    }

    public function show(Post $post)
    {
        return view('posts.show', compact('post'));
    }

    public function edit(Post $post)
    {
        return view('posts.edit', compact('post'));
    }

    public function update(Request $request, Post $post)
    {
        $post->update($request->all());
        return redirect()->route('posts.index');
    }

    public function destroy(Post $post)
    {
        $post->delete();
        return redirect()->route('posts.index');
    }

}
```

## 7. Kiểm tra ứng dụng

Mở trình duyệt và truy cập http://localhost:8000/posts để kiểm tra ứng dụng CRUD của bạn. Bạn sẽ có thể tạo, đọc, cập nhật và xóa các bài viết.

Với hướng dẫn này, bạn đã tạo thành công một ứng dụng CRUD đơn giản trong Laravel. Laravel cung cấp nhiều tính năng mạnh mẽ giúp phát triển ứng dụng nhanh chóng và dễ dàng.
