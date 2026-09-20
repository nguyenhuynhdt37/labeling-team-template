# CVAT Reviewer & Backlog Management Guidelines

## 1. Quy cách ghi chép Reviewer Backlog (`reviewer-backlog.md`)
- Mọi Issue ghi vào `reviewer-backlog.md` BẮT BUỘC tuân thủ cú pháp chuẩn:
  `Issue #MÃ_ISSUE : MÃ_LOẠI_LỖI (Tọa độ x,y) - Mô tả chi tiết lỗi và Hướng xử lý`
  - Ví dụ: `Issue #5086 : INCORRECT_CLASS (Tọa độ x:284, y:241): Nhãn building (#464646) vẽ tòa nhà chưa hết.`

## 2. Phân biệt Semantic Class vs Issue Tag
- **Semantic Class (Nhãn gán mask):** `road`, `sidewalk`, `building`, `wall`, `fence`, `pole`, `vegetation`, `terrain`, `sky`, `person`, `rider`, `car`, `truck`, `bus`, `train`, `motorcycle`, `bicycle`.
- **Issue Tag (Thẻ báo lỗi):** `INCORRECT_CLASS`, `MISSING_OBJECT`, `UNCERTAIN_CLASS`, `UNCERTAIN_BOUNDARY`, `UNCERTAIN_SMALL_OBJECT`, `UNCERTAIN_SCOPE`, `OVERLAP_MASK`, `IMPROPER_BOUNDARY`.
- KHÔNG BAO GIỜ nhầm lẫn giữa Tên nhãn gán mask và Tên thẻ báo lỗi.

## 3. Quy trình Re-scan Live CVAT API
- Khi người dùng tạo hoặc cập nhật Issue mới trên CVAT, BẮT BUỘC gọi script kiểm tra CVAT REST API (`/api/issues?job_id=...`) để lấy đúng Mã Issue (Issue ID), Tọa độ, Thời gian và Loại Tag mới nhất.
- Luôn đối chiếu quy chuẩn nhãn với tài liệu taxonomy của dự án (`Semantic_Segmentation_Taxonomy_Translation.md`).
