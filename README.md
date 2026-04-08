# README.md

# HƯỚNG DẪN TỔNG QUAN: UPLOAD / DOWNLOAD FILE TỪ CÁC LOẠI DRIVE CƠ BẢN BẰNG LỆNH BASH

Tài liệu này viết cho người mới hoàn toàn, đọc để hiểu **bức tranh tổng thể** trước khi đụng vào lệnh thật.  
Mục tiêu là giúp bạn hiểu:

- Drive là gì
- Bash là gì
- Vì sao có thể upload / download bằng dòng lệnh
- Thường người ta dùng cách nào với Google Drive, OneDrive và các loại cloud drive khác
- Khi nào nên dùng cách nào
- Các lỗi tư duy người mới rất hay gặp

**Lưu ý:**  
Tài liệu này **không viết code**, không dạy copy-paste lệnh ngay.  
Đây là bản **giải thích lý thuyết dễ hiểu**, để sau đó bạn học lệnh sẽ đỡ loạn.

---

# 1. HIỂU RÕ MÌNH ĐANG LÀM GÌ

## 1.1. “Drive” là gì?

“Drive” ở đây nghĩa là nơi lưu file trên mây, ví dụ:

- Google Drive
- Microsoft OneDrive
- Dropbox
- Mega
- Box
- Nextcloud

Bạn có thể tưởng tượng đơn giản:

- Ổ cứng trong máy = kho file ở nhà
- Drive trên cloud = kho file ở ngoài internet

Khi bạn **upload**, nghĩa là bạn đẩy file từ máy lên cloud.  
Khi bạn **download**, nghĩa là bạn kéo file từ cloud về máy.

---

## 1.2. Bash là gì?

Bash là môi trường gõ lệnh trong terminal.

Bạn không bấm chuột nhiều như giao diện bình thường, mà bạn ra lệnh bằng chữ.  
Ví dụ trong đầu bạn có thể hiểu đơn giản:

- “Hãy tải file này xuống”
- “Hãy đẩy thư mục này lên”
- “Hãy đồng bộ 2 nơi”

Bash chỉ là nơi bạn gõ lệnh.  
Còn việc upload/download được là nhờ **công cụ** chạy trong Bash.

Nói dễ hiểu:

- Terminal/Bash = chỗ bạn nói chuyện với máy
- Tool = người đi làm việc cho bạn

---

# 2. TẠI SAO PHẢI HỌC UPLOAD / DOWNLOAD BẰNG BASH?

Vì có nhiều tình huống giao diện chuột không tiện:

- Bạn đang dùng máy Linux server
- Bạn đang làm trên HPC
- Bạn remote vào máy từ xa
- File quá lớn
- Có hàng ngàn file nhỏ
- Muốn chạy ban đêm, tự động, ít bấm tay
- Muốn resume khi mạng đứt
- Muốn copy giữa cloud và server mà không cần ngồi canh

Nói ngắn gọn:

**Giao diện chuột phù hợp cho việc ít file, đơn giản.**  
**Dòng lệnh phù hợp cho việc nhiều file, file lớn, làm việc chuyên nghiệp, tự động hóa.**

---

# 3. CÓ NHỮNG KIỂU LÀM VIỆC NÀO?

Khi nói upload/download drive bằng Bash, thường có 4 kiểu tư duy chính.

## 3.1. Tải trực tiếp 1 file hoặc 1 link

Đây là kiểu đơn giản nhất.

Ví dụ trong đầu là:

- Tôi có 1 link chia sẻ
- Tôi muốn kéo file đó về máy

Kiểu này phù hợp khi:

- Chỉ cần 1 file
- Không cần đồng bộ lâu dài
- Muốn làm nhanh

Nhược điểm:

- Thường chỉ tiện cho vài file
- Dễ lỗi nếu link chia sẻ không đúng quyền
- Không hợp cho thư mục lớn

---

## 3.2. Upload hoặc download cả thư mục

Kiểu này là:

- Một thư mục trên máy
- Một thư mục trên cloud
- Bạn muốn chuyển nguyên cụm dữ liệu qua lại

Phù hợp khi:

- Backup dữ liệu
- Chuyển project
- Upload kết quả phân tích
- Tải dataset lớn

Nhược điểm:

- Nếu làm không cẩn thận, dễ upload nhầm chỗ
- Dễ đè file cũ nếu không hiểu cơ chế tool

---

## 3.3. Đồng bộ hai nơi

“Đồng bộ” khác với “copy một lần”.

Copy một lần nghĩa là:
- chuyển xong là thôi

Đồng bộ nghĩa là:
- giữ cho 2 nơi giống nhau theo một quy tắc nào đó

Ví dụ:
- thư mục trong máy giống thư mục trên drive
- file mới thì thêm
- file sửa thì cập nhật
- có tool còn có thể xóa file ở bên kia để giống hoàn toàn

Đây là kiểu mạnh nhưng nguy hiểm nhất cho người mới.

Vì sao?
Vì nếu bạn không hiểu chiều đồng bộ, bạn có thể xóa nhầm dữ liệu.

---

## 3.4. Mount drive như một ổ đĩa

Có công cụ cho phép bạn làm cloud drive trông giống như một thư mục trong máy.

Bạn mở thư mục đó lên và cảm giác như đang dùng ổ cứng bình thường.

Ưu điểm:
- Dễ hình dung
- Thuận tiện cho thao tác kiểu “mở thư mục ra xem”

Nhược điểm:
- Không phải lúc nào cũng ổn định
- Phụ thuộc mạng
- File lớn hoặc quá nhiều file có thể chậm
- Một số phần mềm không thích làm việc trực tiếp với cloud mounted drive

---

# 4. CÔNG CỤ DÒNG LỆNH THƯỜNG DÙNG LÀ GÌ?

Có nhiều công cụ khác nhau, nhưng về mặt lý thuyết bạn chỉ cần hiểu có 3 nhóm.

## 4.1. Nhóm tải file từ link

Nhóm này thường dùng khi:
- bạn có URL trực tiếp
- bạn chỉ cần tải xuống

Tư duy:
- đưa cho tool một địa chỉ
- tool kéo file về

Nhóm này đơn giản, nhưng thường chỉ mạnh khi link là link tải thực sự.  
Nếu là link chia sẻ kiểu web, có khi không tải được ngay.

---

## 4.2. Nhóm chuyên làm việc với cloud drive

Đây là nhóm quan trọng nhất.

Nhóm này hiểu các dịch vụ như:
- Google Drive
- OneDrive
- Dropbox
- Box
- S3
- WebDAV
- Nextcloud
- nhiều loại remote storage khác

Nhóm này thường có khả năng:

- kết nối tài khoản
- xem file trên cloud
- upload
- download
- copy
- sync
- mount
- resume khi gián đoạn
- kiểm tra tiến độ

Đây là kiểu công cụ người làm server, bioinformatics, data, HPC rất hay dùng.

---

## 4.3. Nhóm đồng bộ kiểu desktop client

Một số công cụ hoặc app hoạt động như phần mềm đồng bộ nền.

Ý tưởng:
- bạn đăng nhập tài khoản
- phần mềm tự giữ thư mục máy và thư mục cloud đồng bộ với nhau

Cái này dễ với người phổ thông, nhưng trong môi trường server/HPC thì thường không phải lựa chọn tốt nhất.

---

# 5. GOOGLE DRIVE: HIỂU CÁCH LÀM

Google Drive nhìn thì quen, nhưng khi dùng bằng Bash lại có vài điểm làm người mới hay rối.

## 5.1. Google Drive không giống ổ cứng thật

Google Drive là dịch vụ lưu trữ có cấu trúc riêng.  
Nó không đơn giản chỉ là “có thư mục là xong” như ổ cứng local.

Một số tool sẽ mô phỏng nó thành thư mục để bạn dễ hiểu, nhưng bên dưới vẫn là dịch vụ cloud.

---

## 5.2. Vấn đề quyền truy cập

Muốn tải hoặc upload lên Google Drive, bạn thường phải có một trong hai thứ:

- link chia sẻ có quyền phù hợp
- hoặc đăng nhập tài khoản và cấp quyền cho tool

Nếu không có quyền:
- nhìn thấy link chưa chắc tải được
- có link chưa chắc tool hiểu
- có file chưa chắc tải được nếu chủ file không chia sẻ đúng

---

## 5.3. Cách nghĩ đúng khi dùng Google Drive bằng Bash

Bạn nên tự hỏi trước:

- Tôi đang tải từ link chia sẻ hay từ tài khoản của chính tôi?
- Tôi muốn tải 1 file, 1 thư mục, hay đồng bộ lâu dài?
- Tôi chỉ cần đọc dữ liệu, hay cần upload sửa đổi?
- Tôi đang dùng máy cá nhân hay server?

Vì mỗi câu trả lời sẽ dẫn đến một kiểu công cụ khác nhau.

---

## 5.4. Khi nào Google Drive hợp?

Hợp khi:
- chia sẻ tài liệu học tập
- chia sẻ dữ liệu mức vừa
- backup file cá nhân
- trao đổi file nhỏ đến trung bình
- làm việc nhóm nhẹ

Kém hợp khi:
- dataset cực lớn
- cần hiệu năng upload/download rất mạnh
- có rất nhiều file nhỏ
- cần môi trường tính toán nặng trên server

---

# 6. ONEDRIVE: HIỂU CÁCH LÀM

OneDrive là hệ lưu trữ của Microsoft.

Về bản chất, logic cũng giống Google Drive:
- có tài khoản
- có thư mục/file trên cloud
- có quyền chia sẻ
- có thể upload/download/sync

Nhưng người mới hay gặp rối ở chỗ:

- tài khoản cá nhân và tài khoản cơ quan/trường học có thể khác nhau
- có thể có nhiều “drive” bên trong cùng hệ sinh thái Microsoft
- quyền truy cập và xác thực đôi khi phức tạp hơn tưởng tượng

---

## 6.1. OneDrive thường mạnh trong hệ sinh thái Microsoft

Nếu bạn dùng:
- Windows
- Office
- tài khoản trường/công ty Microsoft 365

thì OneDrive thường rất tiện.

Nhưng khi chuyển sang Linux, server, HPC, terminal:
- bạn sẽ cần tool phù hợp để nói chuyện với OneDrive
- không thể nghĩ đơn giản là “mở thư mục như Windows”

---

## 6.2. Những điều người mới hay nhầm với OneDrive

### Nhầm 1: Có link là tải được hết
Sai.  
Có link nhưng quyền không đúng thì vẫn không tải được.

### Nhầm 2: File thấy trên web thì dòng lệnh sẽ thấy y hệt
Không phải lúc nào cũng vậy.  
Tool dòng lệnh còn phụ thuộc xác thực và loại tài khoản.

### Nhầm 3: OneDrive cá nhân và OneDrive cơ quan giống hệt nhau
Không hẳn.  
Thực tế có thể khác về cách xác thực, quyền, và cấu trúc.

---

# 7. DROPBOX, BOX, MEGA, NEXTCLOUD VÀ CÁC DRIVE KHÁC

Về tư duy, các drive này khá giống nhau.

Bạn cứ nhớ 4 câu:

1. Có nơi chứa file trên cloud  
2. Có quyền truy cập  
3. Có tool để nói chuyện với dịch vụ đó  
4. Có thao tác cơ bản: upload, download, copy, sync, mount

Khác nhau chủ yếu ở:

- cách đăng nhập
- mức độ hỗ trợ của tool
- tốc độ
- giới hạn dung lượng / API / quota
- cách xử lý link chia sẻ
- độ dễ dùng trên Linux/server

---

# 8. CÁC CÁCH THƯỜNG DÙNG NHẤT THEO MỤC TIÊU

## 8.1. Chỉ cần tải 1 file về máy
Bạn cần kiểu:
- tải từ link trực tiếp
- hoặc tool cloud truy cập đúng file đó

Đây là bài toán dễ nhất.

---

## 8.2. Muốn tải cả thư mục dữ liệu lớn
Bạn cần kiểu:
- cloud tool hỗ trợ thư mục
- có resume
- có kiểm tra tiến độ
- chịu được mạng chập chờn

Đây là tình huống rất phổ biến khi làm dữ liệu.

---

## 8.3. Muốn upload kết quả phân tích lên drive
Bạn cần kiểu:
- upload thư mục
- có thể tiếp tục nếu đứt mạng
- tránh upload trùng
- nhìn được log hoặc tiến độ

---

## 8.4. Muốn giữ file trên server và cloud giống nhau
Bạn đang nói tới:
- sync
- hoặc backup theo chu kỳ

Cần cực kỳ cẩn thận vì dễ xóa nhầm dữ liệu.

---

## 8.5. Muốn dùng cloud như một ổ đĩa tạm thời
Bạn đang nói tới:
- mount

Phù hợp khi muốn duyệt file, xem nhanh, thao tác nhẹ.  
Không phải lúc nào cũng phù hợp cho tính toán nặng.

---

# 9. TƯ DUY QUAN TRỌNG NHẤT: COPY KHÁC SYNC

Đây là chỗ người mới ngu nhất rất hay chết.

## 9.1. Copy
Copy nghĩa là:
- chép từ A sang B
- thường không động vào file cũ ở nguồn
- đích có thêm bản sao

An toàn hơn.

---

## 9.2. Sync
Sync nghĩa là:
- làm cho A và B giống nhau theo quy tắc
- có thể thêm file mới
- có thể cập nhật file sửa
- có thể xóa file ở một bên

Nguy hiểm hơn rất nhiều.

Nếu chưa hiểu rõ, ưu tiên học **copy** trước, đừng đụng **sync** quá sớm.

---

# 10. VẤN ĐỀ XÁC THỰC: TẠI SAO NÓ PHIỀN?

Để tool dòng lệnh làm việc với cloud, bạn thường phải xác thực.

Hiểu đơn giản:
- cloud phải biết bạn là ai
- và bạn có quyền gì

Các kiểu xác thực phổ biến có thể gồm:

- đăng nhập trên trình duyệt rồi cấp quyền
- dán mã xác thực
- dùng token
- dùng tài khoản dịch vụ
- dùng link chia sẻ công khai

Người mới không cần hiểu sâu kỹ thuật ngay.  
Chỉ cần nhớ:

**Nếu tool không được cấp quyền, thì nó không làm được gì cả.**

---

# 11. NHỮNG KHÁC BIỆT GIỮA MÁY CÁ NHÂN, SERVER, VÀ HPC

## 11.1. Máy cá nhân
Dễ nhất vì:
- có trình duyệt
- dễ đăng nhập
- dễ cấp quyền
- dễ kiểm tra file

---

## 11.2. Server
Khó hơn vì:
- có thể không có giao diện đồ họa
- cần xác thực bằng cách gián tiếp
- dễ nhầm đường dẫn
- dễ ghi đè dữ liệu nếu không cẩn thận

---

## 11.3. HPC
Khó hơn nữa vì:
- thường hạn chế kết nối
- không phải lúc nào cũng cho thoải mái dùng cloud
- có quota lưu trữ
- nên phân biệt rõ chỗ nào là:
  - home
  - scratch
  - project storage
- upload/download trực tiếp có thể bị giới hạn theo chính sách hệ thống

Tức là: không phải cứ biết lệnh là dùng bừa được trên HPC.

---

# 12. CÁC TIÊU CHÍ ĐỂ CHỌN CÁCH LÀM

Trước khi quyết định dùng cách nào, hãy tự hỏi:

## 12.1. File nhỏ hay lớn?
- File nhỏ: nhiều cách đều ổn
- File lớn: cần công cụ có resume, ổn định

## 12.2. Ít file hay rất nhiều file?
- Ít file: làm đơn giản
- Rất nhiều file nhỏ: cần tool xử lý tốt metadata và retry

## 12.3. Chỉ làm một lần hay làm thường xuyên?
- Một lần: cách đơn giản là đủ
- Lặp lại nhiều lần: nên dùng tool cloud chuyên nghiệp hơn

## 12.4. Chỉ tải xuống hay cần upload ngược?
- Chỉ tải xuống: dễ hơn
- Upload ngược: phải hiểu rõ thư mục đích

## 12.5. Có cần tự động hóa không?
- Nếu có: nên dùng công cụ ổn định, log rõ ràng, dễ script hóa

---

# 13. LỖI TƯ DUY NGƯỜI MỚI HAY GẶP

## 13.1. Nghĩ rằng “thấy được trên web” thì “dòng lệnh làm được ngay”
Sai.  
Web và tool dòng lệnh là hai lớp khác nhau.

---

## 13.2. Không phân biệt file và thư mục
Tải 1 file khác với tải cả thư mục.  
Upload 1 file cũng khác với upload nguyên project.

---

## 13.3. Không phân biệt copy và sync
Đây là lỗi nguy hiểm nhất.  
Không hiểu mà sync là rất dễ mất dữ liệu.

---

## 13.4. Không kiểm tra mình đang đứng ở thư mục nào
Trong Bash, vị trí hiện tại rất quan trọng.

Nếu bạn tưởng đang ở thư mục A nhưng thật ra đang ở B:
- upload sai chỗ
- download sai chỗ
- ghi đè file
- tưởng mất file

---

## 13.5. Không hiểu đường dẫn
Đường dẫn là “địa chỉ” của file/thư mục.

Người mới thường rối vì:
- tên thư mục na ná nhau
- nhầm giữa thư mục local và remote
- không biết mình đang thao tác ở máy hay trên cloud

---

## 13.6. Tưởng upload xong là chắc chắn đủ dữ liệu
Không phải.  
Có khi:
- thiếu file
- lỗi giữa chừng
- mạng đứt
- file quá lớn
- quota đầy
- quyền không đủ

Cho nên luôn phải có tư duy:
- kiểm tra lại
- xác nhận dung lượng
- xác nhận số file
- xác nhận vị trí đích

---

# 14. NHỮNG RỦI RO THỰC TẾ

## 14.1. Mạng yếu hoặc đứt giữa chừng
Dễ làm upload/download bị lỗi hoặc dở dang.

## 14.2. Quota đầy
Cloud đầy thì không upload được nữa.

## 14.3. Sai quyền truy cập
Có link nhưng không tải được.

## 14.4. Trùng tên file
Có thể:
- bị ghi đè
- bị bỏ qua
- bị tạo bản khác
Tùy tool.

## 14.5. Xóa nhầm khi sync
Đây là ác mộng kinh điển.

## 14.6. Thư mục đích không đúng
Tưởng đang upload vào chỗ A, thật ra vào chỗ B.

---

# 15. KHI NÀO NÊN DÙNG GIAO DIỆN CHUỘT, KHI NÀO NÊN DÙNG BASH?

## Dùng giao diện chuột khi:
- bạn chỉ có vài file
- file không lớn
- không cần tự động hóa
- bạn mới học, muốn kiểm tra trực quan

## Dùng Bash khi:
- file lớn
- nhiều file
- làm trên server/HPC
- cần lặp lại nhiều lần
- cần log rõ
- muốn tự động hóa
- muốn làm việc chuyên nghiệp, ít bấm tay

Người mới không cần cực đoan.  
Có thể bắt đầu bằng giao diện chuột để hiểu luồng, sau đó mới học Bash.

---

# 16. TƯ DUY HỌC ĐÚNG CHO NGƯỜI MỚI

Đừng học kiểu:
- thấy lệnh đâu đó
- copy-paste đại
- cầu trời chạy

Hãy học theo thứ tự này:

## Bước 1: Hiểu mình muốn làm gì
Ví dụ:
- tải 1 file
- tải 1 thư mục
- upload kết quả
- backup
- sync

## Bước 2: Xác định dữ liệu đang ở đâu
- trong máy
- trên cloud
- link chia sẻ
- tài khoản của mình

## Bước 3: Xác định công cụ phù hợp
- tải từ link
- cloud tool
- mount
- sync

## Bước 4: Hiểu rủi ro trước khi làm
- có ghi đè không?
- có xóa file không?
- có cần quyền gì không?
- có đủ dung lượng không?

## Bước 5: Luôn kiểm tra sau khi làm
- file đã qua chưa?
- đủ dung lượng chưa?
- đúng thư mục chưa?
- thiếu file nào không?

---

# 17. TÓM TẮT DỄ NHỚ

Nếu bạn quá rối, chỉ cần nhớ thế này:

## Một:
Drive là kho file trên internet.

## Hai:
Bash là chỗ bạn gõ lệnh.

## Ba:
Muốn upload/download bằng Bash thì phải có tool phù hợp.

## Bốn:
Google Drive, OneDrive, Dropbox... khác nhau về dịch vụ, nhưng tư duy dùng gần giống nhau.

## Năm:
Có 4 kiểu làm việc chính:
- tải 1 file
- chuyển cả thư mục
- đồng bộ
- mount như ổ đĩa

## Sáu:
Người mới nên ưu tiên hiểu:
- local là gì
- remote là gì
- file khác thư mục thế nào
- copy khác sync thế nào

## Bảy:
Chưa hiểu rõ thì đừng sync bừa.

---

# 18. KẾT LUẬN

Upload/download từ Google Drive, OneDrive hay các cloud drive khác bằng Bash thực chất không đáng sợ.  
Nó chỉ đáng sợ khi bạn:

- không hiểu mình đang làm gì
- không phân biệt local với remote
- không phân biệt copy với sync
- không kiểm tra quyền truy cập
- không kiểm tra lại kết quả

Nếu hiểu đúng bản chất, thì mọi dịch vụ cloud cũng chỉ xoay quanh mấy việc quen thuộc:

- kết nối tài khoản hoặc dùng link
- chọn file/thư mục nguồn
- chọn file/thư mục đích
- upload hoặc download
- kiểm tra kết quả

---

# 19. GỢI Ý HỌC TIẾP

Sau khi đọc xong tài liệu lý thuyết này, bước học tiếp theo hợp lý là:

1. Học cấu trúc cơ bản của đường dẫn trong Bash  
2. Hiểu local và remote  
3. Học một công cụ cloud phổ biến  
4. Tập thao tác an toàn với 1 file nhỏ trước  
5. Sau đó mới học thư mục lớn, resume, sync, mount

---

**Tài liệu này là bản tổng quan lý thuyết dành cho người mới hoàn toàn.**  
Mục tiêu không phải để bạn thành chuyên gia ngay, mà để bạn **đỡ ngu đúng chỗ nguy hiểm** trước khi học lệnh thật.
