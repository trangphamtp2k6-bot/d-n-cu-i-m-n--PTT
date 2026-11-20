# AI Data Agent Pipeline - Sẵn Sàng Deploy Vercel

Ứng dụng **AI Data Agent** được xây dựng tuân thủ 100% theo đúng sơ đồ quy trình nghiệp vụ:

```
[Nhập link Google Sheet / File Excel, CSV, XLSX]
           │
           ▼
     <Kiểm tra đúng> ──(No)──► [Báo lỗi cho người dùng] ──► (Bắt người dùng nhập lại)
           │ (Yes)
           ▼
       [Cleaning]
           │
           ▼
       <Kiểm tra>   ──(No)──► [Báo lỗi cho người dùng] ──► (Bắt người dùng nhập lại)
           │ (Yes)
           ▼
        [Process]
           │
           ▼
        [Verify]
           │
           ▼
       <Xác nhận>   ──(No)──► [Feedback] ──► (Quay lại Process)
           │ (Yes)
           ▼
[Xuất file báo cáo, file excel... (dưới dạng HTML)]
           │
           ▼
        (Kết thúc)
```

---

## 📁 Cấu trúc thư mục dự án

```text
├── api/
│   ├── index.py       # FastAPI Entrypoint cho Serverless Function trên Vercel & Web UI
│   └── agent.py       # Core logic AI Agent: Validate, Clean, Process, Verify, HTML Render
├── test_pipeline.py   # Script kiểm thử tự động toàn bộ pipeline
├── requirements.txt   # Các thư viện Python cần thiết
├── vercel.json        # Cấu hình Vercel Serverless Function (@vercel/python)
├── package.json       # Metadata & Scripts
├── .gitignore         # Bỏ qua các file cache, temp, env
├── .env.example       # Mẫu biến môi trường (GEMINI_API_KEY)
└── README.md          # Tài liệu hướng dẫn sử dụng và triển khai
```

---

## 🚀 Hướng Dẫn Deploy Lên Vercel

### Cách 1: Deploy qua GitHub & Vercel Dashboard (Khuyên dùng)
1. Đẩy thư mục mã nguồn này lên một repository trên **GitHub**.
2. Đăng nhập vào [Vercel](https://vercel.com).
3. Nhấn **"Add New..."** $\rightarrow$ **"Project"** $\rightarrow$ Chọn repository vừa tạo.
4. Ở mục **Environment Variables**, thêm biến:
   - `GEMINI_API_KEY`: *(API Key lấy miễn phí tại [Google AI Studio](https://aistudio.google.com/app/apikey))*.
5. Nhấn **Deploy**. Vercel sẽ tự động build và cấp một tên miền `https://ten-du-an.vercel.app`.

### Cách 2: Deploy qua Vercel CLI
Nếu máy bạn có cài đặt `vercel` CLI:
```bash
vercel login
vercel --prod
```

---

## 💻 Hướng Dẫn Chạy Thử Trên Máy Local

1. Kích hoạt môi trường và khởi chạy server:
```bash
python -m uvicorn api.index:app --reload --port 3000
```
2. Mở trình duyệt truy cập: `http://localhost:3000`
3. Nhập file dữ liệu hoặc link Google Sheet để trải nghiệm quy trình tự động.

---

## 🌟 Các Tính Năng Nổi Bật Của Agent

- **Kiểm tra đầu vào thông minh**: Tự nhận diện link Google Sheet, tự động chuyển đổi sang format export CSV. Báo lỗi chi tiết nếu link riêng tư hoặc sai định dạng.
- **Làm sạch chuẩn hóa (Cleaning)**: Khử trùng lặp, xử lý giá trị khuyết (missing values), ép kiểu số/tiền tệ tự động.
- **Kiểm định chặt chẽ (Validation Gate)**: Nếu dữ liệu rỗng sau khi clean sẽ chặn lại ngay và yêu cầu nhập lại.
- **AI Analytics & Verify**: Tổng hợp các chỉ số KPI, tự sinh biểu đồ trực quan (Bar chart, Doughnut chart), AI đưa ra nhận định chuyên sâu.
- **Cơ chế Feedback**: Người dùng có thể gửi feedback để AI điều chỉnh phân tích lại theo ý muốn trước khi xuất bản.
- **Xuất báo cáo HTML hiện đại**: Dashboard HTML hoàn chỉnh với Chart.js, Bootstrap 5, hỗ trợ In/Xuất PDF hoặc tải về máy dưới dạng tệp HTML/Excel độc lập.
