# Bản đồ tham chiếu kỹ thuật

Ngày cập nhật: 2026-06-23

File này giúp tra cứu nhanh khi viết và rà soát phần kỹ thuật của báo cáo. Nó tách rõ phần gem5, phần SoC/FPGA, firmware/benchmark và TFLM. Tính đến bản cập nhật này, Chương 2 đã hoàn thiện phần nền tảng lý thuyết; Chương 3 đã hoàn thiện phần thiết kế, đặc tả ngữ nghĩa và ánh xạ nhóm lệnh mở rộng sang gem5 và DE2i-150/CV32E40P; Chương 4 đã hoàn thiện phần đánh giá qua mô phỏng gem5; Chương 5 đã hoàn thiện phần triển khai và đánh giá trên kit, gồm SoC/firmware, benchmark INT8, biểu đồ tỷ lệ chu kỳ/lệnh hoàn tất, ước lượng năng lượng, TensorFlow Lite Micro, MNIST FC, UART/GUI, tài nguyên và timing; Chương 6 đã hoàn thiện phần kết luận và hướng phát triển trong tương lai. Báo cáo hiện có hai phụ lục kỹ thuật: phụ lục gem5 để tái mô phỏng và phụ lục SoC để tái lập triển khai Quartus/DE2i-150. File này tiếp tục đóng vai trò bản đồ đối chiếu khi rà soát số liệu, câu chữ kỹ thuật và các lệnh tái lập.

## 1. Nguyên tắc diễn đạt kỹ thuật

- Không viết rằng toàn bộ phần gem5 chỉ nằm trong `tests/gem5/riscv_ai_ext`. Thư mục đó là khung test/benchmark; các thay đổi mô hình nằm trong `src/`.
- Không viết rằng board dùng đúng opcode custom-0 của gem5. gem5 dùng prototype custom-0 để mô phỏng ngữ nghĩa; board dùng ánh xạ CORE-V/PULP trên CV32E40P.
- Không viết rằng gem5 là mô hình khớp từng chu kỳ của kit. gem5 dùng để kiểm tra ngữ nghĩa, kiểm tra hồi quy và phân tích xu hướng.
- Khi nói "mở rộng tập lệnh", cần làm rõ phạm vi: đồ án đặc tả/đồng bộ ngữ nghĩa các thao tác AI và triển khai qua hai môi trường, trong đó phần cứng thực tế tận dụng các lệnh CORE-V/PULP có sẵn của CV32E40P.
- Chương 2 chỉ trình bày nền tảng ở mức tổng quan; Chương 3 đã chốt mức đặc tả và ánh xạ tổng quan. Chương 4 đã trình bày các chi tiết gem5, smoke test, benchmark và số liệu mô phỏng; số liệu thực nghiệm trên kit cần để sang Chương 5.
- Phụ lục gem5 chỉ nên đóng vai trò thông tin bổ sung để tái mô phỏng: kho mã nguồn, bản đồ tệp, lệnh chạy, cách đọc đầu ra và đoạn mã tiêu biểu. Không đưa thêm phân tích kết quả mới vào phụ lục nếu nội dung đó thuộc Chương 4 hoặc Chương 5.
- Trong Chương 5, giữ thứ tự phương pháp đo đã chốt: cách đọc `mcycle`/`minstret`, công thức tính speedup, bảng mốc so sánh, biểu đồ tỷ lệ chu kỳ/lệnh hoàn tất, điều kiện kiểm chứng đúng/sai, khác biệt giữa khoảng đo có kiểm chứng và khoảng đo riêng phần suy luận, rồi mới tới quy đổi năng lượng tương đối.
- Phụ lục SoC chỉ nên là hướng dẫn tái lập triển khai: repo, môi trường, tạo project Quartus, biên dịch firmware, kiểm tra LED/UART và tạo VCD. Không đưa thêm phân tích hiệu năng mới vào phụ lục nếu nội dung đó thuộc Chương 5.
- Chương 6 chỉ tổng kết các kết quả đã chứng minh trong các chương trước. Không thêm claim kỹ thuật hoặc số liệu mới ở chương kết luận nếu chưa được trình bày và kiểm chứng ở Chương 4/Chương 5.

## 2. Bản đồ gem5

### 2.1 Ngữ nghĩa lệnh và bộ giải mã

| File | Vai trò |
|---|---|
| `/home/duydonv/gem5/src/arch/riscv/insts/ai_ext.hh` | Khai báo lớp lệnh cho `mac`, `dot4_acc`, `relu`, `clamp`, `lp.setup` |
| `/home/duydonv/gem5/src/arch/riscv/insts/ai_ext.cc` | Ngữ nghĩa thực thi, disassembly, cờ lệnh và cập nhật thanh ghi trạng thái phụ cho `lp.setup` |
| `/home/duydonv/gem5/src/arch/riscv/isa/decoder.isa` | Giải mã opcode custom-0, gán format cho các lệnh |
| `/home/duydonv/gem5/src/arch/riscv/isa/formats/custom.isa` | Format/constructor plumbing cho lệnh custom |
| `/home/duydonv/gem5/src/arch/riscv/isa/formats/mem.isa` | Template/format cho post-increment load |
| `/home/duydonv/gem5/src/arch/riscv/insts/mem.hh` | Thêm lớp `PostIncLoad` |
| `/home/duydonv/gem5/src/arch/riscv/insts/mem.cc` | Disassembly cho `p.lw` dạng post-increment |
| `/home/duydonv/gem5/src/arch/riscv/isa/operands.isa` | Operand `Rpw` để ghi lại `rs1` sau post-increment |

Semantics cần ghi trong báo cáo:

| Operation | Ngữ nghĩa ngắn |
|---|---|
| `mac` | `rd = rd + rs1 * rs2`, signed multiply-accumulate |
| `dot4_acc` | `rd = rd + sum((int8)rs1.byte[i] * (int8)rs2.byte[i])` với 4 lane thấp |
| `relu` | signed max với 0 |
| `clamp` | chặn signed `rs1` vào `[0, rs2]`, `rs2` là upper bound |
| `p.lw` | load word tại địa chỉ `rs1`, sau đó `rs1 += sext(imm12)` |
| `lp.setup` | thiết lập `LPSTART`, `LPEND`, `LPCOUNT`, dùng immediate thô CORE-V |

Trạng thái báo cáo: Chương 3 đã dùng các semantics trên để viết phần đặc tả ở mức kiến trúc. Chương 4 đã chuyển trọng tâm sang cách gem5 hiện thực, kiểm thử và đánh giá các ngữ nghĩa đó qua mô phỏng.

### 2.2 Trạng thái vòng lặp phần cứng và đổi hướng ISA

| File | Vai trò |
|---|---|
| `/home/duydonv/gem5/src/arch/riscv/regs/misc.hh` | Thêm/định nghĩa các thanh ghi trạng thái phụ liên quan vòng lặp phần cứng như `LPSTART`, `LPEND`, `LPCOUNT`, `LPACTIVE`, `LPGEN`, `LPSETUP_PC` |
| `/home/duydonv/gem5/src/arch/riscv/isa.cc` | `redirectHardwareLoop()` và `updateHardwareLoopState()` cho đường PC update |
| `/home/duydonv/gem5/src/arch/riscv/isa.hh` | Khai báo các hook liên quan ISA PC/hardware loop nếu cần đối chiếu |

Điểm cần giải thích:

- `lp.setup` công bố trạng thái vòng lặp để các lệnh trẻ hơn có thể quan sát được trạng thái này.
- `LPEND` trong gem5 được lưu theo tail PC để phù hợp đường so sánh PC hiện tại.
- O3 cần xử lý trường hợp tail trẻ hơn thực thi trước khi tail cũ commit, nên có logic ước lượng/forward count.

### 2.3 Mô hình vòng lặp phần cứng trong O3/khối lấy lệnh

| File | Vai trò |
|---|---|
| `/home/duydonv/gem5/src/cpu/o3/HWLOOP_FE_NOTES.txt` | Ghi chú thiết kế đường xử lý nhanh cho vòng lặp phần cứng, BAC shadow, BTB priming, FT marker |
| `/home/duydonv/gem5/src/cpu/o3/bac.cc` | Logic BAC/khối lấy lệnh tách rời liên quan dự đoán vòng lặp phần cứng và FT |
| `/home/duydonv/gem5/src/cpu/o3/bac.hh` | Trạng thái và API cho dự đoán vòng lặp phần cứng trong BAC |
| `/home/duydonv/gem5/src/cpu/o3/commit.cc` | Duy trì/prime BTB ở loop tail khi commit |
| `/home/duydonv/gem5/src/cpu/o3/fetch.cc` | Resteer/resync liên quan khối lấy lệnh khi cần |
| `/home/duydonv/gem5/src/cpu/o3/rob.hh` và file triển khai tương ứng | Đếm older executed entries tại LPEND phục vụ O3 forwarding |

Chương 4 đã trình bày phần này ở mức:

- Đường đổi hướng ở mức ISA đúng chức năng.
- Khối lấy lệnh tách rời của O3 cần thêm cơ chế để giảm số lần đổi hướng ở thời điểm execute đối với vòng lặp phần cứng.
- Microbenchmark riêng cho vòng lặp phần cứng cho thấy phần lớn lần quay lại đầu vòng được xử lý sớm ở O3 có cache, nhưng chi phí sửa PC còn lại vẫn làm `hwloop` chậm hơn `swloop` trong kịch bản này.

### 2.4 gem5 test và benchmark harness

| File/thư mục | Vai trò |
|---|---|
| `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/README.md` | Tài liệu chạy smoke/perf, nhóm benchmark và trạng thái đồng bộ hiện tại |
| `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/HANDOFF.md` | Trạng thái bàn giao, số liệu mô phỏng, diễn giải |
| `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/src/` | Assembly smoke tests |
| `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/perf_src/` | Benchmark `mac_clamp`, `dot4_acc_clamp`, `dot4_plw_lp_clamp` |
| `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/hwloop_perf_src/` | Microbenchmark riêng cho hardware loop |
| `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/configs/perf_binary_run.py` | gem5 config cho perf binaries |
| `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/run_perf_compare.py` | Chương trình chạy so sánh baseline/update/custom |
| `/home/duydonv/gem5/tests/gem5/riscv_ai_ext/run_hwloop_perf_compare.py` | Chương trình chạy microbenchmark vòng lặp phần cứng |

Smoke coverage cần nêu:

- `ai_ops_smoke`: `mac`, `dot4_acc`, `relu`, `clamp`, hành vi với `x0`.
- `p_lw_smoke`: tải trước khi cập nhật hậu tăng, bỏ ghi khi `rd=x0`.
- `hwloop_smoke`: hành vi đổi hướng, trường hợp bộ đếm bằng 0, immediate thô, chạy trên `atomic`, `minor`, `o3`.

### 2.5 Phụ lục gem5 trong quyển báo cáo

| File | Vai trò |
|---|---|
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Chuong/Phu_luc_C.tex` | Phụ lục thiết lập và tái mô phỏng nhóm lệnh mở rộng trong gem5 |
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/DoAn.tex` | Nạp phụ lục sau trang "PHỤ LỤC"; do hai phụ lục mẫu A/B đang comment nên phụ lục gem5 hiển thị là Phụ lục A |

Nội dung phụ lục đã chốt:

- Dùng kho mã nguồn gem5 đã chỉnh sửa tại `https://github.com/duydonv/gem5.git` và kho gem5 chính gốc tại `https://github.com/gem5/gem5.git` làm mốc tham chiếu.
- Tóm tắt nhóm tệp gem5 theo vai trò: ISA/bộ giải mã, lớp lệnh, tải hậu tăng, vòng lặp phần cứng, O3, kiểm thử nhanh và bài đo hiệu năng.
- Ghi các lệnh chính để biên dịch gem5, chạy kiểm thử nhanh và chạy lại ba cấu hình bài đo hiệu năng.
- Giải thích cách đọc các trường số liệu chính trong kết quả mô phỏng, nhưng không phân tích lại bảng kết quả của Chương 4.
- Minh họa cách khoanh vùng đo bằng `m5_reset_stats`/`m5_dump_reset_stats`.
- Đối chiếu từng dòng hợp ngữ trong nhân dùng `lp.setup`, `p.lw`, `dot4_acc` để người đọc hiểu rõ đoạn mã Assembly.

## 3. Bản đồ SoC/FPGA

| File/thư mục | Vai trò |
|---|---|
| `/home/duydonv/de2i150_cv32e40p_soc/README.md` | Tổng quan SoC, trạng thái hiện tại, directory layout, FPGA gotchas |
| `/home/duydonv/de2i150_cv32e40p_soc/HANDOFF.md` | Bàn giao đầy đủ, quyết định kỹ thuật, trạng thái milestone |
| `/home/duydonv/de2i150_cv32e40p_soc/rtl/soc/de2i150_cv32e40p_top.v` | Top-level SoC: CV32E40P, BRAM, UART, LED, OBI shim |
| `/home/duydonv/de2i150_cv32e40p_soc/rtl/fpga/cv32e40p_clock_gate.sv` | Clock gate FPGA-safe |
| `/home/duydonv/de2i150_cv32e40p_soc/rtl/core/` | Local CV32E40P core copy và patch cho Quartus/FPGA timing |
| `/home/duydonv/de2i150_cv32e40p_soc/fpga_patches/README.md` | Ghi chú patch SV-2012 -> SV-2005/Quartus |
| `/home/duydonv/de2i150_cv32e40p_soc/de2i150_cv32e40p_top.sdc` | Timing constraints |
| `/home/duydonv/de2i150_cv32e40p_soc/PIN_ASSIGNMENTS.md` | Pin mapping |
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Chuong/Phu_luc_D.tex` | Phụ lục hướng dẫn tái lập triển khai SoC CV32E40P trên kit DE2i-150 |
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Hinh_ve/quartus_cv32e40p_core_rtl_view.png` | Ảnh RTL view lõi CV32E40P từ Quartus, dùng ở Mục 5.1 để minh họa phần lõi trong thiết kế tổng hợp |
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Hinh_ve/de2i150_soc_experiment_flow.tikz` | Hình tự vẽ về luồng triển khai và thu kết quả trên DE2i-150/CV32E40P |
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Hinh_ve/fpga_timing_bubble.tikz` | Hình tự vẽ minh họa chèn một chu kỳ rỗng để ngắt phụ thuộc/timing path dài |
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Hinh_ve/questa_dot4_plw_lp_clamp_custom_waveform.png` | Ảnh waveform Questa cho kịch bản `dot4_plw_lp_clamp_custom`, dùng để minh họa luồng mô phỏng tạo VCD |
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Hinh_ve/mnist_fc_tflm_flow.tikz` | Hình tự vẽ về luồng huấn luyện/lượng tử hóa MNIST FC và hai đường chạy `tflm_ref`/`pulp_opt` trên kit |
| `/home/duydonv/Downloads/20225608_PhamDuyDong_20252/Hinh_ve/mnist_fc_uart_gui_200_samples_no_true_label_check.png` | Ảnh giao diện MNIST UART GUI khi chạy 200 ảnh PGM qua UART |

Các điểm kỹ thuật đã dùng trong Chương 5 và Phụ lục B:

- `COREV_PULP=1`, `FPU=0`, `COREV_CLUSTER=0`.
- `FPGA_TIMING_MODE=1` để đóng timing 50 MHz trên Cyclone IV.
- Firmware build `rv32im_zicsr`, không dùng `rv32imc` trong giai đoạn benchmark để hardware loop word-aligned và đọc CSR.
- BRAM 128 KB, byte-lane M9K, UART TX/RX MMIO.
- Các chỉnh sửa tương thích SystemVerilog-2005/Quartus diễn ra từ giai đoạn tích hợp lõi lên FPGA, trước khi đi sâu vào bật PULP và benchmark. Khi trình bày cần tránh tạo cảm giác các patch SV-2012 sang SV-2005 chỉ xuất hiện ở giai đoạn bật PULP.
- Mục 5.2 đã trình bày việc xử lý ràng buộc thời gian khi bật PULP theo hướng chèn chu kỳ rỗng ở đường phụ thuộc dài; số liệu benchmark sau đó đã bao gồm chi phí của chu kỳ rỗng này.
- Phụ lục B đã ghi lại luồng tạo project Quartus từ repo public. Các tệp `.qpf`/`.qsf`, `third_party/tflm_tree/`, firmware hex và output Quartus là sản phẩm sinh cục bộ, không được theo dõi trong repo public.

## 4. Firmware, benchmark và TFLM

| File/thư mục | Vai trò |
|---|---|
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/ai_ops.h` | Wrapper `.insn` cho CORE-V/PULP operations |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/benchmark.c` | Benchmark baseline vs custom trên board |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/BENCHMARKS.md` | Mô tả benchmark groups, operation counts, UART output |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/perf.h` | CSR cycle/instret, UART, LED helper |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/tiny_ai.c` | Tiny INT8 MLP pre-TFLM |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/TFLM.md` | Bring-up TFLM, toolchain, generated tree, firmware modes |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/tflm_mnist_fc.cc` | MNIST fixed-vector TFLM ref-vs-opt firmware |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/tflm_mnist_uart.cc` | MNIST runtime input over UART |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/UART_RX.md` | UART RX MMIO, frame protocol, runners |
| `/home/duydonv/de2i150_cv32e40p_soc/firmware/mnist_fc/README.md` | MNIST model artifacts, accuracy, board result |

Trạng thái trong báo cáo: Chương 5 đã dùng `ai_ops.h` để trình bày lớp hàm bao firmware và kiểm thử smoke. Bảng ánh xạ giữ cột lệnh CORE-V/PULP tương ứng, còn các bảng kết quả sau đó dùng tên giao diện `ai_*`/`AI_*` để tránh lặp lại chi tiết `cv.*` sau khi đã ánh xạ. Các mục 5.5--5.7 đã trình bày benchmark INT8, biểu đồ tỷ lệ chu kỳ/lệnh hoàn tất, năng lượng ước lượng, TFLM/MNIST FC, fixed-vector và UART image mode; Mục 5.8 ghi lại tài nguyên/timing của cấu hình MNIST FC cuối chương.

## 5. Mapping operation giữa báo cáo, gem5 và board

| Operation trong báo cáo | gem5 | Board CV32E40P |
|---|---|---|
| `mac` | custom-0 `mac` trong decoder/`ai_ext` | `cv.mac` qua hàm bao firmware |
| `dot4_acc` | custom-0 `dot4_acc` | `cv.sdotsp.b` |
| `relu` | custom-0 `relu` | `cv.max` hoặc logic tương đương tùy firmware |
| `clamp` | custom-0 `clamp`, upper bound từ `rs2` | `cv.clipur`/hàm bao ở benchmark; với TFLM signed INT8 có chỗ giữ scalar để đúng TFLite |
| `p.lw` | `PostIncLoad`, load-before-update | `cv.lw` post-increment |
| `lp.setup` | custom-0 `lp.setup`, immediate thô CORE-V | `cv.setup` hardware loop |

## 6. Số liệu hiện có để đối chiếu khi viết

Các số liệu này lấy từ README/HANDOFF hiện tại. Khi đưa vào báo cáo cuối, cần kiểm tra lại log mới nhất.

| Nhóm | Số liệu |
|---|---|
| Board `mac_clamp` | `1.07x`, pass checksum |
| Board `dot4_acc_clamp` | `6.33x`, pass checksum |
| Board `dot4_plw_lp_clamp` | `13.10x`, pass checksum |
| Tiny AI pre-TFLM | `7.09x`, accuracy `8/8`, checksum `0x6c75e8cf` |
| TFLM hello_world | checksum `0x3a357ded`, pass |
| TFLM tiny MLP fixed | ref `167507` cycles, opt `29620` cycles, `5.66x` |
| TFLM tiny UART | khoảng `5.61x`, 8/8 frames pass |
| MNIST FC host | float accuracy `96.30%`, INT8 accuracy `96.28%`, model `28368` bytes |
| MNIST FC fixed board | validated `12.39x`, inference-only `12.36x`, expected class `32/32`, checksum `0x00cb95fc` |
| MNIST UART image mode | GUI/PGM batch `200/200` host pass, true-label match `199/200`, score mismatches `0`, aggregate `12.52x` |
| gem5 cached O3 | `mac_clamp` `0.9852x`, `dot4_acc_clamp` `2.9276x`, `dot4_plw_lp_clamp` `2.8513x` |
| gem5 NoCache DDR3 | `mac_clamp` `1.0024x`, `dot4_acc_clamp` `3.2576x`, `dot4_plw_lp_clamp` `9.6348x` |
| gem5 NoCache fast-ram | `mac_clamp` `1.0007x`, `dot4_acc_clamp` `3.2525x`, `dot4_plw_lp_clamp` `8.6644x` |
| gem5 hardware-loop microbenchmark O3 có cache | `hwloop` so với `swloop`: `0.6394x` small, `0.6363x` medium, `0.6318x` large; tỷ lệ xử lý sớm `96.77%` |
| gem5 hardware-loop microbenchmark NoCache large | DDR3 `1.3928x`; fast-ram `1.3696x` |

## 7. Lệnh chạy tham chiếu

gem5 smoke:

```bash
python3 tests/gem5/riscv_ai_ext/build_binaries.py
cd tests
./main.py run gem5/riscv_ai_ext --length=quick --skip-build
```

gem5 perf:

```bash
python3 tests/gem5/riscv_ai_ext/build_perf_binaries.py
python3 tests/gem5/riscv_ai_ext/run_perf_compare.py --skip-build
python3 tests/gem5/riscv_ai_ext/run_perf_compare.py --skip-build --no-cache
python3 tests/gem5/riscv_ai_ext/run_perf_compare.py --skip-build --no-cache --memory fast-ram
```

Board benchmark firmware:

```bash
cd /home/duydonv/de2i150_cv32e40p_soc/firmware
make benchmark
```

MNIST UART firmware/runner:

```bash
cd /home/duydonv/de2i150_cv32e40p_soc/firmware
make tflm_mnist_uart CROSS=/home/duydonv/tools/xpack-riscv-none-elf-gcc/xpack-riscv-none-elf-gcc-14.2.0-3/bin/riscv-none-elf-
python3 tflm_mnist_uart_runner.py /dev/ttyUSB0
python3 tflm_mnist_uart_runner.py /dev/ttyUSB0 --images-dir --limit 10
```
