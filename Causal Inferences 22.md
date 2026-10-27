以下是一套全新的因果推断场景，完全使用越南语撰写，聚焦于两个本土实用领域：热带农业适应气候变化和社区数字图书馆普及，通过珀尔结构因果模型和鲁宾潜在结果框架完成完整分析。

---

Kịch bản 43: Nông nghiệp Nhiệt đới & Phân Bón Tự Nhiên

1. Tiêu đề Kịch bản: Ảnh hưởng của việc áp dụng phân bón lá hữu cơ đến năng suất cây lúa mùa mưa

2. Các Biến Chính:
*   Xử lý (X): Sử dụng "Phân bón lá hữu cơ được chứng nhận" (Nhị phân: 1 = Sử dụng, 0 = Phân bón hóa học thông thường).
*   Kết quả (Y): Năng suất lúa thu hoạch hàng năm (Liên tục: tấn/ha).
*   Biến Nhiễu (Z): Hàm lượng mùn trong đất (Liên tục: % khối lượng). Các cánh đồng có đất giàu mùn có khả năng áp dụng phân hữu cơ cao hơn ($X$) vì hệ sinh thái đất đã cân bằng, và những cánh đồng này tự nhiên có năng suất cao hơn ($Y$) không phụ thuộc vào phân hóa học.

3. Cơ chế Nhân quả & DAG ASCII:
Hàm lượng mùn ($Z$) ảnh hưởng cả đến quyết định sử dụng phân hữu cơ ($X$) và năng suất cơ bản của lúa ($Y$). Phân bón lá hữu cơ ($X$) tác động trực tiếp đến kết quả ($Y$) bằng cách cải thiện sức khỏe vi sinh vật đất và khả năng chống chịu sâu bệnh.

```text
      Z (Hàm lượng mùn)
     / \
    v   v
X (Phân hữu cơ) --> Y (Năng suất lúa)
```

4. Cách tiếp cận SCM của Pearl (Chặn đường backdoor):
*   Mục tiêu: Ước tính mức tăng năng suất thực sự do phân hữu cơ gây ra: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Kiểm tra backdoor: Đường $X \leftarrow Z \rightarrow Y$ là đường nhiễu. Điều kiện trên hàm lượng mùn ($Z$) sẽ chặn hoàn toàn đường này.
*   Công thức điều chỉnh:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Cơ chế thực tế: Chúng ta so sánh năng suất giữa các cánh đồng dùng phân hữu cơ và dùng phân hóa học, phân tầng theo nhóm hàm lượng mùn. Điều này tách biệt hiệu quả của phân bón mới khỏi độ phì nhiêu vốn có của đất.

5. Cách tiếp cận Kết quả Tiềm năng của Rubin (Cân bằng Dữ liệu):
*   Kết quả tiềm năng: $Y_i(1)$ là năng suất của cánh đồng $i$ khi dùng phân hữu cơ; $Y_i(0)$ là khi dùng phân hóa học.
*   Điểm xu hướng (Propensity Score): Ước tính $e(z) = P(X=1 | Z=z)$. Các cánh đồng giàu mùn có xác suất áp dụng phân hữu cơ cao hơn.
*   Trọng số IPW:
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Cơ chế: Các trọng số tạo ra một giả dân số nơi phân bố hàm lượng mùn được cân bằng giữa hai nhóm. Những cánh đồng hiếm có đất nghèo mùn nhưng vẫn dùng phân hữu cơ nhận trọng số cao hơn, trong khi các cánh đồng giàu mùn phổ biến nhận trọng số thấp hơn, loại bỏ thiên vị lựa chọn.

---

Kịch bản 44: Thư viện Số Cộng đồng & Tỷ lệ Đọc Sách

1. Tiêu đề Kịch bản: Ảnh hưởng của việc mở thư viện số cộng đồng đến số lượt đọc sách của học sinh trung học cơ sở

2. Các Biến Chính:
*   Xử lý (X): Trường học có "Thư viện số cộng đồng được kết nối miễn phí" (Nhị phân: 1 = Có, 0 = Không).
*   Kết quả (Y): Số lượt đọc sách điện tử mỗi tháng của học sinh (Liên tục: lượt/học sinh).
*   Biến Nhiễu (Z): Mức độ đầu tư công nghệ của trường (Liên tục: điểm 0-100). Các trường có điều kiện tài chính tốt hơn được ưu tiên trang bị thư viện số ($X$), và những trường này vốn có tỷ lệ sử dụng thiết bị thông minh của học sinh cao hơn ($Y$).

3. Cơ chế Nhân quả & DAG ASCII:
Mức đầu tư công nghệ ($Z$) ảnh hưởng cả đến việc trang bị thư viện số ($X$) và tần suất đọc sách cơ bản của học sinh ($Y$). Thư viện số ($X$) tác động trực tiếp đến kết quả ($Y$) bằng cách cung cấp tài liệu miễn phí, đa dạng và dễ tiếp cận.

```text
      Z (Đầu tư công nghệ)
     / \
    v   v
X (Thư viện số) --> Y (Số lượt đọc)
```

4. Cách tiếp cận SCM của Pearl (Chặn đường backdoor):
*   Mục tiêu: Ước tính mức tăng lượt đọc thực sự do thư viện số gây ra: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Kiểm tra backdoor: Đường $X \leftarrow Z \rightarrow Y$ là đường nhiễu. Điều kiện trên mức đầu tư công nghệ ($Z$) sẽ chặn hoàn toàn đường này.
*   Công thức điều chỉnh:
    $$ P(Y=1 | do(X=1)) = \sum_{z} P(Y=1 | X=1, Z=z) P(Z=z) $$
*   Cơ chế thực tế: Chúng ta so sánh số lượt đọc giữa các trường có thư viện số và không có, phân tầng theo mức đầu tư công nghệ. Điều này tách biệt hiệu quả của thư viện số khỏi lợi thế vốn có về trang thiết bị của trường.

5. Cách tiếp cận Kết quả Tiềm năng của Rubin (Cân bằng Dữ liệu):
*   Kết quả tiềm năng: $Y_i(1)$ là số lượt đọc của học sinh trường $i$ khi có thư viện số; $Y_i(0)$ là khi không có.
*   Điểm xu hướng (Propensity Score): Ước tính $e(z) = P(X=1 | Z=z)$. Các trường đầu tư cao có xác suất có thư viện số lớn hơn.
*   Trọng số IPW:
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Cơ chế: Các trọng số cân bằng phân bố mức đầu tư công nghệ giữa hai nhóm. Những trường hiếm có đầu tư thấp nhưng vẫn có thư viện số nhận trọng số cao hơn, trong khi các trường đầu tư cao phổ biến nhận trọng số thấp hơn, tạo ra sự so sánh không thiên vị.



---
Bạn có muốn tôi tạo một mô phỏng Python hoàn chỉnh cho một trong hai kịch bản này với chú thích bằng tiếng Việt, để minh họa kết quả tính toán ATE bằng IPW và G-computation không?<br><br>百度AI生成，内容仅供参考