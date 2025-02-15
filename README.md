#   📚 **Autosar C Coding Guidelines**

![Build Status](https://img.shields.io/badge/build-in%20progress-yellow)            
![Language: C](https://img.shields.io/badge/Language-C-yellow?logo=c&style=flat-square)   
![Version](https://img.shields.io/badge/Version-1.0-green?style=flat-square)  

---

## 📌 **Table of Contents**  

---
## 1. Giới thiệu

Autosar C Coding Guidelines là bộ quy tắc lập trình được sử dụng trong ngành công nghiệp ô tô để đảm bảo tính an toàn, dễ bảo trì, tối ưu hiệu suất và giảm thiểu lỗi trong phần mềm nhúng.

Bộ quy tắc này giúp:
    - Chuẩn hóa mã nguồn, giúp dễ đọc và bảo trì.
    - Cải thiện tính an toàn trong hệ thống nhúng Automotive.
    - Tăng tính ổn định và hiệu suất của phần mềm.
    - Giảm thiểu lỗi phần mềm, đặc biệt là lỗi thời gian thực (real-time errors).

## 2. Quy tắc chung

### 2.1 Sử dụng tên biến, hằng số, hàm và cấu trúc có ý nghĩa

-Mục đích:
    - Giúp code dễ đọc, dễ hiểu, dễ bảo trì.
    - Giúp các lập trình viên khác (hoặc chính bạn sau này) nhanh chóng hiểu được mục đích của biến/hàm mà không cần đọc quá nhiều code.
    - Tránh gây nhầm lẫn khi sử dụng biến hoặc hằng số có tên mơ hồ.

1️⃣ Đặt tên biến có ý nghĩa

❌ Không tốt (tên không rõ ràng, không thể hiện chức năng):

```
int x, y, z;   // Không biết x, y, z dùng làm gì
float a;       // Giá trị gì? 
```
✔️ Tốt (đặt tên rõ ràng, phản ánh chức năng):

```
int vehicleSpeed;   // Tốc độ xe (km/h)
float fuelLevel;    // Mức nhiên liệu (%)
int numberOfPassengers; // Số lượng hành khách
```
- Lưu ý:
    - Tránh các tên chung chung như temp, val, data, info, x1, x2, y1, y2 nếu không có ý nghĩa cụ thể.
    - Sử dụng danh từ cho biến số (VD: batteryVoltage, engineTemperature).
    - Sử dụng động từ cho hàm (VD: calculateSpeed(), getTemperature()).

2️⃣ Đặt tên hằng số có ý nghĩa

Hằng số thường dùng để tránh các "magic numbers" (số cứng trong code).

❌ Không tốt (dùng số trực tiếp, không rõ ràng):

```
if (speed > 120) {   // 120 nghĩa là gì?
    applyBrakes();
}
```
✔️ Tốt (sử dụng hằng số có tên rõ ràng):

```
 #define MAX_SPEED 120  // Giới hạn tốc độ (km/h)

if (speed > MAX_SPEED) {
    applyBrakes();
}
```

- Lưu ý:
    - Tên hằng số nên viết hoa (MAX_SPEED, MIN_TEMPERATURE).
    - Dùng từ khóa const hoặc #define để khai báo hằng số.
    - Hằng số giúp thay đổi giá trị dễ dàng mà không phải sửa toàn bộ code.

3️⃣ Đặt tên hàm có ý nghĩa

Hàm nên có tên phản ánh chính xác chức năng của nó.

❌ Không tốt (tên mơ hồ, không rõ ý nghĩa):

```
void calc();       // Hàm này tính toán cái gì?
void set();        // Cài đặt gì?
```

✔️ Tốt (tên phản ánh chức năng cụ thể):
```
float calculateFuelEfficiency();  // Tính toán hiệu suất nhiên liệu
void setVehicleSpeed(int speed);  // Cài đặt tốc độ xe
bool isBatteryLow();              // Kiểm tra pin có yếu không
```
- Lưu ý:
    - Tên hàm bắt đầu bằng động từ (VD: get, set, calculate, check).
    - Tránh viết tắt khó hiểu (calcEff() -> calculateEfficiency()).
    - Hàm trả về giá trị boolean nên có tiền tố is, has, can (isDoorOpen(), hasLowFuel()).

4️⃣ Đặt tên struct và enum có ý nghĩa

Struct và enum giúp tổ chức dữ liệu có liên quan một cách rõ ràng.

❌ Không tốt (tên không mô tả chức năng):

```
struct data {
    int a, b;
};
```
✔️ Tốt (tên phản ánh mục đích của struct và các thành phần bên trong):
```
struct VehicleStatus {
    int speed;         // km/h
    float fuelLevel;   // %
    bool isEngineOn;
};
```
### 2.2 Sử dụng biến cục bộ để giảm thiểu phạm vi
- Mục đích
    - Tránh ảnh hưởng không mong muốn: Biến toàn cục có thể bị thay đổi ở bất kỳ đâu, dẫn đến lỗi khó kiểm soát.
    - Tối ưu bộ nhớ: Biến cục bộ được lưu trữ trong stack, trong khi biến toàn cục chiếm bộ nhớ trong suốt vòng đời của chương trình.
    - Dễ bảo trì: Khi biến chỉ được sử dụng trong một hàm hoặc một phạm vi nhất định, việc sửa đổi trở nên dễ dàng hơn.
    - Tránh lỗi xung đột biến: Khi nhiều module có biến cùng tên, việc dùng biến toàn cục có thể gây lỗi.

1️⃣ Sử dụng các biến cục bộ để giảm thiểu phạm vi của chúng

📌 Giải thích

- Biến cục bộ chỉ tồn tại trong hàm hoặc khối lệnh nơi nó được khai báo.
- Khi chương trình thoát khỏi phạm vi đó, biến sẽ tự động bị hủy, giúp tiết kiệm bộ nhớ.
- Tránh được lỗi do thay đổi ngoài ý muốn, giúp code dễ hiểu và dễ bảo trì hơn.





---
## 📞 Contact
Email: individual.thuongnguyen@gmail.com    
GitHub: [github.com/thuongnvLK](https://github.com/thuongnvLK)