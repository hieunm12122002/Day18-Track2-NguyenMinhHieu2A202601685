Trong các Lakehouse Anti-Patterns, vấn đề team chúng em dễ vướng phải nhất là **"Thiếu chiến lược bảo trì định kỳ" (Bỏ bê dọn rác metadata và data mồ côi)**.

Lý do là trong quá trình phát triển dự án, team thường chỉ tập trung vào việc đẩy dữ liệu vào data lake sao cho nhanh nhất, mà quên thiết lập các job bảo trì chạy ngầm như `OPTIMIZE` hay `VACUUM`. Như bài lab số 6 đã minh chứng, một hệ thống Iceberg hay Delta Lake nếu không dọn dẹp sẽ sinh ra hàng ngàn "small files" và file mồ côi (tombstoned files chưa bị xóa vật lý). 

Nguy hiểm nhất là việc lầm tưởng rằng gọi lệnh `expire_snapshots` của Iceberg là hệ thống đã sạch, nhưng thực chất nó chỉ dọn metadata mà không xóa file rác (orphan files). Nếu không cấu hình đúng hoặc không lên lịch dọn dẹp thường xuyên, chi phí lưu trữ S3 sẽ phình to thầm lặng và tốc độ truy vấn ngày càng chậm đi, đến khi phát hiện thì nợ kỹ thuật (technical debt) để khắc phục đã rất lớn.
