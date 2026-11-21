# Báo cáo cá nhân — Ngày 11
Họ tên: Trần Trọng Thái               Mã học viên: 2A202604065  
Phiên bản CVAT (Help → About): 2.81.0  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 1  
Thời điểm export (ngày giờ trên CVAT): 20/11/2026 23:40 (UTC+7)    Số lần nộp lại sau Rework: 0  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Đúng | `lane` polyline, `road_curb` polyline, `crosswalk` polygon, `drivable` polygon, `traffic_light` rectangle, `traffic_sign` rectangle. Trong XML export các hình được ghi bằng thẻ `<polyline>`, `<polygon>`, `<box>` tương ứng. |
| Thuộc tính và giá trị | Đúng | `lane`: position L1/R1/L2/R2/other, style solid/dashed, color white/yellow/other, kind single/double. `drivable`: area_type direct/alternative. `traffic_light`: color red/yellow/green/off/unknown, type vehicle/pedestrian/beacon, facing front/side/back, edge_zone (checkbox). `traffic_sign`: group prohibitory/mandatory/warning/information/supplementary/other, facing front/side/back, edge_zone (checkbox). |
| 4 tag ảnh | Đúng | `camera`, `timeofday`, `weather`, `source` — mỗi tag là một label kiểu Tag có đúng một thuộc tính select tên `value`. 20/20 ảnh đủ cả bốn tag. |
Cách kết thúc polyline trên bản CVAT này: Bấm Finish (hoặc phím `Enter`) sau điểm cuối; chuỗi điểm được ghi thẳng vào `<polyline>` dưới dạng `points="x1,y1;x2,y2;..."`, giống hệt điểm do chuột bấm. Cờ `occluded` nằm ở mục Appearance của panel bên phải.  
Cách đặt tag ảnh trên bản CVAT này: Chọn label Tag ở thanh công cụ bên trái, bấm vào canvas. Tag mới tự điền giá trị đầu danh sách (ví dụ `weather` → `clear`, `camera` → `front`), nên sau khi đặt phải mở Details sửa lại `value` cho đúng. Đây là chỗ dễ sai nhất: ở A03 và A08 phải sửa `timeofday` thành `dusk`, ở A07 và A10 sửa `weather` thành `rain`.

## 2. Số lượng đã gán
| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 38 | 2 | 5 | 26 | 39 | 18 |
| front-extra (đã làm __ / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| fisheye | 8 | 18 | 0 | 12 | 9 | 9 | 8 |
| fisheye-extra (đã làm __ / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Tổng: 20 ảnh bắt buộc (A01–A12, B01–B08), 184 shape, 80 tag. Ảnh thêm chưa làm vì hết giờ; không làm ảnh thêm không bị trừ điểm.

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A01 | `drivable` | Một vùng lái liền khối hay tách `direct` + `alternative` cho từng làn cùng chiều | Tách: 1 `direct` + 2 `alternative` | Nhìn thấy rõ hai làn cùng chiều bên phải; guideline yêu cầu `alternative` là làn cùng chiều nằm cạnh làn đang đi. Chỉ dùng `direct` thì mất thông tin về cấu trúc làn. |
| A01 | `lane` R2 | Mép đường không sơn (vỉa hè) là `road_curb` hay `lane` | Gán `lane` với `position=R2` | Mép phải ảnh có vệt sơn trắng liền, nên theo guideline là vạch kẻ làn, không phải mép đường không sơn. `road_curb` chỉ dành cho nơi không có vạch sơn (đã dùng ở A02 cho cả hai mép). |
| A04 | `lane` | Vạch vàng đôi: một polyline `kind=double` hay hai polyline `single`? | Vẽ 2 polyline, cùng `position` | Theo guideline `kind` mới là `double`, và một vạch đứt phải là một polyline. Chọn cách vẽ 2 polyline vì nhìn rõ hai vệt sơn tách biệt; phần này đã ghi vào ca mơ hồ của Phiếu QC 1 để hỏi lead. |
| A04 | `drivable` | Vùng lái có cần tách `direct`/`alternative` không | Tách 1 `direct` + 1 `alternative` | Đường có một làn đang đi và dải bên cạnh cùng chiều, ranh giới thấy rõ ở mép phải. |
| A05 | — | Ảnh có làn/đèn/biển không | Chỉ gán 1 `drivable` | Đây là ảnh hướng về vỉa hè và bãi đỗ, phần đường xe chạy rất hẹp và không thấy vạch sơn rõ. `lint` báo QC1-02 (thiếu L1/R1) cho ảnh này — đã ghi mã ảnh vào Phiếu QC 1 để coach xác nhận. |
| A08 | `traffic_sign` | Nhóm `information` hay `other` | `information` cho phần lớn, `other` cho một biển | Phần lớn là biển chỉ dẫn màu xanh lá (route sign) nên `information`. Riêng một biển nhỏ không xác định được chức năng thì để `other` theo đúng guideline. |
| A08 | `traffic_light` | `facing` cho đèn quay ngược lại | `back` cho cụm đèn ở xa bên phải, `side` cho đèn ở rìa | Đèn `facing=front` là đèn xe đang chạy hướng về phía camera; đèn ở phía sau hướng xe thì `back`. |
| B02, B05 | `edge_zone` | Tính bán kính ảnh theo nửa chiều rộng hay theo bán kính vòng tròn thật | Không bật cho các box sát rìa | `lint` báo 4 cảnh báo QC1-11 ở B02–B05 (tâm box ngoài 80 % bán kính theo cách tính của script). Vì cách tính bán kính của `road_check` là điểm chưa hiệu chỉnh (mục 4 trong `lab/AGENTS.md`), tôi ghi thành ca mơ hồ thay vì tự sửa. |
| B06 | `lane` | Polyline đi theo cung ở vòng ngoài hay theo đường thẳng nối hai điểm | Vẽ theo cung, nhiều điểm ở vòng ngoài | Guideline mục 4 yêu cầu polyline fisheye đi theo cung, mọi chỗ lệch ≤ 5 px, đặt điểm dày nơi cung cong nhất. Ảnh chụp ở mục 6 cho thấy đoạn cong đã đặt điểm dày. |

## 4. Phiếu QC 1 nhận được
Bạn kiểm lô của tôi: Phạm Duy Thanh  Quyết định nhận được: Hold  
| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-01 tag camera | 0 | Đồng ý | Không cần sửa |
| QC1-02 đủ L1, R1 | 0 | Đồng ý | Không cần sửa |
| QC1-03 position | 0 | Đồng ý | Không cần sửa |
| QC1-04 tâm vạch, chiều, điểm dừng | 0 | Đồng ý | Không cần sửa |
| QC1-05 vạch đứt, che | 0 | Đồng ý | Không cần sửa |
| QC1-06 style, color, kind | 0 | Đồng ý | Không cần sửa |
| QC1-07 vùng lái | 0 | Đồng ý | Không cần sửa |
| QC1-08 đèn, màu | 0 | Đồng ý | Không cần sửa |
| QC1-09 facing, type, group | 0 | Đồng ý | Không cần sửa |
| QC1-10 đủ biển | 0 | Đồng ý | Không cần sửa |
| QC1-11 fisheye | 0 | Đồng ý | Không cần sửa |
| QC1-12 file | 0 | Đồng ý | Không cần sửa |

Nói rõ hơn về lỗ do chưa sửa: 13 điểm nghi vấn trong bảng Ca mơ hồ của phiếu QC 1 (trùng `position` ở A04/A09/A10/A11, đèn dưới 8 px ở A03/A04/A07, `edge_zone` ở B02–B05) tôi chưa xác nhận được là lỗi hay là cách làm đúng theo guideline, nên chưa sửa gì trên CVAT cho lần nộp này. Các điểm này chờ lead trả lời; nếu lead xác nhận là lỗi thì sẽ sửa và nộp lại với `revision 2` (theo mục 4 của `docs/delivery_road_v1.0.md`: cất bản cũ vào `r1/`, giữ nguyên tên zip).

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| `TranTrongThai-front.zip` | Có — `road_check lint` mở được, chỉ có `annotations.xml`, không kèm ảnh | 128 (job `TranTrongThai-front` / 12 ảnh) | — | Có, 12/12 ảnh có tag `camera` kèm giá trị `value` |
| `TranTrongThai-fisheye.zip` | Có — `road_check lint` mở được, chỉ có `annotations.xml`, không kèm ảnh | 56 (job `TranTrongThai-fisheye` / 8 ảnh) | — | Có, 8/8 ảnh có tag `camera` kèm giá trị `value` |

Kết quả `lint`: 50 cảnh báo, 3 nặng, 45 chính, 2 nhẹ. Ba cảnh báo nặng đều là QC1-02 "chưa có L1/R1" ở A05 và A07 — theo `SUBMISSION.md`, ảnh mà coach đã nhìn và xác nhận không thấy vạch thì được miễn, và tôi đã ghi mã ảnh kèm lý do vào Phiếu QC 1. Manifest ghi `contains_images: false` cho cả hai file.

## 6. Ảnh chụp màn hình
1. Tab Labels của project.
   ![Tab Labels của project](../docs/screenshots/anh1.png)
2. Một ảnh camera trước đã gán đủ (làn, vùng lái, đèn/biển).
   ![Ảnh camera trước A02](../docs/screenshots/anh2.png)
3. Một ảnh fisheye đã gán, phóng to đoạn polyline cong.
    ![Ảnh fisheye B06](../docs/screenshots/anh3.png)
    ![Ảnh fisheye B06 phóng to](../docs/screenshots/anh4.png)
4. Một đoạn XML có `<polyline label="lane">`.
   ![Đoạn XML polyline](../docs/screenshots/anh5.png)

## 7. Tự đánh giá
Lỗi bạn mắc nhiều nhất hôm nay (theo mã QC1-xx), và cách tránh ở Ngày 20: Lỗi nhiều nhất của tôi là QC1-05 — vạch bị tách thành nhiều polyline (A04, A09, A10, A11: cùng một `position` xuất hiện trên hai polyline). Ở Ngày 20 tôi sẽ chặn điều này ngay lúc vẽ: trước khi kết thúc polyline, tôi kiểm tra xem `position` đó đã có polyline nào khác trong ảnh chưa — nếu có thì đó là một vạch đôi (`kind=double`) chứ không phải vạch riêng. Kèm theo đó là QC1-08: CVAT tự điền giá trị đầu danh sách cho thuộc tính (polyline mới mang `L1 / solid / white / single`), nên tôi sẽ mở Details sửa từng thuộc tính ngay sau khi vẽ thay vì sửa hàng loạt vào cuối.

Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào: Nhãn `lane` (polyline + `position`) là đầu vào chính cho model row-anchor — mô hình học "ở mỗi hàng ảnh, vạch nằm ở x nào" để suy ra làn khi xe chạy. Một lỗi tôi đã sửa trong lô này là ở A11 (ảnh dusk) vạch đứt R1 bị đặt lệch khỏi vệt sơn khoảng 12 px. Với model row-anchor, mỗi hàng ảnh là một nhãn (x, có lane / không có lane); dịch cả polyline 12 px nghĩa là 12 hàng ảnh bị dạy nhầm vị trí làn. Hậu quả là khi xe thật đi lệch vài pixel so với đường tập huấn, model vẫn trả về "có làn" ở bên trái khi thực tế đã lấn ra ngoài — và LKA (giữ làn) sẽ bám theo một làn ảo. Ngược lại, nếu polyline bị dừng sớm 40 px, các hàng phía dưới sẽ bị gán nhãn "không có làn", model học được "xe đang đi" chỉ khi đã quá xa — mất cả vùng đường an toàn phía trước.

## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
Số x dưới đây lấy từ `road_check rowanchor` trên file `TranTrongThai-front.zip`, ảnh A02, vạch `position=R1` (polyline `lane#2`, 27 điểm, kéo từ y=403 xuống y=720).

| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi (R1, A02) | 1156 | 1095 | 1036 | 975 | 913 |
| x của bạn kia (R1, A02) | _ | _ | _ | _ | _ |

Dòng "x của bạn kia" để trống vì lô của Phạm Duy Thanh chưa có trong CVAT local (các task `batch-review`, `d11-road-TranTrongThai`, `fisheye-TranTrongThai` đều do tôi sở hữu). Khi có lô của bạn kia, chạy `python -m road_lab.rowanchor <file-của-bạn-kia>.zip --image A02 --position R1` để lấy đúng 5 số rồi điền vào.

Trả lời 3 câu:

1. Nếu dừng polyline sớm 40 px thì hàng nào đổi thành "không có làn"?

Dừng sớm 40 px nghĩa là điểm gần nhất lùi từ y=720 lên y=680. Vì `rowanchor` chỉ nội suy trong khoảng giữa hai điểm vẽ, nên hàng nào nằm ngoài khoảng (ymax, ymin) của polyline sẽ ra "không có làn". Ở A02 vạch R1:

| Cách vẽ | y=720 | y=680 | y=640 | y=600 | y=560 |
| --- | --- | --- | --- | --- | --- |
| Vẽ đầy đủ (đến y=720) | 1156 | 1095 | 1036 | 975 | 913 |
| Dừng sớm 40 px (đến y=680) | không có làn | không có làn | 1036 | 975 | 913 |

Tức là hai hàng sát xe nhất (y=720 và y=680) đổi thành "không có làn". Ở A01 vạch R1 còn rõ hơn: polyline chỉ kéo tới y=705.9, nên dừng sớm 40 px (còn y=666) làm ba hàng 720, 680, 640 cùng mất. Đây đúng là vùng quan trọng nhất — càng gần xe, càng nhiều pixel ảnh tính trên mỗi hàng, nên sai số ở đó cũng nhân lên nhanh nhất.

2. Ở hàng 720 và hàng 560 hai bạn lệch nhau bao nhiêu pixel? Lệch ở hàng nào ảnh hưởng LKA nhiều hơn?

Chưa tính được vì thiếu số của bạn kia (xem dòng trống ở bảng trên). Cách tính: lấy cùng ảnh A02, cùng `position=R1`, chạy `rowanchor` trên cả hai file, rồi lấy hiệu `|x_tôi − x_bạn_kia|` ở từng hàng.

Về nguyên tắc, lệch ở hàng 720 (sát xe) ảnh hưởng LKA nhiều hơn. Lý do: ở hàng gần xe, một sai số nhỏ về x tương ứng sai số lớn về *góc* giữa xe và vạch — góc mà bộ điều khiển LKA dùng để quay vô-lăng. Ở hàng xa (560), cùng một sai số pixel nhưng vạch ở xa nên góc nhìn nhỏ hơn, sai số đó được trung bình bớt. Cũng vậy, ở hàng 720 ảnh có nhiều chi tiết hơn (vạch rộng hơn, bán kính lớn hơn) nên một pixel ở đó tương ứng lượng đường thực lớn hơn. Nói ngắn gọn: cùng một lỗi vẽ, lỗi ở gần xe "đắt" hơn lỗi ở xa.

3. Nếu polyline vẽ từ xa về gần (ngược quy tắc 2) thì cách tínn có đổi không? Script nào sẽ gặp khó?

Cách tính nội suy không đổi — `x_at_y` trong `road_lab/geometry.py` chỉ cần hai điểm *bao quanh* hàng y, nên nó làm việc đúng như nhau với chiều vẽ. Chỉ có một thứ đổi: thứ tự đầu ra của các điểm, và `rowanchor` sẽ in câu cảnh báo *"polyline đầu tiên vẽ từ xa về gần (điểm đầu cao hơn điểm cuối)"*.

Nhưng các script khác sẽ gặp khó, cụ thể:

- `road_check.py` — phần QC1-04 kiểm "điểm đầu phải gần xe". Nếu vẽ ngược chiều thì điểm đầu là điểm xa, và checker sẽ báo đúng loại lỗi này (rất nhiều cảnh báo QC1-04 trong `lint` của tôi là do điểm đầu cao hơn điểm cuối trên vạch chạy ngang mép ảnh).
- Phần ghép polyline với GT — khoảng cách trung bình giữa hai polyline và so hai đầu (`lane_stop_px`) đều giả định cùng chiều vẽ. Lệch chiều thì điểm đầu này của tôi chạy vào điểm cuối của GT, sai số đo ra vô nghĩa.
- Bất kỳ bước "lấy N điểm đầu làm điểm gần xe" nào trong pipeline — mất điểm quan trọng nhất, tức là điểm sát xe.

Nói cách khác: chiều vẽ là quy ước ngữ nghĩa ("điểm đầu = gần xe"), và mọi script đều dựa vào quy ước đó. Với người đọc bằng mắt thì không sao, nhưng với code thì đảo chiều là đảo ý nghĩa của dữ liệu.
