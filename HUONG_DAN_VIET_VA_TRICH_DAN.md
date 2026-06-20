# Hướng dẫn viết và trích dẫn cho báo cáo

Ngày cập nhật: 2026-06-20

File này tóm tắt các lưu ý quan trọng từ `Chuong/1_Gioi_thieu.tex` và `Chuong/Phu_luc_A.tex`, đồng thời chuyển chúng thành checklist thực hành cho đồ án này.

## 1. Trạng thái template

Repo báo cáo hiện đã có bìa, lời cảm ơn, tóm tắt tiếng Việt, abstract tiếng Anh, Chương 1, Chương 2, Chương 3 và Chương 4 ở mức nội dung hoàn chỉnh ban đầu. Chương 2 đã hoàn thiện phần nền tảng về ngữ cảnh bài toán, nghiên cứu liên quan, suy luận INT8, RISC-V, CV32E40P, gem5 và TensorFlow Lite Micro. Chương 3 đã hoàn thiện phần thiết kế và đặc tả nhóm lệnh mở rộng, bao gồm tổng quan giải pháp, cơ sở lựa chọn nhóm lệnh, đặc tả ngữ nghĩa sáu lệnh, ánh xạ sang gem5/DE2i-150-CV32E40P và đoạn kết nối sang Chương 4. Chương 4 đã hoàn thiện phần đánh giá qua mô phỏng gem5, gồm hiện thực, smoke test, benchmark mô phỏng, phân tích kết quả và mô hình vòng lặp phần cứng trong O3. Báo cáo cũng đã có phụ lục gem5 bổ sung phần thiết lập và tái mô phỏng, gồm kho mã nguồn, bản đồ tệp, lệnh chạy, cách đọc số liệu và đoạn mã tiêu biểu. Các chương từ Chương 5 đến Chương 6 vẫn cần được thay bằng nội dung đồ án khi viết tiếp. Khi viết từng chương:

- Không giữ nguyên các đoạn "Sinh viên cần..." trong bản thảo cuối.
- Không để các dòng `\textcolor{red}{Chú ý...}` của template trong bản nộp.
- Không dùng cấu trúc mẫu một cách máy móc nếu không phù hợp; có thể đổi tên mục con miễn là vẫn đúng logic đồ án nghiên cứu.

## 1.1. Vai trò biên tập khi viết cùng trợ lý

Khi chỉnh báo cáo, trợ lý cần đóng vai trò hỗ trợ biên tập học thuật: đọc yêu cầu, kiểm tra mạch lập luận, chỉ ra điểm chưa hợp lý và đề xuất cách viết tốt hơn. Không sửa máy móc theo từng câu người viết nêu ra nếu thay đổi đó làm báo cáo kém rõ ràng hoặc kém chuẩn hơn. Với các góp ý mang tính chủ quan, cần cân nhắc theo mục tiêu cuối cùng của báo cáo: rõ vấn đề, đúng kỹ thuật, văn phong tự nhiên, và phù hợp chuẩn đồ án nghiên cứu.

## 2. Cách hành văn

Các nguyên tắc cần bám:

- Viết thành đoạn văn hoàn chỉnh, không viết kiểu slide.
- Mỗi đoạn nên có một ý chính rõ ràng, các câu sau phân tích hoặc bổ trợ cho ý đó.
- Câu phải đầy đủ chủ ngữ, vị ngữ; tránh câu cụt hoặc câu chỉ là cụm danh từ.
- Câu sau phải nối logic với câu trước; đoạn sau phải nối logic với đoạn trước.
- Tránh văn nói và từ cảm tính như "cực kỳ", "rất hay", "tuyệt vời", "siêu nhanh".
- Ưu tiên diễn đạt ngắn gọn, có chủ thể kỹ thuật rõ ràng.
- Không phóng đại kết quả. Ví dụ nên viết "đạt speedup 13.10x trong benchmark `dot4_plw_lp_clamp` trên cấu hình đã đo" thay vì "tăng tốc vượt trội".

## 3. Dùng gạch đầu dòng thế nào

Template nhấn mạnh báo cáo không phải slide, vì vậy:

- Trong nội dung chính, hạn chế bullet.
- Khi cần liệt kê trong văn xuôi, ưu tiên dạng "(i) ..., (ii) ..., và (iii) ...".
- Chỉ dùng `itemize`/`enumerate` khi danh sách thật sự giúp rõ nghĩa, ví dụ liệt kê đóng góp hoặc thông số thí nghiệm.
- Nếu dùng bullet, giữ style thống nhất toàn báo cáo.
- Sau danh sách, cần có đoạn diễn giải hoặc kết luận, không để danh sách đứng một mình.

## 4. Cấu trúc mỗi chương

Mỗi chương nên có phần mở đầu và kết chương, dù không nhất thiết đặt heading riêng nếu thấy không phù hợp.

Phần mở đầu chương:

- Liên kết với chương trước.
- Nêu lý do chương này cần có trong mạch báo cáo.
- Tóm tắt các nhóm nội dung sẽ trình bày.

Phần kết chương:

- Tóm tắt lại các ý đã giải quyết.
- Không lặp lại nguyên văn phần mở đầu.
- Nêu ý chuyển tiếp sang chương sau.

Ví dụ áp dụng cho đồ án:

- Kết Chương 3 nên dẫn sang Chương 4 bằng ý: sau khi đặc tả các operation và mapping, cần kiểm tra semantics và phân tích hiệu năng bằng mô phỏng gem5.
- Kết Chương 4 nên dẫn sang Chương 5 bằng ý: mô phỏng cho thấy xu hướng và kiểm tra logic, còn hiệu quả thực tế cần đánh giá trên kit DE2i-150/CV32E40P.
- Với Chương 3, bản hiện tại đã chốt mạch từ cơ sở lựa chọn nhóm lệnh đến đặc tả thao tác và ánh xạ sang gem5/CV32E40P. Khi rà soát lại, cần giữ cách diễn đạt hiện có: nhánh gem5 là mô hình thử nghiệm dùng `custom-0`, còn nhánh phần cứng ánh xạ cùng ngữ nghĩa sang CORE-V/PULP thông qua lớp firmware và vẫn bao gồm nhiều công việc tích hợp/kiểm chứng.
- Với Chương 4, bản hiện tại đã chốt mạch từ hiện thực gem5, kiểm thử chức năng, thiết lập benchmark, phân tích số liệu mô phỏng đến giới hạn của mô hình O3. Khi viết Chương 5, cần giữ cách phân vai này: gem5 dùng để kiểm chứng và phân tích xu hướng, còn kit DE2i-150/CV32E40P dùng để đánh giá hiệu quả thực nghiệm.
- Với phụ lục gem5, cần giữ giọng văn như thông tin bổ sung để người đọc có thể lần theo quy trình tái mô phỏng. Không biến phụ lục thành một tutorial quá dài, cũng không lặp lại đặc tả ngữ nghĩa ở Chương 3 hoặc phân tích kết quả ở Chương 4.

## 5. Hình, bảng, công thức

Các quy tắc bắt buộc:

- Mọi hình, bảng, công thức phải được tham chiếu ít nhất một lần trong nội dung.
- Không chèn hình/bảng chỉ để minh họa cho đẹp; phải có đoạn giải thích người đọc cần rút ra điều gì.
- Caption phải đủ rõ để người đọc hiểu nội dung chính.
- Với bảng số liệu, cần nói rõ đơn vị, điều kiện đo, baseline và cách tính speedup.
- Với hình tự vẽ dựa trên kiến trúc repo, vẫn nên ghi rõ trong phần mô tả rằng hình được tổng hợp từ thiết kế/triển khai của đồ án.
- Các hình TikZ tự vẽ nên đặt trong `Hinh_ve/*.tikz` và nạp bằng `\input{Hinh_ve/...}` để file chương gọn và dễ theo dõi diff.
- Với phụ lục có đoạn mã dài, cần có đoạn dẫn hoặc bảng đối chiếu giúp người đọc hiểu đoạn mã đang minh họa thao tác nào. Không nên đưa nguyên một listing Assembly mà thiếu giải thích.

## 6. Trích dẫn và chống đạo văn

Các nguyên tắc từ phụ lục:

- Tuyệt đối không đạo văn.
- Mọi nội dung không tự viết/tự vẽ/tự đo cần có nguồn.
- Nguồn cần rõ ràng và đáng tin cậy.
- Hạn chế dùng website không chính thống hoặc Wikipedia làm nguồn chính.
- Báo cáo dùng kiểu IEEE, trích bằng `\cite{...}`.
- Chỉ tài liệu được `\cite{...}` mới xuất hiện trong danh mục tài liệu tham khảo.

Áp dụng cho đồ án này:

- README/HANDOFF/log trong repo là nguồn nội bộ để viết đúng triển khai của mình, nhưng không thay thế citation học thuật/official.
- Các phần như RISC-V ISA, gem5, CV32E40P, TFLM, quantization INT8 phải có nguồn ngoài trong `.bib`.
- Khi dùng một hình từ tài liệu ngoài, cần trích nguồn trong caption hoặc đoạn văn liên quan.
- Khi diễn giải lại kiến thức từ tài liệu ngoài, vẫn cần citation; không chỉ citation khi trích nguyên văn.
- Không copy nguyên văn đoạn mô tả từ tài liệu ngoài vào báo cáo. Nên đọc, hiểu, rồi viết lại bằng văn phong của mình và trích nguồn.

## 7. Trạng thái `.bib` hiện tại

`.bib` hiện đã có các nguồn nền tảng dùng cho Chương 1, Chương 2 và các phần mô tả gem5 trong Chương 4, gồm RISC-V ISA, CV32E40P, gem5, TensorFlow Lite Micro, tài liệu LiteRT Microcontrollers, đặc tả lượng tử hóa LiteRT/INT8 và các nghiên cứu liên quan về suy luận học sâu trên hệ nhúng, tối ưu phần mềm, SIMD/RVV và bộ tăng tốc chuyên dụng. Các mục template không dùng đã được comment lại để tránh xuất hiện trong tài liệu tham khảo. Tuy nhiên, chưa nên giả định `.bib` đã đủ nguồn cho toàn bộ báo cáo. Khi viết Chương 5 trở đi, cần tiếp tục tạo dần danh sách citation cho các claim kỹ thuật mới.

Nhóm tài liệu cần bổ sung:

- Tài liệu CORE-V/PULP instruction documentation nếu dùng làm nguồn giải thích instruction và mapping cụ thể ở Chương 3/Chương 5.
- Tài liệu RISC-V liên quan tới custom extension/vector nếu cần trình bày sâu hơn ngoài phần nền tảng đã có ở Chương 2.
- Tài liệu TensorFlow Lite Micro chi tiết hơn cho phần triển khai thực nghiệm nếu cần mô tả API/kernel cụ thể hơn.
- Các bài nghiên cứu liên quan đến suy luận lượng tử hóa trên hệ nhúng.
- Tài liệu về benchmark/performance metrics nếu cần giải thích cách đo cycles, speedup.

Ghi chú định dạng hiện tại:

- Báo cáo dùng `biblatex-ieee` với `babel` tiếng Anh để các chuỗi như `et al.`, `vol.`, `pp.` và `url` được hiển thị đúng thay vì lộ khóa nội bộ của style.
- URL trong danh mục tài liệu tham khảo đang để dạng monospace bằng `\urlstyle{tt}` và cho phép ngắt dòng bằng các `biburl...penalty`, nhằm giữ đúng chuẩn trình bày mà không làm link tràn lề.
- Chuỗi `urlseen` được Việt hóa thành "truy cập ngày" để đồng bộ với báo cáo tiếng Việt. Nếu sau này chuyển toàn bộ bibliography sang tiếng Anh thì có thể bỏ override này.
- Do `babel` đang để tiếng Anh, các chuỗi tự động của LaTeX có thể hiện bằng tiếng Anh nếu không override. Hiện `DoAn.tex` đã Việt hóa caption hình/bảng và header chương bằng cấu hình riêng; khi sửa định dạng cần kiểm tra lại PDF sau khi build.

Quy trình nên dùng:

1. Khi viết một claim lý thuyết, ghi ngay `TODO cite: ...` nếu chưa có BibTeX.
2. Thêm BibTeX vào `Danh_sach_tai_lieu_tham_khao.bib`.
3. Thay `TODO cite` bằng `\cite{key}`.
4. Kiểm tra biên dịch LaTeX để đảm bảo citation hiện đúng.

## 7.1. Danh mục thuật ngữ và từ viết tắt

`Tu_viet_tat.tex` hiện đã được thay từ danh sách mẫu sang danh mục thuật ngữ phục vụ đồ án. Chỉ nên đưa các thuật ngữ thật sự hỗ trợ người đọc theo dõi mạch báo cáo, chẳng hạn MAC, SIMD, RVV, SoC, TensorFlow Lite Micro/LiteRT, MNIST, ReLU, TPU và TOPS. Không nên đưa các thuật ngữ quá phổ thông với người đọc kỹ thuật hoặc đã được giải thích ngay trong phần nội dung, ví dụ AI, RISC-V, INT8, INT32, gem5, CV32E40P, FPGA và UART.

Style bảng thuật ngữ trong `DoAn.tex` đã được chỉnh để dùng gần hết chiều rộng trang. Nếu thêm thuật ngữ mới, cần kiểm tra lại PDF để bảo đảm bảng không tràn và mô tả không quá dài.

## 8. Checklist trước khi coi một chương là ổn

- Không còn câu hướng dẫn template.
- Không còn đoạn viết kiểu gạch đầu dòng thay cho phân tích.
- Mỗi section có đoạn mở và kết nối logic.
- Mọi hình/bảng/công thức đều được gọi tên trong nội dung.
- Các số liệu có nguồn nội bộ rõ ràng.
- Các kiến thức nền có citation hoặc TODO citation rõ ràng.
- Không dùng từ cảm tính/phóng đại.
- Có đoạn kết chương và câu nối sang chương tiếp theo.
