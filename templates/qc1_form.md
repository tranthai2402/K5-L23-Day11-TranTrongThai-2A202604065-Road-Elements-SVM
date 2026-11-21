# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): Trần Trọng Thái        Người kiểm (học viên): Phạm Duy Thanh  
Ngày: 20/11/2026                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): CVAT task 49 `TranTrongThai-front` + task 50 `TranTrongThai-fisheye` (revision 1)  
Số ảnh kiểm: 20 (A01–A12, B01–B08)   Số nhãn đã kiểm (shape + tag): 183 shape + 80 tag

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 20 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 12 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 12 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 47 | 0 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 47 | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 47 | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 20 | 0 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 45 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 21 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 21 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 8 | 0 | chính |
| QC1-12 file | lỗi file | 2 | 0 | nhẹ / nặng |

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

> Không ghi nhận lỗi trong danh sách này. Các điểm nghi vấn về `position` trùng, vạch đôi tách đôi, và độ sát của polyline được chuyển sang bảng "Ca mơ hồ" bên dưới để trao đổi với lead trước khi tính là lỗi.

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A04 | Lane #2331 và #2326 cùng `position=L1`; lane #2325 và #2327 cùng `position=R2`. Đây là một vạch đôi bị tách thành 2 polyline, hay thực sự là 4 vạch? Nếu là vạch đôi thì theo guideline nên gộp thành 1 polyline `kind=double` — nhưng cách xử lý này cần coach xác nhận. | |
| A09 | Lane #2380 và #2382 cùng `position=L1`, cùng `kind=double`. Tương tự A04. | |
| A10 | Lane #2387 và #2390 cùng `position=L2`; #2390 chỉ có 2 điểm và rất ngắn. Là mảnh vạch riêng hay vạch đứt bị cắt? | |
| A11 | Lane #2402 và #2407 cùng `position=L1`, cùng `kind=double`. Tương tự A04. | |
| A03 | Ba đèn #2312 (5×11), #2321 (5×7), #2317 (6×9) nhỏ hơn 8 px. Là ba đèn riêng hay thuộc cụm đèn lớn đã gán? Nếu là cùng cụm thì bỏ và không tính lỗi. | |
| A04 | Đèn #2332 (7×12) nhỏ hơn 8 px. Cùng câu hỏi như A03. | |
| A07 | Ba đèn #2346 (4×9), #2351 (4×10), #2352 (4×8) nhỏ hơn 8 px. Cùng câu hỏi như A03. | |
| B02 | Đèn #2427, #2430, #2431 có tâm box ở 0.93–0.98 bán kính ảnh (> 0.8R) nhưng `edge_zone=false`. Guideline yêu cầu bật `edge_zone` khi tâm box nằm ngoài 80 % bán kính — nhưng có thể coach tính bán kính theo cách khác (theo chiều cao ảnh thay vì nửa chiều rộng). Cần lead chốt cách tính. | |
| B03 | Biển #2438 có tâm box ở đúng 0.80R, `edge_zone=false`. Chênh lệch rất sát ngưỡng, cần xác nhận. | |
| B04 | Biển #2449 có tâm box ở 0.82R (> 0.8R) nhưng `edge_zone=false`. Cùng câu hỏi như B02. | |
| B05 | Đèn #2456 có tâm box ở 1.00R (> 0.8R) nhưng `edge_zone=false`. Cùng câu hỏi như B02. | |
| A01 | Ba polygon `drivable` (1 `direct` + 2 `alternative`) chồng lấn nhau; ranh giới giữa `direct` và `alternative` không rõ. Có thể đơn giản hoá còn 1 `direct` + 1 `alternative` không? | |
| A05 | Ảnh chỉ có 1 shape `drivable` (`direct`), không có `alternative` và không có làn/đèn/biển. Đúng theo bảng kế hoạch ảnh hay là bỏ sót? | |
| A08 | Biển #2356 để `group=other`. Guideline chỉ dùng `other` khi không xếp được; cần xác nhận lại nhóm (information hoặc warning). | |

## Quyết định
Lỗi nặng: 0   Lỗi chính: 0 / 183 nhãn = 0 %   Lỗi nhẹ: 0   Ca chờ lead: 13  
Quyết định (Release / Hold / Rework): Hold  
Lý do (theo luật của phiếu): Không phát hiện lỗi nặng và chưa ghi nhận lỗi chính nào trong danh sách. Tuy nhiên còn 13 ca mơ hồ về `position` trùng, ngưỡng 8 px của đèn và cách tính `edge_zone` ở ảnh fisheye — các điểm này có thể lật ngược quyết định nếu lead xác nhận là lỗi. Theo luật của phiếu, lô chỉ chuyển sang Release khi ca mơ hồ đã được giải quyết; vì vậy giữ Hold. Nếu lead xác nhận các điểm trên là cách làm đúng theo guideline thì chuyển Release.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](../docs/qc_severity_road_v1.0.md).
