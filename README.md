# smartEYE-AI: Intelligent Edge-Computing Assistant

[![Python Version](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org)
[![Framework](https://img.shields.io/badge/framework-Flask%20%2F%20FastAPI-green.svg)](https://flask.palletsprojects.com/)
[![License](https://img.shields.io/badge/license-MIT-purple.svg)](LICENSE)

**smartEYE-AI** là một hệ thống trí tuệ nhân tạo ứng dụng mô hình điện toán biên (Edge Computing) tận dụng tối đa phần cứng sẵn có. Hệ thống biến bất kỳ chiếc điện thoại thông minh nào thành "mắt và tai" (thu thập dữ liệu hình ảnh, âm thanh) và sử dụng máy tính cá nhân làm "bộ não" trung tâm để xử lý AI theo thời gian thực thông qua nền tảng Web Application.

---

## 📌 Tóm Tắt Dự Án (Project Abstract)

Việc di chuyển và định hướng trong không gian kín như lớp học đối với robot tự hành hoặc người khiếm thị đòi hỏi một hệ thống nhận diện vật cản nhanh và chính xác. **smartEYE-AI** thu hẹp phạm vi hoạt động chuyên biệt trong **môi trường lớp học** với các mục tiêu cốt lõi:
* **Đối tượng nhận diện chính (YOLOv8):** Tập trung học và phát hiện chính xác 4 yếu tố môi trường bao gồm: **Con người (Human)**, **Bàn học (Desk)**, **Bậc thang (Stairs)**, **Cánh cửa (Door)**, **Tường (Wall)**, và **Đường thẳng/Vạch kẻ đường (Lines)** để xác định lối đi an toàn.
* **Cơ chế hoạt động:**
    * Camera điện thoại stream luồng video thời gian thực lên Server Flask/FastAPI trên laptop thông qua giao thức WebSocket (độ trễ thấp).
    * Laptop chạy mô hình **YOLOv8 (Ultralytics)** để bounding box vật cản và tính toán khoảng cách/nguy cơ va chạm.
    * Nếu phát hiện vật cản quá gần hoặc lệch khỏi đường thẳng an toàn, server lập tức gửi tín hiệu hạ lệnh cho điện thoại phát âm thanh cảnh báo (ví dụ: *"Chú ý có bàn phía trước"*, *"Chuẩn bị tới bậc thang"*).
---




## ⚙️ Kiến Trúc Hệ Thống (System Architecture)

[ Smartphone: Eye/Ear ] --(Mạng Local Wi-Fi / WebSockets/HTTP)--> [ Laptop Server: Brain ]
▲                                                                  │
└───────────────────(Phản hồi Âm thanh / Lệnh)─────────────────────┘




1.  **Frontend (Mobile Web):** HTML5 Camera API (`getUserMedia`), Socket.io-client gửi frame ảnh dạng Base64/Binary, Web Audio API đảm nhận phát âm thanh cảnh báo.
2.  **Backend (Laptop):** Python, Flask-SocketIO nhận diện luồng ảnh liên tục, chuyển đổi sang định dạng OpenCV.
3.  **AI Core:** Thư viện `ultralytics` chạy mô hình **YOLOv8** (sử dụng phiên bản `yolov8n.pt` - Nano để tối ưu hóa tốc độ trên laptop cá nhân không có GPU rời).

---

## 📅 Lộ Trình Triển Khai 2 Tuần (Timeline & Roadmap)

### Tuần 1: Thu Thập Dữ Liệu Lớp Học & Thông Luồng Hệ Thống
* **Ngày 1 - 2:** Cả nhóm tiến hành thu thập tập dữ liệu (Dataset) thực tế ngay trong lớp học: quay video/chụp ảnh các góc có *bàn, bậc thang, cánh cửa, các đường thẳng kẻ sàn/hành lang*. Tiến hành gán nhãn (Annotation) bằng Roboflow.
* **Ngày 3 - 5:** Thiết lập Server Flask-SocketIO. Hoàn thiện giao diện Web Mobile để điện thoại truy cập qua mạng Local (hoặc qua Ngrok HTTPS) có thể mở camera và stream mượt mà về máy tính với tốc độ >= 15 FPS.
* **Ngày 6 - 7:** Chạy thử nghiệm mô hình YOLOv8 pre-trained trên laptop để kiểm tra tốc độ xử lý frame nhận được từ socket.

### Tuần 2: Custom Training YOLOv8, Logic Cảnh Báo & Đóng Gói
* **Ngày 8 - 10:** Tiến hành Fine-tune / Train mô hình **YOLOv8** với tập dữ liệu lớp học đã chuẩn bị (4 lớp: `table`, `stairs`, `door`, `line`). Tối ưu hóa kích thước mô hình để đạt độ trễ thấp nhất.
* **Ngày 11 - 12:** Xây dựng logic cảnh báo (ví dụ: nếu diện tích bounding box của `table` chiếm > 30% khung hình nghĩa là vật cản đang ở rất gần). Tích hợp Text-to-Speech hoặc các file âm thanh `.mp3` định sẵn để phản hồi về loa điện thoại.
* **Ngày 13 - 14:** Kiểm thử thực tế khả năng né vật cản trong lớp học. Hoàn thiện báo cáo học thuật bằng **LaTeX (Overleaf)**, chuẩn hóa source code và chuẩn bị bài thuyết trình.

---

## 👥 Phân Chia Công Việc Trong Nhóm (Task Allocation)

| Thành viên | Vai trò chính | Nhiệm vụ chi tiết |
| :--- | :--- | :--- |
| **Thành viên 1** | **Project Leader & Backend Dev** | - Xây dựng kiến trúc Flask-SocketIO Server trên Laptop. Xử lý luồng nhận nhận/gửi dữ liệu thời gian thực giữa Laptop và Phone. Thiết lập logic kích hoạt âm thanh dựa trên tọa độ AI trả về. |
| **Thành viên 2** | **Frontend Dev & Mobile UX** | - Thiết kế giao diện Web tối giản, tối ưu trên điện thoại. Xử lý Javascript điều khiển Camera, truyền dữ liệu Frame và tối ưu hóa Web Audio API để phát âm thanh cảnh báo ngay lập tức. |
| **Thành viên 3** | **AI/ML Engineer** | - Thu thập, làm sạch và gán nhãn Dataset lớp học (Bàn, bậc thang, cửa, đường thẳng). Cấu hình, huấn luyện và tối ưu hóa mô hình **YOLOv8** trên môi trường Local/Google Colab. Tích hợp mã nguồn Inference YOLOv8 vào Backend. |
| **Thành viên 4** | **QA Engineer & Technical Writer** | - Kiểm thử hệ thống trong điều kiện thực tế (ánh sáng lớp học thay đổi, đo đạc độ trễ phản hồi âm thanh). Xây dựng cấu trúc và viết báo cáo khoa học hoàn chỉnh bằng **LaTeX**. Quản lý tiến độ GitHub. |

---
