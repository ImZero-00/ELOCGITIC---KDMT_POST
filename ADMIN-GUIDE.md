# 🔧 CÀI ĐẶT VÀ SỬ DỤNG CHỨC NĂNG ADMIN

## 🚀 HƯỚNG DẪN CÀI ĐẶT

### Bước 1: Chạy Migration mới
```bash
cd "d:\ELOGICTIC KDMT\khoik"
php artisan migrate
```

### Bước 2: Tạo tài khoản Admin
```bash
php artisan db:seed --class=AdminUserSeeder
```

Hoặc chạy lại toàn bộ database:
```bash
php artisan migrate:fresh --seed
```

---

## 🔐 THÔNG TIN ĐĂNG NHẬP

### Admin (Quản trị viên):
- **Email**: admin@gmail.com
- **Password**: 15042004
- **URL**: http://localhost:8000/admin/login

### Staff (Nhân viên):
- **Email**: khoi@gmail.com  
- **Password**: 15042004
- **URL**: http://localhost:8000/staff/login

### Business (Doanh nghiệp):
- **Email**: doanhnghiep@gmail.com
- **Password**: 15042004
- **URL**: http://localhost:8000/login

### Individual (Cá nhân):
- **Email**: canhan@gmail.com
- **Password**: 15042004
- **URL**: http://localhost:8000/login

---

## 📋 CHỨC NĂNG ADMIN

### 1. Dashboard Tổng Quan
- **URL**: `/admin/dashboard`
- Thống kê tổng số khách hàng, nhân viên, đơn hàng
- Doanh thu tổng
- Biểu đồ đơn hàng theo trạng thái
- Top 5 nhân viên xuất sắc
- Top 5 khách hàng VIP
- Danh sách 10 đơn hàng gần nhất

### 2. Quản lý Người dùng
- **URL**: `/admin/users`
- **Chức năng**:
  - ✅ Xem danh sách tất cả người dùng (khách hàng, nhân viên, admin)
  - ✅ Lọc theo vai trò (role), loại tài khoản (user_type)
  - ✅ Tìm kiếm theo tên, email, SĐT, tên công ty
  - ✅ Thêm người dùng mới (Create)
  - ✅ Xem chi tiết người dùng (Read)
  - ✅ Chỉnh sửa thông tin (Update)
  - ✅ Xóa người dùng (Delete)
  - ✅ Xem thống kê đơn hàng của từng user
  - ✅ Xem lịch sử đơn hàng

### 3. Quản lý Đơn hàng (Kế thừa Staff)
- **URL**: `/admin/orders`
- Admin có toàn bộ quyền của Staff:
  - Xem tất cả đơn hàng
  - Cập nhật trạng thái đơn hàng
  - Quản lý sự cố (incidents)

---

## 🎯 PHÂN QUYỀN HỆ THỐNG

### Khách hàng (Customer)
- Portal: `/` (Customer Portal)
- Chức năng:
  - Tạo đơn lẻ / đơn theo lô
  - Xem đơn hàng của mình
  - Tra cứu vận đơn
  - Liên kết shop (business)
  - Quản lý tài khoản

### Nhân viên (Staff)
- Portal: `/staff` (Staff Portal)
- Chức năng:
  - Dashboard nhân viên
  - Xem đơn được phân công
  - Cập nhật trạng thái đơn hàng
  - Báo cáo sự cố
  - Xem thông tin hướng dẫn

### Quản trị viên (Admin)
- Portal: `/admin` (Admin Panel)
- Chức năng:
  - **Tất cả chức năng của Staff** +
  - Dashboard tổng quan hệ thống
  - Quản lý người dùng (CRUD)
  - Xem thống kê toàn hệ thống
  - Phân quyền và quản lý tài khoản

---

## 🛡️ BẢO MẬT

### Middleware được áp dụng:
- `auth` - Yêu cầu đăng nhập
- `role:admin` - Chỉ admin mới truy cập được
- Tự động chuyển hướng nếu:
  - Admin/Staff đăng nhập nhầm portal khách hàng
  - Khách hàng cố truy cập portal admin/staff

### Bảo vệ các route:
```php
Route::middleware(['auth', 'role:admin'])->group(function () {
    // Admin routes
});
```

---

## 📊 THỐNG KÊ DASHBOARD

### Metrics hiển thị:
1. **Tổng khách hàng** (phân loại cá nhân/doanh nghiệp)
2. **Tổng nhân viên** (đang hoạt động)
3. **Tổng đơn hàng** (tất cả trạng thái)
4. **Doanh thu** (từ đơn đã giao)

### Phân tích đơn hàng:
- ⏳ Chờ xử lý (pending, confirmed)
- 🚚 Đang vận chuyển (picked_up, in_transit, out_delivery)
- ✅ Đã giao (delivered)
- ❌ Thất bại (cancelled, returned)

### Bảng xếp hạng:
- **Top Staff**: Xếp theo số đơn hoàn thành
- **Top Customers**: Xếp theo tổng chi tiêu

---

## 🔧 CẤU TRÚC MÃ NGUỒN

### Controllers:
```
app/Http/Controllers/Admin/
├── Auth/
│   └── LoginController.php       # Đăng nhập admin
├── DashboardController.php        # Dashboard tổng quan
└── UserController.php             # Quản lý người dùng (CRUD)
```

### Views:
```
resources/views/admin/
├── auth/
│   └── login.blade.php           # Form đăng nhập admin
├── users/
│   ├── index.blade.php           # Danh sách users
│   ├── create.blade.php          # Form tạo user
│   ├── edit.blade.php            # Form sửa user
│   └── show.blade.php            # Chi tiết user
├── dashboard.blade.php           # Dashboard
└── layout.blade.php              # Layout chung admin
```

### Routes:
```php
/admin/login                      # Đăng nhập admin
/admin/dashboard                  # Dashboard
/admin/users                      # Danh sách users
/admin/users/create               # Tạo user mới
/admin/users/{id}                 # Chi tiết user
/admin/users/{id}/edit            # Sửa user
/admin/orders                     # Quản lý đơn hàng
/admin/logout                     # Đăng xuất
```

---

## 💡 TIPS SỬ DỤNG

### Tạo Admin thêm bằng code:
```php
use App\Models\User;
use Illuminate\Support\Facades\Hash;

User::create([
    'name' => 'Admin Name',
    'email' => 'admin@example.com',
    'password' => Hash::make('password'),
    'role' => 'admin',
    'user_type' => 'individual',
    'phone' => '0900000000',
]);
```

### Kiểm tra quyền trong Blade:
```blade
@if(auth()->user()->isAdmin())
    <!-- Chỉ admin thấy -->
@endif

@if(auth()->user()->isStaffOrAdmin())
    <!-- Staff hoặc Admin thấy -->
@endif
```

### Kiểm tra quyền trong Controller:
```php
if (!auth()->user()->isAdmin()) {
    abort(403);
}
```

---

## 🎨 GIAO DIỆN

### Theme màu Admin:
- **Primary**: Purple (#9333EA) - Riêng cho admin
- **Gradient**: Purple 600 → Purple 800
- **Accent**: Giữ nguyên orange cho các element chung

### Design:
- Navbar màu tím gradient
- Cards với shadow và hover effects
- Badge màu sắc theo role/status
- Responsive, tương thích mobile

---

## ✅ HOÀN THÀNH

Hệ thống Admin Panel đã được tích hợp đầy đủ với:
- ✅ Dashboard tổng quan với thống kê real-time
- ✅ Quản lý người dùng (CRUD hoàn chỉnh)
- ✅ Kế thừa tất cả chức năng Staff
- ✅ Phân quyền chặt chẽ
- ✅ Giao diện chuyên nghiệp
- ✅ Bảo mật tốt

**Sẵn sàng sử dụng ngay! 🚀**
