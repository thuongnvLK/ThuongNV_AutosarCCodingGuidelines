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

❌ Không tốt (Dùng biến toàn cục không cần thiết)
```
#include <stdio.h>

int speed;  // Biến toàn cục - có thể bị thay đổi ở bất kỳ đâu

void setSpeed(int s) {
    speed = s;  // Không rõ ai có thể thay đổi giá trị này
}

void printSpeed() {
    printf("Tốc độ xe: %d km/h\n", speed);
}

int main() {
    setSpeed(80);
    printSpeed();  // Có thể bị thay đổi ở nơi khác mà không rõ nguyên nhân
    return 0;
}
```
🚨 Vấn đề:
- speed là biến toàn cục có thể bị thay đổi bởi bất kỳ hàm nào, gây lỗi khó kiểm soát.
- Nếu chương trình lớn, việc theo dõi giá trị của nó trở nên phức tạp.

✔️ Tốt (Dùng biến cục bộ để tránh lỗi ngoài ý muốn)
```
#include <stdio.h>

void printSpeed(int speed) {
    printf("Tốc độ xe: %d km/h\n", speed);
}

int main() {
    int speed = 80;  // Biến cục bộ
    printSpeed(speed);
    return 0;
}
```
2️⃣ Biến toàn cục chỉ nên được sử dụng khi cần thiết
📌 Giải thích
- Biến toàn cục chỉ nên được dùng khi nhiều hàm cần truy cập cùng một dữ liệu.
- Nếu một biến không thể thay thế bằng biến cục bộ, nên giới hạn phạm vi của nó bằng static.
- Biến toàn cục không nên được thay đổi tùy ý, có thể sử dụng const nếu cần.

❌ Không tốt (Dùng biến toàn cục một cách không cần thiết)
```
#include <stdio.h>

int temperature;  // Biến toàn cục có thể bị thay đổi bất kỳ lúc nào

void setTemperature(int t) {
    temperature = t;  // Không có kiểm soát
}

void printTemperature() {
    printf("Nhiệt độ hiện tại: %d°C\n", temperature);
}

int main() {
    setTemperature(25);
    printTemperature();
    return 0;
}
```
🚨 Vấn đề:
- Bất kỳ hàm nào cũng có thể thay đổi temperature, gây khó khăn trong debug.
- Nếu chương trình lớn, việc kiểm soát giá trị của temperature trở nên khó khăn.

✔️ Tốt (Sử dụng static để giới hạn phạm vi nếu cần thiết)
```
#include <stdio.h>

static int temperature;  // Biến toàn cục nhưng giới hạn phạm vi trong file

void setTemperature(int t) {
    temperature = t;
}

void printTemperature() {
    printf("Nhiệt độ hiện tại: %d°C\n", temperature);
}

int main() {
    setTemperature(25);
    printTemperature();
    return 0;
}
```
✅ Lợi ích:
- temperature vẫn là biến toàn cục nhưng chỉ có thể truy cập trong file hiện tại.
- Tránh lỗi do thay đổi giá trị ngoài phạm vi mong muốn.

📌 Khi nào nên sử dụng biến toàn cục?

| **Trường hợp**                     | **Dùng biến cục bộ** | **Dùng biến toàn cục**            |
|-------------------------------------|----------------------|----------------------------------|
| Giá trị chỉ dùng trong một hàm     | ✅                    | ❌                               |
| Giá trị cần chia sẻ giữa nhiều hàm  | ❌                    | ✅                               |
| Giá trị không cần thay đổi nhiều    | ✅                    | ❌ (Nên dùng `const`)            |
| Giá trị lưu trạng thái hệ thống     | ❌                    | ✅ (Nên dùng `static`)           |

✅ Luôn ưu tiên sử dụng biến cục bộ để giảm thiểu phạm vi sử dụng.
✅ Biến toàn cục chỉ nên dùng khi thực sự cần thiết (ví dụ: lưu trạng thái hệ thống).
✅ Sử dụng static nếu cần giới hạn phạm vi biến toàn cục.
✅ Sử dụng const nếu biến không cần thay đổi.

### 2.3 Sử dụng các hằng số để định nghĩa các giá trị không thay đổi trong chương trình

- Tránh sử dụng số cứng (magic numbers) trong chương trình.
- Dùng #define hoặc const để định nghĩa giá trị không đổi.
- Tên hằng số nên được viết hoa và sử dụng dấu gạch dưới (_) để phân tách từ.

❌ Không tốt (Dùng số cứng):
```
if (speed > 120) {   // 120 nghĩa là gì?
    applyBrakes();
}
```
✔️ Tốt (Dùng hằng số):
```
#define MAX_SPEED 120
if (speed > MAX_SPEED) {
    applyBrakes();
}
```
✔️ Tốt (Dùng const để định nghĩa hằng số kiểu dữ liệu cụ thể):
```
const int MAX_TEMPERATURE = 100;
const float PI = 3.14159;
```
✅ Lợi ích:

- Dễ hiểu hơn khi đọc code.
- Khi cần thay đổi giá trị, chỉ cần sửa đổi một lần tại nơi định nghĩa hằng số.
- Giúp chương trình dễ bảo trì và tránh lỗi do nhập sai số.

### 2.4 Khai báo các biến ở đầu của khối mã. Nếu có thể, hãy khai báo và gán giá trị ban đầu cho biến cùng lúc.

1️⃣ Tại sao nên khai báo biến ở đầu khối mã?

- Dễ đọc và bảo trì: Khi tất cả biến được khai báo ở đầu khối mã, lập trình viên dễ dàng thấy tất cả biến cần thiết mà không cần tìm kiếm trong khối mã.
- Hạn chế lỗi truy cập biến chưa được khởi tạo: Nếu khai báo biến rải rác trong khối mã, có thể xảy ra trường hợp sử dụng biến trước khi được gán giá trị.
- Tăng tính nhất quán trong lập trình: Việc khai báo ở đầu giúp người đọc dễ theo dõi biến nào được sử dụng trong một khối mã cụ thể.

❌ Không tốt (Khai báo biến giữa khối mã một cách lộn xộn)
```
#include <stdio.h>

void calculateArea() {
    int length = 5; // Biến khai báo giữa hàm
    int area;
    
    printf("Chiều dài: %d\n", length);

    int width = 10;  // Khai báo sau khi đã có logic code phía trên
    area = length * width;  // Lúc này mới đủ biến để tính toán

    printf("Diện tích: %d\n", area);
}
```
🚨 Vấn đề:
- Biến width được khai báo sau khi đã có logic khác. Điều này khiến người đọc khó xác định danh sách biến của hàm ngay từ đầu.
- area không được gán giá trị ngay khi khai báo, có thể gây lỗi nếu sử dụng trước khi được khởi tạo.

✔️ Tốt (Khai báo biến ngay đầu khối mã và gán giá trị ban đầu)

```
#include <stdio.h>

void calculateArea() {
    int length = 5;  // Khai báo và gán giá trị ngay lập tức
    int width = 10;  // Biến được khai báo ở đầu khối mã
    int area = length * width;  // Gán giá trị ngay khi khai báo

    printf("Chiều dài: %d\n", length);
    printf("Diện tích: %d\n", area);
}
```
✅ Lợi ích:
- Dễ đọc: Mọi biến cần thiết đều xuất hiện ngay khi vào khối mã.
- Tránh lỗi truy cập biến chưa được khởi tạo.
- Gọn gàng, dễ bảo trì hơn.

2️⃣ Nên gán giá trị ban đầu khi khai báo nếu có thể
- Hạn chế lỗi khi biến chưa được gán giá trị – Nếu không khởi tạo ngay, biến có thể chứa giá trị rác.
- Tăng hiệu suất – Tránh việc phải gán giá trị nhiều lần trong các điều kiện khác nhau.

❌ Không tốt (Khai báo nhưng không gán giá trị ngay)
```
#include <stdio.h>

void processTemperature() {
    int temperature; // Chưa gán giá trị ngay
    int threshold = 30;

    if (threshold > 25) {
        temperature = 35;  // Gán giá trị sau khi đã có điều kiện
    }

    printf("Nhiệt độ: %d°C\n", temperature);
}
```
🚨 Vấn đề:
- Nếu threshold <= 25, biến temperature có thể chứa giá trị rác khi in ra.
- Có thể dẫn đến lỗi không mong muốn trong chương trình.

✔️ Tốt (Gán giá trị ngay khi khai báo)
```
#include <stdio.h>

void processTemperature() {
    int temperature = 20;  // Gán giá trị mặc định ngay khi khai báo
    int threshold = 30;

    if (threshold > 25) {
        temperature = 35;  // Cập nhật giá trị nếu cần
    }

    printf("Nhiệt độ: %d°C\n", temperature);
}
```
✅ Lợi ích:
- Biến luôn có giá trị hợp lệ trước khi sử dụng.
- Không có rủi ro đọc giá trị rác từ bộ nhớ.
- Tránh được lỗi logic khi chương trình thực thi.

📌 Tổng kết

| **Trường hợp**                     | **Không tốt ❌**                                | **Tốt ✔️**                                      |
|-------------------------------------|-----------------------------------------------|------------------------------------------------|
| **Khai báo biến trong khối mã**     | Biến được khai báo rải rác trong code         | Biến được khai báo ngay đầu khối mã            |
| **Gán giá trị ngay khi khai báo**   | Biến có thể chưa được khởi tạo trước khi sử dụng | Biến luôn có giá trị hợp lệ                     |
| **Dễ đọc & bảo trì**                | Khó theo dõi danh sách biến của một hàm      | Dễ dàng hiểu tất cả biến cần thiết khi vào hàm |


✅ Khai báo tất cả các biến ở đầu khối mã để giúp code dễ đọc và tránh lỗi.

✅ Gán giá trị ngay khi khai báo nếu có thể để tránh lỗi do sử dụng biến chưa được khởi tạo.

✅ Tránh khai báo biến rải rác giữa khối mã để tăng tính nhất quán và dễ bảo trì.


---
## 📞 Contact
Email: individual.thuongnguyen@gmail.com    
GitHub: [github.com/thuongnvLK](https://github.com/thuongnvLK)