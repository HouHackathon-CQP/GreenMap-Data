# 🗺️ GreenMap-Data

> Bộ dữ liệu địa lý mở cho ứng dụng GreenMap - Thu thập từ OpenStreetMap (OSM) và SUMO

## 📖 Giới thiệu dự án

GreenMap-Data là kho dữ liệu chứa thông tin địa lý về các điểm quan trọng trong khu vực Hà Nội, bao gồm:
- 🚲 Điểm thuê xe đạp công cộng
- ⚡ Trạm sạc xe điện
- 🌳 Công viên và không gian xanh
- 🏛️ Điểm du lịch và di tích lịch sử

Dữ liệu được thu thập từ **OpenStreetMap (OSM)** thông qua Overpass API và mô phỏng giao thông từ **SUMO** (Simulation of Urban MObility).

## 📊 Cấu trúc dữ liệu

### Dữ liệu OSM (GeoJSON)
| Loại dữ liệu | File | Mô tả |
|-------------|------|-------|
| Điểm thuê xe đạp | `bicycle_rental.geojson` | Các trạm MBI Bike Sharing và các dịch vụ thuê xe đạp |
| Trạm sạc xe điện | `charging_station.geojson` | Trạm sạc VinFast và các nhà cung cấp khác |
| Công viên | `park.geojson` | Công viên, vườn hoa, không gian xanh công cộng |
| Điểm du lịch | `tourist_attractions.geojson` | Di tích lịch sử, đền chùa, điểm tham quan |

### Dữ liệu SUMO (JSON)
- `simulation_data_part1.json` + `simulation_data_part2.json`: Dữ liệu mô phỏng giao thông (đã tách do kích thước lớn)

## 🗺️ Phạm vi dữ liệu (Bounding Box)

Dữ liệu bao phủ khu vực Hà Nội và vùng lân cận:

```
Bounding Box: (20.57, 105.28, 21.39, 106.02)

┌─────────────────────────────────────┐
│  Min Latitude:  20.57 (Nam)         │
│  Max Latitude:  21.39 (Bắc)         │
│  Min Longitude: 105.28 (Tây)        │
│  Max Longitude: 106.02 (Đông)       │
└─────────────────────────────────────┘
```

## ⚙️ Thiết lập môi trường (Khuyến nghị) 

### Tại sao cần Môi trường ảo (Virtual Environment)?

✅ **Lợi ích:**
- **Cô lập dự án**: Các package cài đặt cho dự án này **không ảnh hưởng** đến Python toàn hệ thống
- **Quản lý phiên bản**: Dễ kiểm soát phiên bản package cho từng dự án
- **Tránh xung đột**: Hai dự án khác nhau có thể dùng các phiên bản package khác nhau
- **Dễ chia sẻ**: Dễ dàng chia sẻ project cho người khác mà không sợ thiếu dependencies

### 🛠️ Hướng dẫn tạo Virtual Environment

#### Trên **Windows** (PowerShell hoặc Command Prompt):

```bash
# 1. Mở Command Prompt hoặc PowerShell
# 2. Di chuyển vào thư mục project
cd D:\GreenMap\GreenMap-Data

# 3. Tạo virtual environment
python -m venv venv

# 4. Kích hoạt virtual environment
# Với Command Prompt:
venv\Scripts\activate

# Hoặc với PowerShell:
venv\Scripts\Activate.ps1

# 5. Cài đặt các thư viện cần thiết (khi venv đã kích hoạt)
pip install requests pandas folium jupyter

# 6. Mở Jupyter Notebook
jupyter notebook
```

#### Trên **macOS/Linux**:

```bash
# 1. Mở Terminal
# 2. Di chuyển vào thư mục project
cd ~/GreenMap/GreenMap-Data

# 3. Tạo virtual environment
python3 -m venv venv

# 4. Kích hoạt virtual environment
source venv/bin/activate

# 5. Cài đặt các thư viện cần thiết (khi venv đã kích hoạt)
pip install requests pandas folium jupyter

# 6. Mở Jupyter Notebook
jupyter notebook
```

### ✅ Kiểm tra Virtual Environment đã kích hoạt

Khi virtual environment đã kích hoạt, bạn sẽ thấy `(venv)` ở đầu dòng lệnh:

```
(venv) D:\GreenMap\GreenMap-Data>
```

hoặc

```
(venv) ~/GreenMap/GreenMap-Data $
```

### 🧹 Thoát khỏi Virtual Environment

Để thoát khỏi virtual environment, chỉ cần gõ:

```bash
deactivate
```

---

## 🚀 Hướng dẫn bắt đầu nhanh

### Cách 1: Tự động (Khuyến nghị) 🤖

Sử dụng Jupyter Notebook để tự động tải dữ liệu:

```bash
# 1. Clone repository
git clone https://github.com/HouHackathon-CQP/GreenMap-Data.git
cd GreenMap-Data

# 2. Tạo và kích hoạt virtual environment (Windows)
python -m venv venv
venv\Scripts\activate

# Hoặc trên macOS/Linux:
python3 -m venv venv
source venv/bin/activate

# 3. Cài đặt thư viện cần thiết (trong virtual environment)
pip install requests pandas folium jupyter

# 4. Mở và chạy notebook
jupyter notebook data_collection.ipynb
```

Sau khi chạy, dữ liệu sẽ được lưu trong thư mục `Data/`.

### Cách 2: Thủ công 🔧

Sử dụng [Overpass Turbo](https://overpass-turbo.eu/) để lấy dữ liệu trực tiếp.

## 📝 Các Query Overpass

Dưới đây là các query để lấy từng loại dữ liệu:

### 1. Query lấy điểm thuê xe đạp 🚲

```overpass
[out:json][timeout:25];
(
  node["amenity"="bicycle_rental"](20.57, 105.28, 21.39, 106.02);
  way["amenity"="bicycle_rental"](20.57, 105.28, 21.39, 106.02);
  relation["amenity"="bicycle_rental"](20.57, 105.28, 21.39, 106.02);
);
out body;
>;
out skel qt;
```

### 2. Query lấy trạm sạc xe điện ⚡

```overpass
[out:json][timeout:25];
(
  node["amenity"="charging_station"](20.57, 105.28, 21.39, 106.02);
  way["amenity"="charging_station"](20.57, 105.28, 21.39, 106.02);
  relation["amenity"="charging_station"](20.57, 105.28, 21.39, 106.02);
);
out body;
>;
out skel qt;
```

### 3. Query lấy công viên 🌳

```overpass
[out:json][timeout:60];
(
  node["leisure"="park"](20.57, 105.28, 21.39, 106.02);
  way["leisure"="park"](20.57, 105.28, 21.39, 106.02);
  relation["leisure"="park"](20.57, 105.28, 21.39, 106.02);
);
out body;
>;
out skel qt;
```

### 4. Query lấy điểm du lịch 🏛️

```overpass
[out:json][timeout:180];
(
  // Điểm du lịch chính
  node["tourism"](20.57, 105.28, 21.39, 106.02);
  way["tourism"](20.57, 105.28, 21.39, 106.02);
  relation["tourism"](20.57, 105.28, 21.39, 106.02);
  
  // Di tích lịch sử
  node["historic"](20.57, 105.28, 21.39, 106.02);
  way["historic"](20.57, 105.28, 21.39, 106.02);
  relation["historic"](20.57, 105.28, 21.39, 106.02);
  
  // Di sản văn hóa
  node["heritage"](20.57, 105.28, 21.39, 106.02);
  way["heritage"](20.57, 105.28, 21.39, 106.02);
  relation["heritage"](20.57, 105.28, 21.39, 106.02);
);
out body;
>;
out skel qt;
```

**Hướng dẫn sử dụng:**
1. Truy cập [Overpass Turbo](https://overpass-turbo.eu/)
2. Dán query vào ô nhập liệu
3. Nhấn "Run" (hoặc Ctrl+Enter)
4. Export dữ liệu → Chọn "GeoJSON"
5. Lưu file vào thư mục `Data/`

## 🚗 Hướng dẫn SUMO

SUMO (Simulation of Urban MObility) là công cụ mô phỏng giao thông mã nguồn mở.

📚 **Tài liệu chính thức:** [https://sumo.dlr.de/docs/](https://sumo.dlr.de/docs/)

### Sử dụng dữ liệu SUMO

```python
import json
from Data.merge_json import merge_json

# Ghép 2 file thành file gốc
merge_json()

# Đọc dữ liệu mô phỏng
with open('Data/simulation_data.json', 'r', encoding='utf-8') as f:
    simulation_data = json.load(f)
```

## 📁 Cấu trúc thư mục dự án

```
GreenMap-Data/
├── 📄 README.md                     # Tài liệu hướng dẫn
├── 📓 data_collection.ipynb         # Notebook tự động thu thập dữ liệu
├── 📜 LICENSE                       # Giấy phép Apache 2.0
└── 📂 Data/
    ├── 🚲 bicycle_rental.geojson    # Điểm thuê xe đạp
    ├── ⚡ charging_station.geojson   # Trạm sạc xe điện
    ├── 🌳 park.geojson               # Công viên
    ├── 🏛️ tourist_attractions.geojson # Điểm du lịch
    ├── 🔧 merge_json.py             # Script ghép file simulation
    ├── 🔧 split_json.py             # Script tách file simulation
    ├── 📊 simulation_data_part1.json # Dữ liệu SUMO (phần 1)
    └── 📊 simulation_data_part2.json # Dữ liệu SUMO (phần 2)
```

## ⚠️ Lưu ý quan trọng

### Về dữ liệu simulation_data
- Dữ liệu mô phỏng SUMO có **dung lượng rất lớn** (>100MB sau khi ghép)
- **Không nên đọc trực tiếp** các file part trong IDE do có thể gây lag
- Sử dụng `merge_json.py` để ghép file trước khi sử dụng
- Xử lý dữ liệu theo batch để tránh tràn bộ nhớ

### Về Overpass API
- Có giới hạn số request mỗi phút, nên chờ giữa các lần gọi
- Timeout được cấu hình sẵn trong mỗi query
- Nếu gặp lỗi timeout, thử lại sau vài phút

## 📜 Giấy phép

Dự án được phát hành dưới giấy phép [Apache 2.0](LICENSE).

Dữ liệu OpenStreetMap được sử dụng theo [Open Database License (ODbL)](https://www.openstreetmap.org/copyright).

## 👥 Đóng góp

Chúng tôi hoan nghênh mọi đóng góp! Hãy mở Issue hoặc Pull Request nếu bạn muốn cải thiện dự án.

---

> 🌿 GreenMap - Vì một Hà Nội xanh và bền vững!