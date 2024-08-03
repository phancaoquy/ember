---
title: "Cài Đặt Laravel: Hướng Dẫn Từng Bước Để Bắt Đầu"
meta_title: ""
description: "Cài Đặt Laravel: Hướng Dẫn Từng Bước Để Bắt Đầu"
date: 2024-07-29T05:00:00Z
image: "/images/image-placeholder.png"
categories: ["Information"]
author: "Lê Huỳnh Trung"
tags: ["Responsive", "Design"]
draft: false
---

## 1. Yêu cầu hệ thống: Trước tiên, hãy đảm bảo rằng máy tính của bạn đáp ứng các yêu cầu hệ thống sau:

```
PHP >= 8.1
Composer (trình quản lý phụ thuộc cho PHP)
Một máy chủ web như Apache hoặc Nginx
```

## 2. Cài đặt Composer: Composer là công cụ quản lý phụ thuộc cần thiết để cài đặt Laravel. Bạn có thể tải Composer từ trang chủ của họ.

o Tải Composer : https://getcomposer.org/download/

## 3. Cài đặt Laravel: Sau khi cài đặt Composer, bạn có thể cài đặt Laravel thông qua Composer. Mở terminal hoặc command prompt và chạy lệnh sau:

```
bash
Sao chép mã
composer global require laravel/installer
Sau khi hoàn tất, bạn có thể kiểm tra xem Laravel đã được cài đặt hay chưa bằng lệnh:
bash
Sao chép mã
laravel --version
```

## 4. Tạo một dự án Laravel mới: Để tạo một dự án Laravel mới, bạn chỉ cần chạy lệnh sau:

```
bash
Sao chép mã
laravel new project-name
Thay project-name bằng tên dự án của bạn. Lệnh này sẽ tạo một thư mục mới với các tệp tin và thư mục cần thiết cho một ứng dụng Laravel.
```

## 5. Cấu hình môi trường: Mở file .env trong thư mục dự án và cấu hình các thông số cơ bản như kết nối cơ sở dữ liệu, thông tin ứng dụng, v.v.

```
dotenv
Sao chép mã
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:...
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

## 6. Chạy server phát triển: Laravel đi kèm với một server phát triển built-in, bạn có thể chạy lệnh sau để khởi động server:

```
bash
Sao chép mã
php artisan serve
Server sẽ chạy tại http://localhost:8000. Mở trình duyệt và truy cập vào URL này để xem trang chủ Laravel mặc định.
```

## 7. Tài liệu và học tập: Để nắm bắt thêm về Laravel, bạn có thể tham khảo các tài liệu và khóa học sau:

Tài liệu chính thức của Laravel: https://laravel.com/docs/11.x

Laracasts : https://laracasts.com/

## 8. Cộng đồng hỗ trợ: Tham gia vào các cộng đồng để nhận hỗ trợ và trao đổi kinh nghiệm:

Laravel Community: https://laravel.io/

Laravel trên Stack Overflow: https://stackoverflow.com/questions/tagged/laravel

## Kết luận

Bằng cách tuân theo các bước trên, bạn sẽ có một dự án Laravel mới sẵn sàng để phát triển. Laravel không chỉ cung cấp một nền tảng mạnh mẽ mà còn có một cộng đồng lớn giúp bạn dễ dàng học hỏi và giải quyết các vấn đề phát sinh. Chúc bạn thành công trong việc phát triển ứng dụng với Laravel!
