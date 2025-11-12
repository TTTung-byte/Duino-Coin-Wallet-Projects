# ESP Duino-Coin Miner

Mã nguồn chính thức cho tất cả các board ESP8266/ESP32 để đào Duino-Coin.

## 📋 Mô tả

Dự án này là một miner Duino-Coin được tối ưu hóa cho các board ESP8266 và ESP32. Duino-Coin là một loại tiền điện tử được thiết kế để có thể đào được trên các thiết bị IoT và vi điều khiển, giúp việc khai thác trở nên dễ dàng và thân thiện với môi trường.

## ✨ Tính năng

- ✅ Hỗ trợ ESP8266 và ESP32 (bao gồm ESP32-S2, ESP32-C3, ESP32-S3)
- ✅ Đào Duino-Coin với hiệu suất cao
- ✅ Hỗ trợ WiFi và Ethernet (LAN8720)
- ✅ Web Dashboard để theo dõi trạng thái mining
- ✅ OTA (Over-The-Air) updates
- ✅ Captive Portal để cấu hình WiFi dễ dàng
- ✅ Hỗ trợ màn hình hiển thị (SSD1306 OLED, 16x2 LCD)
- ✅ Hỗ trợ các cảm biến IoT (DS18B20, DHT11/22, HSU07M, cảm biến nhiệt độ nội bộ)
- ✅ Hỗ trợ BlushyBox device
- ✅ Multi-core mining cho ESP32 (2 cores)
- ✅ Watchdog timer để đảm bảo ổn định

## 🚀 Cài đặt

### Yêu cầu

1. **Arduino IDE** (phiên bản 1.8.x hoặc 2.x)
2. **Board Manager URLs**:
   - ESP8266: `http://arduino.esp8266.com/stable/package_esp8266com_index.json`
   - ESP32: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`

3. **Thư viện cần thiết**:
   - ArduinoJSON (bởi Benoit Blanchon)
   - WiFiManager (nếu sử dụng Captive Portal)
   - U8g2 (nếu sử dụng màn hình SSD1306)
   - Adafruit LiquidCrystal (nếu sử dụng màn hình 16x2)
   - OneWire và DallasTemperature (nếu sử dụng DS18B20)
   - DHT sensor library (nếu sử dụng DHT11/22)
   - TridentTD_EasyFreeRTOS32 (cho ESP32 multi-core)

### Các bước cài đặt

1. **Cài đặt Board Support Packages**:
   - Mở Arduino IDE
   - Vào `File` → `Preferences`
   - Thêm URLs vào "Additional Board Manager URLs"
   - Vào `Tools` → `Board` → `Boards Manager`
   - Tìm và cài đặt "ESP8266" và/hoặc "ESP32"

2. **Cài đặt thư viện**:
   - Vào `Tools` → `Manage Libraries`
   - Tìm và cài đặt các thư viện cần thiết

3. **Cấu hình dự án**:
   - Mở file `Settings.h`
   - Cập nhật các thông tin sau:
     ```cpp
     extern char *DUCO_USER = "your_username";      // Tên người dùng Duino-Coin
     extern char *MINER_KEY = "your_mining_key";    // Mining key (nếu có)
     extern char *RIG_IDENTIFIER = "None";           // Tên rig (hoặc "Auto" để tự động)
     extern const char SSID[] = "your_wifi_ssid";    // Tên WiFi
     extern const char PASSWORD[] = "your_password"; // Mật khẩu WiFi
     ```

4. **Tùy chọn nâng cao**:
   - Bỏ comment các dòng trong `Settings.h` để kích hoạt tính năng:
     - `#define WEB_DASHBOARD` - Bật web dashboard
     - `#define DISPLAY_SSD1306` - Bật màn hình OLED
     - `#define DISPLAY_16X2` - Bật màn hình LCD
     - `#define USE_DS18B20` - Sử dụng cảm biến DS18B20
     - `#define USE_DHT` - Sử dụng cảm biến DHT11/22
     - `#define USE_INTERNAL_SENSOR` - Sử dụng cảm biến nhiệt độ nội bộ
     - `#define CAPTIVE_PORTAL` - Bật captive portal
     - `#define USE_LAN` - Sử dụng Ethernet thay vì WiFi

5. **Upload code**:
   - Chọn board phù hợp trong `Tools` → `Board`
   - Chọn cổng COM trong `Tools` → `Port`
   - Nhấn `Upload`

## 📖 Hướng dẫn sử dụng

### Kết nối WiFi

Sau khi upload code, ESP sẽ tự động kết nối với WiFi đã cấu hình. Nếu sử dụng Captive Portal, ESP sẽ tạo một mạng WiFi riêng để bạn có thể cấu hình.

### Web Dashboard

Nếu đã bật `WEB_DASHBOARD`, bạn có thể truy cập dashboard tại:
- `http://<IP_ADDRESS>` (IP của ESP)
- `http://<RIG_IDENTIFIER>.local` (nếu mDNS được hỗ trợ)

Dashboard hiển thị:
- Hashrate (kH/s)
- Difficulty
- Số lượng shares
- Node đang kết nối
- Thông tin thiết bị
- Dữ liệu cảm biến (nếu có)

### OTA Updates

ESP hỗ trợ cập nhật qua không khí (OTA). Trong Arduino IDE:
1. Vào `Tools` → `Port` → Chọn port có tên ESP của bạn
2. Upload code như bình thường, code sẽ được gửi qua WiFi

### Cảm biến IoT

Dự án hỗ trợ nhiều loại cảm biến để gửi dữ liệu lên Duino-Coin network:

- **DS18B20**: Cảm biến nhiệt độ (kết nối GPIO 12)
- **DHT11/22**: Cảm biến nhiệt độ và độ ẩm (kết nối GPIO 12)
- **HSU07M**: Cảm biến nhiệt độ I2C
- **Internal Sensor**: Cảm biến nhiệt độ nội bộ của ESP32

Dữ liệu từ cảm biến sẽ được gửi kèm với mỗi mining job.

## 🔧 Cấu trúc dự án

```
ESP_Code/
├── ESP_Code.ino      # File chính
├── Settings.h         # Cấu hình và tùy chọn
├── MiningJob.h       # Logic mining
├── DSHA1.h           # Thuật toán hash DSHA1
├── Counter.h          # Counter class cho mining
├── DisplayHal.h      # Abstraction layer cho màn hình
└── Dashboard.h       # HTML cho web dashboard
```

## ⚙️ Cấu hình nâng cao

### Tối ưu hiệu suất

- ESP8266: Tự động chạy ở 160MHz
- ESP32: Tự động chạy ở 240MHz
- ESP32 dual-core: Sử dụng cả 2 cores để mining

### Watchdog Timer

ESP8266 có watchdog timer để tự động khởi động lại nếu bị treo. Timeout mặc định là 30 giây.

### Brownout Detector

Nếu gặp vấn đề với brownout detector trên ESP32, có thể tắt bằng cách bỏ comment:
```cpp
#define DISABLE_BROWNOUT
```

## 📊 Hiệu suất

Hiệu suất mining phụ thuộc vào loại board:

- **ESP8266**: ~50-100 kH/s
- **ESP32**: ~200-400 kH/s (single core) hoặc ~400-800 kH/s (dual core)
- **ESP32-S2/C3**: ~150-300 kH/s

*Lưu ý: Hiệu suất có thể thay đổi tùy thuộc vào cấu hình và điều kiện mạng.*

## 🐛 Xử lý lỗi

### ESP không kết nối WiFi

- Kiểm tra SSID và password trong `Settings.h`
- Đảm bảo WiFi ở gần ESP
- Thử sử dụng Captive Portal để cấu hình lại

### Lỗi biên dịch

- Đảm bảo đã cài đặt đầy đủ các thư viện cần thiết
- Kiểm tra phiên bản Arduino IDE và Board Support Packages
- Xem phần "Yêu cầu" ở trên

### Mining không hoạt động

- Kiểm tra kết nối internet
- Xem Serial Monitor để kiểm tra log
- Đảm bảo username và mining key đúng

## 📝 Ghi chú

- Phiên bản hiện tại: **4.3**
- License: MIT
- Dự án chính thức: [Duino-Coin](https://duinocoin.com)
- GitHub: [revoxhere/duino-coin](https://github.com/revoxhere/duino-coin)

## 🤝 Đóng góp

Mọi đóng góp đều được chào đón! Vui lòng tạo issue hoặc pull request trên GitHub.

## 📄 License

MIT License - Xem file LICENSE để biết thêm chi tiết.

## 🔗 Liên kết hữu ích

- [Duino-Coin Website](https://duinocoin.com)
- [Duino-Coin GitHub](https://github.com/revoxhere/duino-coin)
- [Getting Started Guide](https://duinocoin.com/getting-started)
- [Duino-Coin Wiki](https://github.com/revoxhere/duino-coin/wiki)

---

**Chúc bạn đào coin vui vẻ! 🚀💰**

