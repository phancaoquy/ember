---
title: "Authencation với package Laravel Sanctum API"
meta_title: ""
description: "Authencation với package Laravel Sanctum API"
date: 2024-07-29T05:00:00Z
image: "/images/truongsancrum.jpg"
categories: ["Laravel"]
author: "Nguyễn Thanh Trường"
tags: ["Authencation"]
draft: false
---

Sanctum giải quyết 2 vấn đề:

### 1. API Tokens

- Sanctum sẽ sinh ra API tokens cho người dùng mà không cần đến sự phức tạp của Oauth. Nó giống như tính năng "access tokens" của GitHub. Hãy tưởng tượng ứng dụng của bạn trong phần cài đặt tài khoản có màn hình nơi user có thể tạo API token cho tài khoản của họ. Bạn có thể sử dụng Sanctum để tạo và quản lý các tokens này. Những tokens này thường có thời gian hết hạn rất dài (tính bằng năm) nhưng user có thể hủy bỏ chúng bất cứ lúc nào.
- Laravel Sanctum cung cấp tính năng này bằng cách lưu trữ API tokens của user trong một bảng cơ sở dữ liệu duy nhất và xác thực các requests thông qua API token hợp lệ được gắn trên Authorization header.

## SPA Authentication

Sanctum cung cấp một cách đơn giản để xử lý việc xác thực trong các SPA cần giao tiếp với API được hỗ trợ bởi Laravel. Các SPA này có thể tồn tại trong cùng một kho lưu trữ như ứng dụng Laravel của bạn hoặc có thể là một kho lưu trữ hoàn toàn riêng biệt, chẳng hạn như SPA được tạo bằng Vue CLI.

- Đối với tính năng này, Sanctum không sử dụng bất kỳ loại token nào. Thay vào đó, Sanctum sử dụng các dịch vụ xác thực phiên dựa trên cookie được tích hợp sẵn của Laravel. Điều này cung cấp các lợi ích của CSRF protection, session authentication, cũng như bảo vệ chống rò rỉ thông tin xác thực qua XSS. Sanctum sẽ chỉ cố gắng xác thực bằng cookie khi request bắt nguồn từ giao diện người dùng SPA của bạn.
- Phía trên là phần giới thiệu qua về package này. Còn hôm nay, chúng ta sẽ cùng xây dựng một ứng dụng demo đơn giản sử dụng Sanctum nhé.

## Tạo project và cài đặt package Sanctum

Yêu cầu:

- Laravel 7.x trở lên
- Composer
- Postman
  Tạo 1 project laravel bằng câu lệnh

```
composer create-project --prefer-dist laravel/laravel:^7.0 sanctum
```

Cài đặt Laravel Sanctum:

```
composer require laravel/sanctum
```

Tiếp theo, publish file config và migration của Sanctum:

```
php artisan vendor:publish --provider=”Laravel\Sanctum\SanctumServiceProvider”
```

Đừng quên migrate database và seed data để test nha

```
php artisan migrate --seed
```

Sử dụng middleware EnsureFrontendRequestsAreStateful của Sanctum bằng cách thêm vào trong file app/Http/Kernel.php

```
use Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful;
    ...
    protected $middlewareGroups = [
        ...

        'api' => [
            EnsureFrontendRequestsAreStateful::class,
            'throttle:api',
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],
    ];

```

Hãy thêm trait HasApiTokens cho model mà bạn muốn sử dụng. Trong project này mình sử dụng model User

```
namespace App\Models;
use Illuminate\Database\Eloquent\Factories\HasFactory;use Illuminate\Foundation\Auth\User as Authenticatable;use Illuminate\Notifications\Notifiable;use Laravel\Sanctum\HasApiTokens;
class User extends Authenticatable{
    use HasFactory, Notifiable, HasApiTokens;
    ...}
```

## Tạo AuthController

Sanctum tạo API tokens bằng method createToken. Chúng sẽ được hash bằng hàm SHA-256 trước khi lưu vào database và chúng ta có thể lấy ra sử dụng thông qua property plainTextToken

```
$token = $user->createToken('token-name');
return $token->plainTextToken;
```

Việc tạo API tokens tất nhiên là dùng để đăng nhập rồi

```
<?php
namespace App\Http\Controllers;
use App\Models\User;use Illuminate\Http\Request;use Illuminate\Support\Facades\Auth;use Illuminate\Support\Facades\Hash;
class AuthController extends Controller{
    public function login(Request $request)
    {
        try {
            $request->validate([
                'email' => 'email|required',
                'password' => 'required'
            ]);

            $credentials = request(['email', 'password']);

            if (!Auth::attempt($credentials)) {
                return response()->json([
                    'status_code' => 500,
                    'message' => 'Unauthorized'
                ]);
            }

            $user = User::where('email', $request->email)->first();

            if (!Hash::check($request->password, $user->password, [])) {
                throw new \Exception('Error in Login');
            }

            $tokenResult = $user->createToken('authToken')->plainTextToken;

            return response()->json([
                'status_code' => 200,
                'access_token' => $tokenResult,
                'token_type' => 'Bearer',
            ]);
        } catch (\Exception $error) {
            return response()->json([
                'status_code' => 500,
                'message' => 'Error in Login',
                'error' => $error,
            ]);
        }
    }}
```

Chúng ta có thể thu hồi những token này bằng cách xóa trong database nhưng tất nhiên là sử dụng method có sẵn:

```
// Revoke all tokens...$user->tokens()->delete();
// Revoke the user's current token...$request->user()->currentAccessToken()->delete();
// Revoke a specific token...$user->tokens()->where('id', $id)->delete();
```

## Tạo route API

Trong laravel, route API sẽ được khai báo trong routes/api.php

```
Route::post('/login', 'AuthController@login');
Route::middleware(['auth:sanctum'])->group(function () {
Route::get('/users', 'UserController@index');});
```

Với tất cả các route có sử dụng middleware Sanctum thì khi gửi request sẽ phải bao gồm cả header Authorization dạng

> Authorization = Bearer [API token]

Ví dụ cách cấu hình trong Postman
![A mushroom-head robot](/images/truongb1.png "Website đầu tiên")

## Chạy demo

Chức năng login:
![A mushroom-head robot](/images/truongb2.png "Website đầu tiên")
Lúc này chúng ta đã có token để sử dụng cho các chức năng khác.

```
<?php
namespace App\Http\Controllers;
use App\Models\User;use Illuminate\Http\Request;
class UserController extends Controller{
    public function index()
    {
        return response()->json([
            'data' => User::all(),
        ]);
    }}
```

Test route GET /users
![A mushroom-head robot](/images/truongb3.png "Website đầu tiên")
