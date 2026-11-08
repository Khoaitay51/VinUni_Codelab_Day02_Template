# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup.

| **#** | **Subsidiary** | **Lens** | **Mô tả ngắn bài toán** |
|---|---|---|---|
| 1 | **VinFast** | Lặp lại | Dự báo nhu cầu đổi pin và tự động đề xuất điều chuyển pin giữa các trạm để tránh thừa pin ở nơi ít khách và thiếu pin tại khu vực cao điểm. |
| 2 | **VinFast** | AI-upgrade | Dự báo nhu cầu sạc theo thời gian và vị trí, đề xuất trạm sạc thay thế phù hợp nhằm giảm tình trạng quá tải và thời gian chờ của khách hàng. |
| 3 | **VinFast** | Tốn thời gian | Nhân viên vận hành phải theo dõi log lỗi, kiểm tra tình trạng trụ sạc và lập lịch bảo trì thủ công, gây chậm phát hiện sự cố và tăng thời gian downtime. |
| 4 | **VinFast** | AI-upgrade | Phân tích dữ liệu BMS để dự báo sức khỏe và tuổi thọ pin, từ đó tối ưu kế hoạch kiểm tra, luân chuyển và thay thế pin. |
| 5 | **VinFast** | Pain từ người khác | Khách hàng có thể gặp tình trạng giao dịch đổi pin thất bại hoặc sai lệch trạng thái pin trong hệ thống, trong khi nhân viên phải đối soát thủ công để xác minh và xử lý. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: **#1 (Tối ưu tồn kho và phân phối pin tại trạm đổi pin), #2 (Dự báo nhu cầu và điều phối xe tại trạm sạc), #4 (Dự báo sức khỏe và tối ưu vòng đời pin).**

## QUICK PROBLEM CARD #1 – Tối ưu tồn kho và phân phối pin tại trạm đổi pin

```text
┌────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                          │
│                                                                │
│ Bài toán: Các trạm đổi pin xe máy điện có tình trạng thừa      │
│ pin đã sạc ở một số nơi nhưng thiếu pin tại các trạm có        │
│ nhu cầu cao, cần điều chuyển pin thủ công.                     │
│ Công ty thành viên: [x] VinFast                                │
│                                                                │
│ Ai đang đau? Nhân viên vận hành trạm, đội logistics,           │
│ khách hàng phải chờ hoặc không thể đổi pin.                    │
│                                                                │
│ Workflow thủ công hiện tại (5 bước):                           │
│   1. Nhân viên kiểm tra số lượng pin đầy tại từng trạm.        │
│   → 2. Tổng hợp lịch sử giao dịch đổi pin và lượng tồn kho.    │
│   → 3. Xác định các trạm thiếu pin hoặc dư pin.                │
│   → 4. Lập kế hoạch điều chuyển pin giữa các trạm.             │
│   → 5. Điều phối nhân viên/phương tiện vận chuyển và cập       │
│      nhật lại số lượng pin sau khi điều chuyển.                │
│                                                                │
│ Bước nào tốn nhất? Bước 2-4: Tổng hợp dữ liệu và lập kế        │
│ hoạch phân phối pin. (⏱ Giả định 30-60 phút/lần lập kế hoạch) │
│                                                                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4                 │
│ (Dự báo nhu cầu -> Phát hiện thiếu/dư pin -> Đề xuất           │
│ phương án điều chuyển tối ưu giữa các trạm).                   │
│                                                                │
│ Đo thành công bằng gì (Metric có số)?                          │
│ Giảm tỷ lệ trạm thiếu pin trong giờ cao điểm 20-30%;           │
│ giảm lượng pin tồn dư không cần thiết 15-20%.                  │
│                                                                │
│ Quick Architecture: [x] Predictive ML + Optimization           │
│ (Demand Forecasting + Inventory Optimization)                  │
└────────────────────────────────────────────────────────────────┘
```

---

## QUICK PROBLEM CARD #2 – Dự báo nhu cầu và điều phối xe tại trạm sạc

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Một số trạm sạc thường xuyên quá tải vào giờ cao  │
│ điểm trong khi các trạm lân cận còn công suất trống, khiến  │
│ khách hàng phải chờ đợi hoặc mất thời gian tìm trạm khác.   │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Khách hàng sử dụng xe điện, nhân viên vận      │
│ hành mạng lưới trạm sạc.                                    │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Nhân viên theo dõi tình trạng hoạt động của các trạm.  │
│   → 2. Tổng hợp lịch sử phiên sạc và lưu lượng xe theo giờ. │
│   → 3. Xác định các trạm có nguy cơ quá tải.                │
│   → 4. Kiểm tra thủ công các trạm lân cận còn khả năng phục │
│      vụ.                                                    │
│   → 5. Thông báo hoặc đề xuất khách hàng chuyển sang trạm   │
│      khác phù hợp.                                          │
│                                                             │
│ Bước nào tốn nhất? Bước 2-4: Phân tích nhu cầu và xác định  │
│ trạm thay thế. (⏱ Giả định 15-30 phút/lần phân tích)       │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-5              │
│ (Dự báo lưu lượng -> Ước tính tình trạng quá tải -> Tính    │
│ thời gian chờ -> Đề xuất trạm thay thế và tuyến đường).     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian chờ trung bình 20-30%; tăng tỷ lệ sử dụng    │
│ công suất trạm sạc 10-15%.                                  │
│                                                             │
│ Quick Architecture: [x] Predictive ML + Optimization        │
│ (Spatio-temporal Forecasting + Smart Routing)               │
└─────────────────────────────────────────────────────────────┘
```

---

## QUICK PROBLEM CARD #3 – Dự báo sức khỏe và tối ưu vòng đời pin

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Pin xe máy điện suy giảm dung lượng không đồng    │
│ đều. Nhân viên vận hành cần theo dõi, kiểm tra và quyết     │
│ định luân chuyển hoặc thay thế pin dựa trên tình trạng thực │
│ tế nhưng dữ liệu phân tích còn phức tạp.                    │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Bộ phận quản lý pin, kỹ thuật bảo trì, đội vận │
│ hành trạm đổi pin và bộ phận quản lý tài sản.               │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Thu thập dữ liệu lịch sử sạc/xả và trạng thái pin.     │
│   → 2. Kiểm tra thông số SOH, số chu kỳ sạc và cảnh báo BMS.│
│   → 3. Phân tích pin nào suy giảm bất thường.               │
│   → 4. Đánh giá pin cần tiếp tục sử dụng, kiểm tra hoặc     │
│      thay thế.                                              │
│   → 5. Lập kế hoạch phân bổ, luân chuyển và thay thế pin.   │
│                                                             │
│ Bước nào tốn nhất? Bước 2-4: Phân tích dữ liệu và đánh giá  │
│ tình trạng pin. (⏱ Giả định 20-40 phút/lần đánh giá)       │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-5              │
│ (Dự báo SOH/RUL -> Phát hiện suy giảm bất thường ->         │
│ Đề xuất kiểm tra, luân chuyển hoặc thay thế pin).           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm 30% thời gian đánh giá tình trạng pin; giảm 15-20%     │
│ chi phí vận hành và thay thế pin không cần thiết.           │
│                                                             │
│ Quick Architecture: [x] Predictive ML                       │
│ (Battery SOH/RUL Prediction + Anomaly Detection)            │
└─────────────────────────────────────────────────────────────┘
```

---