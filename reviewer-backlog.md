# Reviewer Backlog - Semantic Segmentation &amp; 2D Annotation

Nhật ký kiểm định chất lượng (QA/QC Audit) và danh sách dồn tồn (Backlog Issues) trên hệ thống CVAT trước khi bàn giao cho Team Leader.

---

## 📋 Quy chuẩn Mã Issue Tags chính thức (QA/QC Issue Taxonomy)

Để đảm bảo tính nhất quán tuyệt đối trong quá trình kiểm định chất lượng (QA/QC Audit) và ghi log backlog, toàn bộ Reviewer và Annotator **BẮT BUỘC** phải tuân thủ danh mục Mã Issue Tags chính thức dưới đây. Tuyệt đối không tự ý đặt tag tự do gây sai lệch báo cáo.

### Bảng danh mục Issue Tags Tiêu chuẩn &amp; Bổ sung


| STT | Mã Issue Tag                 | Nhóm lỗi / Mục đích | Mô tả chi tiết &amp; Trường hợp áp dụng thực tế                                                                                                               | Hướng xử lý yêu cầu Annotator                                  |
| :---: | :---------------------------- | :------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------- |
| 1   | `**MISSING_OBJECT**`         | Bỏ sót đối tượng    | Phát hiện vùng/vật thể rõ ràng trong ảnh thuộc 19 nhãn taxonomy nhưng Annotator chưa vẽ mask/polygon.                                                         | Vẽ bổ sung polygon/mask đúng class.                            |
| 2   | `**INCORRECT_CLASS**`        | Gán sai Class       | Vật thể có mask nhưng bị chọn sai nhãn (ví dụ: nhầm `person` thành `rider`, `building` thành `fence`, hoặc `car` thành `person`).                             | Đổi nhãn polygon về đúng class taxonomy.                       |
| 3   | `**EXTRA_OBJECT**`           | Đánh thừa đối tượng | Vẽ dư thừa mask cho đối tượng không tồn tại (False Positive) hoặc vật thể không thuộc bộ 19 nhãn quy định (ví dụ: cầu `bridge`, con vật, đồ vật ngoài scope). | Xóa hoàn toàn polygon/mask dư thừa.                            |
| 4   | `**IMPROPER_BOUNDARY**`      | Ranh giới lem nhem  | Polygon vẽ tràn ra bầu trời/mặt đường hoặc cắt xẻ lấn quá nhiều vào vật thể, chưa phủ trọn vẹn bề mặt (vi phạm RULE 02).                                      | Tỉa lại ranh giới mask bám sát biên thị giác của vật thể.      |
| 5   | `**OVERLAP_MASK**`           | Chồng lấn Mask      | Hai hoặc nhiều mask/class khác nhau bị vẽ đè lấn lên nhau trên cùng một vùng pixel (vi phạm RULE 01).                                                         | Cắt tỉa hoặc xóa phần mask bị đè lấn.                          |
| 6   | `**DUPLICATE_MASK**`         | Vẽ đúp / Trùng lặp  | Annotator vẽ 2 hoặc nhiều mask/polygon trùng đúp lên cùng 1 vật thể (nhân bản polygon dư thừa cho 1 đối tượng).                                               | Xóa bớt các polygon bị trùng đúp, chỉ giữ lại 1 mask duy nhất. |
| 7   | `**UNCERTAIN_CLASS**`        | Phân vân Class      | Nhãn quá mờ/xa không xác định chắc thuộc class nào (ví dụ: mờ không phân biệt `car` vs `truck`).                                                              | Đưa Lead/Mentor hỗ trợ chốt hoặc escalate.                     |
| 8   | `**UNCERTAIN_BOUNDARY**`     | Phân vân Ranh giới  | Biết class nhưng ranh giới bị bóng râm che đen/mờ tối không xác định được pixel chính xác.                                                                    | Thống nhất quy tắc bóc tách bóng râm với Lead/Mentor.          |
| 9   | `**UNCERTAIN_SMALL_OBJECT**` | Vật thể quá nhỏ     | Vật thể quá xa/mỏng (như chi tiết cột nhỏ chân trời) không đủ bằng chứng hình ảnh để vẽ mask.                                                                 | Đưa Lead/Mentor chốt ngưỡng gán nhãn tối thiểu.                |


---

## 📊 Bảng tổng hợp Group Job


| Group Job             | Task ID  | Job ID   | Annotator      | Ảnh kiểm mẫu | Số ảnh lỗi | Tỷ lệ lỗi (%) | Trạng thái Review | Kết quả bàn giao |
| :---------------------: | :--------: | :--------: | :--------------: | :------------: | :----------: | :-------------: | :-----------------: | :----------------: |
| [JOB-1615](#job-1615) | Task 191 | Job 1615 | `@2A202602206` | 20           | 15         | 75%           | 🟡 Đang Review    | Chờ sửa Issue    |
| [JOB-1614](#job-1614) | Task 191 | Job 1614 | `@2A202602206` | 25           | 5          | 20%           | 🟡 Đang Review    | Chờ sửa Issue    |
| [JOB-1616](#job-1616) | Task 191 | Job 1616 | `@2A202602206` | 15           | 14         | 93%           | 🟡 Đang Review    | Chờ sửa Issue    |


---

## 📂 Đánh giá chi tiết theo từng Group Job

### JOB-1615

- **Task ID**: 191 | **Job ID**: 1615 | **Annotator**: `@2A202602206` | **Reviewer**: `@huynh`
- **Kiểm mẫu**: 20/100 ảnh | **Số ảnh lỗi**: 15 ảnh (Frame 27, Frame 25, Frame 28, Frame 29, Frame 30, Frame 31, Frame 32, Frame 33, Frame 34, Frame 35, Frame 36, Frame 39, Frame 40, Frame 41, Frame 42)

#### Minh chứng lỗi phát hiện / Issue tồn đọng:

1. [**Frame 27**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=27):
  - `**Issue #5086 : IMPROPER_BOUNDARY**` *(Tọa độ x:284, y:241)*: Nhãn `building` (#464646) vẽ tòa nhà chưa hết (chưa bám trọn vẹn bề mặt công trình/tòa nhà).
  - `**Issue #5085 : INCORRECT_CLASS**` *(Tọa độ x:824, y:243)*: Gán nhầm `pole` (#999999 - cột điện/cột đèn) thành `building` (#464646 - tòa nhà).
  - `**Issue #5084 : INCORRECT_CLASS**` *(Tọa độ x:1103, y:281)*: Gán nhầm `pole` (#999999 - cột điện/cột biển báo) thành `building` (#464646 - tòa nhà).
  - `**Issue #5083 : INCORRECT_CLASS**` *(Tọa độ x:1202, y:252)*: Gán nhầm `pole` (#999999 - cột) thành `building` (#464646 - tòa nhà).
2. [**Frame 25**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=25):
  - `**Issue #5091 : INCORRECT_CLASS**` *(Tọa độ x:1070, y:285)*: Gán sai class hàng rào sắt (`fence` #BE9999) thành tòa nhà (`building` #464646).
3. [**Frame 28**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=28):
  - `**Issue #5100 : UNCERTAIN_BOUNDARY**` *(Tọa độ x:426, y:308)*: Gốc cây (`vegetation` #6B8E23) bị che bóng đen/bóng râm không thấy rõ ranh giới pixel.
  - `**Issue #5098 : MISSING_OBJECT**` *(Tọa độ x:22, y:343)*: Bỏ sót hàng rào sắt (`fence` #BE9999).
  - `**Issue #5097 : MISSING_OBJECT**` *(Tọa độ x:80, y:398)*: Bỏ sót vỉa hè (`sidewalk` #F423E8).
  - `**Issue #5096 : MISSING_OBJECT**` *(Tọa độ x:259, y:368)*: Bỏ sót vỉa hè (`sidewalk` #F423E8).
  - `**Issue #5095 : MISSING_OBJECT**` *(Tọa độ x:819, y:378)*: Bỏ sót vỉa hè (`sidewalk` #F423E8).
4. [**Frame 29**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=29):
  - `**Issue #5103 : IMPROPER_BOUNDARY**` *(Tọa độ x:462, y:331)*: Vẽ thiếu ô tô (`car` #00008E - chưa bám trọn vẹn phần thân xe).
  - `**Issue #5105 : MISSING_OBJECT**` *(Tọa độ x:279, y:365)*: Chưa vẽ vỉa hè (`sidewalk` #F423E8).
5. [**Frame 30**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=30):
  - `**Issue #5108 : MISSING_OBJECT**` *(Tọa độ x:62, y:357)*: Chưa vẽ vỉa hè (`sidewalk` #F423E8).
6. [**Frame 31**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=31):
  - `**Issue #5112 : INCORRECT_CLASS**` *(Tọa độ x:615, y:86)*: Gán nhầm cột (`pole` #999999) thành tòa nhà (`building` #464646).
  - `**Issue #5114 : INCORRECT_CLASS**` *(Tọa độ x:587, y:79)*: Gán nhầm biển báo giao thông (`traffic_sign` #DCDC00) thành tòa nhà (`building` #464646).
  - `**Issue #5111 : INCORRECT_CLASS**` *(Tọa độ x:613, y:59)*: Gán nhầm biển báo giao thông (`traffic_sign` #DCDC00) thành tòa nhà (`building` #464646).
7. [**Frame 32**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=32):
  - `**Issue #5116 : MISSING_OBJECT**` *(Tọa độ x:407, y:384)*: Bỏ sót người đi bộ (`person` #DC143C).
8. [**Frame 33**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=33):
  - `**Issue #5117 : MISSING_OBJECT**` *(Tọa độ x:1100, y:393)*: Bỏ sót vỉa hè (`sidewalk` #F423E8).
  - `**Issue #5125 : MISSING_OBJECT**` *(Tọa độ x:191, y:330)*: Bỏ sót hàng rào kín (`fence` #BE9999).
  - `**Issue #5126 : MISSING_OBJECT**` *(Tọa độ x:260, y:287)*: Bỏ sót cột (`pole` #999999).
  - `**Issue #5124 : MISSING_OBJECT**` *(Tọa độ x:96, y:322)*: Bỏ sót cột (`pole` #999999).
  - `**Issue #5123 : MISSING_OBJECT**` *(Tọa độ x:668, y:317)*: Bỏ sót cột (`pole` #999999).
  - `**Issue #5122 : MISSING_OBJECT**` *(Tọa độ x:760, y:310)*: Bỏ sót cột (`pole` #999999).
  - `**Issue #5121 : MISSING_OBJECT**` *(Tọa độ x:1187, y:299)*: Bỏ sót cột (`pole` #999999).
  - `**Issue #5120 : MISSING_OBJECT**` *(Tọa độ x:1252, y:271)*: Bỏ sót cột (`pole` #999999).
  - `**Issue #5119 : MISSING_OBJECT**` *(Tọa độ x:1037, y:312)*: Bỏ sót cột (`pole` #999999).
  - `**Issue #5118 : MISSING_OBJECT**` *(Tọa độ x:139, y:369)*: Bỏ sót cột (`pole` #999999).
9. [**Frame 34**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=34):
  - `**Issue #5131 : MISSING_OBJECT**` *(Tọa độ x:461, y:188)*: Bỏ sót biển báo giao thông (`traffic_sign` #DCDC00).
  - `**Issue #5127 : MISSING_OBJECT**` *(Tọa độ x:466, y:319)*: Bỏ sót vỉa hè (`sidewalk` #F423E8).
  - `**Issue #5130 : MISSING_OBJECT**` *(Tọa độ x:428, y:275)*: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5129 : MISSING_OBJECT**` *(Tọa độ x:471, y:267)*: Bỏ sót cột (`pole` #999999).
  - `**Issue #5128 : MISSING_OBJECT**` *(Tọa độ x:758, y:171)*: Bỏ sót cột (`pole` #999999).
10. [**Frame 35**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=35):
  - `**Issue #5132 : MISSING_OBJECT**` *(Tọa độ x:692, y:409)*: Bỏ sót vỉa hè (`sidewalk` #F423E8).
11. [**Frame 36**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=36):
  - `**Issue #5135 : MISSING_OBJECT**` *(Tọa độ x:1022, y:168)*: Bỏ sót cây / thảm thực vật (`vegetation` #6B8E23).
    - `**Issue #5134 : MISSING_OBJECT**` *(Tọa độ x:836, y:384)*: Bỏ sót bề mặt tự nhiên / đất đá (`terrain` #98FB98).
    - `**Issue #5133 : MISSING_OBJECT**` *(Tọa độ x:319, y:430)*: Bỏ sót bề mặt tự nhiên / đất đá (`terrain` #98FB98).
12. [**Frame 39**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=39):
  - `**Issue #5137 : MISSING_OBJECT**` *(Tọa độ x:76, y:310)*: Bỏ sót tường đứng (`wall` #66669C).
    - `**Issue #5136 : MISSING_OBJECT**` *(Tọa độ x:984, y:405)*: Bỏ sót mask mặt đường (`road` #804080).
13. [**Frame 40**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=40):
  - `**Issue #5138 : MISSING_OBJECT**` *(Tọa độ x:910, y:402)*: Bỏ sót vỉa hè (`sidewalk` #F423E8).
14. [**Frame 41**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=41):
  - `**Issue #5143 : MISSING_OBJECT**` *(Tọa độ x:38, y:416)*: Bỏ sót biển báo giao thông (`traffic_sign` #DCDC00).
    - `**Issue #5142 : MISSING_OBJECT**` *(Tọa độ x:865, y:544)*: Bỏ sót vỉa hè (`sidewalk` #F423E8).
    - `**Issue #5141 : IMPROPER_BOUNDARY**` *(Tọa độ x:479, y:428)*: Chưa vẽ hết cây / thảm thực vật (`vegetation` #6B8E23).
    - `**Issue #5139 : INCORRECT_CLASS**` *(Tọa độ x:69, y:529)*: Gán nhầm người điều khiển phương tiện (`rider` #FF0000) thành người đi bộ (`person` #DC143C).
15. [**Frame 42**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=42):
  - `**Issue #5171 : MISSING_OBJECT**` *(Tọa độ x:1235, y:357)*: Bỏ sót cột (`pole` #999999).
    - `**Issue #5170 : MISSING_OBJECT**` *(Tọa độ x:336, y:388)*: Bỏ sót cây / thảm thực vật (`vegetation` #6B8E23).
    - `**Issue #5169 : MISSING_OBJECT**` *(Tọa độ x:84, y:423)*: Bỏ sót lề đường / vỉa hè (`sidewalk` #F423E8).

- **Hướng xử lý**: 
  - Đối chiếu nhãn chuẩn theo `Semantic_Segmentation_Taxonomy_Translation.md` và Bảng Mã Issue Tags quy chuẩn.
  - **Frame 27:** Yêu cầu tỉa trọn vẹn bề mặt `building` (#5086) và đổi nhãn 3 cột từ `building` sang `pole` (#5085, #5084, #5083).
  - **Frame 25:** Yêu cầu bóc tách và đổi nhãn hàng rào sắt từ `building` sang `fence` (#5091).
  - **Frame 28:** Đưa Mentor/Lead hỗ trợ chốt ranh giới gốc cây bị bóng che tối (#5100); Yêu cầu vẽ bổ sung hàng rào sắt (`fence` #BE9999 tại #5098) và vỉa hè (`sidewalk` #F423E8 tại #5097, #5096, #5095).
  - **Frame 29:** Yêu cầu vẽ phủ trọn vẹn thân xe ô tô (`car` #00008E tại #5103) và vẽ bổ sung vỉa hè (`sidewalk` #F423E8 tại #5105).
  - **Frame 30:** Yêu cầu vẽ bổ sung vỉa hè (`sidewalk` #F423E8 tại #5108).
  - **Frame 31:** Yêu cầu đổi nhãn cột từ `building` sang `pole` (#5112) và đổi nhãn 2 biển báo từ `building` sang `traffic_sign` (#5114, #5111).
  - **Frame 32:** Yêu cầu vẽ bổ sung người đi bộ (`person` #DC143C tại #5116).
  - **Frame 33:** Yêu cầu vẽ bổ sung vỉa hè (`sidewalk` #F423E8 tại #5117), hàng rào kín (`fence` #BE9999 tại #5125) và toàn bộ 8 cột (`pole` #999999 từ #5118 đến #5126).
  - **Frame 34:** Yêu cầu vẽ bổ sung biển báo (`traffic_sign` #DCDC00 tại #5131), vỉa hè (`sidewalk` #F423E8 tại #5127), xe ô tô (`car` #00008E tại #5130) và 2 cột (`pole` #999999 tại #5129, #5128).
  - **Frame 35:** Yêu cầu vẽ bổ sung vỉa hè (`sidewalk` #F423E8 tại #5132).
  - **Frame 36:** Yêu cầu vẽ bổ sung thảm thực vật (`vegetation` #6B8E23 tại #5135) và đất đá tự nhiên (`terrain` #98FB98 tại #5134, #5133).
  - **Frame 39:** Yêu cầu vẽ bổ sung tường đứng (`wall` #66669C tại #5137) và bám biên phủ trọn vẹn mặt đường (`road` #804080 tại #5136).
  - **Frame 40:** Yêu cầu vẽ bổ sung vỉa hè (`sidewalk` #F423E8 tại #5138).
  - **Frame 41:** Yêu cầu vẽ bổ sung biển báo (`traffic_sign` #DCDC00 tại #5143), vỉa hè (`sidewalk` #F423E8 tại #5142), tô trọn vẹn thảm thực vật (`vegetation` #6B8E23 tại #5141) và đổi nhãn từ `person` sang `rider` (#FF0000 tại #5139).
  - **Frame 42:** Yêu cầu vẽ bổ sung cột (`pole` #999999 tại #5171), cây / thảm thực vật (`vegetation` #6B8E23 tại #5170) và lề đường / vỉa hè (`sidewalk` #F423E8 tại #5169).
- **Kết luận**: ⏳ **ĐANG REVIEW** ➔ Chờ Annotator sửa các Issues từ Frame 25 đến Frame 42 trước khi bàn giao cho Team Leader.

---

### JOB-1614

- **Task ID**: 191 | **Job ID**: 1614 | **Annotator**: `@2A202602206` | **Reviewer**: `@huynh`
- **Kiểm mẫu**: 25/125 ảnh | **Số ảnh lỗi**: 5 ảnh (Frame 1, Frame 2, Frame 9, Frame 15, Frame 18)

#### Minh chứng lỗi phát hiện / Issue tồn đọng:

1. [**Frame 1**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1614?frame=1):
  - `**Issue #5175 : MISSING_OBJECT**` *(Tọa độ x:1199, y:260)*: Bỏ sót cột (`pole` #999999).
2. [**Frame 2**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1614?frame=2):
  - `**Issue #5177 : MISSING_OBJECT**` *(Tọa độ x:224, y:267)*: Bỏ sót cây / thảm thực vật (`vegetation` #6B8E23).
3. [**Frame 9**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1614?frame=9):
  - `**Issue #5178 : EXTRA_OBJECT**` *(Tọa độ x:860, y:140)*: Đánh thừa đối tượng cầu thành tòa nhà (`building` #464646). Do cầu (`bridge`) không thuộc bộ 19 nhãn quy định, Annotator không được tự ý ép vào nhãn `building`.
4. [**Frame 15**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1614?frame=15):
  - `**Issue #5179 : INCORRECT_CLASS**` *(Tọa độ x:25, y:354)*: Đánh nhầm xe ô tô (`car` #00008E) thành người đi bộ (`person` #DC143C).
  - `**Issue #5180 : INCORRECT_CLASS**` *(Tọa độ x:822, y:447)*: Đánh nhầm xe ô tô (`car` #00008E) thành người đi bộ (`person` #DC143C).
  - `**Issue #5181 : INCORRECT_CLASS**` *(Tọa độ x:1165, y:438)*: Đánh nhầm xe ô tô (`car` #00008E) thành người đi bộ (`person` #DC143C).
5. [**Frame 18**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1614?frame=18):
  - `**Issue #5182 : MISSING_OBJECT**` *(Tọa độ x:777, y:254)*: Bỏ sót cột / cột điện (`pole` #999999).

- **Hướng xử lý**: 
  - Đối chiếu nhãn chuẩn theo `Semantic_Segmentation_Taxonomy_Translation.md` và Bảng Mã Issue Tags quy chuẩn.
  - **Frame 1:** Yêu cầu Annotator vẽ bổ sung cột (`pole` #999999 tại #5175).
  - **Frame 2:** Yêu cầu Annotator vẽ bổ sung cây / thảm thực vật (`vegetation` #6B8E23 tại #5177).
  - **Frame 9:** Yêu cầu Annotator xóa mask đối tượng cầu đang bị gắn ép sai sang nhãn tòa nhà (`building` #464646 tại #5178), vì cầu không nằm trong danh mục 19 nhãn được phép gán.
  - **Frame 15:** Yêu cầu Annotator đổi nhãn cả 3 vị trí từ người đi bộ (`person` #DC143C) sang xe ô tô (`car` #00008E tại #5179, #5180, #5181).
  - **Frame 18:** Yêu cầu Annotator vẽ bổ sung cột / cột điện (`pole` #999999 tại #5182).
- **Kết luận**: ⏳ **ĐANG REVIEW** ➔ Chờ Annotator sửa các Issues ở Frame 1, Frame 2, Frame 9, Frame 15 và Frame 18 trước khi bàn giao cho Team Leader.

---

### JOB-1616

- **Task ID**: 191 | **Job ID**: 1616 | **Annotator**: `@2A202602206` | **Reviewer**: `@huynh`
- **Kiểm mẫu**: 15/100 ảnh | **Số ảnh lỗi**: 14 ảnh (Frame 50, Frame 52, Frame 53, Frame 54, Frame 55, Frame 56, Frame 57, Frame 58, Frame 59, Frame 60, Frame 61, Frame 62, Frame 63, Frame 64)

#### Minh chứng lỗi phát hiện / Issue tồn đọng:

1. [**Frame 50**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=50):
  - `**Issue #5184 : MISSING_OBJECT**`: Bỏ sót cột / cột đèn (`pole` #999999).
  - `**Issue #5183 : MISSING_OBJECT**`: Bỏ sót hàng rào lưới (`fence` #BE9999).
  - `**Issue #5185 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5186 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5187 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5188 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5189 : IMPROPER_BOUNDARY**`: Vẽ thiếu / chưa trọn vẹn đèn giao thông (`traffic_light` #FAAA1E).
  - `**Issue #5190 : IMPROPER_BOUNDARY**`: Vẽ thiếu / chưa trọn vẹn đèn giao thông (`traffic_light` #FAAA1E).
  - `**Issue #5205 : EXTRA_OBJECT**`: Đánh thừa vạch kẻ đường / line đường (không nằm trong danh mục 19 nhãn quy định).
  - `**Issue #5206 : EXTRA_OBJECT**`: Đánh thừa vạch kẻ đường / line đường (không nằm trong danh mục 19 nhãn quy định).
  - `**Issue #5207 : EXTRA_OBJECT**`: Đánh thừa vạch kẻ đường / line đường (không nằm trong danh mục 19 nhãn quy định).
  - `**Issue #5208 : EXTRA_OBJECT**`: Đánh thừa vạch kẻ đường / line đường (không nằm trong danh mục 19 nhãn quy định).
  - `**Issue #5209 : EXTRA_OBJECT**`: Đánh thừa vạch kẻ đường / line đường (không nằm trong danh mục 19 nhãn quy định).
2. [**Frame 52**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=52):
  - `**Issue #5079 : MISSING_OBJECT**`: Bỏ sót cây / thảm thực vật (`vegetation` #6B8E23).
  - `**Issue #5269 : MISSING_OBJECT**`: Bỏ sót đèn giao thông (`traffic_light` #FAAA1E).
  - `**Issue #1747 : MISSING_OBJECT**`: Bỏ sót đèn giao thông (`traffic_light` #FAAA1E).
  - `**Issue #1750 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5211 : MISSING_OBJECT**`: Bỏ sót không đánh nhãn người đi bộ (`person` #DC143C) / người điều khiển phương tiện (`rider` #FF0000).
3. [**Frame 53**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=53):
  - `**Issue #5271 : MISSING_OBJECT**`: Bỏ sót vỉa hè / lề đường (`sidewalk` #F423E8).
  - `**Issue #5272 : MISSING_OBJECT**`: Bỏ sót vỉa hè / lề đường (`sidewalk` #F423E8).
  - `**Issue #5273 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5275 : INCORRECT_CLASS**`: Gán nhầm xe xe khách/xe buýt (`bus` #003C64) thành xe ô tô con (`car` #00008E), đồng thời vẽ thiếu phần thân xe.
4. [**Frame 54**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=54):
  - `**Issue #5281 : MISSING_OBJECT**`: Bỏ sót vỉa hè / lề đường (`sidewalk` #F423E8).
  - `**Issue #1805 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #1806 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #1810 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5276 : MISSING_OBJECT**`: Bỏ sót không đánh nhãn người đi bộ (`person` #DC143C) / người điều khiển phương tiện (`rider` #FF0000).
5. [**Frame 55**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=55):
  - `**Issue #5285 : MISSING_OBJECT**`: Bỏ sót đèn giao thông (`traffic_light` #FAAA1E).
  - `**Issue #5286 : MISSING_OBJECT**`: Bỏ sót biển báo giao thông (`traffic_sign` #DCDC00).
  - `**Issue #5287 : MISSING_OBJECT**`: Bỏ sót biển báo giao thông (`traffic_sign` #DCDC00).
  - `**Issue #5288 : MISSING_OBJECT**`: Bỏ sót vỉa hè / lề đường (`sidewalk` #F423E8).
6. [**Frame 56**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=56):
  - `**Issue #5291 : MISSING_OBJECT**`: Bỏ sót người đi bộ (`person` #DC143C) / người điều khiển phương tiện (`rider` #FF0000).
  - `**Issue #5292 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5293 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
  - `**Issue #5294 : MISSING_OBJECT**`: Bỏ sót vỉa hè / lề đường (`sidewalk` #F423E8).
7. [**Frame 57**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=57):
  - `**Issue #5227 : DUPLICATE_MASK**`: Vẽ trùng đúp polygon thảm thực vật / cây (`vegetation` #6B8E23).
  - `**Issue #5228 : MISSING_OBJECT**`: Bỏ sót vỉa hè / lề đường (`sidewalk` #F423E8).
  - `**Issue #5229 : INCORRECT_CLASS**`: Gán nhầm xe buýt / xe khách (`bus` #003C64) thành xe ô tô con (`car` #00008E).
  - `**Issue #5230 : INCORRECT_CLASS**`: Gán nhầm xe buýt / xe khách (`bus` #003C64) thành xe ô tô con (`car` #00008E).
  - `**Issue #5231 : MISSING_OBJECT**`: Bỏ sót xe tải (`truck` #000046).
  - `**Issue #5232 : MISSING_OBJECT**`: Bỏ sót xe buýt / xe khách (`bus` #003C64).
  - `**Issue #5233 : MISSING_OBJECT**`: Bỏ sót xe buýt / xe khách (`bus` #003C64).
  - `**Issue #5234 : MISSING_OBJECT**`: Bỏ sót xe buýt / xe khách (`bus` #003C64).
  - `**Issue #5235 : MISSING_OBJECT**`: Bỏ sót xe buýt / xe khách (`bus` #003C64).
8. [**Frame 58**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=58):
  - `**Issue #5236 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
9. [**Frame 59**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=59):
  - `**Issue #5237 : MISSING_OBJECT**`: Bỏ sót vỉa hè / lề đường (`sidewalk` #F423E8).
10. [**Frame 60**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=60):
  - `**Issue #5238 : DUPLICATE_MASK**`: Vẽ trùng đúp polygon / mask đè đúp lên cùng 1 vật thể.
    - `**Issue #5239 : IMPROPER_BOUNDARY**`: Vẽ thiếu mask / chưa phủ trọn vẹn bề mặt vật thể.
11. [**Frame 61**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=61):
  - `**Issue #5312 : MISSING_OBJECT**`: Bỏ sót công trình / tòa nhà (`building` #464646).
    - `**Issue #5313 : MISSING_OBJECT**`: Bỏ sót xe ô tô (`car` #00008E).
12. [**Frame 62**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=62):
  - `**Issue #5314 : MISSING_OBJECT**`: Bỏ sót công trình / tòa nhà (`building` #464646).
    - `**Issue #5315 : MISSING_OBJECT**`: Bỏ sót đèn giao thông (`traffic_light` #FAAA1E).
13. [**Frame 63**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=63):
  - `**Issue #5316 : MISSING_OBJECT**`: Bỏ sót biển báo giao thông (`traffic_sign` #DCDC00).
14. [**Frame 64**](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1616?frame=64):
  - `**Issue #5318 : MISSING_OBJECT**`: Bỏ sót biển báo giao thông (`traffic_sign` #DCDC00).
  - `**Issue #5320 : MISSING_OBJECT**`: Bỏ sót biển báo giao thông (`traffic_sign` #DCDC00).
  - `**Issue #5321 : MISSING_OBJECT**`: Bỏ sót biển báo giao thông (`traffic_sign` #DCDC00).
  - `**Issue #5322 : MISSING_OBJECT**`: Bỏ sót vỉa hè / lề đường (`sidewalk` #F423E8).
  - `**Issue #5323 : MISSING_OBJECT**`: Bỏ sót đèn giao thông (`traffic_light` #FAAA1E).
  - `**Issue #5324 : MISSING_OBJECT**`: Bỏ sót đèn giao thông (`traffic_light` #FAAA1E).

- **Hướng xử lý**: 
  - Đối chiếu nhãn chuẩn theo `Semantic_Segmentation_Taxonomy_Translation.md` và Bảng Mã Issue Tags quy chuẩn.
  - **Frame 50:** Yêu cầu Annotator:
    1. Vẽ bổ sung cột đèn (`pole` #999999 tại #5184) và hàng rào lưới (`fence` #BE9999 tại #5183).
    2. Vẽ bổ sung toàn bộ 4 xe ô tô (`car` #00008E tại #5185, #5186, #5187, #5188).
    3. Tỉa và vẽ phủ trọn vẹn đèn giao thông (`traffic_light` #FAAA1E tại #5189, #5190).
    4. Xóa toàn bộ mask vạch kẻ / line đường đang bị đánh thừa (`#5205`, `#5206`, `#5207`, `#5208`, `#5209`), do vạch kẻ đường không nằm trong danh mục 19 nhãn được phép gán nhãn riêng.
  - **Frame 52:** Yêu cầu Annotator:
    1. Vẽ bổ sung cây / thảm thực vật (`vegetation` #6B8E23 tại #5079).
    2. Vẽ bổ sung đèn giao thông (`traffic_light` #FAAA1E tại #5269 và #1747).
    3. Vẽ bổ sung xe ô tô (`car` #00008E tại #1750).
    4. Vẽ bổ sung toàn bộ người đi bộ (`person` #DC143C) / người lái xe (`rider` #FF0000) còn thiếu trên ảnh.
  - **Frame 53:** Yêu cầu Annotator:
    1. Vẽ bổ sung vỉa hè / lề đường (`sidewalk` #F423E8 tại #5271 và #5272).
    2. Vẽ bổ sung xe ô tô (`car` #00008E tại #5273).
    3. Đổi nhãn từ xe ô tô (`car`) sang xe buýt / xe khách (`bus` #003C64 tại #5275) và bổ sung trọn vẹn thân xe.
  - **Frame 54:** Yêu cầu Annotator:
    1. Vẽ bổ sung vỉa hè / lề đường (`sidewalk` #F423E8 tại #5281).
    2. Vẽ bổ sung toàn bộ 3 xe ô tô (`car` #00008E tại #1805, #1806, #1810).
    3. Vẽ bổ sung toàn bộ người đi bộ (`person` #DC143C) / người lái xe (`rider` #FF0000) còn thiếu trên ảnh.
  - **Frame 55:** Yêu cầu Annotator:
    1. Vẽ bổ sung đèn giao thông (`traffic_light` #FAAA1E tại #5285).
    2. Vẽ bổ sung 2 biển báo giao thông (`traffic_sign` #DCDC00 tại #5286, #5287).
    3. Vẽ bổ sung vỉa hè / lề đường (`sidewalk` #F423E8 tại #5288).
  - **Frame 56:** Yêu cầu Annotator:
    1. Vẽ bổ sung người đi bộ (`person` #DC143C / `rider` #FF0000 tại #5291).
    2. Vẽ bổ sung 2 xe ô tô (`car` #00008E tại #5292, #5293).
    3. Vẽ bổ sung vỉa hè / lề đường (`sidewalk` #F423E8 tại #5294).
  - **Frame 57:** Yêu cầu Annotator:
    1. Xóa bớt polygon bị trùng đúp thảm thực vật / cây (`vegetation` #6B8E23 tại #5227).
    2. Vẽ bổ sung vỉa hè / lề đường (`sidewalk` #F423E8 tại #5228).
    3. Đổi nhãn từ `car` sang xe buýt / xe khách (`bus` #003C64 tại #5229, #5230).
    4. Vẽ bổ sung xe tải (`truck` #000046 tại #5231).
    5. Vẽ bổ sung toàn bộ 4 xe buýt / xe khách (`bus` #003C64 tại #5232, #5233, #5234, #5235).
  - **Frame 58:** Yêu cầu Annotator:
    1. Vẽ bổ sung xe ô tô (`car` #00008E tại #5236).
  - **Frame 59:** Yêu cầu Annotator:
    1. Vẽ bổ sung vỉa hè / lề đường (`sidewalk` #F423E8 tại #5237).
  - **Frame 60:** Yêu cầu Annotator:
    1. Xóa bớt mask bị trùng đúp đè lên vật thể tại #5238.
    2. Vẽ bổ sung phủ trọn vẹn phần mask còn thiếu tại #5239.
  - **Frame 61:** Yêu cầu Annotator:
    1. Vẽ bổ sung tòa nhà / công trình (`building` #464646 tại #5312).
    2. Vẽ bổ sung xe ô tô (`car` #00008E tại #5313).
  - **Frame 62:** Yêu cầu Annotator:
    1. Vẽ bổ sung tòa nhà / công trình (`building` #464646 tại #5314).
    2. Vẽ bổ sung đèn giao thông (`traffic_light` #FAAA1E tại #5315).
  - **Frame 63:** Yêu cầu Annotator:
    1. Vẽ bổ sung biển báo giao thông (`traffic_sign` #DCDC00 tại #5316).
  - **Frame 64:** Yêu cầu Annotator:
    1. Vẽ bổ sung 3 biển báo giao thông (`traffic_sign` #DCDC00 tại #5318, #5320, #5321).
    2. Vẽ bổ sung vỉa hè / lề đường (`sidewalk` #F423E8 tại #5322).
    3. Vẽ bổ sung 2 đèn giao thông (`traffic_light` #FAAA1E tại #5323, #5324).
- **Kết luận**: ⏳ **ĐANG REVIEW** ➔ Chờ Annotator sửa các Issues từ Frame 50 đến Frame 64 trước khi bàn giao cho Team Leader.

