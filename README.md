# I learn Git

### PHẦN 1: KHỞI TẠO DỰ ÁN (Chỉ làm 1 lần đầu)

**Bước 1: Tạo thư mục & Git**

```powershell
# 1. Tạo thư mục dự án (Thay tên dự án của bạn vào)
mkdir Ten-Du-An-Moi

# 2. Đi vào thư mục
cd Ten-Du-An-Moi

# 3. Kích hoạt Git
git init

# 4. Đổi nhánh chính sang 'main' (Chuẩn quốc tế)
git branch -M main

```

**Bước 2: Tạo bộ 3 file "quyền lực"**

1. **File `.gitignore**` (Chặn rác hệ thống & bảo mật):
```powershell
# Tạo file và thêm các dòng loại trừ phổ biến
echo ".DS_Store" > .gitignore
echo ".env" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "node_modules/" >> .gitignore
echo "*.csv" >> .gitignore

```


2. **File `README.md**` (Bìa dự án):
```powershell
echo "# Ten Du An Cua Toi" > README.md

```


3. **File `LICENSE**` (Bản quyền MIT):
```powershell
# Mở Notepad (hoặc VS Code) để tạo file
code LICENSE

```


*Dán nội dung sau vào cửa sổ vừa hiện ra, rồi Lưu (Ctrl+S) & Đóng lại:*
```text
MIT License
Copyright (c) 2026 Kien Thai

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

```



**Bước 3: Kết nối GitHub & Đẩy lên**

* *Lưu ý: Lên web GitHub tạo Repo rỗng trước, rồi copy link HTTPS.*

```powershell
# 1. Kết nối
git remote add origin https://github.com/User/Ten-Repo.git

# 2. Gom hàng (Lần đầu)
git add .

# 3. Đóng gói
git commit -m "Khoi tao du an: Readme, License, Gitignore"

# 4. Đẩy lên
git push -u origin main

```

---

### PHẦN 2: QUY TRÌNH HẰNG NGÀY (Lặp đi lặp lại)

Mỗi khi code xong một tính năng, sửa lỗi, hoặc hết ngày làm việc.

**1. GOM (Add)**

```powershell
git add .

```

**2. GÓI (Commit)**

```powershell
git commit -m "Ghi chú ngắn gọn về việc vừa làm"

```

**3. GỬI (Push)**

```powershell
git push

```

---

### PHẦN 3: Cheatsheet

| Tình huống | Lệnh giải quyết |
| --- | --- |
| **Quên mình đang làm gì / Check file** | `git status` |
| **Lỡ tay Add nhầm file rác** | `git restore --staged .` |
| **Code trên GitHub mới hơn trong máy** | `git pull` |
| **Xem lịch sử chỉnh sửa** | `git log` |
| **Link nhầm Repo GitHub** | `git remote remove origin` (rồi add lại) |
