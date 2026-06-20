# Bản đồ tài liệu phục vụ viết báo cáo đồ án

Ngày cập nhật: 2026-06-20

Đồ án: Mở rộng tập lệnh RISC-V cho các phép toán AI lượng tử hóa, mô phỏng trên gem5 và triển khai thực nghiệm trên kit FPGA DE2i-150/CV32E40P.

File này là điểm vào của bộ tài liệu làm việc khi viết báo cáo. Repo báo cáo đã có bìa, lời cảm ơn, tóm tắt tiếng Việt, abstract tiếng Anh, Chương 1 và Chương 2 ở mức nội dung hoàn chỉnh ban đầu. Chương 3 đã có bản nội dung hoàn chỉnh ban đầu cho phần thiết kế và đặc tả nhóm lệnh mở rộng, gồm tổng quan giải pháp, cơ sở lựa chọn nhóm lệnh, đặc tả ngữ nghĩa sáu lệnh và ánh xạ sang hai môi trường triển khai. Chương 4 đã được thay từ nội dung mẫu sang chương "Đánh giá qua mô phỏng gem5", gồm hiện thực gem5, kiểm thử chức năng, thiết lập benchmark, kết quả mô phỏng và phân tích riêng cho vòng lặp phần cứng trong O3. Báo cáo cũng đã bổ sung phụ lục gem5 để ghi lại quy trình thiết lập và tái mô phỏng, giúp người đọc có thể đối chiếu các lệnh chạy, tệp liên quan và đoạn mã tiêu biểu mà không làm loãng Chương 4. Các chương từ Chương 5 đến Chương 6 vẫn cần được viết tiếp theo mạch kỹ thuật đã chốt. Vì vậy nội dung dưới đây không phải bản thảo cuối, mà là bản đồ để khi viết từng chương có thể biết cần đọc gì, lấy số liệu ở đâu và cần giữ chuẩn trình bày nào.

## 1. Bộ file hỗ trợ viết báo cáo

| File | Vai trò |
|---|---|
| `BAN_DO_TAI_LIEU_DO_AN.md` | Điểm vào, cấu trúc chương dự kiến, cách dùng các file phụ |
| `HUONG_DAN_VIET_VA_TRICH_DAN.md` | Quy tắc hành văn, bố cục chương, trích dẫn, chống đạo văn theo template |
| `BAN_DO_THAM_CHIEU_KY_THUAT.md` | Bản đồ kỹ thuật: gem5, SoC/FPGA, firmware, benchmark, TFLM, UART |

Khi bắt đầu viết một chương, đọc file này trước để xác định phạm vi chương, sau đó đọc hai file phụ theo nhu cầu. Các chi tiết câu chữ của từng chương sẽ chốt dần trong quá trình viết.

## 2. Trạng thái hiện tại cần nhớ

- Các phần đầu báo cáo đã có nội dung: `0_2_Loi_cam_on.tex`, `0_3_Tom_tat_noi_dung.tex` và `0_4_Tom_tat_noi_dung_eng.tex`.
- Chương 1 đã được thay nội dung theo hướng hoàn chỉnh, gồm đặt vấn đề, các giải pháp hiện tại và hạn chế, mục tiêu và định hướng giải pháp, đóng góp của đồ án và bố cục báo cáo.
- Chương 2 đã có bản hoàn chỉnh ban đầu cho các phần nền tảng: ngữ cảnh bài toán, nghiên cứu liên quan, suy luận AI lượng tử hóa, RISC-V và mở rộng tập lệnh, CV32E40P, gem5 và TensorFlow Lite Micro. Chương này hiện có hình tự vẽ về luồng INT8, bảng vùng opcode custom của RISC-V và hình sơ đồ khối CV32E40P trích từ tài liệu OpenHW.
- Chương 3 đã có bản hoàn chỉnh ban đầu với tên "Thiết kế và đặc tả nhóm lệnh mở rộng". Chương này đã trình bày tổng quan giải pháp, cơ sở lựa chọn nhóm lệnh, đặc tả ngữ nghĩa cho `mac`, `dot4_acc`, `relu`, `clamp`, `p.lw`, `lp.setup`, bảng ánh xạ sang gem5 và DE2i-150/CV32E40P, cùng đoạn kết nối sang Chương 4.
- Chương 4 đã có bản hoàn chỉnh ban đầu với tên "Đánh giá qua mô phỏng gem5". Chương này đã trình bày luồng hiện thực nhóm lệnh trong gem5, phạm vi smoke test, thiết lập benchmark mô phỏng, bảng số liệu chu kỳ/lệnh động, biểu đồ tăng tốc và phân tích riêng cho mô hình vòng lặp phần cứng trong O3.
- Báo cáo đã có thêm phụ lục về thiết lập và tái mô phỏng nhóm lệnh mở rộng trong gem5. Phụ lục được nạp từ `Chuong/Phu_luc_C.tex` và hiện hiển thị là Phụ lục A do các phụ lục mẫu A/B đang được comment trong `DoAn.tex`. Nội dung phụ lục gồm kho mã nguồn tham chiếu, bản đồ thành phần gem5, lệnh biên dịch/chạy, cách đọc số liệu, khoanh vùng đo bằng m5ops và bảng đối chiếu từng dòng hợp ngữ trong nhân dùng `lp.setup`, `p.lw`, `dot4_acc`.
- Chương 5 và Chương 6 vẫn còn nội dung cần viết tiếp.
- Các hình TikZ tự vẽ hiện đã được quản lý trong `Hinh_ve/` và được nạp bằng `\input{Hinh_ve/...}`. Các file đáng chú ý gồm `int8_inference_flow.tikz`, `solution_overview.tikz`, `fc_mac_model_layers.tikz`, `fc_mac_growth.tikz`, `ai_ext_loop_semantics.tikz`, `dot4_acc_semantics.tikz`, `gem5_ai_ext_implementation.tikz`, `gem5_perf_speedup.tikz` và `hwloop_o3_frontend.tikz`.
- `Danh_sach_tai_lieu_tham_khao.bib` đã có các nguồn nền tảng dùng trong Chương 1 và Chương 2, gồm RISC-V ISA, CV32E40P, gem5, TensorFlow Lite Micro, tài liệu LiteRT Microcontrollers, đặc tả lượng tử hóa LiteRT/INT8 và một số nghiên cứu liên quan về suy luận trên hệ nhúng, tối ưu phần mềm, SIMD/RVV và bộ tăng tốc chuyên dụng. Khi viết Chương 3 trở đi vẫn cần bổ sung nguồn chính thức hoặc bài báo phù hợp với từng claim kỹ thuật mới.
- `Tu_viet_tat.tex` đã được thay từ danh sách mẫu sang danh mục thuật ngữ của đồ án, chỉ giữ các mục cần hỗ trợ người đọc như MAC, SIMD, RVV, SoC, TFLM/LiteRT, MNIST, ReLU, TPU, TOPS. Các thuật ngữ quá phổ thông hoặc đã được giải thích ngay trong nội dung không nên đưa vào để tránh làm danh mục bị loãng.
- `DoAn.tex` đang giữ `babel` tiếng Anh để danh mục tài liệu tham khảo hiển thị đúng chuẩn IEEE, đồng thời đã Việt hóa caption hình/bảng và header chương bằng cấu hình riêng. Khi sửa format, cần tránh đổi `babel` nếu chưa kiểm tra lại bibliography.
- Các file README/HANDOFF trong repo gem5 và SoC là nguồn nội bộ để hiểu đồ án và lấy số liệu/log. Chúng không thay thế tài liệu tham khảo học thuật hoặc tài liệu chính thức trong `.bib`.
- Phần gem5 không chỉ nằm trong `tests/gem5/riscv_ai_ext`. Đồ án đã chỉnh cả phần ISA, bộ giải mã, ngữ nghĩa lệnh, định dạng truy cập bộ nhớ, thanh ghi trạng thái phụ, cơ chế đổi hướng của vòng lặp phần cứng và một số đường xử lý trong O3/khối lấy lệnh. Chương 4 hiện đã làm rõ điểm này ở mức báo cáo, không đi sâu quá mức vào từng dòng code.
- Phần thực nghiệm trên kit là nguồn kết quả chính. gem5 dùng để phân tích, mô phỏng, đánh giá độ nhạy và hỗ trợ câu chuyện kỹ thuật, không khẳng định là mô hình khớp từng chu kỳ của CV32E40P.

## 3. Cấu trúc báo cáo dự kiến

### Phần đầu báo cáo: Lời cảm ơn và tóm tắt

Trạng thái hiện tại: đã có bản nội dung cho lời cảm ơn, tóm tắt tiếng Việt và abstract tiếng Anh. Khi các chương chính đã ổn định, cần quay lại rà soát thuật ngữ trong hai phần tóm tắt để bảo đảm khớp với cách gọi cuối cùng ở Chương 3, Chương 4 và Chương 5.

Các file liên quan:

- `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Chuong/0_2_Loi_cam_on.tex`
- `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Chuong/0_3_Tom_tat_noi_dung.tex`
- `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Chuong/0_4_Tom_tat_noi_dung_eng.tex`

### Chương 1: Giới thiệu

Trạng thái hiện tại: đã có nội dung hoàn chỉnh ban đầu trong `Chuong/1_Gioi_thieu.tex`. Khi các chương sau ổn định, cần quay lại rà soát thuật ngữ và mức độ khớp giữa phần đóng góp, bố cục đồ án và nội dung thực tế của Chương 3, Chương 4, Chương 5.

Mục tiêu: nêu vấn đề, động lực, hạn chế của xử lý scalar cho AI lượng tử hóa trên hệ nhúng, mục tiêu đồ án, đóng góp và bố cục báo cáo.

Nên bám cấu trúc trong template `1_Gioi_thieu.tex`:

- Đặt vấn đề.
- Các giải pháp hiện tại và hạn chế.
- Mục tiêu và định hướng giải pháp.
- Đóng góp của đồ án.
- Bố cục đồ án.

Tài liệu nội bộ nên đọc trước:

- `/home/duydonv/de2i150_cv32e40p_soc/README.md`
- `/home/duydonv/de2i150_cv32e40p_soc/HANDOFF.md`
- `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/HANDOFF.md`

Lưu ý viết: Chương này nên viết thành văn xuôi mạch lạc, tránh gạch đầu dòng trong bản nộp trừ phần liệt kê đóng góp nếu thật sự cần.

### Chương 2: Cơ sở lý thuyết

Trạng thái hiện tại: đã có bản hoàn chỉnh ban đầu trong `Chuong/2_Co_so_ly_thuyet.tex`. Chương gồm phần dẫn nhập/ngữ cảnh, nghiên cứu liên quan, suy luận AI lượng tử hóa, RISC-V và mở rộng tập lệnh, CV32E40P, gem5 và TensorFlow Lite Micro. Phần nghiên cứu liên quan trình bày theo ba hướng chính: tối ưu phần mềm, SIMD/RVV và bộ tăng tốc chuyên dụng. Phần suy luận INT8 có công thức lượng tử hóa, tích vô hướng INT8 và hình luồng tính toán tự vẽ được tách ra `Hinh_ve/int8_inference_flow.tikz`. Phần RISC-V có bảng vùng opcode custom; phần CV32E40P dùng sơ đồ khối chính thức của OpenHW; phần gem5 và TFLM được viết cô đọng để giữ dung lượng chương.

Mục tiêu: đặt nền cho người đọc hiểu vì sao cần các lệnh AI lượng tử hóa và vì sao chọn các nền tảng RISC-V/gem5/CV32E40P/TFLM.

Các nhóm nội dung nên có:

- RISC-V ISA và khái niệm mở rộng tập lệnh.
- AI lượng tử hóa INT8: MAC, dot product, activation, clamp, requantization.
- CV32E40P và CORE-V/PULP.
- gem5 và vai trò của mô phỏng kiến trúc.
- TensorFlow Lite Micro cho inference bare-metal.

Tài liệu nội bộ nên đọc:

- `/home/duydonv/de2i150_cv32e40p_soc/README.md`
- `/home/duydonv/de2i150_cv32e40p_soc/HANDOFF.md`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/TFLM.md`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/mnist_fc/README.md`

Tài liệu ngoài đã dùng trong Chương 2 gồm đặc tả RISC-V, bài TFLM đã xuất bản, tài liệu LiteRT/TFLM chính thức, tài liệu CV32E40P, bài gem5, đặc tả lượng tử hóa LiteRT và các bài khảo sát/đánh giá liên quan. Khi viết Chương 3 và Chương 5, có thể cần bổ sung thêm tài liệu CORE-V/PULP instruction hoặc tài liệu chính thức khác nếu giải thích sâu về opcode/mapping trên CV32E40P.

### Chương 3: Thiết kế và đặc tả nhóm lệnh mở rộng

Trạng thái hiện tại: đã có bản hoàn chỉnh ban đầu trong `Chuong/3_De_xuat.tex`. Phần mở đầu liên kết với Chương 2 và nêu vai trò của chương. Mục tổng quan giải pháp có sơ đồ luồng thực hiện đã tách sang `Hinh_ve/solution_overview.tikz`. Mục cơ sở lựa chọn nhóm lệnh giải thích động lực từ số phép MAC trong các lớp fully connected và chi phí vòng lặp INT8. Mục đặc tả ngữ nghĩa đã trình bày sáu lệnh `mac`, `dot4_acc`, `relu`, `clamp`, `p.lw`, `lp.setup`; các hình minh họa đều nằm trong `Hinh_ve/` và được tham chiếu trong nội dung. Mục ánh xạ đã phân biệt rõ nhánh gem5 dùng mã hóa thử nghiệm `custom-0` và nhánh DE2i-150/CV32E40P dùng CORE-V/PULP thông qua lớp firmware. Cuối chương có đoạn kết ngắn nối sang Chương 4.

Mục tiêu: trình bày giải pháp mở rộng tập lệnh và đặc tả semantics của các operation/lệnh.

Các nội dung chính:

- Tổng quan luồng giải pháp: nhận diện mẫu tính toán INT8 -> đặc tả nhóm thao tác -> mô phỏng gem5 -> triển khai SoC/firmware -> đánh giá bằng microbenchmark và mô hình suy luận INT8 trên TFLM.
- Đặc tả các operation: `mac`, `dot4_acc`, `relu`, `clamp`, `p.lw`, `lp.setup`.
- Làm rõ hai không gian triển khai:
  - gem5 dùng prototype custom-0 encoding để mô phỏng semantics;
  - kit dùng các lệnh CORE-V/PULP có sẵn trên CV32E40P thông qua hàm bao `.insn`.
- Giải thích vì sao nhánh phần cứng ánh xạ sang CORE-V/PULP thông qua firmware, đồng thời nhấn mạnh rằng phần này vẫn bao gồm cấu hình lõi, tích hợp SoC, xây dựng hàm bao, thống nhất quy ước dữ liệu và kiểm chứng trên mô hình suy luận INT8.

Tài liệu nội bộ nên đọc:

- `/home/duydonv/gem5/src/arch/riscv/insts/ai_ext.hh`
- `/home/duydonv/gem5/src/arch/riscv/insts/ai_ext.cc`
- `/home/duydonv/gem5/src/arch/riscv/isa/decoder.isa`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/ai_ops.h`
- `/home/duydonv/de2i150_cv32e40p_soc/HANDOFF.md`
- `BAN_DO_THAM_CHIEU_KY_THUAT.md`

### Chương 4: Đánh giá qua mô phỏng gem5

Trạng thái hiện tại: đã có bản hoàn chỉnh ban đầu trong `Chuong/4_Phan_tich_ly_thuyet.tex`. Tên chương trong `DoAn.tex` đã đổi thành "Đánh giá qua mô phỏng gem5". Chương gồm phần mở đầu, hiện thực nhóm lệnh trong gem5, kiểm thử chức năng, thiết lập benchmark mô phỏng, kết quả mô phỏng và phân tích xu hướng, phân tích mô hình vòng lặp phần cứng trong O3, và một đoạn kết chương nối sang Chương 5.

Mục tiêu: dùng gem5 để kiểm chứng ngữ nghĩa, kiểm tra hồi quy và phân tích xu hướng hiệu năng trước khi đối chiếu bằng số liệu thực nghiệm trên kit.

Các nội dung đã trình bày:

- Cấu hình các lệnh trong gem5: bộ giải mã, lớp lệnh, ngữ nghĩa thực thi, tải hậu tăng và trạng thái vòng lặp phần cứng.
- Các thay đổi liên quan O3/vòng lặp phần cứng: đổi hướng và cập nhật trạng thái ở ISA, thanh ghi trạng thái phụ, BAC/commit/fetch/ROB và hành vi của BTB/FDP.
- Smoke test để chứng minh ngữ nghĩa đúng: `ai_ops_smoke`, `p_lw_smoke`, `hwloop_smoke`.
- Benchmark gem5: `mac_clamp`, `dot4_acc_clamp`, `dot4_plw_lp_clamp` và microbenchmark vòng lặp phần cứng.
- Phân tích giới hạn: gem5 O3 không thay thế mô hình cycle-accurate của CV32E40P; kết quả mô phỏng dùng để giải thích xu hướng, kiểm tra logic và làm nền cho Chương 5.

Tài liệu nội bộ nên đọc:

- `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/README.md`
- `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/HANDOFF.md`
- `/home/duydonv/gem5/src/cpu/o3/HWLOOP_FE_NOTES.txt`
- `BAN_DO_THAM_CHIEU_KY_THUAT.md`, phần gem5.

### Phụ lục A: Thiết lập và tái mô phỏng nhóm lệnh mở rộng trong gem5

Trạng thái hiện tại: đã có nội dung trong `Chuong/Phu_luc_C.tex` và đã được nạp vào cuối `DoAn.tex` sau trang phụ lục. Phụ lục này không lặp lại đặc tả Chương 3 hoặc phân tích kết quả Chương 4, mà đóng vai trò tài liệu bổ sung để người đọc lần theo quy trình tái mô phỏng gem5.

Nội dung chính:

- Hai kho mã nguồn tham chiếu: bản gem5 đã chỉnh sửa của đồ án và gem5 chính gốc.
- Bản đồ các nhóm tệp liên quan đến ISA, bộ giải mã, ngữ nghĩa lệnh, tải hậu tăng, vòng lặp phần cứng, O3, kiểm thử nhanh và bài đo hiệu năng.
- Các lệnh chính để biên dịch gem5, chạy kiểm thử nhanh và chạy lại bài đo hiệu năng.
- Quy ước đọc `summary.csv`, `summary.json`, `stats.txt`, `checksum`, `numCycles`, `simInsts`, `ipc/cpi` và chỉ số đổi hướng.
- Hai đoạn mã tiêu biểu: khoanh vùng đo bằng `m5_reset_stats`/`m5_dump_reset_stats` và nhân dùng `lp.setup`, `p.lw`, `dot4_acc`.

### Chương 5: Đánh giá thực nghiệm, triển khai thực tế trên kit

Mục tiêu: trình bày phần thực nghiệm trên phần cứng thật, gồm SoC, firmware, benchmark UART, TFLM và MNIST runtime input.

Các nội dung chính:

- Môi trường: DE2i-150, CV32E40P, `COREV_PULP=1`, 50 MHz, BRAM 128 KB, UART 115200 8N1.
- Firmware benchmark 3 nhóm: `mac_clamp`, `dot4_acc_clamp`, `dot4_plw_lp_clamp`.
- Tiny AI và TFLM bring-up.
- MNIST FC INT8 `784 -> 32 -> 10`, TFLM reference và `pulp_opt`.
- UART runtime input, image-mode và GUI nếu cần đưa vào báo cáo.
- Nếu có số liệu tài nguyên/timing/power: trình bày Quartus compile, timing closure, Power Analyzer.

Tài liệu nội bộ nên đọc:

- `/home/duydonv/de2i150_cv32e40p_soc/README.md`
- `/home/duydonv/de2i150_cv32e40p_soc/HANDOFF.md`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/BENCHMARKS.md`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/TFLM.md`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/UART_RX.md`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/mnist_fc/README.md`

### Chương 6: Kết luận

Mục tiêu: kết luận theo chuẩn đồ án nghiên cứu: nhắc lại vấn đề, tóm tắt giải pháp, đóng góp, kết quả chính, hạn chế và hướng phát triển.

Các ý cần có:

- Đã đặc tả và mô phỏng các operation phục vụ AI lượng tử hóa.
- Đã triển khai và đánh giá trên kit DE2i-150/CV32E40P.
- Kết quả thực nghiệm cho thấy lợi ích rõ nhất ở phép tích vô hướng đóng gói kết hợp tải hậu tăng và vòng lặp phần cứng.
- Đã chứng minh khả năng chạy TFLM/MNIST INT8 trên SoC và tối ưu bằng CORE-V/PULP.
- Hạn chế: gem5 không khớp từng chu kỳ với board; board dùng ánh xạ CORE-V/PULP; một số tối ưu cho kernel, requantization và công suất còn có thể phát triển.

## 4. Nguyên tắc phân vai gem5 và kit trong báo cáo

- Chương 3 đặc tả operation/lệnh và mapping giữa mô phỏng với phần cứng.
- Chương 4 nói về gem5: cách cấu hình, mô hình hóa, smoke test, benchmark mô phỏng, phân tích xu hướng.
- Phụ lục A bổ sung quy trình tái mô phỏng gem5: lệnh chạy, cách đọc đầu ra và đoạn mã tiêu biểu. Phụ lục không thay thế Chương 4 và không phân tích lại bảng kết quả.
- Chương 5 nói về kit: SoC, firmware, UART benchmark, TFLM, MNIST, số liệu thực nghiệm.
- Không trộn số liệu gem5 và board như thể cùng một loại đo. Nếu so sánh, phải nói rõ khác mục tiêu và khác mô hình.

## 5. Checklist trước khi viết từng chương

- Đọc `HUONG_DAN_VIET_VA_TRICH_DAN.md` để tránh lối viết gạch đầu dòng/template.
- Với các chương còn template, xóa nội dung mẫu trong file `.tex` trước khi viết thật.
- Ghi lại nguồn nội bộ đã dùng trong quá trình viết để sau này dễ đối chiếu.
- Với kiến thức nền hoặc claim không phải do mình triển khai/đo, thêm TODO citation và cập nhật `.bib` sau.
- Với số liệu, luôn ghi nguồn log/file và ngày chạy nếu có.
- Mỗi hình, bảng, công thức phải được nhắc tới và phân tích trong đoạn văn.
