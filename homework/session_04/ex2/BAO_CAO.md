# BÁO CÁO BÀI THỰC HÀNH BÀI 2: QUẢN LÝ NHÁNH VÀ GIẢI QUYẾT XUNG ĐỘT (MERGE CONFLICT)

**Họ và tên học viên:** Nguyễn Phúc  
**Email:** phuchb24@gmail.com  
**Đường dẫn nộp bài trên GitHub:** `homework/session_04/ex2/`  

---

## 1. Mục tiêu bài thực hành
- Thực hành tạo và di chuyển giữa các nhánh cục bộ trong Git (`main` và `feature-update`).
- Cố ý tạo tình huống xung đột gộp nhánh (**Merge Conflict**) khi cả hai nhánh cùng thay đổi trên cùng một file `README.md`.
- Nhận biết và hiểu cơ chế gộp 3 vùng (**3-Way Merge**).
- Xử lý thủ công các ký hiệu xung đột (`<<<<<<<`, `=======`, `>>>>>>>`) và thực hiện Merge Commit hoàn chỉnh.

---

## 2. Các bước thực hiện chi tiết

### Bước 1: Khởi tạo repository và commit ban đầu trên nhánh `main`
- Khởi tạo Git repository và thiết lập nhánh chính `main`:
  ```bash
  git init
  git checkout -b main
  ```
- Tạo tệp `README.md` ban đầu với nội dung:
  ```markdown
  # Dự án Quản lý Học tập DevOps SS04

  Đây là tệp README ban đầu được tạo trên nhánh main.
  Phiên bản: 1.0.0
  Mô tả: Dự án phục vụ cho bài thực hành Git Branching & Merge Conflict Resolution.
  ```
- Commit ban đầu trên nhánh `main`:
  ```bash
  git add README.md
  git commit -m "Initial commit: Add README.md on main branch"
  ```

---

### Bước 2: Tạo nhánh `feature-update` và thực hiện thay đổi
- Tạo và chuyển sang nhánh mới `feature-update`:
  ```bash
  git checkout -b feature-update
  ```
- Thay đổi nội dung tệp `README.md`:
  ```markdown
  # Dự án Quản lý Học tập DevOps SS04

  Đây là phiên bản được cập nhật tính năng mới từ nhánh feature-update.
  Phiên bản: 1.1.0-feature
  Mô tả: Thêm tính năng quản lý danh sách học viên và theo dõi tiến độ bài tập.
  ```
- Commit thay đổi trên nhánh `feature-update`:
  ```bash
  git add README.md
  git commit -m "feat: Update README.md with feature specs on feature-update branch"
  ```

---

### Bước 3: Quay lại nhánh `main` và thực hiện thay đổi gây xung đột
- Quay trở lại nhánh `main`:
  ```bash
  git checkout main
  ```
- Sửa file `README.md` trên nhánh `main` tại cùng dòng code với nhánh `feature-update`:
  ```markdown
  # Dự án Quản lý Học tập DevOps SS04

  Đây là phiên bản cập nhật khẩn cấp (Hotfix) trực tiếp trên nhánh main.
  Phiên bản: 1.0.1-hotfix
  Mô tả: Sửa lỗi bảo mật hệ thống và cập nhật thông tin liên hệ hỗ trợ kỹ thuật.
  ```
- Commit thay đổi trên nhánh `main`:
  ```bash
  git add README.md
  git commit -m "fix: Hotfix README.md on main branch"
  ```

---

### Bước 4: Thực hiện Gộp Nhánh và Phát hiện Xung đột (Merge Conflict)
- Khi đang ở nhánh `main`, thực hiện lệnh gộp nhánh `feature-update` vào `main`:
  ```bash
  git merge feature-update
  ```
- **Thông báo lỗi từ Git:**
  ```text
  Auto-merging README.md
  CONFLICT (content): Merge conflict in README.md
  Automatic merge failed; fix conflicts and then commit the result.
  ```

---

### Bước 5: Phân tích Ký hiệu Xung đột và Cơ chế 3-Way Merge

#### Cơ chế 3-Way Merge:
Git sử dụng 3 mốc dữ liệu để xác định cách gộp:
1. **Base Commit (Tổ tiên chung)**: Commit `cd5f3b5` (trước khi phân nhánh).
2. **HEAD Commit (Nhánh hiện tại `main`)**: Commit `71b1fe9`.
3. **Target Commit (Nhánh gộp vào `feature-update`)**: Commit `6383138`.

Vì cả 2 nhánh đều chỉnh sửa cùng dòng so với Base Commit, Git không thể tự động quyết định lấy phiên bản nào nên đã phát sinh xung đột.

#### Phân tích các ký hiệu xung đột trong `README.md`:
```markdown
<<<<<<< HEAD
Đây là phiên bản cập nhật khẩn cấp (Hotfix) trực tiếp trên nhánh main.
Phiên bản: 1.0.1-hotfix
Mô tả: Sửa lỗi bảo mật hệ thống và cập nhật thông tin liên hệ hỗ trợ kỹ thuật.
=======
Đây là phiên bản được cập nhật tính năng mới từ nhánh feature-update.
Phiên bản: 1.1.0-feature
Mô tả: Thêm tính năng quản lý danh sách học viên và theo dõi tiến độ bài tập.
>>>>>>> feature-update
```

- `<<<<<<< HEAD`: Bắt đầu phần thay đổi trên nhánh hiện tại (`main`).
- `=======`: Ranh giới phân chia giữa 2 nhánh.
- `>>>>>>> feature-update`: Kết thúc phần thay đổi đến từ nhánh được gộp (`feature-update`).

---

### Bước 6: Giải quyết Xung đột Thủ công (Manual Conflict Resolution)
- Mở file `README.md`, tiến hành xóa hoàn toàn các ký hiệu đánh dấu (`<<<<<<<`, `=======`, `>>>>>>>`).
- Kết hợp thông tin từ cả hai nhánh để tạo ra phiên bản hoàn chỉnh nhất:
  ```markdown
  # Dự án Quản lý Học tập DevOps SS04

  Đây là phiên bản đã giải quyết xung đột gộp nhánh (Merge Conflict) thành công giữa `main` và `feature-update`.

  ## Thông tin tổng hợp phiên bản:
  - **Phiên bản hệ thống**: 1.1.0 (Tổng hợp từ Hotfix 1.0.1-hotfix trên `main` và Feature 1.1.0-feature trên `feature-update`)
  - **Nội dung bản hotfix (Main)**: Sửa lỗi bảo mật hệ thống và cập nhật thông tin liên hệ hỗ trợ kỹ thuật.
  - **Nội dung tính năng mới (Feature)**: Thêm tính năng quản lý danh sách học viên và theo dõi tiến độ bài tập.
  ```

- Đánh dấu file đã giải quyết xong và tạo **Merge Commit**:
  ```bash
  git add README.md
  git commit -m "merge: Resolve merge conflict between main and feature-update in README.md"
  ```

---

## 3. Kết quả Kiểm tra Lịch sử Commit (`git log --graph --oneline`)

Chạy lệnh kiểm tra lịch sử commit dưới dạng đồ thị:
```bash
git log --graph --oneline
```

### Kết quả hiển thị thực tế:
```text
*   b72e098 merge: Resolve merge conflict between main and feature-update in README.md
|\  
| * 6383138 feat: Update README.md with feature specs on feature-update branch
* | 71b1fe9 fix: Hotfix README.md on main branch
|/  
* cd5f3b5 Initial commit: Add README.md on main branch
```

### Nhận xét đồ thị:
- Commit `cd5f3b5` là mốc khởi tạo ban đầu (Base Commit).
- Đồ thị tách ra 2 nhánh:
  - Nhánh `main` đi tiếp với commit `71b1fe9` (Hotfix).
  - Nhánh `feature-update` tách ra với commit `6383138` (Feature update).
- Hai nhánh hội tụ lại tại merge commit `b72e098` có 2 commit cha (parents), hoàn thành đúng cơ chế 3-Way Merge và giải quyết xung đột thành công.
