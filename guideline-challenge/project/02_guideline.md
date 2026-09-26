# Annotation guideline — Traffic Light Road Elements

**Version:** v2

## 1. Objective + scope

- **Mục tiêu:** Gán nhãn các hộp đầu đèn giao thông (`traffic_light`) phục vụ mô hình tự hành xác định chính xác tín hiệu điều khiển làn đường hiện tại của xe.
- **In-scope (Bắt buộc gán nhãn):** Toàn bộ đầu đèn giao thông dành cho xe cơ giới nhìn thấy phía trước đường đi.
- **Out-of-scope (Bỏ qua/Ignore):** Đèn giao thông dành cho người đi bộ, đèn cảnh báo công trình, đèn trang trí.

## 2. Annotation unit

- **Loại nhãn:** Rectangle (Bounding box 2D) cho từng instance đầu đèn giao thông riêng biệt.
- **Quy tắc:** Mỗi hộp vỏ đầu đèn (housing) tính là 1 instance duy nhất, không gộp chung nhiều đầu đèn trên cùng một giá treo.

## 3. Geometry rule

- **Định dạng:** Rectangle (Bounding Box 2D).
- **Quy cách:** Tight bounding box ôm sát phần vỏ outer housing nhìn thấy được của đầu đèn.
- **Tolerance:** Sai số đường biên ≤ 2px mỗi cạnh. Không bao gồm giá treo hoặc cột đèn.

## 4. Taxonomy

- **Class name:** `traffic_light`
- **Attributes:**
  - `color`: `red` | `yellow` | `green` | `off` | `unknown` (Mặc định: `unknown`)
  - `relevance`: `controlling_my_lane` | `other_lane` | `unknown` (Mặc định: `unknown`)
  - `occlusion`: `none` | `partially` | `heavily` (Mặc định: `none`)

## 5. Inclusion / exclusion

- **Inclusion (Label):** Tất cả các đầu đèn giao thông xe hơi có kích thước chiều cao ≥ 8px.
- **Exclusion (Ignore):** Các đầu đèn có chiều cao < 8px hoặc bị che khuất > 80% diện tích không thể phân biệt được màu sắc hay hình dáng vỏ.

## 6. Visibility / occlusion

- `occlusion = none`: Nhìn thấy hoàn toàn vỏ đầu đèn.
- `occlusion = partially`: Bị tán cây, biển báo hay vật thể che khuất từ 10% đến 50% diện tích.
- `occlusion = heavily`: Bị che khuất > 50% nhưng vẫn nhận biết được màu tín hiệu đèn đang bật.

## 7. Ambiguity / escalation

- Trường hợp không thể xác định đầu đèn đang điều khiển làn nào hay làn khác: Đặt `relevance = unknown`.
- Trường hợp bóng đèn bị lóa ánh mặt trời không rõ đang bật hay tắt: Đặt `color = unknown`.
- Trường hợp quá nghi ngờ: Gán nhãn và thêm comment `ESCALATE: [lý do]` vào thuộc tính của object trên CVAT.

## 8. Temporal rule

Không áp dụng — task ảnh tĩnh.

## 9. Examples

| sample_id | Thấy gì | Expected output | Rule áp dụng |
|---|---|---|---|
| BDD01 | Đèn đỏ ngã tư rõ ràng phía trước | `class: traffic_light`, `color: red`, `relevance: controlling_my_lane` | Rule 1 & Rule 4 |
| BDD02 | Đèn xanh góc phải làn phụ | `class: traffic_light`, `color: green`, `relevance: other_lane` | Rule 4 |
| BDD03 | Đèn bị tán cây che một phần | `class: traffic_light`, `color: red`, `occlusion: partially` | Rule 6 |
| BDD04 | Đèn cực nhỏ ở xa (>100m) | Ignore (không label) | Rule 5 |

## 10. Common mistakes

1. Gộp chung cả cột đèn hoặc thanh treo vào Bounding Box -> **Cách tránh:** Chỉ khoanh phần vỏ hình hộp chứa bóng đèn.
2. Quên chọn attribute `relevance` dẫn đến sai lệch downstream -> **Cách tránh:** Kiểm tra kỹ xem đèn có điều khiển trực tiếp làn đường của xe mình không.
3. Nhầm lẫn đèn đi bộ với đèn giao thông xe hơi -> **Cách tránh:** Đèn đi bộ thường có biểu tượng hình người, bỏ qua theo Scope.
