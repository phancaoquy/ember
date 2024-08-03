---
title: "Khi bắt đầu với Laravel"
meta_title: ""
description: "Khi bắt đầu với Laravel"
date: 2024-07-30T05:00:00Z
image: "/images/image-placeholder.png"
categories: ["Information"]
author: "Lê Huỳnh Trung"
tags: ["Laravel"]
draft: false
---

Khi bắt đầu với Laravel, việc làm quen với cấu trúc thư mục của nó là điều quan trọng giúp bạn hiểu rõ hơn về cách tổ chức mã nguồn và dễ dàng quản lý dự ## án. Dưới đây là một hướng dẫn chi tiết về cấu trúc thư mục của Laravel:

## 1. Thư mục gốc của dự án Laravel:

Sau khi tạo một dự án Laravel mới, bạn sẽ thấy các thư mục và tệp tin sau trong thư mục gốc của dự án.##

## 2. Thư mục app/:

Chứa mã nguồn chính của ứng dụng, bao gồm các mô hình (Models), bộ điều khiển (Controllers), và các dịch vụ khác.

- Console: Chứa các lệnh Artisan tuỳ chỉnh.
- Exceptions: Chứa các lớp để xử lý lỗi và ngoại lệ.
- Http: Chứa các lớp liên quan đến HTTP, bao gồm Controllers, Middleware, và Requests.
  - Controllers: Chứa các bộ điều khiển của ứng dụng.
  - Middleware: Chứa các middleware của ứng dụng.
  - Requests: Chứa các lớp request tùy chỉnh.
- Models: Chứa các mô hình Eloquent của ứng dụng.

## 3. Thư mục bootstrap/:

Chứa tệp tin app.php giúp khởi động framework và thư mục cache để chứa các file bộ nhớ đệm.##

## 4. Thư mục config/:

Chứa tất cả các tệp tin cấu hình của ứng dụ## ng. Bạn có thể cấu hình mọi thứ từ cơ sở dữ liệu, mail, đến các dịch vụ bên thứ ba tại đây.##

## 5. Thư mục database/:

Chứa các tệp tin liên quan đến cơ sở dữ liệu.

- factories: Chứa các factory để tạo các đối tượng mô hình cho việc kiểm thử.
- migrations: Chứa các tệp tin migration để tạo và quản lý cấu trúc cơ sở dữ liệu.
- seeders: Chứa các seeder để điền dữ liệu mẫu vào cơ sở dữ liệu.

## 6. Thư mục public/:

Thư mục này chứa tệp tin index.php, là điểm bắt đầu của ứng dụng khi nó được truy cập từ w## eb. Thư mục này cũng chứa các tài nguyên công khai như hình ảnh, JavaScript, và CSS.##

## 7. Thư mục resources/:

Chứa các tệp tin view, các tài nguyên thô như CSS và JavaScript, và các tệp dịch ngôn ngữ.

- views: Chứa các tệp tin Blade template.
- lang: Chứa các tệp tin dịch ngôn ngữ.
- js, css: Chứa các tệp tin JavaScript và CSS thô.

## 8. Thư mục routes/:

Chứa các tệp tin định nghĩa route cho ứng dụng.

- web.php: Chứa các route cho ứng dụng web.
- api.php: Chứa các route cho API.
- console.php: Định nghĩa các route console dựa trên Artisan command.
- channels.php: Định nghĩa các kênh event broadcasting.

## 9. Thư mục storage/:

Chứa các tệp tin bộ nhớ đệm, file session, file log, và các file đã biên dịch.

- app: Chứa các file ứng dụng.
- framework: Chứa bộ nhớ đệm framework và các file session.
- logs: Chứa các tệp log của ứng dụng.

## 10. Thư mục tests/:

    Chứa các tệp tin kiểm thử (test) của ứng dụng.

- Feature: Chứa các bài test kiểm thử tính năng.
- Unit: Chứa các bài test kiểm thử đơn vị.

## 11. Thư mục vendor/:

    Chứa các gói (packages) bên thứ ba được cài đặt thông qua Composer.

## 12. Các tệp tin gốc:

- .env: Tệp tin cấu hình môi trường của ứng dụng.
- artisan: Tệp tin console command-line của Laravel.
- composer.json: Tệp tin cấu hình Composer.
- package.json: Tệp tin cấu hình npm/yarn.
- webpack.mix.js: Tệp tin cấu hình Laravel Mix.

Để hiểu rõ hơn về cấu trúc thư mục của Laravel, bạn có thể tham khảo tài liệu chính thức:

- Laravel Directory Structure: https://laravel.com/docs/10.x/structure

Việc làm quen với cấu trúc thư mục của Laravel giúp bạn dễ dàng hơn trong việc tổ chức mã nguồn và phát triển ứng dụ## ng. Với các thư mục và tệp tin được sắp xếp một cách khoa học, Laravel giúp bạn quản lý dự án một cách hiệu quả và dễ dàng bảo trì trong tương lai.
