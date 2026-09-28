# Peer feedback + owner response

Phần 1 do **nhóm peer** trả lời (gửi kèm file export). Phần 2 do **nhóm owner** điền. Thay mọi placeholder mới là xong (gate G5).

- **Nhóm peer:** Nhóm Peer Evaluators
- **Người label blind:** Peer Annotators

## 1. Peer trả lời

1. **Rule nào rõ nhất / giúp quyết định nhanh nhất?**
   Taxonomy rõ ràng, dễ hiểu.

2. **Rule nào mơ hồ hoặc phải tự suy diễn?**
   Trong trường hợp mình có thể tự suy đoán được loại biển dựa vào hình dáng mà nó bị quay lưng thì trong guideline không được nhắc đến.

3. **Sample nào khiến guideline "vỡ"?**
   Hiện chưa có sample nào như vậy.

4. **Attribute / default nào trong CVAT dễ gây thao tác sai?**
   Blurred hơi bị khó hiểu do không có định nghĩa như thế nào là mờ (ví dụ: kích thước nhỏ quá, <16px).

5. **Một thay đổi cụ thể giúp annotator mới ít hỏi hơn?**
   Thêm trường hợp và hướng giải quyết vụ biển bị quay lưng nhưng có thể đoán được là occluded.

## 2. Owner phân loại

Owner không tranh luận để bảo vệ guideline. Mỗi feedback và mỗi decision peer làm sai được xếp vào một hướng xử lý.

| Feedback / decision sai | Nguyên nhân (guideline gap / data ambiguity / execution error) | Xử lý (accept + revise / reject with evidence / add escalation rule) | Bằng chứng |
|---|---|---|---|
| Chưa rõ quy định gán nhãn biển báo bị quay mặt lưng | guideline gap | accept + revise (Bổ sung rõ quy tắc phân loại biển quay mặt lưng vào Guideline v3) | Peer Feedback Câu 2 & 5 |
| Định nghĩa thuộc tính blurred chưa có ngưỡng định lượng cụ thể | guideline gap | accept + revise (Bổ sung rõ ngưỡng kích thước <16px chọn mờ blurred=true vào Guideline v3) | Peer Feedback Câu 4 |
| Bỏ sót thuộc tính occluded=true khi biển bị che khuất | execution error | reject with evidence (Nhắc lại Mục 3.1 Guideline v3: biển che >= 10% bề mặt phải tích occluded=true) | Transfer Score GTS14/d1 |
