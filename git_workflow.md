# **Git Workflow Notes**  
*(Dựa trên bài ["Beginner-Friendly Git Workflow" của Ajmal Hasan – DEV Community](https://dev.to/ajmal_hasan/beginner-friendly-git-workflow-for-developers-2g3g). Note lại chi tiết để dùng lâu dài, dễ áp dụng cho project cá nhân/freelance/team nhỏ.)*

## 1. Tại sao cần Git Workflow? (Mục đích chính – Lý do phải có)
- Tránh merge conflict khi nhiều người làm chung code.
- Giữ nhánh `main` (production) luôn sạch sẽ, ổn định, sẵn sàng deploy.
- Hợp tác mượt mà với team (hoặc solo vẫn cần để code traceable và dễ revert).
- Dễ review code, theo dõi lịch sử thay đổi, scale dự án lớn.
- **Note**: Không có workflow = Git thành đống lộn xộn. Mình hay dùng **Feature Branch Workflow** cho mọi project hiện tại vì đơn giản và hiệu quả.

## 2. Setup ban đầu (Bước nền tảng – Luôn làm trước khi code bất kỳ dự án nào)
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
- Tạo repo mới: `git init`
- Clone repo có sẵn: `git clone <url>`

**Note**: Config email đúng để GitHub nhận diện commit của mình, tránh commit unknown author.

## 3. Branching Strategy (Chiến lược nhánh – Cốt lõi của mọi workflow)
Đây là cách tổ chức code thành các nhánh song song để làm việc độc lập mà không ảnh hưởng lẫn nhau.

- **main**: Production-ready (không bao giờ commit thẳng lên đây).
- **develop**: Nhánh tích hợp (dùng trong Gitflow để merge feature trước khi release).
- **feature/***: Nhánh tạm cho từng tính năng/bug (ví dụ: `feature/login-ui`).

**Cách tạo branch an toàn (luôn làm theo thứ tự này)**:
```bash
git checkout main          # hoặc develop
git pull origin main       # lấy code mới nhất trước khi tạo nhánh
git checkout -b feature/ten-tinh-nang
```

## 4. Feature Branch Workflow (Workflow đơn giản nhất)
**Tên workflow**: Feature Branch Workflow (còn gọi là GitHub Flow cơ bản).  
**Ý nghĩa**: Mọi tính năng hoặc bug fix được phát triển trên nhánh riêng biệt, sau đó merge qua Pull Request để review. Giúp giữ main/develop luôn ổn định. Phù hợp solo hoặc team nhỏ.

**Quy trình đầy đủ từng bước**:
1. **Bắt đầu từ nhánh mới nhất** (để tránh conflict ngay từ đầu):  
   `git checkout develop && git pull origin develop`
2. **Tạo feature branch** (nơi mình làm việc độc lập):  
   `git checkout -b feature/ten-tinh-nang`
3. **Làm việc & commit** (commit nhỏ, thường xuyên, atomic – mỗi commit chỉ làm một việc rõ ràng):  
   Code → `git commit -m "feat: thêm login bằng Google"`
4. **Push branch lên remote**:  
   `git push origin feature/ten-tinh-nang`
5. **Tạo Pull Request** trên GitHub → review code.
6. **Approve & merge** vào develop.
7. **Dọn dẹp branch thừa** (giữ repo sạch sẽ):  
   ```bash
   git checkout develop
   git pull origin develop
   git branch -d feature/ten-tinh-nang
   git push origin --delete feature/ten-tinh-nang
   ```

**Note**: Luôn tạo PR dù solo, để mình review lại code sau 1-2 ngày.

## 5. Gitflow Workflow (Workflow có cấu trúc – Dùng khi dự án có release định kỳ)
**Tên workflow**: Gitflow Workflow.  
**Ý nghĩa**: Phân tách rõ ràng giữa phát triển tính năng, chuẩn bị release và fix khẩn cấp. Phù hợp team lớn, dự án có QA hoặc lịch release chính thức.

**Các branch chính**:
- main + develop
- feature/*
- release/* (chuẩn bị release)
- hotfix/* (fix khẩn production)

**Quy trình release**:
```bash
git checkout develop
git checkout -b release/v1.0.0          # Tạo release branch để stabilize code
# test & fix nhỏ
git checkout main
git merge release/v1.0.0                # Merge vào main (production)
git push origin main
git checkout develop
git merge release/v1.0.0                # Merge lại vào develop
git push origin develop
git branch -d release/v1.0.0            # Xóa release branch
```

**Hotfix nhanh (fix khẩn cấp trên production)**:
```bash
git checkout main
git checkout -b hotfix/urgent-fix
# fix → commit → merge vào main + develop
```

**Note**: Hiện tại mình chưa cần Gitflow, nhưng khi team > 5 người hoặc có QA thì chuyển sang.

## 6. Rebase Workflow (Giữ lịch sử commit sạch đẹp)
**Tên workflow**: Rebase Workflow.  
**Ý nghĩa**: Viết lại lịch sử commit thành đường thẳng (linear history), không có merge commit thừa → dễ review và trace thay đổi.

**Các bước thực hiện**:
```bash
git checkout feature/ten-tinh-nang
git fetch origin
git rebase origin/develop               # Viết lại commit của mình lên trên develop mới nhất
# resolve conflict nếu có → git rebase --continue
git push --force-with-lease             # Push lại (vì history đã thay đổi)
```

**Cảnh báo quan trọng**: Chỉ rebase trên **feature branch của mình**. Không bao giờ rebase main/develop!

## 7. Forking Workflow (Dành cho Open Source / Contributor ngoài)
**Tên workflow**: Forking Workflow.  
**Ý nghĩa**: An toàn cho người ngoài không có quyền push trực tiếp vào repo gốc (thường dùng trong open-source).

**Quy trình từng bước**:
1. Fork repo trên GitHub.
2. `git clone <fork-url>`
3. Tạo branch → code → push vào fork của mình.
4. Tạo PR về repo gốc.

## 8. Best Practices (Những thói quen tốt – Làm theo để tránh 90% lỗi)
- **Giữ branch luôn update**: `git fetch origin && git rebase origin/develop`
- **Commit message rõ ràng** (Conventional Commits):  
  ```bash
  git commit -m "feat: thêm login bằng Google"
  ```
- Rebase thay vì merge (trên feature branch của mình).
- Review PR kỹ trước khi merge.
- Xóa branch thừa sau merge.
- Dùng `.gitignore` và `.gitattributes`.

## Checklist nhanh khi bắt đầu task mới
- [ ] Checkout & pull latest develop/main
- [ ] Tạo feature branch
- [ ] Code + commit atomic
- [ ] Push + tạo PR
- [ ] Sau merge: delete branch
- [ ] Rebase thường xuyên

**Final Note**:
- Bắt đầu với **Feature Branch Workflow** cho mọi dự án hiện tại (freelance, side project).
- Khi team lớn hoặc cần release chính thức → chuyển Gitflow.
- Công cụ hỗ trợ: GitHub Desktop hoặc GitKraken nếu lười gõ lệnh.
- Mục tiêu: Git phải trở thành thói quen, không phải vấn đề.
