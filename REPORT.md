# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: Nhữ Đình Chiến
- Ngày / CVAT local: 17/09/2026 / http://localhost:8080
- Công cụ đã dùng: Brush, Polygon, SAM ViT-B (nuctl interactor)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg` (task `medium_instance`), đối tượng `car` ở phía dưới bên trái góc đường.
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Tôi dùng Polygon vẽ bám sát theo phần nhìn thấy (visible part) của thân xe. Dừng đường biên mask ngay tại mép cột đèn và đuôi xe bị che khuất; không tự suy đoán hay kéo dài đường biên phía sau phần bị che.
- Nếu dùng gợi ý sau đó: Khi dùng thử công cụ SAM ViT-B (nuctl interactor), vùng đề xuất tự động có xu hướng phủ thừa ra phần bóng râm (`shadow`) phía dưới gầm xe. Tôi đã chủ động chỉnh sửa thu hẹp các đỉnh polygon về sát lốp xe để tránh gộp bóng vào mask đối tượng.
- Nếu không dùng gợi ý: N/A (Đã dùng Polygon tự vẽ ban đầu và kiểm chứng chỉnh sửa với SAM).

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `cp2_slice`, khu vực hai xe ô tô cùng lớp (`car`) đỗ sát cạnh nhau ở góc phố.
- Lỗi thuộc loại: gộp-tách (gộp nhầm 2 instance sát nhau thành 1 mask).
- Bằng chứng tôi nhìn thấy: Hai chiếc xe ô tô đỗ liền kề ban đầu bị phủ chung một dải mask màu duy nhất. Khi phóng to quan sát khe hở giữa 2 thân xe và ranh giới kính lái thấy rõ hai đối tượng riêng biệt.
- Quy tắc và hành động sửa: Áp dụng quy tắc hình học "hai vật cùng lớp sát nhau vẫn là hai instance riêng biệt". Đã dùng Polygon tách thành 2 mask/object `car` riêng biệt trên danh sách Objects.
- Sau sửa đã Save và export lại chưa?: Đã Save trên CVAT local và export lại file `cp2_slice.zip` hợp lệ lưu tại `submissions/cp2_slice.zip`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Đã chạy `python scripts/inspect_submissions.py` xác nhận 13 annotations cho `cp2_slice.zip` hợp lệ / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp4_curb` (vùng bó vỉa hè) | `road` (mặt đường) hay `sidewalk` (vỉa hè)? | Mặt đường và bó vỉa hè có chất liệu rải nhựa gần tương đồng, nhưng độ cao bờ bó vỉa nhô cao rõ rệt so với lòng đường. | Quyết định chọn `sidewalk` theo quy tắc chức năng và cấu trúc nâng cao của bó vỉa. |
| 2. `cp5_occlusion` (xe bị cột che) | Tách thành 2 instance hay giữ làm 1 instance bị chia cắt? | Ô tô bị thân cột biển báo giao thông đè ngang ở giữa, chia phần nhìn thấy thành 2 mảng tách rời. | Quyết định giữ làm 1 instance duy nhất vì cùng thuộc về một chiếc xe bị che khuất (occluded). |
| 3. `cp1_holes` (kính lái ô tô) | Khoét lỗ phần kính trong suốt hay phủ kín mask toàn bộ xe? | Kính lái trong suốt nhìn xuyên qua phía sau nhưng là bộ phận cấu trúc không thể tách rời của ô tô. | Quyết định phủ kín toàn bộ khung xe, không khoét lỗ cửa kính theo đúng quy tắc edge case của `cp1_holes`. |
