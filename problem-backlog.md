# Problem Backlog

Tập trung các lỗi gán nhãn thực tế phát sinh trên CVAT do Guideline chưa đề cập hoặc mơ hồ.

---

## I. Bounding Box & 2D Annotation

### P-001: Vật ở xa zoom lên thấy được
- **Lỗi**: Chưa rõ vật thể ở xa kích thước nhỏ nhưng zoom lên thấy được thì vẽ bbox hay chỉ viết báo cáo.
- **Minh chứng CVAT**: [Task 137 / Job 1401 - Frame 89](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=89)
- **Guideline**: Chưa quy định ngưỡng kích thước (pixel threshold) hoặc quy chuẩn gán nhãn/báo cáo vật ở xa.

### P-003: Xe ban đêm bị chói lóa đèn pha (Headlight Glare)
- **Lỗi**: Chưa rõ BBox `car` ôm sát thân xe hay kéo rộng trùm lên cả vệt ánh sáng chói tỏa ra.
- **Minh chứng CVAT**: 
  - [Task 137 / Job 1401 - Frame 75](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=75)
  - [Task 137 / Job 1401 - Frame 87](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=87)
- **Guideline**: §3 (Quy tắc Bounding Box) chưa làm rõ ranh giới BBox đối với phản xạ / vệt sáng chói lóa ban đêm.

### P-004: Đèn giao thông ở xa chỉ lấp ló bóng đèn
- **Lỗi**: Chưa rõ đèn giao thông (`traffic_light`) ở xa chỉ thấy đốm sáng bóng đèn có khoanh box không và khoanh ôm đốm sáng hay cả hộp đèn.
- **Minh chứng CVAT**: 
  - [Task 137 / Job 1401 - Frame 76](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=76)
  - [Task 137 / Job 1401 - Frame 77](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=77)
  - [Task 137 / Job 1401 - Frame 87](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=87)
  - [Task 137 / Job 1401 - Frame 89](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=89)
- **Guideline**: §3 mơ hồ về bằng chứng thị giác đối với `traffic_light` bị che khuất / lấp ló ở xa.

### P-005: Xe bị bóng tối/vùng tối che khuất chỉ thấy dáng mờ
- **Lỗi**: Chưa rõ xe bị vùng tối ban đêm che gần hết chỉ còn phom mờ có vẽ BBox không.
- **Minh chứng CVAT**: *(Cần bổ sung link khi gặp)*
- **Guideline**: §3 mơ hồ về quy chuẩn gán nhãn `occluded` cho đối tượng trong điều kiện cực kỳ thiếu sáng.

### P-006: Làn đường phụ có chiều rộng hẹp ô tô không lọt vừa
- **Lỗi**: Chưa rõ làn đường phụ/lối hẹp ô tô không lưu thông vừa thì gán Polygon `area/drivable` hay `area/alternative`.
- **Minh chứng CVAT**: [Task 137 / Job 1401 - Frame 96](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=96)
- **Guideline**: §2 & §4.1 chưa tiêu chuẩn hóa định nghĩa bề rộng tối thiểu của `area/drivable` vs `area/alternative`.

---

## II. Semantic Segmentation

### P-101: Phân biệt `road` vs `sidewalk` tại làn đường hẹp
- **Lỗi**: Chưa rõ các pixel mặt đường tại làn hẹp ô tô không lọt vừa gán class `road` hay `sidewalk`.
- **Minh chứng CVAT**: [Task 137 / Job 1401 - Frame 96](https://cvat.note.transformerlabs.ai/tasks/137/jobs/1401?frame=96)
- **Guideline**: §4 (Cặp class dễ nhầm `road vs sidewalk`) chưa quy định về làn đường hẹp.

### P-102: Hàng rào/lan can kết cấu trên cầu vượt
- **Lỗi**: Chưa rõ hàng rào/lan can khung thép trên cầu vượt có tô mask không và gán `fence`, `wall` hay `building`.
- **Minh chứng CVAT**: [Task 191 / Job 1617 - Frame 82](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1617?frame=82)
- **Guideline**: §1 & §4 chưa định nghĩa phạm vi (`UNCERTAIN_SCOPE`) cho kết cấu bảo vệ trên cầu cao.

### P-103: Phân biệt `building` vs `fence` khi hàng rào bao quanh tòa nhà
- **Lỗi**: Annotator hay gộp bôi toàn bộ hàng rào phía trước tòa nhà thành `building`.
- **Minh chứng CVAT**: [Task 191 / Job 1615 - Frame 25](https://cvat.note.transformerlabs.ai/tasks/191/jobs/1615?frame=25)
- **Guideline**: §4 mơ hồ trong việc yêu cầu bóc tách riêng mask `fence` nằm sát/bao quanh `building`.
