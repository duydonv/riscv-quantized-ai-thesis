# Hướng dẫn viết và trích dẫn cho báo cáo

Ngày cập nhật: 2026-06-12

File này tóm tắt các lưu ý quan trọng từ `Chuong/1_Gioi_thieu.tex` và `Chuong/Phu_luc_A.tex`, đồng thời chuyển chúng thành checklist thực hành cho đồ án này.

## 1. Trạng thái template

Repo báo cáo hiện đã có bìa, lời cảm ơn, tóm tắt tiếng Việt và abstract tiếng Anh ở mức bản nháp hoàn chỉnh. Các chương nội dung chính vẫn còn nội dung hướng dẫn mẫu. Khi viết từng chương:

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

## 5. Hình, bảng, công thức

Các quy tắc bắt buộc:

- Mọi hình, bảng, công thức phải được tham chiếu ít nhất một lần trong nội dung.
- Không chèn hình/bảng chỉ để minh họa cho đẹp; phải có đoạn giải thích người đọc cần rút ra điều gì.
- Caption phải đủ rõ để người đọc hiểu nội dung chính.
- Với bảng số liệu, cần nói rõ đơn vị, điều kiện đo, baseline và cách tính speedup.
- Với hình tự vẽ dựa trên kiến trúc repo, vẫn nên ghi rõ trong phần mô tả rằng hình được tổng hợp từ thiết kế/triển khai của đồ án.

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

Hiện chưa nên giả định `.bib` đã có đủ nguồn liên quan. Khi viết Chương 2 và các phần lý thuyết, cần tạo dần danh sách citation.

Nhóm tài liệu cần bổ sung:

- RISC-V ISA manual.
- CV32E40P/OpenHW documentation.
- CORE-V/PULP instruction documentation nếu dùng làm nguồn giải thích instruction.
- gem5 documentation hoặc paper liên quan.
- TensorFlow Lite Micro documentation.
- Tài liệu quantization INT8/integer-only neural network inference.
- Tài liệu về benchmark/performance metrics nếu cần giải thích cách đo cycles, speedup.

Quy trình nên dùng:

1. Khi viết một claim lý thuyết, ghi ngay `TODO cite: ...` nếu chưa có BibTeX.
2. Thêm BibTeX vào `Danh_sach_tai_lieu_tham_khao.bib`.
3. Thay `TODO cite` bằng `\cite{key}`.
4. Kiểm tra biên dịch LaTeX để đảm bảo citation hiện đúng.

## 8. Checklist trước khi coi một chương là ổn

- Không còn câu hướng dẫn template.
- Không còn đoạn viết kiểu gạch đầu dòng thay cho phân tích.
- Mỗi section có đoạn mở và kết nối logic.
- Mọi hình/bảng/công thức đều được gọi tên trong nội dung.
- Các số liệu có nguồn nội bộ rõ ràng.
- Các kiến thức nền có citation hoặc TODO citation rõ ràng.
- Không dùng từ cảm tính/phóng đại.
- Có đoạn kết chương và câu nối sang chương tiếp theo.
