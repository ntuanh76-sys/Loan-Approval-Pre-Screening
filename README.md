# Company X – Sàng lọc Hồ sơ Phê duyệt Khoản vay (Loan Approval Pre-Screening)

**Môn học:** FDC104 – Lập trình cho phân tích dữ liệu và tính toán khoa học
**Phiên bản:** Final
**Ngày nộp:** 12/07/2026

---

## 1. Giới thiệu dự án

Company X cung cấp dịch vụ cho vay mua nhà cho khách hàng tại khu vực thành thị, bán đô thị và nông thôn. Khi số lượng hồ sơ vay ngày càng tăng, quy trình sàng lọc thủ công trở nên tốn thời gian và thiếu nhất quán.

**Mục tiêu dự án:** Xây dựng giải pháp Machine Learning hoàn chỉnh nhằm phân tích dữ liệu lịch sử phê duyệt khoản vay và đề xuất mô hình dự đoán phù hợp, hỗ trợ Company X nâng cao hiệu quả sàng lọc hồ sơ vay vốn — **hỗ trợ**, không thay thế, chuyên viên tín dụng.

> **Lưu ý về phạm vi:** Mô hình dự đoán *quyết định phê duyệt* trong dữ liệu lịch sử, không dự đoán rủi ro vỡ nợ của khách hàng. Kết quả phản ánh mối liên hệ thống kê, không khẳng định quan hệ nhân quả.

## 2. Quy trình thực hiện (Workflow)

Notebook được xây dựng theo quy trình CRISP-DM:
Business Understanding → Data Understanding → Data Quality Assessment → Exploratory Data Analysis → Feature Engineering → Data Preprocessing → Model Development → Model Evaluation → Business Recommendation
Ba mô hình học máy có giám sát được xây dựng và so sánh: **Logistic Regression, Decision Tree và Random Forest**, nhằm đánh giá sự cân bằng giữa hiệu quả dự đoán, khả năng giải thích và tính ứng dụng trong thực tiễn.

## 3. Bộ dữ liệu

- **Số lượng:** 614 hồ sơ vay mua nhà lịch sử của Company X.
- **File đầu vào:** `company-x-loan.csv` (cần đặt cùng thư mục với notebook trước khi chạy).
- **Biến mục tiêu:** `Loan_Status` (Y = được phê duyệt, N = bị từ chối).
- **Nhóm biến chính:** thông tin nhân khẩu học (Gender, Married, Dependents, Education, Self_Employed), tài chính (ApplicantIncome, CoapplicantIncome, LoanAmount, Loan_Amount_Term), lịch sử tín dụng (Credit_History) và khu vực bất động sản (Property_Area).

## 4. Nội dung notebook

| Mục | Nội dung chính |
|---|---|
| 1. Business Understanding | Bối cảnh kinh doanh, bài toán, mục tiêu và các câu hỏi nghiệp vụ |
| 2. Dataset Understanding | Tổng quan dữ liệu, từ điển dữ liệu, thống kê mô tả |
| 3. Data Quality Assessment | Kiểm tra giá trị thiếu, dữ liệu trùng lặp, kiểu dữ liệu |
| 4. Exploratory Data Analysis | Phân bổ biến mục tiêu, biến số, biến phân loại, tương quan |
| 5. Feature Engineering | Xử lý giá trị thiếu, biến đổi log, tạo biến mới (Total_Income, EMI, Loan_to_Income_Ratio) |
| 6. Data Preprocessing | Mã hóa biến phân loại, tổng hợp tập dữ liệu sạch |
| 7. Model Development | Chia Train/Test, tiền xử lý leak-safe, huấn luyện và tối ưu 3 mô hình bằng GridSearchCV |
| 8. Model Evaluation | Đánh giá từng mô hình, so sánh và lựa chọn mô hình tốt nhất |
| 9. Business Recommendation | Khuyến nghị áp dụng cho Company X |
| 10. Conclusion | Tổng kết quy trình và kết quả |
| 11. Limitations & Future Work | Hạn chế và hướng phát triển tiếp theo |

## 5. Điểm nổi bật về phương pháp

- **Leak-safe workflow:** Toàn bộ bước học tham số (điền khuyết, chuẩn hóa) chỉ được thực hiện trên tập Training sau khi chia Train/Test (80/20, stratified), sau đó áp dụng lại cho tập Test.
- **Missing value flag:** Biến `Credit_History_Missing` được giữ lại vì nhóm hồ sơ thiếu thông tin tín dụng có tỷ lệ phê duyệt khác biệt rõ rệt so với nhóm còn lại.
- **Feature Engineering:** Tạo các biến phản ánh khả năng chi trả — `Total_Income`, `EMI` (khoản trả hàng tháng), `Loan_to_Income_Ratio`.
- **Feature set theo từng mô hình:** Logistic Regression dùng biến đã log-transform; Decision Tree và Random Forest dùng biến gốc.
- **Tiêu chí tối ưu:** Precision được chọn làm thước đo chính khi tối ưu hyperparameter (GridSearchCV, 5-fold Stratified CV), vì phê duyệt sai một hồ sơ rủi ro gây tổn thất lớn hơn.

## 6. Kết quả chính

| Business Question | Kết quả |
|---|---|
| Khách hàng vay vốn điển hình | Nam giới, đã kết hôn, tốt nghiệp đại học |
| Yếu tố ảnh hưởng mạnh nhất đến phê duyệt | Credit History |
| Biến tạo thêm hiệu quả nhất | Loan-to-Income Ratio, EMI |
| Mô hình tốt nhất trên tập Test | Decision Tree (Accuracy 0.886, Precision 0.882, F1-score 0.921) |
| Khuyến nghị sử dụng | Hỗ trợ ưu tiên hồ sơ để chuyên viên tín dụng xem xét, không thay thế con người |

**Lựa chọn mô hình theo mục tiêu triển khai:**

- **Logistic Regression** – phù hợp khi cần minh bạch tối đa với cơ quan quản lý/kiểm toán.
- **Decision Tree** *(khuyến nghị chính)* – quy tắc trực quan, dễ giải thích, độ chính xác cao nhất; phù hợp hỗ trợ chuyên viên tín dụng.
- **Random Forest** – ổn định hơn khi mở rộng quy mô tự động hóa, ít phụ thuộc vào một biến duy nhất.

## 7. Yêu cầu cài đặt

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

## 8. Hướng dẫn chạy

1. Đặt file dữ liệu `company-x-loan.csv` cùng thư mục với notebook `Loan_Approval_Pre-Screening.ipynb`.
2. Mở notebook bằng Jupyter Notebook/JupyterLab.
3. Chạy tuần tự các cell từ đầu đến cuối (Kernel → Restart & Run All).
4. Notebook sẽ xuất ra file trung gian `loan_cleaned.csv` sau bước Feature Engineering/Preprocessing, được dùng lại ở phần Model Development.

## 9. Hạn chế và hướng phát triển

**Hạn chế:**
- Quy mô dữ liệu nhỏ (614 dòng), hạn chế độ tin cậy khi tổng quát hóa.
- Chưa có dữ liệu lịch sử việc làm, nợ hiện tại, hành vi trả nợ sau giải ngân.
- Chưa có điểm tín dụng từ tổ chức bên ngoài, ngoài biến nhị phân `Credit_History`.

**Hướng phát triển:**
- Thu thập thêm dữ liệu lịch sử khách hàng theo thời gian.
- Chuẩn hóa pipeline tiền xử lý bằng `Pipeline` + `ColumnTransformer`.
- Thử nghiệm mô hình Gradient Boosting (XGBoost).
- Bổ sung giải thích mô hình bằng SHAP.
