# Bản đồ tài liệu phục vụ viết báo cáo đồ án

Ngày cập nhật: 2026-06-11

Đồ án: Mở rộng tập lệnh RISC-V cho các phép toán AI lượng tử hóa, mô phỏng trên gem5 và triển khai thực nghiệm trên kit FPGA DE2i-150/CV32E40P.

File này là điểm vào của bộ tài liệu làm việc khi viết báo cáo. Repo báo cáo hiện mới chỉnh bìa; các chương còn lại về cơ bản vẫn là template. Vì vậy nội dung dưới đây không phải bản thảo cuối, mà là bản đồ để khi viết từng chương có thể biết cần đọc gì, lấy số liệu ở đâu và cần giữ chuẩn trình bày nào.

## 1. Bộ file hỗ trợ viết báo cáo

| File | Vai trò |
|---|---|
| `BAN_DO_TAI_LIEU_DO_AN.md` | Điểm vào, cấu trúc chương dự kiến, cách dùng các file phụ |
| `HUONG_DAN_VIET_VA_TRICH_DAN.md` | Quy tắc hành văn, bố cục chương, trích dẫn, chống đạo văn theo template |
| `BAN_DO_THAM_CHIEU_KY_THUAT.md` | Bản đồ kỹ thuật: gem5, SoC/FPGA, firmware, benchmark, TFLM, UART |

Khi bắt đầu viết một chương, đọc file này trước để xác định phạm vi chương, sau đó đọc hai file phụ theo nhu cầu. Các chi tiết câu chữ của từng chương sẽ chốt dần trong quá trình viết.

## 2. Trạng thái hiện tại cần nhớ

- Các file LaTeX trong `Chuong/` vẫn còn nhiều nội dung mẫu. Khi viết thật cần xóa hoặc thay toàn bộ đoạn hướng dẫn mẫu bằng nội dung đồ án.
- `Danh_sach_tai_lieu_tham_khao.bib` hiện chưa được xem như đã có đủ tài liệu liên quan cho đề tài. Những mục như RISC-V ISA, CV32E40P, gem5, TFLM, quantization INT8 là danh sách tài liệu cần bổ sung và trích dẫn sau, không phải tài liệu đã sẵn sàng.
- Các file README/HANDOFF trong repo gem5 và SoC là nguồn nội bộ để hiểu đồ án và lấy số liệu/log. Chúng không thay thế tài liệu tham khảo học thuật hoặc tài liệu chính thức trong `.bib`.
- Phần gem5 không chỉ nằm trong `tests/gem5/riscv_ai_ext`. Đồ án đã chỉnh cả phần ISA, decoder, instruction semantics, memory format, misc registers, redirect hardware loop và một số đường O3/front-end. Khi viết Chương 4 phải làm rõ điểm này.
- Phần thực nghiệm trên kit là nguồn kết quả chính. gem5 dùng để phân tích/mô phỏng/sensitivity và hỗ trợ câu chuyện kỹ thuật, không khẳng định là mô hình cycle-for-cycle của CV32E40P.

## 3. Cấu trúc báo cáo dự kiến

### Chương 1: Giới thiệu

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

Mục tiêu: đặt nền cho người đọc hiểu vì sao cần các lệnh AI lượng tử hóa và vì sao chọn các nền tảng RISC-V/gem5/CV32E40P/TFLM.

Các nhóm nội dung nên có:

- RISC-V ISA và khái niệm mở rộng tập lệnh.
- AI lượng tử hóa INT8: MAC, dot product, activation, clamp, requantization.
- CV32E40P và CORE-V/PULP.
- gem5 và vai trò của mô phỏng kiến trúc.
- FPGA/SoC bare-metal: BRAM, UART MMIO, CSR đo cycle.
- TensorFlow Lite Micro cho inference bare-metal.

Tài liệu nội bộ nên đọc:

- `/home/duydonv/de2i150_cv32e40p_soc/README.md`
- `/home/duydonv/de2i150_cv32e40p_soc/HANDOFF.md`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/TFLM.md`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/mnist_fc/README.md`

Tài liệu ngoài cần bổ sung sau vào `.bib`: RISC-V ISA manual, CV32E40P/OpenHW documentation, gem5 documentation/paper, TensorFlow Lite Micro documentation, tài liệu về integer-only/INT8 quantized inference.

### Chương 3: Phương pháp, đặc tả các lệnh

Mục tiêu: trình bày giải pháp đề xuất và đặc tả semantics của các operation/lệnh.

Các nội dung chính:

- Tổng quan luồng giải pháp: đặc tả lệnh -> mô phỏng gem5 -> triển khai SoC/firmware -> đánh giá bằng benchmark và TFLM.
- Đặc tả các operation: `mac`, `dot4_acc`, `relu`, `clamp`, `p.lw`, `lp.setup`.
- Làm rõ hai không gian triển khai:
  - gem5 dùng prototype custom-0 encoding để mô phỏng semantics;
  - kit dùng các lệnh CORE-V/PULP có sẵn trên CV32E40P thông qua wrapper `.insn`.
- Giải thích vì sao chọn mapping CORE-V/PULP thay vì tự viết toàn bộ custom RTL riêng ngay từ đầu.

Tài liệu nội bộ nên đọc:

- `/home/duydonv/gem5/src/arch/riscv/insts/ai_ext.hh`
- `/home/duydonv/gem5/src/arch/riscv/insts/ai_ext.cc`
- `/home/duydonv/gem5/src/arch/riscv/isa/decoder.isa`
- `/home/duydonv/de2i150_cv32e40p_soc/firmware/ai_ops.h`
- `/home/duydonv/de2i150_cv32e40p_soc/HANDOFF.md`
- `BAN_DO_THAM_CHIEU_KY_THUAT.md`

### Chương 4: Phân tích lý thuyết, đánh giá qua mô phỏng gem5

Mục tiêu: biến chương "Phân tích lý thuyết" thành chương phân tích bằng mô phỏng và mô hình hóa. Chương này tập trung vào gem5, operation count, expected trend, smoke/regression và sensitivity.

Các nội dung chính:

- Cấu hình các lệnh trong gem5: decoder, instruction class, semantics, post-increment load, hardware-loop state.
- Các thay đổi liên quan O3/hardware loop: redirect/update ở ISA, misc registers, BAC/commit/fetch/ROB, BTB/FDP behavior.
- Smoke test để chứng minh semantics đúng: `ai_ops_smoke`, `p_lw_smoke`, `hwloop_smoke`.
- Benchmark gem5: `mac_clamp`, `dot4_acc_clamp`, `dot4_plw_lp_clamp`, hardware-loop microbenchmark.
- Phân tích giới hạn: gem5 O3 không thay thế cycle-accurate CV32E40P; kết quả mô phỏng dùng để giải thích xu hướng và kiểm tra logic.

Tài liệu nội bộ nên đọc:

- `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/README.md`
- `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/HANDOFF.md`
- `/home/duydonv/gem5/src/cpu/o3/HWLOOP_FE_NOTES.txt`
- `BAN_DO_THAM_CHIEU_KY_THUAT.md`, phần gem5.

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
- Kết quả thực nghiệm cho thấy lợi ích rõ nhất ở packed dot product kết hợp post-increment load và hardware loop.
- Đã chứng minh khả năng chạy TFLM/MNIST INT8 trên SoC và tối ưu bằng CORE-V/PULP.
- Hạn chế: gem5 không cycle-for-cycle với board; board dùng CORE-V/PULP mapping; một số tối ưu kernel/requantization/power còn có thể phát triển.

## 4. Nguyên tắc phân vai gem5 và kit trong báo cáo

- Chương 3 đặc tả operation/lệnh và mapping giữa mô phỏng với phần cứng.
- Chương 4 nói về gem5: cách cấu hình, mô hình hóa, smoke test, benchmark mô phỏng, phân tích xu hướng.
- Chương 5 nói về kit: SoC, firmware, UART benchmark, TFLM, MNIST, số liệu thực nghiệm.
- Không trộn số liệu gem5 và board như thể cùng một loại đo. Nếu so sánh, phải nói rõ khác mục tiêu và khác mô hình.

## 5. Checklist trước khi viết từng chương

- Đọc `HUONG_DAN_VIET_VA_TRICH_DAN.md` để tránh lối viết gạch đầu dòng/template.
- Xóa nội dung mẫu trong file `.tex` của chương trước khi viết thật.
- Ghi lại nguồn nội bộ đã dùng trong quá trình viết để sau này dễ đối chiếu.
- Với kiến thức nền hoặc claim không phải do mình triển khai/đo, thêm TODO citation và cập nhật `.bib` sau.
- Với số liệu, luôn ghi nguồn log/file và ngày chạy nếu có.
- Mỗi hình, bảng, công thức phải được nhắc tới và phân tích trong đoạn văn.

