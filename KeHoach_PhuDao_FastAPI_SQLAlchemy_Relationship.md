# KẾ HOẠCH PHỤ ĐẠO FASTAPI – SQLALCHEMY RELATIONSHIP

> Chủ đề: Thiết kế quan hệ giữa các bảng và sử dụng Relationship trong SQLAlchemy với FastAPI   
> Thời lượng: 120 phút  

---

## 1. Thông tin buổi học

| Nội dung | Thông tin |
|---|---|
| Số  | 40 |
| Kiến thức đã có | Python ứng dụng; FastAPI cơ bản; tạo bảng và SQLAlchemy model |
| Cơ sở dữ liệu | MySQL |
| ORM | SQLAlchemy |
| Kiến thức cần củng cố | 1–1, 1–N, N–N; khóa ngoại; UNIQUE; bảng trung gian; <code>relationship()</code>; query qua relationship |
| Bối cảnh xuyên suốt | Hệ thống quản lý nhân sự và phân công dự án trong doanh nghiệp |

Nghiệp vụ được tổ chức theo ba mức:

- **Demo chính:** quản lý nhân viên, phòng ban và dự án.
- **Bài thực hành:** vận hành trung tâm hỗ trợ khách hàng đa kênh.
- **Final-test:** bán hàng và quản lý nhà cung cấp.

Buổi phụ đạo không dạy lại toàn bộ SQLAlchemy. Trọng tâm là giúp  đi được theo chuỗi:

> Yêu cầu nghiệp vụ → loại quan hệ → khóa ngoại/UNIQUE/bảng trung gian → thiết kế bảng → SQLAlchemy model → <code>relationship()</code> → truy cập dữ liệu.

---

## 2. Mục tiêu đầu ra

Kết thúc buổi học,  cần làm được tối thiểu:

1. Đọc yêu cầu đơn giản và nhận diện đúng quan hệ 1–1, 1–N hoặc N–N.
2. Chọn đúng bảng chứa Foreign Key (khóa ngoại).
3. Biết <code>UNIQUE</code> khi triển khai quan hệ 1–1.
4. Biết tạo Association Table (bảng trung gian) cho quan hệ N–N.
5. Phân biệt đúng vai trò của <code>ForeignKey</code> và <code>relationship()</code>.
6. Đọc và hoàn thiện SQLAlchemy model cơ bản có <code>back_populates</code>.
7. Truy cập được một object hoặc danh sách object liên quan.
8. Áp dụng được cùng quy trình vào mô hình khác trong project cuối kỳ.

### Tiêu chí đạt tối thiểu cuối buổi

- Từng nhóm năng lực trong Final-test đạt từ 60% câu trả lời đúng trở lên.
- Không còn lỗi như đặt FK phía 1 trong quan hệ 1–N hoặc bỏ bảng trung gian trong N–N.
- Sinh viên giải thích được: “Vì sao vị trí FK/UNIQUE/bảng trung gian này là cần thiết?”

### Ngoài phạm vi

Không dạy cascade nâng cao, eager/lazy loading chuyên sâu, <code>joinedload</code>, <code>selectinload</code>, association proxy, self-reference hoặc tối ưu truy vấn phức tạp.

---

## 3. Tổng quan chiến lược phụ đạo

Buổi học sử dụng sáu bước ngắn:

1. **Đo:** First-test tự động chấm để xác định điểm vướng.
2. **Chọn trọng tâm:** đọc tỷ lệ đúng theo bốn nhóm năng lực (Nhận diện relationship, Thiết kế database, SQLAlchemy relationship, Query relationship), không chỉ nhìn tổng điểm.
3. **Chữa bản chất:** database trước, SQLAlchemy sau.
4. **Làm mẫu ngắn:** mỗi quan hệ đi từ nghiệp vụ tới query.
5. **Sinh viên làm:** hoàn thành mô hình tương tự, có code khung.
6. **Đo lại:** Final-test bằng nghiệp vụ khác.

---

## 4. Timeline 120 phút

Mỗi khoảng có phần “nội dung thực” và phần dự phòng nằm ngay trong hoạt động. Tổng nội dung thực là **101 phút**; tổng dự phòng là **19 phút**.

| Thời gian | Nội dung thực / dự phòng | Hoạt động |  làm | Sinh viên làm | Đầu ra | Ưu tiên | Nếu trễ giờ |
|---|---:|---|---|---|---|---|---|
| 0–15 | 13 / 2 phút | First-test | Mở Form, nhắc đây là chẩn đoán, không giải bài | Làm cá nhân 8 câu | Dữ liệu theo 4 nhóm | MUST | Khóa Form ở phút 15; không kéo dài |
| 15–25 | 8 / 2 phút | Đọc kết quả nhanh | Xem tỷ lệ từng câu/nhóm, chọn lỗi lớn nhất | Đối chiếu đáp án, giơ thẻ A/B/C/D khi chữa | Bản đồ điểm vướng | MUST | Chỉ chữa 2–3 lỗi có tỷ lệ đúng thấp nhất |
| 25–36 | 9 / 2 phút | Quy trình database → ORM | Vẽ sơ đồ tổng, chốt FK/UNIQUE/bảng trung gian và ForeignKey vs relationship | Trả lời câu hỏi nhanh | Khung tư duy chung | MUST | Bỏ ví dụ phụ, giữ ba quy tắc chốt |
| 36–48 | 10 / 2 phút | Vòng 1: 1–1 | Dẫn dắt Employee–EmployeeProfile, demo ngắn | Dự đoán, hoàn thiện một dòng FK và query | Hiểu FK + UNIQUE | MUST | Không mở rộng thêm endpoint |
| 48–61 | 11 / 2 phút | Vòng 2: 1–N | Dẫn dắt Department–Employee | Chọn phía đặt FK, đọc hai chiều | Hiểu một object/danh sách | MUST | Chữa chung bằng sơ đồ, không chữa từng máy |
| 61–77 | 14 / 2 phút | Vòng 3: N–N | Cho dữ liệu cụ thể, dựng bảng trung gian rồi mới viết ORM | Điền các cặp employee_id/project_id | Hiểu hai quan hệ 1–N | MUST | Dùng code hoàn chỉnh có sẵn, chỉ yêu cầu giải thích |
| 77–87 | 8 / 2 phút | Query qua relationship | Chạy 5 truy cập cơ bản | Dự đoán kiểu kết quả trước khi chạy | Biết object hay list | MUST | Chỉ giữ 4 query bắt buộc |
| 87–101 | 12 / 2 phút | Bài tổng hợp | Phát đề quản lý nhà cung cấp và sản phẩm, theo dõi bằng thẻ trạng thái | Làm Phần A–E theo cặp | Sản phẩm chuyển giao | SHOULD/MUST | Cấp code khung nhiều hơn; vẫn giữ ít nhất 1 query mỗi loại |
| 101–105 | 4 / 0 phút | Chữa theo nhóm lỗi | Chiếu 4 lỗi phổ biến, sửa trực tiếp | Tự đánh dấu lỗi của nhóm | Mẫu đúng chung | MUST | Chỉ chữa lỗi ảnh hưởng cấu trúc dữ liệu |
| 105–113 | 8 / 0 phút | Final-test | Mở Form, không gợi ý | Làm cá nhân 6 câu | Dữ liệu sau phụ đạo | MUST | Không cắt |
| 113–117 | 4 / 0 phút | So sánh và chốt | Công bố thay đổi theo từng năng lực | Viết “một điều đã rõ, một điều còn vướng” | Hướng phụ đạo tiếp theo | MUST | Công bố sau buổi nếu bảng tính cập nhật chậm |
| 117–120 | 0 / 3 phút | Dự phòng cuối | Xử lý nộp bài, lỗi mạng hoặc câu hỏi cuối | Hoàn tất nộp | Kết thúc đúng giờ | BUFFER | Không dùng để mở nội dung mới |

### Quy tắc cắt nội dung

- **MUST HAVE:** First-test; chữa lỗi chính; 1–1/1–N/N–N; FK/UNIQUE/bảng trung gian; ForeignKey vs <code>relationship()</code>; query cơ bản; Final-test.
- **SHOULD HAVE:** <code>back_populates</code> hai chiều; hoàn thành đủ toàn bộ bài tổng hợp; nhiều ví dụ query.
- **COULD HAVE:** endpoint mở rộng; thêm dữ liệu mẫu; giải thích chi tiết cơ chế ORM.

Nếu trễ, cắt COULD HAVE trước, sau đó cấp thêm code khung để rút ngắn SHOULD HAVE. Không cắt Final-test.

---

## 5. First-test chẩn đoán

### Hướng dẫn thiết lập

- Tạo Google Form ở chế độ Quiz.
- Mỗi câu 1 điểm; bắt buộc trả lời; không trộn câu vì cần đọc kết quả theo nhóm.
- Giữ tiền tố A/B/C/D trong phương án để bảng tính dễ đếm.
- Không hiện đáp án cho tới khi tất cả  đã nộp.
- Thời gian mục tiêu 12–13 phút, tối đa 15 phút.

### PRE01

- **Nhóm năng lực:** A – Nhận diện relationship
- **Câu hỏi:** Mỗi <code>Employee</code> có tối đa một <code>EmployeeProfile</code>, và mỗi <code>EmployeeProfile</code> chỉ thuộc về một <code>Employee</code>. Đây là quan hệ nào?

| Phương án | Nội dung | Ý nghĩa nếu  chọn sai |
|---|---|---|
| A | 1–N | Chỉ nhìn chiều “Employee có profile” nhưng bỏ qua giới hạn một profile |
| B | 1–1 | **Đúng** |
| C | N–N | Nghĩ mọi quan hệ hai bảng đều cần bảng trung gian |
| D | Không có quan hệ | Chưa liên hệ được mô tả nghiệp vụ với mô hình dữ liệu |

- **Đáp án:** B.
- **Giải thích:** Hai phía đều chỉ liên kết tối đa một bản ghi.
- **Năng lực/lỗi đo:** nhận diện 1–1 từ giới hạn số lượng ở cả hai phía.

### PRE02

- **Nhóm năng lực:** A – Nhận diện relationship
- **Câu hỏi:** Một <code>Employee</code> có thể tham gia nhiều <code>Project</code>; một <code>Project</code> có nhiều <code>Employee</code>. Quan hệ đúng là gì?

| Phương án | Nội dung | Ý nghĩa nếu  chọn sai |
|---|---|---|
| A | 1–1 | Chưa đọc được từ khóa “nhiều” ở cả hai phía |
| B | 1–N | Chỉ xét một chiều Employee → Project |
| C | N–N | **Đúng** |
| D | Hai quan hệ 1–1 độc lập | Đang cố tránh bảng trung gian nhưng không biểu diễn được nghiệp vụ |

- **Đáp án:** C.
- **Giải thích:** Mỗi phía đều có thể liên kết nhiều bản ghi phía còn lại.
- **Năng lực/lỗi đo:** nhận diện N–N và nhu cầu chuyển thành hai quan hệ 1–N.

### PRE03

- **Nhóm năng lực:** B – Thiết kế database
- **Câu hỏi:** Một <code>Department</code> có nhiều <code>Employee</code>; mỗi <code>Employee</code> chỉ thuộc một <code>Department</code>. Thiết kế nào đúng?

| Phương án | Nội dung | Ý nghĩa nếu  chọn sai |
|---|---|---|
| A | Thêm <code>department_id</code> làm FK trong bảng <code>employees</code>, không UNIQUE | **Đúng** |
| B | Thêm <code>employee_id</code> làm FK trong bảng <code>departments</code> | Đặt FK ở phía 1, chỉ lưu được một nhân viên |
| C | Thêm <code>department_id</code> trong <code>employees</code> và đặt UNIQUE | Vô tình biến 1–N thành 1–1 |
| D | Tạo bảng trung gian <code>department_employees</code> | Nhầm 1–N thành N–N |

- **Đáp án:** A.
- **Giải thích:** FK nằm ở phía N; nhiều nhân viên được phép có cùng <code>department_id</code>.
- **Năng lực/lỗi đo:** vị trí FK và tác động của UNIQUE.

### PRE04

- **Nhóm năng lực:** B – Thiết kế database
- **Câu hỏi:** Cách nào triển khai đúng quan hệ 1–1 giữa <code>Employee</code> và <code>EmployeeProfile</code>?

| Phương án | Nội dung | Ý nghĩa nếu  chọn sai |
|---|---|---|
| A | Chỉ thêm <code>employee_id</code> vào <code>employee_profiles</code>, không FK | Có cột nhưng database không bảo vệ tính hợp lệ |
| B | Thêm FK ở cả hai bảng | Tạo phụ thuộc vòng và dư thừa |
| C | Tạo bảng trung gian có hai FK | Áp dụng máy móc cách làm N–N |
| D | <code>employee_profiles.employee_id</code> là FK tới <code>employees.id</code> và có UNIQUE | **Đúng** |

- **Đáp án:** D.
- **Giải thích:** FK tạo liên kết; UNIQUE ngăn một Employee xuất hiện ở nhiều profile.
- **Năng lực/lỗi đo:** vai trò kết hợp của FK và UNIQUE trong 1–1.

### PRE05

- **Nhóm năng lực:** C – SQLAlchemy
- **Câu hỏi:** Phát biểu nào đúng nhất?

| Phương án | Nội dung | Ý nghĩa nếu  chọn sai |
|---|---|---|
| A | <code>relationship()</code> tự tạo cột FK trong MySQL | Nhầm cấu hình ORM với ràng buộc database |
| B | <code>ForeignKey</code> tạo liên kết ở database; <code>relationship()</code> giúp truy cập object trong Python | **Đúng** |
| C | <code>ForeignKey</code> chỉ dùng để đổi tên thuộc tính Python | Chưa hiểu khóa ngoại |
| D | Chỉ cần <code>relationship()</code>, không cần FK | Mô hình ORM thiếu đường nối dữ liệu |

- **Đáp án:** B.
- **Giải thích:** Hai thành phần hỗ trợ hai tầng khác nhau nhưng phải nhất quán.
- **Năng lực/lỗi đo:** phân biệt database constraint và thuộc tính điều hướng ORM.

### PRE06

- **Nhóm năng lực:** C – SQLAlchemy
- **Câu hỏi:** Với <code>Department.employees = relationship("Employee", back_populates="department")</code>, dòng phù hợp trong model <code>Employee</code> là gì?

| Phương án | Nội dung | Ý nghĩa nếu  chọn sai |
|---|---|---|
| A | <code>department = ForeignKey("departments.id")</code> | Nhầm ForeignKey với thuộc tính relationship |
| B | <code>employees = relationship("Department", back_populates="employees")</code> | Sai số ít/số nhiều và sai tên cặp |
| C | <code>department = relationship("Department", back_populates="employees")</code> | **Đúng** |
| D | <code>department = relationship("Department", back_populates="department")</code> | Hai giá trị <code>back_populates</code> không trỏ tới thuộc tính đối diện |

- **Đáp án:** C.
- **Giải thích:** <code>Department.employees</code> ghép cặp với <code>Employee.department</code>.
- **Năng lực/lỗi đo:** cấu hình hai chiều và tên <code>back_populates</code>.

### PRE07

- **Nhóm năng lực:** D – Query relationship
- **Câu hỏi:** Biến <code>employee</code> là một object <code>Employee</code> đã lấy từ database. Thuộc tính nào dùng để lấy hồ sơ của nhân viên theo model đã cấu hình?

| Phương án | Nội dung | Ý nghĩa nếu  chọn sai |
|---|---|---|
| A | <code>employee.profile</code> | **Đúng** |
| B | <code>employee.profiles[0]</code> | Nghĩ phía 1–1 luôn trả về danh sách |
| C | <code>employee.employee_id.profile</code> | Nhầm giá trị khóa với object |
| D | <code>employee.ForeignKey("profile")</code> | Nhầm định nghĩa model với truy cập dữ liệu |

- **Đáp án:** A.
- **Giải thích:** Phía 1–1 được cấu hình <code>uselist=False</code>, nên trả về một object hoặc <code>None</code>.
- **Năng lực/lỗi đo:** truy cập object phía 1–1.

### PRE08

- **Nhóm năng lực:** D – Query relationship
- **Câu hỏi:** Biến <code>project</code> là một object <code>Project</code>. Cách lấy danh sách nhân viên tham gia project này là gì?

| Phương án | Nội dung | Ý nghĩa nếu  chọn sai |
|---|---|---|
| A | <code>project.employee</code> | Dùng số ít cho phía có nhiều |
| B | <code>project.employee_id</code> | Cho rằng bảng Project phải giữ một employee_id |
| C | <code>project.employee_projects.employee</code> | Chưa hiểu thuộc tính trực tiếp qua <code>secondary</code> |
| D | <code>project.employees</code> | **Đúng** |

- **Đáp án:** D.
- **Giải thích:** Quan hệ N–N được cấu hình hai chiều nên <code>project.employees</code> trả về danh sách Employee.
- **Năng lực/lỗi đo:** truy cập collection qua N–N.

---

## 6. Bảng mapping First-test → năng lực

| Nhóm | Năng lực | Câu | Đáp án | Số câu |
|---|---|---|---|---:|
| A | Nhận diện relationship | PRE01, PRE02 | B, C | 2 |
| B | Thiết kế database | PRE03, PRE04 | A, D | 2 |
| C | SQLAlchemy relationship | PRE05, PRE06 | B, C | 2 |
| D | Query relationship | PRE07, PRE08 | A, D | 2 |

### Cách tổng hợp nhanh

Trong Google Sheets nhận từ Form, giả sử:

- Cột A là thời gian nộp.
- Cột B đến I lần lượt là PRE01 đến PRE08.
- Mỗi phương án bắt đầu bằng “A.”, “B.”, “C.” hoặc “D.”.

Công thức tỷ lệ đúng nhóm A:

~~~text
=(COUNTIF(B2:B41,"B.*")+COUNTIF(C2:C41,"C.*"))/(2*COUNTA(A2:A41))
~~~

Làm tương tự:

- Nhóm B: PRE03 đúng A, PRE04 đúng D.
- Nhóm C: PRE05 đúng B, PRE06 đúng C.
- Nhóm D: PRE07 đúng A, PRE08 đúng D.

Định dạng ô kết quả là phần trăm. Nếu số người nộp không đúng 40, dùng chính số dòng có timestamp thay vì cố định mẫu số 40.

---

## 7. Cách đọc kết quả First-test

| Tỷ lệ đúng của nhóm | Kết luận | Cách xử lý |
|---:|---|---|
| ≥ 70% | Tương đối nắm được | Chữa nhanh bằng một câu hỏi kiểm tra |
| 40–69% | Chưa chắc | Demo ngắn và cho điền một phần code |
| < 40% | Điểm vướng nghiêm trọng | Giải thích lại từ nghiệp vụ/database, cho làm ngay và kiểm tra lại |

Không dùng tổng điểm để kết luận “ yếu”. Một  có thể nhận diện đúng nhưng không chuyển được sang code; đây là hai vấn đề khác nhau.

### Tình huống điều chỉnh 1

| A | B | C | D |
|---:|---:|---:|---:|
| 85% | 55% | 35% | 20% |

**Điều chỉnh:** Chỉ nhắc lại định nghĩa quan hệ trong 2 phút. Dành thời gian cho vị trí FK, cặp <code>back_populates</code> và dự đoán object/list trước khi chạy query. Ở bài tổng hợp, cấp sẵn sơ đồ quan hệ nhưng để trống code ORM và query.

### Tình huống điều chỉnh 2

| A | B | C | D |
|---:|---:|---:|---:|
| 35% | 30% | 60% | 55% |

**Điều chỉnh:** Sinh viên có thể nhớ cú pháp nhưng chưa hiểu mô hình. Tạm chưa chiếu code. Dùng thẻ giấy Employee/Department/Project để ghép bản ghi và vẽ FK; đặc biệt cho điền bốn dòng dữ liệu vào bảng trung gian. Sau đó mới quay lại SQLAlchemy.

### Tình huống điều chỉnh 3

| A | B | C | D |
|---:|---:|---:|---:|
| 75% | 80% | 65% | 30% |

**Điều chỉnh:** Chữa model thật nhanh. Tăng thời gian query bằng cách yêu cầu  nói trước kiểu kết quả: một object, danh sách hay <code>None</code>. Bài tổng hợp giữ Phần E đầy đủ, rút ngắn Phần A–B.

### Tình huống điều chỉnh 4

Nếu cả bốn nhóm đều dưới 40%, không cố hoàn thành nhiều code.  dùng mô hình dữ liệu có sẵn, yêu cầu  chỉ hoàn thiện các dòng FK/UNIQUE/bảng trung gian và bốn query bắt buộc. Mục tiêu là đúng bản chất, không phải gõ nhiều.

---

## 8. Nội dung giảng/chữa theo từng nhóm lỗi

### Nhóm A – Không nhận diện được quan hệ

 luôn hỏi hai câu:

1. Một bản ghi A liên kết tối đa bao nhiêu bản ghi B?
2. Một bản ghi B liên kết tối đa bao nhiêu bản ghi A?

| Câu trả lời | Kết luận |
|---|---|
| Một – Một | 1–1 |
| Một – Nhiều | 1–N |
| Nhiều – Nhiều | N–N |

Không cho  đoán quan hệ chỉ bằng tên bảng.

### Nhóm B – Sai thiết kế database

Ba quy tắc bắt buộc viết trên bảng:

1. **1–1 = FK + UNIQUE.**
2. **1–N = FK nằm ở phía N.**
3. **N–N = bảng trung gian có hai FK; mỗi phía nhìn bảng trung gian là 1–N.**

Hoạt động chữa nhanh: đưa ba thẻ “FK”, “UNIQUE”, “BẢNG TRUNG GIAN”;  giơ đúng thẻ theo tình huống.

### Nhóm C – Nhầm SQLAlchemy

Chốt bằng hai tầng:

| Thành phần | Nằm ở đâu? | Làm gì? |
|---|---|---|
| <code>ForeignKey</code> | Cột/table, database | Bảo đảm giá trị tham chiếu tới bản ghi hợp lệ |
| <code>relationship()</code> | ORM/Python | Cho phép đi từ object này sang object liên quan |

> <code>relationship()</code> không tự tạo Foreign Key trong database.

> Có Foreign Key giúp database biết liên kết cột; muốn truy cập object thuận tiện và hai chiều cần cấu hình ORM phù hợp.

### Nhóm D – Không query được

Cho  đọc thuộc tính như câu tiếng Việt:

- <code>employee.profile</code>: hồ sơ của một nhân viên → một object hoặc <code>None</code>.
- <code>employee.department</code>: phòng ban của một nhân viên → một object hoặc <code>None</code>.
- <code>department.employees</code>: các nhân viên của một phòng ban → danh sách.
- <code>employee.projects</code>: các dự án một nhân viên đang tham gia → danh sách.

Trước khi chạy code,  phải dự đoán kết quả là **một object** hay **danh sách**.

---

## 9. Demo 1–1: Employee – EmployeeProfile

### 9.1. Đi từ nghiệp vụ tới database

**Nghiệp vụ:** Một Employee có tối đa một EmployeeProfile; mỗi EmployeeProfile bắt buộc thuộc về một Employee.

 hỏi theo thứ tự:

1. Đây là quan hệ gì? → 1–1.
2. Có thể đặt <code>employee_id</code> trong <code>employee_profiles</code> không? → Có.
3. Chỉ có FK đã đủ chưa? → Chưa; nhiều profile vẫn có thể cùng trỏ tới một employee.
4. Cần thêm gì? → UNIQUE.

Thiết kế:

| employees | employee_profiles |
|---|---|
| id (PK) | id (PK) |
| name | phone |
|  | employee_id (FK, UNIQUE, NOT NULL) |

Quy tắc chốt:

> 1–1 = Foreign Key + UNIQUE. <code>uselist=False</code> chỉ điều chỉnh cách ORM trả kết quả; nó không thay thế UNIQUE ở database.

### 9.2. SQLAlchemy model

~~~python
class Employee(Base):
    __tablename__ = "employees"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    profile = relationship(
        "EmployeeProfile",
        back_populates="employee",
        uselist=False
    )


class EmployeeProfile(Base):
    __tablename__ = "employee_profiles"

    id = Column(Integer, primary_key=True)
    phone = Column(String(20))
    employee_id = Column(
        Integer,
        ForeignKey("employees.id"),
        unique=True,
        nullable=False
    )

    employee = relationship("Employee", back_populates="profile")
~~~

### 9.3. Demo truy cập

~~~python
employee = db.get(Employee, 1)

if employee is None:
    print("Không tìm thấy nhân viên")
elif employee.profile is None:
    print("Nhân viên chưa có hồ sơ")
else:
    print(employee.profile.phone)
~~~

**Dữ liệu dự kiến:** nếu Employee 1 có profile với số điện thoại <code>0901234567</code>, kết quả là <code>0901234567</code>. Đây là một object, không phải danh sách.

### 9.4. Bài làm ngay trong 1 phút

Nghiệp vụ: Một <code>User</code> có tối đa một <code>UserProfile</code>.

Điền chỗ trống:

~~~python
user_id = Column(
    Integer,
    ForeignKey("__________.id"),
    __________=True
)
~~~

**Đáp án:** <code>users</code>, <code>unique</code>.

---

## 10. Demo 1–N: Department – Employee

### 10.1. Đi từ nghiệp vụ tới database

**Nghiệp vụ:** Một Department có nhiều Employee; mỗi Employee thuộc tối đa một Department.

 dùng hai phòng ban và bốn nhân viên. Hỏi: “Muốn biết mỗi nhân viên thuộc phòng ban nào, cột nào phải lặp lại được?” Câu trả lời là <code>employees.department_id</code>.

| departments | employees |
|---|---|
| id (PK) | id (PK) |
| name | name |
|  | department_id (FK, không UNIQUE) |

Quy tắc chốt:

> Trong 1–N, FK nằm ở phía N. Không đặt UNIQUE vì nhiều Employee cần được phép có cùng department_id.

### 10.2. SQLAlchemy model

~~~python
class Department(Base):
    __tablename__ = "departments"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    employees = relationship("Employee", back_populates="department")


class Employee(Base):
    __tablename__ = "employees"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    department_id = Column(Integer, ForeignKey("departments.id"))

    department = relationship("Department", back_populates="employees")
~~~

Tên thuộc tính <code>department</code> dùng số ít vì mỗi Employee chỉ thuộc tối đa một Department.

### 10.3. Hai chiều truy cập

~~~python
employee = db.get(Employee, 1)
print(employee.department)          # Một Department hoặc None

department = db.get(Department, 1)
print(department.employees)      # Danh sách Employee
~~~

### 10.4. Bài làm ngay trong 1 phút

Một <code>Department</code> có nhiều <code>Employee</code>. Chọn thiết kế đúng:

- <code>departments.employee_id</code>
- <code>employees.department_id</code>

**Đáp án:** <code>employees.department_id</code>, vì Employee là phía N.

---

## 11. Demo N–N: Employee – Project

### 11.1. Bắt đầu bằng dữ liệu, chưa viết code

Nghiệp vụ:

- Employee 1 tham gia Project 1 và Project 2.
- Employee 2 tham gia Project 1 và Project 3.

Không thể đặt một <code>project_id</code> duy nhất trong Employee vì Employee 1 tham gia hai project. Cũng không thể đặt một <code>employee_id</code> duy nhất trong Project vì Project 1 có nhiều employee.

| employee_id | project_id |
|---:|---:|
| 1 | 1 |
| 1 | 2 |
| 2 | 1 |
| 2 | 3 |

Bảng trên là <code>employee_projects</code>. Mỗi dòng biểu diễn một lần phân công nhân viên vào dự án.

### 11.2. Phân rã quan hệ

~~~mermaid
erDiagram
    DEPARTMENT ||--o{ EMPLOYEE : "quản lý"
    EMPLOYEE ||--o| EMPLOYEE_PROFILE : "có hồ sơ"
    EMPLOYEE ||--o{ EMPLOYEE_PROJECT : "được phân công"
    PROJECT ||--o{ EMPLOYEE_PROJECT : "có thành viên"
~~~

Nhìn theo database:

- Một Employee có nhiều dòng EmployeeProject.
- Một Project có nhiều dòng EmployeeProject.
- Vì vậy N–N được triển khai bằng hai quan hệ 1–N.

### 11.3. Association Table trong SQLAlchemy

~~~python
employee_projects = Table(
    "employee_projects",
    Base.metadata,
    Column(
        "employee_id",
        ForeignKey("employees.id"),
        primary_key=True
    ),
    Column(
        "project_id",
        ForeignKey("projects.id"),
        primary_key=True
    )
)


class Employee(Base):
    __tablename__ = "employees"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    projects = relationship(
        "Project",
        secondary=employee_projects,
        back_populates="employees"
    )


class Project(Base):
    __tablename__ = "projects"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    employees = relationship(
        "Employee",
        secondary=employee_projects,
        back_populates="projects"
    )
~~~

Hai cột cùng là primary key giúp ngăn cặp <code>(employee_id, project_id)</code> bị lặp. Trong hệ thống production, bảng phân công thường có thêm <code>role</code>, <code>allocation_percent</code> hoặc <code>assigned_at</code>. Buổi phụ đạo dùng phiên bản tối thiểu để tập trung vào relationship; khi có các cột nghiệp vụ đó, bảng trung gian sẽ được khai báo thành model riêng.

### 11.4. Demo truy cập

~~~python
employee = db.get(Employee, 1)
for project in employee.projects:
    print(project.name)

project = db.get(Project, 1)
for employee in project.employees:
    print(employee.name)
~~~

**Dữ liệu dự kiến:**

- <code>employee.projects</code> của Employee 1 trả về Project 1 và Project 2.
- <code>project.employees</code> của Project 1 trả về Employee 1 và Employee 2.

### 11.5. Kiểm tra hiểu nhanh

 hỏi: “Nếu bỏ bảng <code>employee_projects</code>, chúng ta lưu Employee 1 tham gia đồng thời hai project ở đâu?” Sinh viên phải chỉ ra rằng một cột đơn không lưu đúng nghiệp vụ và không nên lưu danh sách ID dạng chuỗi.

---

## 12. SQLAlchemy model hoàn chỉnh

### 12.1. Cấu trúc demo tối giản

~~~text
app/
├── main.py
├── database.py
├── models.py
└── seed_demo.py
~~~

Chạy lệnh từ thư mục project:

~~~bash
uvicorn app.main:app --reload
~~~

Nếu import module lỗi, kiểm tra đang đứng ở thư mục chứa <code>app</code> và dùng dấu chấm trong module path (<code>app.main:app</code>), không dùng <code>app/main:app</code>.

### 12.2. File app/database.py

~~~python
from sqlalchemy import create_engine
from sqlalchemy.orm import declarative_base, sessionmaker

DATABASE_URL = (
    "mysql+pymysql://root:password"
    "@localhost:3306/enterprise_relationship_demo"
)

engine = create_engine(DATABASE_URL, pool_pre_ping=True)

SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine
)

Base = declarative_base()


def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
~~~

Cài thư viện và tạo trước database rỗng:

~~~bash
pip install fastapi uvicorn sqlalchemy pymysql
~~~

~~~sql
CREATE DATABASE enterprise_relationship_demo
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
~~~

Thay <code>root:password</code> bằng tài khoản MySQL trên máy demo.

### 12.3. File app/models.py

~~~python
from sqlalchemy import Column, ForeignKey, Integer, String, Table
from sqlalchemy.orm import relationship

from app.database import Base


employee_projects = Table(
    "employee_projects",
    Base.metadata,
    Column(
        "employee_id",
        ForeignKey("employees.id"),
        primary_key=True
    ),
    Column(
        "project_id",
        ForeignKey("projects.id"),
        primary_key=True
    )
)


class Department(Base):
    __tablename__ = "departments"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    employees = relationship("Employee", back_populates="department")


class Employee(Base):
    __tablename__ = "employees"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    department_id = Column(Integer, ForeignKey("departments.id"))

    department = relationship("Department", back_populates="employees")
    profile = relationship(
        "EmployeeProfile",
        back_populates="employee",
        uselist=False
    )
    projects = relationship(
        "Project",
        secondary=employee_projects,
        back_populates="employees"
    )


class EmployeeProfile(Base):
    __tablename__ = "employee_profiles"

    id = Column(Integer, primary_key=True)
    phone = Column(String(20))
    address = Column(String(255))
    employee_id = Column(
        Integer,
        ForeignKey("employees.id"),
        unique=True,
        nullable=False
    )

    employee = relationship("Employee", back_populates="profile")


class Project(Base):
    __tablename__ = "projects"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    employees = relationship(
        "Employee",
        secondary=employee_projects,
        back_populates="projects"
    )
~~~

### 12.4. File app/seed_demo.py

Chạy một lần bằng <code>python -m app.seed_demo</code>.

~~~python
from app.database import Base, SessionLocal, engine
from app.models import Department, Project, Employee, EmployeeProfile


Base.metadata.create_all(bind=engine)
db = SessionLocal()

try:
    if db.query(Employee).count() == 0:
        engineering = Department(name="Khối Công nghệ")
        erp_project = Project(name="Triển khai ERP nội bộ")
        portal_project = Project(name="Cổng khách hàng B2B")
        warehouse_project = Project(name="Số hóa vận hành kho")

        lan = Employee(name="Nguyễn Thị Lan", department=engineering)
        minh = Employee(name="Trần Quốc Minh", department=engineering)

        lan.profile = EmployeeProfile(
            phone="0901234567",
            address="Văn phòng Hà Nội"
        )
        minh.profile = EmployeeProfile(
            phone="0912345678",
            address="Chi nhánh Đà Nẵng"
        )

        lan.projects.extend([erp_project, portal_project])
        minh.projects.extend([erp_project, warehouse_project])

        db.add_all([engineering, lan, minh])
        db.commit()
        print("Đã tạo dữ liệu demo")
    else:
        print("Dữ liệu demo đã tồn tại")
finally:
    db.close()
~~~

### 12.5. File app/main.py

~~~python
from fastapi import Depends, FastAPI, HTTPException
from sqlalchemy.orm import Session

from app.database import Base, engine, get_db
from app.models import Department, Project, Employee


Base.metadata.create_all(bind=engine)
app = FastAPI(title="Enterprise Relationship Demo")


def require_object(value, message):
    if value is None:
        raise HTTPException(status_code=404, detail=message)
    return value


@app.get("/employees/{employee_id}/profile")
def get_employee_profile(
    employee_id: int,
    db: Session = Depends(get_db)
):
    employee = require_object(
        db.get(Employee, employee_id),
        "Không tìm thấy nhân viên"
    )
    if employee.profile is None:
        return {"profile": None}
    return {
        "id": employee.profile.id,
        "phone": employee.profile.phone,
        "address": employee.profile.address
    }


@app.get("/employees/{employee_id}/department")
def get_employee_department(
    employee_id: int,
    db: Session = Depends(get_db)
):
    employee = require_object(
        db.get(Employee, employee_id),
        "Không tìm thấy nhân viên"
    )
    if employee.department is None:
        return {"department": None}
    return {
        "id": employee.department.id,
        "name": employee.department.name
    }


@app.get("/departments/{department_id}/employees")
def get_employees_of_department(
    department_id: int,
    db: Session = Depends(get_db)
):
    department = require_object(
        db.get(Department, department_id),
        "Không tìm thấy phòng ban"
    )
    return [
        {"id": employee.id, "name": employee.name}
        for employee in department.employees
    ]


@app.get("/employees/{employee_id}/projects")
def get_projects_of_employee(
    employee_id: int,
    db: Session = Depends(get_db)
):
    employee = require_object(
        db.get(Employee, employee_id),
        "Không tìm thấy nhân viên"
    )
    return [
        {"id": project.id, "name": project.name}
        for project in employee.projects
    ]


@app.get("/projects/{project_id}/employees")
def get_employees_of_project(
    project_id: int,
    db: Session = Depends(get_db)
):
    project = require_object(
        db.get(Project, project_id),
        "Không tìm thấy dự án"
    )
    return [
        {"id": employee.id, "name": employee.name}
        for employee in project.employees
    ]
~~~

Mở <code>http://127.0.0.1:8000/docs</code> để chạy thử. Bộ code cố ý để model trong một file nhằm tập trung vào relationship và tránh lỗi import chéo trong buổi phụ đạo.

---

## 13. Query qua relationship

Các ví dụ dưới đây sử dụng <code>db.get(Model, id)</code> để tìm theo primary key. Phải kiểm tra object có tồn tại trước khi đọc relationship.

### 13.1. Lấy profile của một Employee – 1–1

~~~python
employee = db.get(Employee, 1)
profile = employee.profile if employee else None
~~~

- **Kiểu kết quả:** một <code>EmployeeProfile</code> hoặc <code>None</code>.
- **Dữ liệu dự kiến:** profile có phone <code>0901234567</code>.

### 13.2. Lấy Department của một Employee – phía N về phía 1

~~~python
employee = db.get(Employee, 1)
department = employee.department if employee else None
~~~

- **Kiểu kết quả:** một <code>Department</code> hoặc <code>None</code>.
- **Dữ liệu dự kiến:** phòng ban <code>Khối Công nghệ</code>.

### 13.3. Lấy danh sách Employee của một Department – phía 1 về phía N

~~~python
department = db.get(Department, 1)
employees = department.employees if department else []
~~~

- **Kiểu kết quả:** danh sách Employee.
- **Dữ liệu dự kiến:** Nguyễn Thị Lan và Trần Quốc Minh.

### 13.4. Lấy danh sách Project của một Employee – N–N

~~~python
employee = db.get(Employee, 1)
projects = employee.projects if employee else []
~~~

- **Kiểu kết quả:** danh sách Project.
- **Dữ liệu dự kiến:** Triển khai ERP nội bộ và Cổng khách hàng B2B.

### 13.5. Lấy danh sách Employee tham gia một Project – N–N

~~~python
project = db.get(Project, 1)
employees = project.employees if project else []
~~~

- **Kiểu kết quả:** danh sách Employee.
- **Dữ liệu dự kiến:** Nguyễn Thị Lan và Trần Quốc Minh.

### Bảng chốt object/list

| Biểu thức | Kết quả |
|---|---|
| <code>employee.profile</code> | Một object hoặc None |
| <code>employee.department</code> | Một object hoặc None |
| <code>department.employees</code> | Danh sách |
| <code>employee.projects</code> | Danh sách |
| <code>project.employees</code> | Danh sách |

---

## 14. Bài thực hành tổng hợp

### 14.1. Cách tổ chức

- Sinh viên làm theo cặp để một người đọc nghiệp vụ, một người kiểm tra code; cuối bài mỗi người tự làm Final-test.
- Mỗi cặp dùng một thẻ trạng thái:
  - Xanh: đang làm được.
  - Vàng: chưa chắc thiết kế.
  - Đỏ: code không chạy hoặc không biết bắt đầu.
-  ưu tiên chữa thẻ đỏ theo nhóm lỗi, không kiểm tra lần lượt 20 máy.
- Thời gian mục tiêu: 12 phút làm và 4 phút chữa chung.

### 14.2. Đề bài 

Xây dựng mô hình cho **hệ thống vận hành trung tâm hỗ trợ khách hàng đa kênh** của doanh nghiệp:

1. Một <code>SupportAgent</code> có tối đa một <code>AgentProfile</code> lưu thông tin nghiệp vụ; mỗi profile thuộc đúng một agent.
2. Một <code>SupportTeam</code> có nhiều <code>SupportAgent</code>; mỗi agent thuộc tối đa một team vận hành.
3. Một <code>SupportAgent</code> có thể được gán nhiều <code>Skill</code>, ví dụ “Thanh toán”, “Khiếu nại giao hàng”.
4. Một <code>Skill</code> có thể được gán cho nhiều agent.

#### Phần A – Nhận diện relationship

Điền loại quan hệ:

| Hai entity | Loại quan hệ |
|---|---|
| SupportAgent – AgentProfile | ... |
| SupportTeam – SupportAgent | ... |
| SupportAgent – Skill | ... |

#### Phần B – Thiết kế database

Hoàn thành:

1. Gạch dưới PK của từng bảng.
2. Đặt <code>agent_id</code>, <code>team_id</code> đúng bảng.
3. Đánh dấu FK nào cần UNIQUE.
4. Đặt tên bảng trung gian cho SupportAgent–Skill.
5. Chọn khóa chính cho bảng trung gian.

Khung:

~~~text
teams(id, name)
agents(id, name, __________)
agent_profiles(id, phone, __________)
skills(id, name)
________________(agent_id, skill_id)
~~~

#### Phần C – SQLAlchemy model

Điền các chỗ <code>TODO</code> quan trọng:

~~~python
agent_skills = Table(
    "agent_skills",
    Base.metadata,
    Column(
        "agent_id",
        ForeignKey("agents.id"),
        primary_key=True
    ),
    Column(
        "skill_id",
        ForeignKey("skills.id"),
        primary_key=True
    )
)


class SupportTeam(Base):
    __tablename__ = "teams"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    agents = relationship(
        "SupportAgent",
        back_populates="team"       # TODO 1
    )


class SupportAgent(Base):
    __tablename__ = "agents"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    team_id = Column(
        Integer,
        ForeignKey("teams.id")       # TODO 2
    )

    team = relationship(
        "SupportTeam",
        back_populates="agents"       # TODO 3
    )
    profile = relationship(
        "AgentProfile",
        back_populates="agent",
        uselist=__________                # TODO 4
    )
    skills = relationship(
        "Skill",
        secondary=agent_skills,
        back_populates="__________"       # TODO 5
    )


class AgentProfile(Base):
    __tablename__ = "agent_profiles"

    id = Column(Integer, primary_key=True)
    phone = Column(String(20))
    agent_id = Column(
        Integer,
        ForeignKey("agents.id"),
        unique=True,               # TODO 6
        nullable=False
    )

    agent = relationship(
        "SupportAgent",
        back_populates="profile"       # TODO 7
    )


class Skill(Base):
    __tablename__ = "skills"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    agents = relationship(
        "SupportAgent",
        secondary=agent_skills,
        back_populates="__________"       # TODO 8
    )
~~~

#### Phần D – Kiểm tra Relationship

Với mỗi cặp <code>back_populates</code>, nối hai thuộc tính đối diện:

- <code>SupportTeam.agents</code> ↔ ...
- <code>SupportAgent.profile</code> ↔ ...
- <code>SupportAgent.skills</code> ↔ ...

#### Phần E – Query

Viết hoặc điền bốn truy cập:

~~~python
agent = db.get(SupportAgent, 1)

profile = agent.__________
team = agent.__________

team_1 = db.get(SupportTeam, 1)
agents = team_1.__________

skills = agent.__________
~~~

Trước mỗi dòng, ghi **ONE**, **LIST** hoặc **NONE POSSIBLE** để dự đoán kiểu kết quả.

### 14.3. Mức hoàn thành

- **Tối thiểu bắt buộc:** đúng Phần A, B, bốn dòng query ở E.
- **Đạt mục tiêu:** thêm đúng mọi FK, UNIQUE và <code>relationship()</code>.
- **Nếu còn thời gian:** thêm dữ liệu mẫu và thử <code>skill.agents</code>.

---

## 15. Đáp án bài thực hành

### 15.1. Phần A

| Hai entity | Đáp án |
|---|---|
| SupportAgent – AgentProfile | 1–1 |
| SupportTeam – SupportAgent | 1–N |
| SupportAgent – Skill | N–N |

### 15.2. Phần B

~~~text
teams(id PK, name)
agents(id PK, name, team_id FK -> teams.id)
agent_profiles(
    id PK,
    phone,
    agent_id FK -> agents.id UNIQUE NOT NULL
)
skills(id PK, name)
agent_skills(
    agent_id PK/FK -> agents.id,
    skill_id PK/FK -> skills.id
)
~~~

Giải thích:

- <code>agents.team_id</code> nằm ở phía N.
- <code>agent_profiles.agent_id</code> có UNIQUE để một SupportAgent không có nhiều profile.
- <code>agent_skills</code> lưu từng cặp agent–skill; composite primary key ngăn một kỹ năng bị gán lặp cho cùng agent.

### 15.3. Phần C và D – Model hoàn chỉnh

~~~python
from sqlalchemy import Column, ForeignKey, Integer, String, Table
from sqlalchemy.orm import relationship

from app.database import Base


agent_skills = Table(
    "agent_skills",
    Base.metadata,
    Column(
        "agent_id",
        ForeignKey("agents.id"),
        primary_key=True
    ),
    Column(
        "skill_id",
        ForeignKey("skills.id"),
        primary_key=True
    )
)


class SupportTeam(Base):
    __tablename__ = "teams"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    agents = relationship("SupportAgent", back_populates="team")


class SupportAgent(Base):
    __tablename__ = "agents"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    team_id = Column(
        Integer,
        ForeignKey("teams.id")
    )

    team = relationship(
        "SupportTeam",
        back_populates="agents"
    )
    profile = relationship(
        "AgentProfile",
        back_populates="agent",
        uselist=False
    )
    skills = relationship(
        "Skill",
        secondary=agent_skills,
        back_populates="agents"
    )


class AgentProfile(Base):
    __tablename__ = "agent_profiles"

    id = Column(Integer, primary_key=True)
    phone = Column(String(20))
    agent_id = Column(
        Integer,
        ForeignKey("agents.id"),
        unique=True,
        nullable=False
    )

    agent = relationship("SupportAgent", back_populates="profile")


class Skill(Base):
    __tablename__ = "skills"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)

    agents = relationship(
        "SupportAgent",
        secondary=agent_skills,
        back_populates="skills"
    )
~~~

Các cặp:

- <code>SupportTeam.agents</code> ↔ <code>SupportAgent.team</code>.
- <code>SupportAgent.profile</code> ↔ <code>AgentProfile.agent</code>.
- <code>SupportAgent.skills</code> ↔ <code>Skill.agents</code>.

### 15.4. Phần E – Query

~~~python
agent = db.get(SupportAgent, 1)

# ONE hoặc NONE POSSIBLE
profile = agent.profile

# ONE hoặc NONE POSSIBLE
team = agent.team

team_1 = db.get(SupportTeam, 1)

# LIST
agents = team_1.agents

# LIST
skills = agent.skills
~~~

Query chiều còn lại nếu còn thời gian:

~~~python
skill = db.get(Skill, 1)
agents_of_skill = skill.agents
~~~

### 15.5. Dữ liệu mẫu để kiểm tra

~~~python
enterprise_support = SupportTeam(
    name="Đội hỗ trợ khách hàng doanh nghiệp"
)
payment_skill = Skill(name="Xử lý sự cố thanh toán")
delivery_skill = Skill(name="Khiếu nại giao hàng")

agent = SupportAgent(
    name="Nguyễn Minh An",
    team=enterprise_support
)
agent.profile = AgentProfile(phone="0909000001")
agent.skills.extend([payment_skill, delivery_skill])

db.add(agent)
db.commit()
~~~

Dự kiến:

- <code>agent.profile.phone</code> → <code>0909000001</code>.
- <code>agent.team.name</code> → <code>Đội hỗ trợ khách hàng doanh nghiệp</code>.
- <code>enterprise_support.agents</code> → danh sách có Nguyễn Minh An.
- <code>agent.skills</code> → Xử lý sự cố thanh toán và Khiếu nại giao hàng.

---

## 16. Các lỗi thường gặp và cách chữa

| Lỗi quan sát được | Dấu hiệu/code sai | Cách chữa nhanh cho cả lớp | Câu chốt |
|---|---|---|---|
| FK đặt sai phía trong 1–N | <code>departments.employee_id</code> | Vẽ 1 department và 3 employee; hỏi một cột có chứa được 3 ID không | FK ở phía N |
| Quên UNIQUE trong 1–1 | <code>employee_id</code> chỉ là FK | Cho hai profile cùng employee_id và hỏi database có chặn không | 1–1 = FK + UNIQUE |
| Đặt UNIQUE trong 1–N | <code>employees.department_id unique=True</code> | Cho hai employee cùng department; chỉ ra bản ghi thứ hai sẽ lỗi | Phía N phải cho phép lặp FK |
| Không có bảng trung gian N–N | <code>employees.project_id</code> | Yêu cầu lưu một employee tham gia hai project bằng một cột | N–N cần từng cặp liên kết |
| Bảng trung gian sai | Chỉ có một FK hoặc cho phép cặp trùng | Tô màu hai cột FK và ghép thành composite PK | Hai FK, ngăn cặp trùng |
| Nhầm ForeignKey với relationship | Chỉ viết <code>relationship()</code> | Mở schema MySQL, chỉ ra không có cột kết nối | relationship không tự tạo FK |
| <code>back_populates</code> không khớp | Hai tên không trỏ tới nhau | Viết hai đầu trên cùng một dòng và nối mũi tên | Giá trị phải là tên thuộc tính ở model đối diện |
| Nhầm object và danh sách | Dùng <code>employee.department[0]</code> | Cho đọc tên số ít/số nhiều và dự đoán ONE/LIST | Phía một trả object; phía nhiều trả list |
| Truy cập trên object None | <code>employee.profile.phone</code> khi chưa có profile | Kiểm tra <code>employee</code> và <code>employee.profile</code> trước | Relationship có thể chưa có dữ liệu |
| Import/model chưa được nạp | Bảng không được tạo | Đảm bảo module model được import trước <code>create_all</code> | Metadata chỉ tạo các model đã được nạp |

### Quy trình chữa 4 phút ở phút 101–105

1. Chiếu một code sai có đủ bốn lỗi: sai phía FK, thiếu UNIQUE, thiếu bảng trung gian, sai <code>back_populates</code>.
2. Sinh viên giơ 1/2/3/4 tương ứng lỗi mình gặp.
3.  sửa từng dòng trên màn hình, không mở code của từng cặp.
4. Mỗi cặp tự đánh dấu: đã sửa database, ORM hay query.

---

## 17. Final-test

### Hướng dẫn

- Google Form Quiz, 6 câu, mỗi câu 1 điểm.
- Làm cá nhân trong 8 phút.
- Không dùng lại nguyên nghiệp vụ của First-test.
- Không công bố đáp án trước khi tất cả  nộp.

### Final01

- **Nhóm năng lực:** A – Nhận diện relationship
- **Câu hỏi:** Trong cổng bán hàng đối tác, mỗi <code>Merchant</code> có tối đa một <code>MerchantProfile</code>; mỗi profile thuộc một Merchant. Đây là quan hệ gì?

| Phương án | Nội dung | Lỗi phản ánh |
|---|---|---|
| A | 1–N | Không chú ý giới hạn ở phía profile |
| B | 1–1 | **Đúng** |
| C | N–N | Áp dụng bảng trung gian cho mọi quan hệ |
| D | Không cần relationship | Chưa chuyển được nghiệp vụ thành mô hình |

- **Đáp án:** B.
- **Giải thích:** Mỗi phía chỉ liên kết tối đa một bản ghi.

### Final02

- **Nhóm năng lực:** B – Thiết kế database
- **Câu hỏi:** Một <code>Customer</code> có nhiều <code>Order</code>; mỗi Order thuộc một Customer. Thiết kế nào đúng?

| Phương án | Nội dung | Lỗi phản ánh |
|---|---|---|
| A | <code>customers.order_id</code> là FK | Đặt FK ở phía 1 |
| B | Tạo bảng trung gian <code>customer_orders</code> | Nhầm 1–N thành N–N |
| C | <code>orders.customer_id</code> là FK và UNIQUE | Vô tình biến thành 1–1 |
| D | <code>orders.customer_id</code> là FK, không UNIQUE | **Đúng** |

- **Đáp án:** D.
- **Giải thích:** Order là phía N; nhiều order được phép có cùng customer_id.

### Final03

- **Nhóm năng lực:** B – Thiết kế database
- **Câu hỏi:** Product và Supplier có quan hệ N–N. Bảng <code>product_suppliers</code> nên được thiết kế thế nào ở mức cơ bản?

| Phương án | Nội dung | Lỗi phản ánh |
|---|---|---|
| A | Có <code>product_id</code> và <code>supplier_id</code>, cả hai là FK và cùng tạo khóa chính ghép | **Đúng** |
| B | Chỉ có <code>product_id</code> | Thiếu phía Supplier |
| C | Lưu danh sách supplier ID vào một cột chuỗi của Product | Vi phạm thiết kế quan hệ, khó ràng buộc/query |
| D | Đặt <code>supplier_id UNIQUE</code> trong Product | Biến quan hệ thành giới hạn sai |

- **Đáp án:** A.
- **Giải thích:** Mỗi dòng lưu một cặp product–supplier; khóa chính ghép ngăn cặp bị lặp.

### Final04

- **Nhóm năng lực:** C – SQLAlchemy
- **Câu hỏi:** Model có <code>Order.customer = relationship("Customer")</code> nhưng không có cột nào khai báo <code>ForeignKey("customers.id")</code>. Nhận xét nào đúng nhất?

| Phương án | Nội dung | Lỗi phản ánh |
|---|---|---|
| A | MySQL vẫn tự tạo customer_id | Nghĩ ORM tự suy ra cột |
| B | <code>relationship()</code> chính là ForeignKey | Nhầm hai khái niệm |
| C | Thiếu đường liên kết ở database; cần thêm cột FK phù hợp | **Đúng** |
| D | Chỉ cần thêm UNIQUE vào <code>relationship()</code> | Đặt constraint sai nơi |

- **Đáp án:** C.
- **Giải thích:** <code>relationship()</code> không tự tạo cột hoặc ràng buộc FK.

### Final05

- **Nhóm năng lực:** D – Query relationship
- **Câu hỏi:** Đã cấu hình <code>Order.customer</code> và biến <code>order</code> tồn tại. Cách lấy tên customer của order, có xử lý trường hợp dữ liệu liên kết chưa tồn tại, là gì?

| Phương án | Nội dung | Lỗi phản ánh |
|---|---|---|
| A | <code>order.customer_id.name</code> | Nhầm ID với object |
| B | <code>order.customer.name if order.customer else None</code> | **Đúng** |
| C | <code>order.customers[0].name</code> | Nhầm phía một thành danh sách |
| D | <code>Customer.order.name</code> | Truy cập từ class thay vì object cụ thể |

- **Đáp án:** B.
- **Giải thích:** <code>order.customer</code> là một object hoặc None.

### Final06

- **Nhóm năng lực:** D – Query relationship
- **Câu hỏi:** Quan hệ Product–Supplier đã cấu hình hai chiều bằng <code>secondary</code>. Với object <code>supplier</code>, cách lấy danh sách product đang cung ứng là gì?

| Phương án | Nội dung | Lỗi phản ánh |
|---|---|---|
| A | <code>supplier.product_id</code> | Nghĩ N–N có một FK trực tiếp |
| B | <code>supplier.product</code> | Dùng số ít cho collection |
| C | <code>supplier.ForeignKey("products.id")</code> | Nhầm định nghĩa schema với query |
| D | <code>supplier.products</code> | **Đúng** |

- **Đáp án:** D.
- **Giải thích:** Phía Supplier trả về collection Product.

---

## 18. Mapping Final-test → năng lực

| Nhóm | Năng lực | Câu | Đáp án | Số câu |
|---|---|---|---|---:|
| A | Nhận diện relationship | Final01 | B | 1 |
| B | Thiết kế database | Final02, Final03 | D, A | 2 |
| C | SQLAlchemy relationship | Final04 | C | 1 |
| D | Query relationship | Final05, Final06 | B, D | 2 |

Vì nhóm A và C chỉ có một câu trong Final-test, kết luận cho hai nhóm này nên kết hợp thêm quan sát ở bài thực hành, không dựa duy nhất vào một câu.

---

## 19. Cách so sánh First-test và Final-test

### 19.1. Bảng tổng hợp

| Năng lực | First-test | Final-test | Thay đổi |
|---|---:|---:|---:|
| Nhận diện relationship | ...% | ...% | ... điểm phần trăm |
| Thiết kế database | ...% | ...% | ... điểm phần trăm |
| SQLAlchemy | ...% | ...% | ... điểm phần trăm |
| Query | ...% | ...% | ... điểm phần trăm |

Công thức:

~~~text
Thay đổi = Tỷ lệ đúng Final-test - Tỷ lệ đúng First-test
~~~

Ghi là **điểm phần trăm**, không ghi phần trăm tăng tương đối. Ví dụ từ 25% lên 65% là tăng 40 điểm phần trăm.

### 19.2. Cách kết luận

| Kết quả Final-test | Kết luận |
|---:|---|
| ≥ 80% | Đạt tốt ở năng lực này |
| 60–79% | Đạt mức tối thiểu, cần luyện thêm |
| < 60% | Chưa đạt, cần hoạt động phụ đạo tiếp theo |

Cần xem đồng thời:

1. Mức sau học.
2. Mức thay đổi.
3. Loại lỗi còn lặp lại.
4. Kết quả bài thực hành.

### 19.3. Ví dụ diễn giải

- Query tăng từ 25% lên 65% nhưng vẫn thấp nhất → buổi phụ đạo tiếp theo tập trung CRUD/query qua relationship.
- Nhận diện lên 90% nhưng SQLAlchemy chỉ đạt 55% →  hiểu mô hình nhưng chưa chuyển thành code tốt; cần bài điền model và sửa lỗi <code>back_populates</code>.
- Database tăng từ 30% lên 75% → cách dạy bằng FK/UNIQUE/bảng trung gian có hiệu quả; tiếp tục củng cố bằng project.
- Một nhóm tăng ít nhưng Final-test đã 85% → First-test ban đầu đã cao; không cần coi là thất bại.
- Tăng mạnh nhưng Final-test vẫn dưới 60% → có tiến bộ nhưng chưa đạt; cần phụ đạo tiếp, không chuyển ngay sang nội dung nâng cao.

### 19.4. Theo dõi  cần hỗ trợ thêm

Không công khai xếp hạng.  lọc riêng :

- Sai cả câu database và query trong Final-test.
- Hoàn thành First-test nhưng bỏ nhiều câu Final-test.
- Bài thực hành vẫn dùng FK sai phía hoặc không có bảng trung gian.

Giao một bài sửa model ngắn trước khi các em tiếp tục phần JWT/bcrypt, vì authentication cuối kỳ vẫn cần liên kết User và các bảng nghiệp vụ đúng.

---

## 20. Checklist cho  trước buổi học

### Form và dữ liệu

- [ ] Google Form First-test đã tạo đủ PRE01–PRE08.
- [ ] Google Form Final-test đã tạo đủ Final01–Final06.
- [ ] Đáp án và điểm tự động đã thiết lập.
- [ ] Không bật hiện đáp án ngay trong lúc các  khác còn làm.
- [ ] Form đã liên kết Google Sheets.
- [ ] Bảng tính đã có công thức tỷ lệ đúng theo bốn nhóm.
- [ ] Đã thử nộp một phản hồi mẫu và kiểm tra công thức.
- [ ] Có mã QR hoặc link ngắn cho cả hai Form.
- [ ] Có bản giấy/thẻ A–D nếu mạng lỗi.

### Môi trường demo

- [ ] MySQL đang chạy.
- [ ] Database <code>enterprise_relationship_demo</code> đã tạo.
- [ ] Chuỗi kết nối đúng với máy demo.
- [ ] Đã cài FastAPI, Uvicorn, SQLAlchemy và PyMySQL.
- [ ] Chạy được <code>python -m app.seed_demo</code>.
- [ ] Chạy được <code>uvicorn app.main:app --reload</code>.
- [ ] Đã thử đủ năm endpoint trong Swagger UI.
- [ ] Database ở trạng thái dữ liệu mẫu đã biết.
- [ ] Có bản code hoàn chỉnh để chuyển sang nếu gõ trực tiếp bị lỗi.

### Tổ chức lớp

- [ ] Máy chiếu hoạt động.
- [ ] Sinh viên biết làm theo cặp.
- [ ] Có thẻ xanh/vàng/đỏ hoặc cơ chế giơ tay tương đương.
- [ ] Timeline được hiển thị; Final-test bắt đầu ở phút 105.
- [ ] Đã xác định nội dung COULD HAVE để cắt nếu trễ.
- [ ] Có ảnh chụp sơ đồ và code mẫu để dùng khi mạng/MySQL lỗi.

---

## 21. Checklist kết thúc buổi học

- [ ] Đã thu đủ phản hồi First-test.
- [ ] Đã thu đủ phản hồi Final-test.
- [ ] Đã tính tỷ lệ đúng của bốn nhóm ở First-test.
- [ ] Đã tính tỷ lệ đúng của bốn nhóm ở Final-test.
- [ ] Đã tính thay đổi theo điểm phần trăm.
- [ ] Đã ghi nhận ba lỗi phổ biến nhất của lớp.
- [ ] Đã xác định năng lực thấp nhất sau buổi học.
- [ ] Đã lưu bài thực hành hoặc ảnh chụp kết quả theo cặp.
- [ ] Đã xác định  cần hỗ trợ thêm, không công khai xếp hạng.
- [ ] Đã đề xuất nội dung phụ đạo tiếp theo.
- [ ] Đã liên hệ lỗi relationship còn lại với project cuối kỳ.

### Mẫu kết luận sau buổi

~~~text
Nhóm tiến bộ rõ nhất: ______________________________
Tăng từ ______% lên ______%.

Nhóm còn yếu nhất: _________________________________
Final-test đạt ______%.

Lỗi còn lặp lại nhiều nhất: _________________________

Hoạt động tiếp theo đề xuất: ________________________
~~~

---

## Phụ lục: Phiếu chốt kiến thức một phút

Sinh viên hoàn thành trước khi rời lớp:

1. 1–1 = ______________________________.
2. Trong 1–N, FK nằm ở __________________.
3. N–N cần ______________________________.
4. <code>ForeignKey</code> làm việc ở tầng __________; <code>relationship()</code> giúp truy cập __________ trong Python.
5. Thuộc tính số nhiều thường trả về __________.

**Đáp án:** FK + UNIQUE; phía N; bảng trung gian có hai FK; database/object liên quan; danh sách.

---

## Tự QA trước khi sử dụng

- [x] Nội dung thực 101 phút; dự phòng 19 phút.
- [x] First-test 8 câu A/B/C/D, tự động chấm và đo đủ bốn nhóm.
- [x] Final-test 6 câu A/B/C/D, nghiệp vụ khác và đo đủ bốn nhóm.
- [x] Đáp án nhiễu gắn với lỗi thực tế.
- [x] 1–1 có FK + UNIQUE.
- [x] 1–N đặt FK phía N và không UNIQUE.
- [x] N–N có bảng trung gian với hai FK.
- [x] Phân biệt đúng <code>ForeignKey</code> và <code>relationship()</code>.
- [x] Có code model, dữ liệu mẫu, FastAPI endpoint và query relationship.
- [x] Có đề thực hành, code khung, đáp án và cách chữa theo nhóm lỗi.
- [x] Nghiệp vụ chính và nghiệp vụ chuyển giao đều gắn với hệ thống production của doanh nghiệp.
- [x] Không đưa nội dung SQLAlchemy nâng cao ngoài mục tiêu.
- [x] Final-test không bị cắt khi trễ giờ.
