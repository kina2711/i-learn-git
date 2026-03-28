# **Git Workflow Notes**  
*(Dựa trên bài ["Beginner-Friendly Git Workflow" của Ajmal Hasan – DEV Community](https://dev.to/ajmal_hasan/beginner-friendly-git-workflow-for-developers-2g3g). Note lại chi tiết để dùng lâu dài, dễ áp dụng cho project cá nhân/freelance/team nhỏ.)*

## 1. Tại sao cần Git Workflow? (Mục đích chính)
- Tránh merge conflict, giữ main/production luôn sạch & ổn định.
- Hợp tác mượt mà với team (hoặc solo vẫn cần để code traceable).
- Dễ revert, review code, scale dự án.
- **Note**: Không có workflow = Git thành “đống lộn xộn”. Mình hay dùng Feature Branch cho mọi project hiện tại.

## 2. Setup ban đầu (Làm trước khi code)
```bash
git config --global user.name "Kina"
git config --global user.email "kienthai2711@email.com"
```
- Tạo repo mới: `git init`  
- Clone repo có sẵn: `git clone <url>`

**Lưu ý cá nhân**: Config email đúng để GitHub nhận diện commit của mình.

## 3. Branching Strategy (Cốt lõi của mọi workflow)
- **main**: Production-ready (không bao giờ commit thẳng lên đây).
- **develop**: Nhánh tích hợp (dùng trong Gitflow).
- **feature/***: Nhánh tạm cho từng tính năng/bug (ví dụ: `feature/login-ui`).

**Cách tạo branch an toàn**:
```bash
git checkout main          # hoặc develop
git pull origin main
git checkout -b feature/ten-tinh-nang
```

## 4. Feature Branch Workflow (Workflow đơn giản nhất)
Dùng cho solo/team nhỏ.

**Quy trình đầy đủ**:
1. `git checkout develop && git pull origin develop`
2. `git checkout -b feature/ten-tinh-nang`
3. Code → commit nhỏ thường xuyên (atomic commits).
4. `git push origin feature/ten-tinh-nang`
5. Tạo **Pull Request** trên GitHub → review code.
6. Approve → merge vào develop.
7. Dọn dẹp:
   ```bash
   git checkout develop
   git pull origin develop
   git branch -d feature/ten-tinh-nang
   git push origin --delete feature/ten-tinh-nang
   ```

**Note**: Luôn tạo PR, dù solo, để mình review lại code sau 1-2 ngày.

## 5. Gitflow Workflow (Dùng khi dự án có release định kỳ)
Phức tạp hơn, phù hợp team lớn hoặc app có QA.

**Các branch chính**:
- main + develop
- feature/*
- release/* (chuẩn bị release)
- hotfix/* (fix khẩn production)

**Quy trình release**:
```bash
git checkout develop
git checkout -b release/v1.0.0
# test & fix nhỏ
git checkout main
git merge release/v1.0.0
git push origin main
git checkout develop
git merge release/v1.0.0
git push origin develop
git branch -d release/v1.0.0
```

**Hotfix nhanh**:
```bash
git checkout main
git checkout -b hotfix/urgent-fix
# fix → commit → merge vào main + develop
```

**Note**: Hiện tại mình chưa cần Gitflow, nhưng khi team > 5 người thì chuyển sang.

## 6. Rebase Workflow (Giữ lịch sử commit sạch đẹp)
```bash
git checkout feature/ten-tinh-nang
git fetch origin
git rebase origin/develop
# resolve conflict nếu có → git rebase --continue
git push --force-with-lease
```
**Cảnh báo quan trọng**: Chỉ rebase trên feature branch **của mình**, không bao giờ rebase main/develop!

## 7. Forking Workflow (Dành cho Open Source / Contributor ngoài)
1. Fork repo trên GitHub.
2. `git clone <fork-url>`
3. Tạo branch → code → push vào fork của mình.
4. Tạo PR về repo gốc.

## 8. Best Practices (Làm theo để tránh 90% lỗi)
- **Giữ branch luôn update**: `git fetch origin && git rebase origin/develop`
- **Commit message rõ ràng** (Conventional Commits):
  ```bash
  git commit -m "feat: thêm login bằng Google"
  ```
- Rebase thay vì merge (trên feature branch).
- Review PR kỹ.
- Xóa branch thừa sau merge.
- Dùng `.gitignore` và `.gitattributes`.

## Checklist nhanh khi bắt đầu task mới (Copy-paste dùng hàng ngày)
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
