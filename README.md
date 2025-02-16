#   📚 **Autosar C Coding Guidelines**

![Build Status](https://img.shields.io/badge/build-in%20progress-yellow)            
![Language: C](https://img.shields.io/badge/Language-C-yellow?logo=c&style=flat-square)   
![Version](https://img.shields.io/badge/Version-1.0-green?style=flat-square)  

---

## 📌 **Table of Contents**  

---
## 1. Giới thiệu

- Autosar C Coding Guidelines là bộ quy tắc lập trình được sử dụng trong ngành công nghiệp ô tô để đảm bảo tính an toàn, dễ bảo trì, tối ưu hiệu suất và giảm thiểu lỗi trong phần mềm nhúng.

-Bộ quy tắc này giúp:
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
✅ Lợi ích:
- Không cần mở struct vẫn hiểu được nó lưu dữ liệu gì.

❌ Không tốt (Tên enum chung chung, khó hiểu)
```
enum Mode {
    OFF, ON, AUTO
};
```
🚨 Vấn đề:
- "ON" có nghĩa gì? ON cái gì?
- Có thể gây nhầm lẫn với các trạng thái khác trong chương trình.

✔️ Tốt (Tên enum rõ ràng, có tiền tố nhận diện)
```
enum EngineMode {
    ENGINE_OFF,
    ENGINE_ON,
    ENGINE_AUTO
};
```
✅ Lợi ích:
- Không bị nhầm với các enum khác.
- Dễ hiểu ngay từ tên gọi.

5️⃣ Đặt tên biến có tiền tố để phân biệt kiểu dữ liệu

- Tiền tố giúp dễ nhận biết kiểu dữ liệu của biến ngay từ tên biến.
❌ Không tốt (Tên biến không rõ kiểu dữ liệu)
```
int num = 10;
float level = 5.5;
```
✔️ Tốt (Dùng tiền tố nhận dạng kiểu dữ liệu)
```
int iNumCars = 10;       // Biến int lưu số xe
float fFuelLevel = 5.5;  // Biến float lưu mức nhiên liệu
```
✅ Lợi ích:
- Dễ hiểu ngay từ tên biến mà không cần đọc dòng khai báo.
- Hạn chế lỗi do nhầm kiểu dữ liệu.


📌 Quy tắc đặt tên theo kiểu CamelCase, PascalCase và Snake_Case

| **Loại**              | **Quy tắc**                       | **Ví dụ tốt**                        |
|----------------------|--------------------------------|------------------------------------|
| **Biến & Hàm**       | Dùng **camelCase**            | `calculateSpeed()`, `fuelLevel`  |
| **Struct, Enum**     | Dùng **PascalCase**           | `struct CarModel`, `enum DriveMode` |
| **Hằng số**          | Dùng **SNAKE_CASE**           | `#define MAX_SPEED 120`           |
| **Tiền tố kiểu dữ liệu** | Dùng **prefix để nhận diện kiểu** | `iNumCars` (int), `fFuelLevel` (float) |


### 📌 So sánh đặt tên không tốt và tốt

| **Loại**   | **Không tốt ❌**         | **Tốt ✔️**                  |
|------------|-------------------------|-----------------------------|
| **Biến**   | `int x, y, z;`          | `int vehicleSpeed;`        |
| **Hằng số**| `#define VALUE 120`      | `#define MAX_SPEED 120`     |
| **Hàm**    | `void process();`       | `void processUserInput();` |
| **Struct** | `struct Info {};`       | `struct CarStatus {};`     |
| **Enum**   | `enum Mode {ON, OFF};`  | `enum EngineMode {ENGINE_ON};` |


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

❌ Không tốt (Khai báo biến bên trong vòng lặp không cần thiết)
```
#include <stdio.h>

void printNumbers() {
    for (int i = 0; i < 5; i++) {
        int number = i * 2;  // Khai báo biến ngay trong vòng lặp
        printf("%d ", number);
    }
}
```
🚨 Vấn đề:
- Biến number được tạo lại trong mỗi vòng lặp, gây tốn tài nguyên không cần thiết.
- Biến bị cấp phát và giải phóng liên tục, không tối ưu hiệu suất.

✔️ Tốt (Khai báo biến trước vòng lặp nếu không cần thay đổi mỗi lần lặp)
```
#include <stdio.h>

void printNumbers() {
    int number;  // Khai báo biến trước vòng lặp để tránh cấp phát lại nhiều lần

    for (int i = 0; i < 5; i++) {
        number = i * 2;
        printf("%d ", number);
    }
}
```
✅ Lợi ích:
Tối ưu bộ nhớ, vì biến không bị cấp phát lại trong mỗi lần lặp.
Hiệu suất tốt hơn so với khai báo bên trong vòng lặp.

📌 Khi nào nên khai báo biến ngay khi cần sử dụng?

Mặc dù Autosar khuyến nghị khai báo biến ở đầu khối mã, nhưng có một số trường hợp ngoại lệ.Ví dụ: Nếu một biến chỉ dùng trong một nhánh if hoặc for, có thể khai báo ngay lúc cần để tiết kiệm bộ nhớ.

✔️ Ví dụ hợp lý (Khai báo trong phạm vi cần thiết)
```
#include <stdio.h>

void checkEvenOdd(int number) {
    if (number % 2 == 0) {
        int isEven = 1;  // Biến chỉ dùng trong nhánh này
        printf("%d là số chẵn\n", number);
    } else {
        int isOdd = 1;  // Biến chỉ dùng trong nhánh này
        printf("%d là số lẻ\n", number);
    }
}
```
✅ Lợi ích:
- Biến isEven và isOdd chỉ tồn tại khi cần, giúp tiết kiệm bộ nhớ.


📌 Tổng kết

| **Trường hợp**                     | **Không tốt ❌**                                | **Tốt ✔️**                                      |
|-------------------------------------|-----------------------------------------------|------------------------------------------------|
| **Khai báo biến trong khối mã**     | Biến được khai báo rải rác trong code         | Biến được khai báo ngay đầu khối mã            |
| **Gán giá trị ngay khi khai báo**   | Biến có thể chưa được khởi tạo trước khi sử dụng | Biến luôn có giá trị hợp lệ                     |
| **Dễ đọc & bảo trì**                | Khó theo dõi danh sách biến của một hàm      | Dễ dàng hiểu tất cả biến cần thiết khi vào hàm |


✅ Khai báo tất cả các biến ở đầu khối mã để giúp code dễ đọc và tránh lỗi.

✅ Gán giá trị ngay khi khai báo nếu có thể để tránh lỗi do sử dụng biến chưa được khởi tạo.

✅ Tránh khai báo biến rải rác giữa khối mã để tăng tính nhất quán và dễ bảo trì.

### 2.5 Sử dụng các toán tử và hàm thư viện chuẩn của ngôn ngữ

- Hàm thư viện chuẩn được tối ưu về hiệu suất và độ tin cậy.
- Tránh viết lại những chức năng có sẵn, giúp code ngắn gọn và dễ đọc.
- Giảm thiểu lỗi do tự viết lại các phép toán hoặc xử lý chuỗi.

❌ Không tốt (Viết lại hàm có sẵn một cách không cần thiết)

```
#include <stdio.h>

// Viết lại hàm tính độ dài chuỗi (không cần thiết)
int stringLength(const char *str) {
    int length = 0;
    while (str[length] != '\0') {
        length++;
    }
    return length;
}

int main() {
    char name[] = "Autosar";
    printf("Length: %d\n", stringLength(name));
    return 0;
}
```
🚨 Vấn đề:
- Hàm stringLength() thực hiện công việc mà strlen() trong <string.h> đã làm.
- Tốn công viết lại và có thể có lỗi không mong muốn.

✔️ Tốt (Sử dụng thư viện chuẩn)

```
#include <stdio.h>
#include <string.h>  // Dùng thư viện có sẵn

int main() {
    char name[] = "Autosar";
    printf("Length: %lu\n", strlen(name)); // Sử dụng hàm chuẩn strlen()
    return 0;
}
```
✅ Lợi ích:
- Code ngắn gọn, dễ hiểu.
- Tận dụng tối ưu hóa của thư viện chuẩn.

### 2.6  Sử dụng lệnh điều kiện và vòng lặp một cách cẩn thận

- Vòng lặp và điều kiện không hợp lý có thể gây hiệu năng kém và khó đọc.
- Lồng ghép if-else quá mức làm code khó hiểu.

❌ Không tốt (Dùng if-else không tối ưu)
```
if (x == 1) {
    action1();
} else if (x == 2) {
    action2();
} else if (x == 3) {
    action3();
} else {
    actionDefault();
}
```
🚨 Vấn đề:
- Quá nhiều lệnh if-else, làm code dài dòng, khó mở rộng.

✔️ Tốt (Dùng switch-case để tối ưu)
```
switch (x) {
    case 1:
        action1();
        break;
    case 2:
        action2();
        break;
    case 3:
        action3();
        break;
    default:
        actionDefault();
}
```
✅ Lợi ích:
- Nhanh hơn và dễ đọc hơn.
- Dễ bảo trì, chỉ cần thêm case mới nếu cần.

### 2.7 Tránh sử dụng quá nhiều tham số trong hàm

- Nếu hàm có quá nhiều tham số, nó trở nên khó hiểu và khó gọi đúng.
- Nên dùng struct hoặc class để truyền nhóm tham số liên quan.

❌ Không tốt (Quá nhiều tham số riêng lẻ)
```
void configureCar(int speed, int fuel, int temperature, int engineMode, int airPressure) {
    // Code xử lý
}
```
🚨 Vấn đề:
- Khi gọi hàm, dễ nhầm lẫn thứ tự các tham số.
- Khi cần thêm một tham số mới, phải sửa đổi tất cả nơi gọi hàm.

✔️ Tốt (Dùng struct để đóng gói các tham số)
```
typedef struct {
    int speed;
    int fuel;
    int temperature;
    int engineMode;
    int airPressure;
} CarConfig;

void configureCar(CarConfig config) {
    // Code xử lý
}

int main() {
    CarConfig car = {120, 80, 30, 1, 35};
    configureCar(car);
    return 0;
}
```
✅ Lợi ích:
- Dễ đọc, dễ mở rộng.
- Không lo nhầm thứ tự tham số.

### 2.8 Sử dụng cấu trúc dữ liệu và lớp đối tượng hợp lý

- Tránh truy cập trực tiếp vào thành viên của struct.
- Sử dụng getter/setter để bảo vệ dữ liệu.

❌ Không tốt (Truy cập trực tiếp vào biến)
```
typedef struct {
    int speed;
} Car;

int main() {
    Car car;
    car.speed = 120;  // Truy cập trực tiếp
    return 0;
}
```
🚨 Vấn đề:
- Nếu sau này cần kiểm soát dữ liệu, phải sửa đổi tất cả nơi truy cập biến.

✔️ Tốt (Dùng hàm getter và setter)
```
#include <stdio.h>

typedef struct {
    int speed;
    int fuelLevel;
} Car;

// Hàm setter để kiểm soát giá trị tốc độ
void setSpeed(Car *car, int speed) {
    if (speed >= 0 && speed <= 200) {  // Giới hạn tốc độ
        car->speed = speed;
    } else {
        printf("Invalid speed value!\n");
    }
}

// Hàm getter để lấy giá trị tốc độ
int getSpeed(Car car) {
    return car.speed;
}

// Hàm setter để kiểm soát mức nhiên liệu
void setFuelLevel(Car *car, int fuel) {
    if (fuel >= 0 && fuel <= 100) {  // Giới hạn mức nhiên liệu
        car->fuelLevel = fuel;
    } else {
        printf("Invalid fuel level!\n");
    }
}

// Hàm getter để lấy mức nhiên liệu
int getFuelLevel(Car car) {
    return car.fuelLevel;
}

int main() {
    Car myCar;
    setSpeed(&myCar, 120);   // Gán tốc độ một cách kiểm soát
    setFuelLevel(&myCar, 80); // Gán mức nhiên liệu một cách kiểm soát

    printf("Speed: %d km/h\n", getSpeed(myCar));
    printf("Fuel Level: %d%%\n", getFuelLevel(myCar));

    return 0;
}
```
✅ Lợi ích:
- Bảo vệ dữ liệu: Không thể thay đổi giá trị speed hoặc fuelLevel ngoài phạm vi cho phép.
- Tăng tính bảo trì: Nếu cần thay đổi logic kiểm tra dữ liệu, chỉ cần sửa trong setter, không cần sửa toàn bộ chương trình.
- Dễ mở rộng: Nếu cần thêm thuộc tính, chỉ cần thêm vào struct và viết thêm getter/setter.

### 2.9 Sử dụng comment để giải thích mã

- Comment giúp người khác hiểu logic của code.
- Không nên viết comment dư thừa.

❌ Không tốt (Comment không cần thiết)

```
int speed = 100; // Đặt tốc độ bằng 100
speed += 10; // Cộng thêm 10 vào tốc độ
```
🚨 Vấn đề:
- Comment không mang lại giá trị bổ sung.

✔️ Tốt (Comment rõ ý nghĩa của code)
```
/* Giới hạn tốc độ tối đa cho xe */
#define MAX_SPEED 120

void setSpeed(int *speed, int value) {
    /* Đảm bảo tốc độ không vượt quá giới hạn */
    if (value > MAX_SPEED) {
        *speed = MAX_SPEED;
    } else {
        *speed = value;
    }
}
```
✅ Lợi ích:
- Comment giải thích phần khó hiểu chứ không lặp lại code.

### 2.10  Sử dụng kiểu dữ liệu phù hợp

- Tránh dùng kiểu dữ liệu lớn hơn mức cần thiết.

❌ Không tốt (Dùng kiểu không phù hợp)
```
int isEngineOn = 1;  // Biến bool nhưng dùng int
```
🚨 Vấn đề:
- Lãng phí bộ nhớ nếu chỉ cần lưu true/false.

✔️ Tốt (Dùng bool thay vì int)
```
#include <stdbool.h>
bool isEngineOn = true;
```
✅ Lợi ích:
- Tiết kiệm bộ nhớ và dễ hiểu hơn.

### 2.11 Kiểm tra lỗi và xử lý ngoại lệ đúng cách

- Không nên bỏ qua giá trị trả về của hàm quan trọng.

❌ Không tốt (Không kiểm tra lỗi khi mở file)
```
FILE *file = fopen("data.txt", "r");
```
🚨 Vấn đề:
- Nếu file không tồn tại, chương trình sẽ bị lỗi.

✔️ Tốt (Kiểm tra lỗi sau khi mở file)
```
FILE *file = fopen("data.txt", "r");
if (file == NULL) {
    perror("Error opening file");
    return -1;
}
```
✅ Lợi ích:
- Chương trình không bị crash nếu có lỗi.

### 3. Quy tắt đặt tên

**1️⃣ Sử dụng tên biến, hằng số, hàm và cấu trúc có ý nghĩa**
**🔍 Giải thích**
- Tên biến, hằng số, hàm và cấu trúc cần **phản ánh chính xác chức năng và mục đích của chúng**.
- Tránh sử dụng tên chung chung như `x`, `y`, `temp`, `data`.

**❌ Không tốt (Tên không có ý nghĩa)**
```c
int x, y, z;
float a;
void func();
```
**✔️ Tốt (Tên có ý nghĩa, dễ hiểu)**
```c
int vehicleSpeed;    // Tốc độ xe
float fuelLevel;     // Mức nhiên liệu
void calculateFuelConsumption(); // Tính toán mức tiêu thụ nhiên liệu
```
**2️⃣ Sử dụng các từ viết tắt chỉ khi cần thiết**

- Tránh dùng từ viết tắt không rõ ràng, trừ khi chúng rất phổ biến (CPU, RAM, LED).
- Nếu bắt buộc phải dùng, hãy đảm bảo tất cả lập trình viên đều hiểu ý nghĩa của nó.

❌ Không tốt (Dùng từ viết tắt khó hiểu)

```
int tmpVar;  // "tmpVar" là gì? Temporary variable hay Temperature Variable?
```
🚨 Vấn đề:
- Từ viết tắt tmpVar có thể gây hiểu lầm.

✔️ Tốt (Chỉ dùng từ viết tắt phổ biến hoặc dễ hiểu)
```
int tempVariable;   // Biến nhiệt độ (Temperature Variable)
int cpuLoad;        // Tải của CPU
```
✅ Lợi ích:
- Tránh nhầm lẫn, dễ đọc.
- Người mới đọc code không cần đoán nghĩa của biến.

3️⃣ Sử dụng CamelCase cho các biến và hàm

- CamelCase: Chữ cái đầu tiên viết thường, các từ sau viết hoa (camelCase).
- Dùng cho tên biến và tên hàm.

❌ Không tốt (Không theo quy tắc CamelCase)
```
int vehicle_speed;
void get_vehicle_info();
```
🚨 Vấn đề:
- Dùng dấu gạch dưới _ thay vì CamelCase.

✔️ Tốt (Dùng CamelCase)
```
int vehicleSpeed;
void getVehicleInfo();
```
✅ Lợi ích:
- Thống nhất với tiêu chuẩn đặt tên của Autosar C.
- Dễ đọc hơn, đặc biệt khi so với snake_case trong ngôn ngữ khác như Python.

4️⃣ Sử dụng PascalCase cho struct và enum

- PascalCase: Chữ cái đầu của mỗi từ viết hoa (PascalCase).
- Dùng cho struct, enum, class.

❌ Không tốt (Không dùng PascalCase)
```
struct car_model { ... };
enum drive_mode { OFF, ON };
```
🚨 Vấn đề:
- car_model và drive_mode không theo chuẩn PascalCase.

✔️ Tốt (Dùng PascalCase)
```
struct CarModel { ... };
enum DriveMode { OFF, ON };
```
✅ Lợi ích:
- Thống nhất cách đặt tên giữa struct và enum.
- Dễ phân biệt với biến và hàm, giúp code dễ đọc.

5️⃣ Sử dụng chữ hoa cho các hằng số

- Hằng số nên viết toàn bộ bằng chữ hoa, cách nhau bằng dấu _ (SNAKE_CASE).
- Giúp phân biệt rõ ràng với biến thông thường.

❌ Không tốt (Không dùng chữ hoa cho hằng số)
```
const int MaxSpeed = 120;
```
🚨 Vấn đề:
- MaxSpeed không tuân theo quy tắc SNAKE_CASE.

✔️ Tốt (Dùng chữ hoa và dấu _)
```
#define MAX_SPEED 120
const int DEFAULT_SPEED_LIMIT = 60;
```
✅ Lợi ích:
- Dễ nhận diện ngay là hằng số.
- Không gây nhầm lẫn với biến thông thường.

6️⃣ Sử dụng từ khóa đặc biệt (const, static) để làm rõ vai trò của biến

- const: Dùng để khai báo biến không thay đổi được.
- static: Dùng để giới hạn phạm vi của biến trong file hoặc hàm.

❌ Không tốt (Không chỉ rõ biến là hằng số hay cục bộ)
```
int maxSpeed = 120;  // Biến này có thể bị thay đổi ngoài ý muốn
```
✔️ Tốt (Dùng const và static để làm rõ vai trò)
```
const int MAX_SPEED = 120;  // Giá trị cố định
static int counter = 0;      // Biến cục bộ, chỉ dùng trong file này
```
7️⃣ Đặt tên biến theo quy tắc "type + name"

- Tiền tố thể hiện kiểu dữ liệu (i cho int, f cho float, p cho con trỏ, v.v.).

❌ Không tốt (Tên biến không rõ kiểu dữ liệu)
```
int numCars;
float salesTotal;
```
🚨 Vấn đề:
- Không rõ kiểu dữ liệu ngay từ tên biến.

✔️ Tốt (Thêm tiền tố để biểu thị kiểu dữ liệu)

```
int iNumCars;
float fSalesTotal;

```
✅ Lợi ích:
- Nhanh chóng nhận biết kiểu dữ liệu chỉ bằng cách đọc tên biến.

8️⃣ Đặt tên hàm callback có hậu tố Callback

- Nếu một hàm là hàm callback, hãy thêm hậu tố Callback để dễ nhận diện.

❌ Không tốt (Không có hậu tố Callback)
```
void ButtonClicked();
```
🚨 Vấn đề:
- Không rõ hàm này là một callback hay một hàm thông thường.

✔️ Tốt (Thêm hậu tố Callback)
```
void ButtonClickedCallback();
```
📌 Hàm Callback
✔️ Tốt (Dùng Callback giúp linh hoạt hơn)
```
#include <stdio.h>

// Hàm callback thực thi một nhiệm vụ (đổi tên để rõ ràng hơn)
void taskExecutionCallback() {
    printf("Executing scheduled task...\n");
}

// Hàm nhận callback để thực thi một nhiệm vụ
void scheduleTaskHandler(void (*callback)()) {
    printf("Running scheduler...\n");
    callback();  // Gọi callback để thực thi nhiệm vụ
}

int main() {
    scheduleTaskHandler(taskExecutionCallback);  // Truyền callback
    return 0;
}
```
📌 Nguyên tắc đặt tên hậu tố cho callback

| **Loại Callback**                          | **Hậu tố nên dùng**                 | **Ví dụ**                              |
|--------------------------------------------|------------------------------------|--------------------------------------|
| **Xử lý sự kiện (Event Handler)**          | `Callback`, `Handler`, `Event`     | `buttonPressCallback()`, `onDataReceivedHandler()` |
| **Thực thi tác vụ (Execution Task)**       | `Execute`, `Task`, `Action`        | `sendDataExecute()`, `logDataTask()` |
| **Cảm biến & tín hiệu (Sensor/Signal Processing)** | `Update`, `Change`, `Trigger`  | `temperatureUpdate()`, `signalChangeHandler()` |
| **Giao diện người dùng (GUI Callback)**    | `On<Event>()`, `Listener`, `Click` | `onButtonClick()`, `mouseMoveListener()` |
| **Giao tiếp (Communication Callback)**     | `Received`, `Sent`, `Process`      | `onDataReceived()`, `messageSentCallback()` |


📌 Tổng kết quy tắc đặt tên

| **Quy tắc**                          | **Không tốt ❌**                  | **Tốt ✔️**                     |
|--------------------------------------|--------------------------------|-------------------------------|
| **Tên biến có ý nghĩa**              | `int x, y, z;`                | `int vehicleSpeed;`           |
| **Dùng từ viết tắt hợp lý**          | `int tmpVar;`                 | `int tempVariable;`           |
| **Dùng CamelCase cho biến & hàm**    | `int vehicle_speed;`          | `int vehicleSpeed;`           |
| **Dùng PascalCase cho struct & enum**| `struct car_model {};`        | `struct CarModel {};`         |
| **Hằng số viết hoa**                 | `const int MaxSpeed = 120;`   | `#define MAX_SPEED 120`       |
| **Dùng `const` và `static` hợp lý**  | `int maxSpeed = 120;`         | `const int MAX_SPEED = 120;`  |
| **Đặt tên hàm callback**             | `void ButtonClicked();`       | `void ButtonClickedCallback();` |

### 4. Quy tắc về comment

1️⃣ Block-Level Comment (Comment cấp khối)

🔍 Giải thích
- Mục đích: Được đặt ở đầu file, module hoặc class, cung cấp thông tin tổng quan về phạm vi code.
- Nội dung: Chứa thông tin về file, tác giả, ngày tạo, mô tả.
- Cú pháp: Được viết trong /* */.

❌ Không tốt (Thiếu thông tin mô tả file)
```
// sample_file.c
int main() {
    return 0;
}
```
🚨 Vấn đề:
- Không rõ file này dùng để làm gì.
- Không có thông tin về tác giả, ngày tạo.

✔️ Tốt (Đầy đủ thông tin với Block-Level Comment)
```
/*
* File: sample_file.c
* Author: John Doe
* Date: 24/03/2023
* Description: This is a sample file for demonstrating block-level comment.
*/
#include <stdio.h>

int main() {
    printf("Hello, world!\n");
    return 0;
}
```
✅ Lợi ích:
- Người đọc biết file này dùng để làm gì.
- Có thông tin về tác giả và ngày tạo, giúp bảo trì dễ dàng hơn.

2️⃣ Function-Level Comment (Comment cấp hàm)

🔍 Giải thích
- Mục đích: Được đặt trước một hàm để mô tả chức năng, tham số, giá trị trả về.
- Cấu trúc:
    - Tên hàm
    - Mô tả chức năng
    - Danh sách tham số (Input)
    - Giá trị trả về (Output)

❌ Không tốt (Không có mô tả rõ ràng)
```
int calculateSum(int a, int b) {
    return a + b;
}
```
🚨 Vấn đề:
- Người đọc không biết hàm này dùng để làm gì.
- Không rõ kiểu dữ liệu của tham số và giá trị trả về.

✔️ Tốt (Đầy đủ thông tin với Function-Level Comment)

```
/*
* Function: calculateSum
* Description: This function calculates the sum of two integers.
* Input:
*   a - an integer value
*   b - an integer value
* Output:
*   returns the sum of a and b
*/
int calculateSum(int a, int b) {
    return a + b;
}
```
✅ Lợi ích:
- Dễ dàng hiểu ý nghĩa của hàm mà không cần đọc code bên trong.
- Giúp hạn chế lỗi khi sử dụng sai kiểu dữ liệu hoặc tham số.

### 5. Quy tắc về xử lý lỗi

1️⃣ Tránh sử dụng goto để xử lý lỗi

🔍 Giải thích
Không nên dùng goto vì nó làm code khó đọc, khó debug và dễ gây lỗi.
Nên dùng if-else hoặc switch-case để xử lý lỗi một cách có tổ chức.

❌ Không tốt (Dùng goto)
```
int process_data(int data) {
    int result = 0;
    if (data < 0) {
        goto error;
    }
    result = data * 2;
    return result;
error:
    return -1;
}
```
🚨 Vấn đề:
- goto error; làm chương trình khó theo dõi khi debug.
- Khi có nhiều lỗi hơn, sẽ có nhiều nhãn error, khiến code rối và khó duy trì.

✔️ Tốt (Dùng if-else)
```
int process_data(int data) {
    if (data < 0) {
        return -1;
    }
    int result = data * 2;
    return result;
}
```
✅ Lợi ích:
- Code gọn gàng, dễ đọc, dễ debug.
- Không cần theo dõi jump của goto, giúp chương trình dễ hiểu hơn.

2️⃣ Dùng hằng số để đại diện cho mã lỗi (Error Codes)

🔍 Giải thích
- Không nên dùng số nguyên trực tiếp (-1, 0, 1) để báo lỗi vì nó không rõ nghĩa.
- Nên định nghĩa hằng số lỗi (#define ERROR_CODE) để code dễ hiểu hơn.

❌ Không tốt (Dùng số cứng -1)
```
int open_file() {
    FILE* fp = fopen("file.txt", "r");
    if (fp == NULL) {
        return -1;  // Không rõ nghĩa
    }
    return 0;
}
```
🚨 Vấn đề:
- Người đọc không biết -1 có nghĩa là gì.
- Nếu có nhiều loại lỗi khác nhau, việc dùng số cứng sẽ gây nhầm lẫn.

✔️ Tốt (Dùng hằng số lỗi)
```
#define FILE_OPEN_FAILED -1

int open_file() {
    FILE* fp = fopen("file.txt", "r");
    if (fp == NULL) {
        return FILE_OPEN_FAILED;
    }
    return 0;
}
```
✅ Lợi ích:
- Dễ đọc, dễ bảo trì, không cần nhớ -1 là lỗi gì.
- Nếu sau này cần thay đổi mã lỗi (-2, -100...), chỉ cần sửa hằng số.

3️⃣ Sử dụng các hàm xử lý lỗi chuẩn (perror(), strerror())

🔍 Giải thích
- Không nên dùng printf() để báo lỗi vì nó không cung cấp thông tin chi tiết.
- Nên dùng perror() hoặc strerror() để hiển thị thông báo lỗi mô tả chi tiết nguyên nhân.

❌ Không tốt (Dùng printf() để báo lỗi)
```
void process_data() {
    int result = some_function();
    if (result == -1) {
        printf("Error: Some error occurred\n");
    }
}
```
🚨 Vấn đề:
- Không rõ lỗi là do tệp tin, bộ nhớ hay kết nối.
- Người dùng phải tự đoán nguyên nhân lỗi.

✔️ Tốt (Dùng perror())
```
#include <stdio.h>
#include <errno.h>

void process_data() {
    int result = some_function();
    if (result == -1) {
        perror("Error");
    }
}
```
✅ Lợi ích:
- perror("Error") sẽ tự động in lỗi hệ thống kèm mô tả chi tiết (ví dụ: "No such file or directory").
- Không cần tự định nghĩa thông báo lỗi, giúp tăng độ chính xác khi debug.

4️⃣ Kiểm tra lỗi ngay sau khi gọi hàm (Check Error Immediately)

- Luôn kiểm tra kết quả của hàm ngay sau khi gọi để tránh lỗi lan rộng.
- Không nên bỏ qua giá trị trả về của hàm, đặc biệt với các hàm mở file, cấp phát bộ nhớ, v.v.

❌ Không tốt (Không kiểm tra lỗi sau khi gọi hàm)
```
FILE *fp = fopen("data.txt", "r");
// Không kiểm tra xem fopen có thành công không
fprintf(fp, "Writing data...\n");
fclose(fp);
```
🚨 Vấn đề:
- Nếu fopen() thất bại (fp == NULL), chương trình sẽ gây lỗi truy cập bộ nhớ (segmentation fault).

✔️ Tốt (Kiểm tra lỗi ngay lập tức)
```
FILE *fp = fopen("data.txt", "r");
if (fp == NULL) {
    perror("Failed to open file");
    return FILE_OPEN_FAILED;
}
fprintf(fp, "Writing data...\n");
fclose(fp);
```
✅ Lợi ích:
- Tránh lỗi truy cập bộ nhớ khi file không thể mở.
- Giúp phát hiện lỗi sớm, dễ debug hơn.

5️⃣ Tránh xử lý lỗi chung chung (Generic Error Handling)

- Không nên trả về mã lỗi chung chung như -1 hoặc NULL.
- Nên sử dụng mã lỗi cụ thể và có ý nghĩa.

❌ Không tốt (Trả về lỗi chung chung)
```
int readSensor() {
    if (sensor_read() < 0) {
        return -1;  // Không rõ lỗi gì
    }
    return 0;
}
```
🚨 Vấn đề:
- Không biết lỗi do cảm biến mất kết nối, dữ liệu sai hay lỗi phần cứng.

✔️ Tốt (Dùng mã lỗi cụ thể)
```
#define SENSOR_DISCONNECTED -1
#define SENSOR_DATA_INVALID -2

int readSensor() {
    int status = sensor_read();
    if (status == NO_CONNECTION) {
        return SENSOR_DISCONNECTED;
    } else if (status == INVALID_DATA) {
        return SENSOR_DATA_INVALID;
    }
    return 0;
}
```
✅ Lợi ích:
- Dễ dàng phân biệt lỗi, từ đó có cách xử lý thích hợp.
- Giúp chương trình ổn định hơn khi có lỗi.

6️⃣ Ghi log lỗi để dễ debug (Logging Errors for Debugging)

- Khi phát hiện lỗi, nên ghi log để giúp debug dễ dàng hơn.
- Dùng fprintf(stderr, ...) hoặc hệ thống logging thay vì chỉ trả về mã lỗi.

❌ Không tốt (Chỉ trả về mã lỗi mà không ghi log)
```
int connectToServer() {
    if (network_connect() < 0) {
        return NETWORK_FAILED;
    }
    return 0;
}
```
🚨 Vấn đề:
- Không biết lỗi xảy ra khi nào và tại đâu.

✔️ Tốt (Ghi log lỗi giúp debug dễ dàng hơn)
```
int connectToServer() {
    if (network_connect() < 0) {
        fprintf(stderr, "Error: Failed to connect to server\n");
        return NETWORK_FAILED;
    }
    return 0;
}
```
✅ Lợi ích:
- Dễ tìm nguyên nhân lỗi hơn khi debug.
- Ghi lại lịch sử lỗi giúp cải thiện phần mềm về lâu dài.

📌 Tổng kết Quy tắc xử lý lỗi

| **Quy tắc xử lý lỗi**      | **Không tốt ❌**                  | **Tốt ✔️**                                |
|---------------------------|--------------------------------|--------------------------------|
| **Tránh dùng `goto`**     | Xử lý lỗi bằng `goto error;`  | Sử dụng `if-else` để xử lý lỗi rõ ràng |
| **Sử dụng hằng số lỗi**    | Trả về số cứng (`-1`, `0`, `1`) | Định nghĩa mã lỗi bằng `#define` |
| **Dùng hàm xử lý lỗi chuẩn** | `printf("Error occurred");`  | `perror("Error");` để in lỗi chi tiết |


### 6. Quy tắc về định dạng code

1️⃣ Khoảng trắng và dòng trống

🔍 Giải thích
- Sử dụng khoảng trắng và dòng trống để phân tách các phần code quan trọng giúp dễ đọc hơn.
- Nên để dòng trống giữa các hàm, khối lệnh lớn, hoặc đoạn code có chức năng khác nhau.

❌ Không tốt (Code dính chặt, khó đọc)
```
if(condition){
// Code xử lý lỗi
printf("Error occurred\n");}
```
🚨 Vấn đề:
- Không có khoảng trắng sau if(condition).
- {} không có dòng trống, gây khó đọc.

✔️ Tốt (Có khoảng trắng và dòng trống hợp lý)
```
if (condition) {
    // Code xử lý lỗi
    printf("Error occurred\n");
}
```
✅ Lợi ích:
- Dễ nhìn, dễ hiểu, code gọn gàng hơn.

2️⃣ Thụt đầu dòng (Indentation - Sử dụng 4 khoảng trắng)

🔍 Giải thích
- Luôn thụt đầu dòng 4 khoảng trắng, KHÔNG dùng tab.
- Giúp code có cấu trúc rõ ràng, dễ theo dõi logic.

❌ Không tốt (Không thụt đầu dòng hoặc dùng tab)
```
void example_function(){
if(condition){
printf("Condition met\n");
}
else{
printf("Condition not met\n");
}
}
```
🚨 Vấn đề:
- {} không thụt đầu dòng rõ ràng.
- if, else không rõ ràng, dễ gây nhầm lẫn.

✔️ Tốt (Thụt đầu dòng 4 khoảng trắng)
```
void example_function() {
    if (condition) {
        printf("Condition met\n");
    } else {
        printf("Condition not met\n");
    }
}
```
✅ Lợi ích:
- Cấu trúc logic rõ ràng, dễ đọc hơn.

3️⃣ Khoảng trắng trong phép toán

🔍 Giải thích
- Nên dùng khoảng trắng trước và sau các toán tử (+, -, =, ==, etc.) để code dễ đọc hơn.

❌ Không tốt (Không có khoảng trắng)
```
int result=num1+num2*value;
```
🚨 Vấn đề:
- Code khó đọc, dính chặt vào nhau.

✔️ Tốt (Có khoảng trắng hợp lý)

```
int result = num1 + num2 * value;
```
✅ Lợi ích:
- Dễ hiểu hơn, giảm lỗi đọc sai công thức.

4️⃣ Khoảng trắng trong danh sách tham số hàm

🔍 Giải thích
- Dùng khoảng trắng sau dấu , trong danh sách tham số hàm để tăng khả năng đọc.

❌ Không tốt (Không có khoảng trắng sau ,)
```
void example_function(int arg1,float arg2,char arg3) {
    // Code here
}
```
🚨 Vấn đề:
- Khó đọc vì các tham số dính vào nhau.

✔️ Tốt (Dùng khoảng trắng sau dấu ,)
```
void example_function(int arg1, float arg2, char arg3) {
    // Code here
}
```
✅ Lợi ích:
- Rõ ràng, dễ đọc danh sách tham số.

5️⃣ Giới hạn chiều dài dòng code (Không quá 80 ký tự)

🔍 Giải thích
- Không nên viết một dòng quá dài, vì sẽ khó đọc trên màn hình nhỏ hoặc cửa sổ terminal.
- Nếu một dòng quá dài (hơn 80 ký tự), hãy chia nhỏ thành nhiều dòng.

❌ Không tốt (Dòng code quá dài, khó đọc)
```
void example_function(int arg1, float arg2, char arg3) { if (arg1 > 0 && arg2 < 10.0 && arg3 == 'A') { // Thực hiện lệnh } }
```
🚨 Vấn đề:
- Khó đọc, dễ bị cuộn ngang khi xem trên màn hình nhỏ.

✔️ Tốt (Chia nhỏ dòng dài thành nhiều dòng)
```
void example_function(int arg1, float arg2, char arg3) {
    if (arg1 > 0 && 
        arg2 < 10.0 && 
        arg3 == 'A') 
    {
        // Thực hiện lệnh
    }
}
```
✅ Lợi ích:
- Dễ đọc, dễ bảo trì, không bị cuộn ngang.

6️⃣ Luôn dùng {} trong if-else, ngay cả khi chỉ có một dòng

- Tránh lỗi logic khi mở rộng code.
- Dễ đọc, dễ duy trì.

❌ Không tốt (Không có {})
```
if (condition)
    printf("Condition met\n");
```
🚨 Vấn đề:
- Nếu thêm dòng mới mà quên {}, chương trình có thể chạy sai logic.

✔️ Tốt (Luôn dùng {})
```
if (condition) {
    printf("Condition met\n");
}
```
✅ Lợi ích:
- Dễ mở rộng code mà không gây lỗi.

7️⃣ Thụt đầu dòng chuẩn trong switch-case

- Tránh lỗi thiếu break; gây fall-through ngoài ý muốn.
Dễ đọc hơn.

❌ Không tốt (Không thụt đầu dòng đúng)
```
switch (status) {
case READY:
printf("Ready\n");
break;
case RUNNING:
printf("Running\n");
break;
default:
printf("Unknown\n");
}
```
🚨 Vấn đề:
- case không thụt đầu dòng đúng, dễ gây lỗi đọc sai logic.

✔️ Tốt (Thụt đầu dòng chuẩn)
```
switch (status) {
    case READY:
        printf("Ready\n");
        break;
    case RUNNING:
        printf("Running\n");
        break;
    default:
        printf("Unknown\n");
}
```
✅ Lợi ích:
- Tránh lỗi fall-through, dễ đọc hơn.

8️⃣ Cách đặt dấu * khi khai báo con trỏ

- Tránh nhầm lẫn giữa con trỏ và biến thông thường.

❌ Không tốt (Dấu * dính vào kiểu dữ liệu)
```
int* ptr, var;  // Không rõ ràng, var không phải là con trỏ
```
🚨 Vấn đề:
- Nhiều người hiểu lầm rằng cả ptr và var đều là con trỏ.

✔️ Tốt (Dấu * gần tên biến)
```
int *ptr, var;  // Rõ ràng: ptr là con trỏ, var là biến thường
```
✅ Lợi ích:
- Tránh nhầm lẫn về kiểu dữ liệu.

9️⃣ Tránh dòng trống dư thừa trong code

❌ Không tốt (Dòng trống dư thừa giữa các lệnh)
```
void example() {

    int x = 10;


    int y = 20;

    printf("Sum: %d\n", x + y);

}
```
🚨 Vấn đề:
- Dòng trống dư thừa làm gián đoạn logic, khó đọc hơn.

✔️ Tốt (Chỉ dùng dòng trống khi cần thiết)
```
void example() {
    int x = 10;
    int y = 20;
    
    printf("Sum: %d\n", x + y);
}
```
✅ Lợi ích:
- Code gọn gàng, không mất không gian không cần thiết.

🔟 Đặt dấu = cách nhau khi gán giá trị

-  Giúp dễ dàng đọc và hiểu các phép gán.

❌ Không tốt (Dính chặt = vào biến hoặc giá trị)
```
int x=10;
float y=2.5;
```
🚨 Vấn đề:
Khó đọc, đặc biệt khi có nhiều phép gán trong một hàm.

✔️ Tốt (Khoảng trắng giữa biến, = và giá trị)
```
int x = 10;
float y = 2.5;
```

### 7. Quy tắc về sử dụng bộ nhớ

1️⃣ Không sử dụng con trỏ NULL

- Nếu một con trỏ NULL được dereference (*ptr), hệ thống có thể bị crash ngay lập tức.
- Lỗi runtime khó debug khi con trỏ NULL không được kiểm tra trước khi sử dụng.

❌ Không tốt (Sử dụng con trỏ NULL mà không kiểm tra)
```
int *ptr = NULL;  // Con trỏ NULL, nếu sử dụng sẽ gây lỗi
*ptr = 10;        // Lỗi runtime: Dereference NULL pointer
```
🚨 Vấn đề:
Nếu ptr chưa được cấp phát bộ nhớ hợp lệ, truy cập vào nó sẽ gây lỗi segmentation fault.

✔️ Tốt (Luôn kiểm tra trước khi sử dụng con trỏ)
```
int num = 10;
int *ptr = &num; // Con trỏ luôn trỏ tới một vùng nhớ hợp lệ

if (ptr != NULL) {
    *ptr = 20;  // Đảm bảo an toàn khi truy cập con trỏ
}
```
✅ Lợi ích:
- Đảm bảo con trỏ luôn hợp lệ trước khi sử dụng.

2️⃣ Sử dụng kích thước phù hợp cho kiểu dữ liệu

- Tránh lãng phí bộ nhớ khi khai báo kiểu dữ liệu không cần thiết.
- Đảm bảo chương trình hoạt động chính xác trên nhiều vi điều khiển khác nhau.

❌ Không tốt (Dùng kiểu dữ liệu quá lớn so với cần thiết)
```
long int counter = 100;  // Không cần thiết dùng long int nếu giá trị nhỏ
```
🚨 Vấn đề:
- long int tốn nhiều bộ nhớ hơn int, trong khi giá trị chỉ nằm trong phạm vi của int.

✔️ Tốt (Dùng kiểu dữ liệu phù hợp với phạm vi giá trị)
```
int counter = 100;  // Tiết kiệm bộ nhớ, đủ dùng
```
✅ Lợi ích:
- Tối ưu hóa bộ nhớ và đảm bảo hiệu suất.

3️⃣ Sử dụng các phép toán bit thích hợp

- Phép toán bit (&, |, ^, <<, >>) nhanh hơn so với các phép toán số học thông thường.
- Giúp tiết kiệm bộ nhớ và tăng tốc độ xử lý.

❌ Không tốt (Dùng phép toán số học thay vì bitwise)
```
int isEven(int num) {
    return num % 2 == 0;  // Phép chia (modulo) tốn nhiều tài nguyên
}
```
✔️ Tốt (Dùng phép toán bit thay vì modulo)
```
int isEven(int num) {
    return (num & 1) == 0;  // Kiểm tra bit cuối để xác định số chẵn
}
```
✅ Lợi ích:
- Tăng tốc độ xử lý do phép toán bit nhanh hơn modulo.

4️⃣ Không sử dụng đệ quy trong lập trình nhúng

- Mỗi lần gọi đệ quy, hệ thống phải tạo một stack frame mới, dẫn đến tốn bộ nhớ stack nhanh chóng.
- Có nguy cơ tràn stack (stack overflow) nếu không có điều kiện dừng hợp lệ.

❌ Không tốt (Dùng đệ quy tính giai thừa)
```
int factorial(int n) {
    if (n == 0) return 1;
    return n * factorial(n - 1);  // Gọi đệ quy liên tục
}
```
🚨 Vấn đề:
- Mỗi lần gọi factorial(n - 1), một stack frame mới được tạo.

✔️ Tốt (Dùng vòng lặp thay vì đệ quy)
```
int factorial(int n) {
    int result = 1;
    for (int i = 1; i <= n; ++i) {
        result *= i;
    }
    return result;
}
```
✅ Lợi ích:
- Không cần stack frame bổ sung, tiết kiệm bộ nhớ hơn.

5️⃣ Tránh sử dụng hàm delay() hoặc usleep() trong hệ thống nhúng

- delay() chặn toàn bộ CPU, làm hệ thống không thể xử lý các tác vụ khác.
- Sử dụng interrupt (ngắt) hoặc timer sẽ tối ưu hơn.

❌ Không tốt (Dùng delay())
```
void loop() {
    digitalWrite(LED_BUILTIN, HIGH);
    delay(1000);  // Chặn CPU 1 giây
    digitalWrite(LED_BUILTIN, LOW);
    delay(1000);
}
```
🚨 Vấn đề:
- Trong 1 giây, CPU không thể làm gì khác ngoài chờ.

✔️ Tốt (Dùng timer thay thế delay())
```
#include <TimerOne.h>
int ledState = LOW;

void setup() {
    pinMode(LED_BUILTIN, OUTPUT);
    Timer1.initialize(1000000);  // 1 giây
    Timer1.attachInterrupt(timerISR);
}

void loop() {
    // Không bị chặn CPU, có thể làm việc khác
}

void timerISR() {
    ledState = !ledState;
    digitalWrite(LED_BUILTIN, ledState);
}
```
✅ Lợi ích:
- Không làm chậm hệ thống, cho phép CPU xử lý các tác vụ khác song song.

6️⃣ Kiểm tra giới hạn bộ nhớ khi cấp phát động

- Tránh lỗi tràn bộ nhớ heap, dẫn đến crash hệ thống.
- Giúp kiểm tra bộ nhớ có sẵn trước khi sử dụng.

❌ Không tốt (Cấp phát bộ nhớ động mà không kiểm tra lỗi)
```
char *str = malloc(100);  // Không kiểm tra malloc có thành công hay không
strcpy(str, "Hello World");  // Nếu malloc thất bại, chương trình có thể crash
```
✔️ Tốt (Luôn kiểm tra kết quả cấp phát bộ nhớ)

```
char *str = malloc(100);
if (str == NULL) {
    printf("Memory allocation failed!\n");
    return;
}
strcpy(str, "Hello World");
```
✅ Lợi ích:
- Tránh lỗi khi bộ nhớ không đủ để cấp phát.

7️⃣ Tránh rò rỉ bộ nhớ (Memory Leak)

- Nếu bộ nhớ được cấp phát bằng malloc() mà không được free(), hệ thống sẽ cạn kiệt bộ nhớ, đặc biệt trong hệ thống nhúng với bộ nhớ hạn chế.


❌ Không tốt (Quên giải phóng bộ nhớ động)
```
char *buffer = (char *)malloc(100);
strcpy(buffer, "Hello World");
// Không có free(buffer), gây rò rỉ bộ nhớ
```
🚨 Vấn đề:
- Nếu chương trình chạy liên tục và tiếp tục cấp phát bộ nhớ, hệ thống sẽ hết RAM.

✔️ Tốt (Luôn giải phóng bộ nhớ sau khi sử dụng)
```
char *buffer = (char *)malloc(100);
if (buffer == NULL) {
    printf("Memory allocation failed!\n");
    return;
}
strcpy(buffer, "Hello World");
printf("%s\n", buffer);
free(buffer);  // Giải phóng bộ nhớ để tránh rò rỉ
```
✅ Lợi ích:
Giải phóng bộ nhớ sau khi sử dụng, tránh làm cạn RAM.

8️⃣ Tránh sử dụng cấp phát động trong vòng lặp hoặc hàm gọi thường xuyên

- Việc gọi malloc() hoặc free() liên tục trong vòng lặp gây chậm hệ thống.
- Hệ thống có thể bị phân mảnh bộ nhớ, làm giảm hiệu suất.

❌ Không tốt (Cấp phát động trong vòng lặp)

```
for (int i = 0; i < 100; i++) {
    int *data = (int *)malloc(sizeof(int)); // Cấp phát động mỗi lần lặp
    *data = i;
    printf("%d\n", *data);
    free(data);
}
```
🚨 Vấn đề:
- Việc cấp phát và giải phóng bộ nhớ lặp lại có thể gây phân mảnh RAM, làm chậm hệ thống.

✔️ Tốt (Dùng bộ nhớ tĩnh hoặc cấp phát một lần)
```
int data[100];  // Dùng bộ nhớ tĩnh thay vì malloc()
for (int i = 0; i < 100; i++) {
    data[i] = i;
    printf("%d\n", data[i]);
}
```
✅ Lợi ích:
- Tránh phân mảnh bộ nhớ và tăng hiệu suất.

9️⃣ Tránh sử dụng biến cục bộ có kích thước lớn

- Biến cục bộ nằm trên stack, nếu kích thước quá lớn có thể gây tràn stack (stack overflow).

❌ Không tốt (Biến cục bộ quá lớn)
```
void process() {
    int largeArray[10000];  // Quá lớn, có thể làm tràn stack
    memset(largeArray, 0, sizeof(largeArray));
}
```
🚨 Vấn đề:
- Tràn stack có thể dẫn đến crash hệ thống.

✔️ Tốt (Dùng cấp phát động hoặc biến toàn cục)
```
#define ARRAY_SIZE 10000
int largeArray[ARRAY_SIZE];  // Sử dụng bộ nhớ toàn cục (bên ngoài hàm)

void process() {
    memset(largeArray, 0, sizeof(largeArray));
}
```
✅ Lợi ích:
- Giảm nguy cơ tràn stack bằng cách sử dụng bộ nhớ toàn cục.

1️⃣0️⃣ Sử dụng volatile khi làm việc với biến thay đổi ngoài chương trình

- Trong hệ thống nhúng, các biến được cập nhật bởi ngắt (interrupt) hoặc bộ nhớ chia sẻ (shared memory) có thể bị trình biên dịch tối ưu sai, dẫn đến lỗi.

❌ Không tốt (Không khai báo volatile)
```
int sensorValue;  // Giá trị này có thể thay đổi bởi ngắt (ISR)
void ISR_Handler() {
    sensorValue = read_sensor();
}
void loop() {
    while (sensorValue == 0) {  // Trình biên dịch có thể tối ưu bỏ vòng lặp này
        // Chờ dữ liệu từ sensor
    }
    printf("Sensor value received!\n");
}
```
🚨 Vấn đề:
- Trình biên dịch có thể tối ưu hóa bỏ vòng lặp vì không thấy sensorValue thay đổi trong chương trình chính.

✔️ Tốt (Dùng volatile để đảm bảo giá trị không bị tối ưu hóa)
```
volatile int sensorValue;  // Biến có thể thay đổi bởi ngắt
void ISR_Handler() {
    sensorValue = read_sensor();
}
void loop() {
    while (sensorValue == 0) {  // Luôn kiểm tra đúng giá trị thực tế
        // Chờ dữ liệu từ sensor
    }
    printf("Sensor value received!\n");
}
```
✅ Lợi ích:
- Đảm bảo trình biên dịch không tối ưu hóa sai biến thay đổi do ngắt hoặc phần cứng.

📌 Tổng kết Quy tắc Sử dụng Bộ Nhớ trong Autosar C Coding Guidelines  

| **Quy tắc**                                   | **Không tốt ❌**                                    | **Tốt ✔️**                                      |
|----------------------------------------------|------------------------------------------------|------------------------------------------------|
| **1️⃣ Không sử dụng con trỏ NULL**       | `int *ptr = NULL;`                            | `if (ptr != NULL) { *ptr = 10; }` |
| **2️⃣ Dùng kích thước phù hợp**          | `long int count = 100;`                        | `int count = 100;`                            |
| **3️⃣ Sử dụng phép toán bit**            | `return num % 2 == 0;`                         | `return (num & 1) == 0;`                      |
| **4️⃣ Không dùng đệ quy**                 | `return n * factorial(n - 1);`                 | `for (int i = 1; i <= n; ++i) result *= i;`  |
| **5️⃣ Không dùng `delay()`**              | `delay(1000);`                                 | `Sử dụng Timer hoặc interrupt`               |
| **6️⃣ Kiểm tra cấp phát bộ nhớ**         | `char *str = malloc(100);`                     | `if (str == NULL) { handle_error(); }`       |
| **7️⃣ Tránh rò rỉ bộ nhớ**                 | `malloc(100);` nhưng không `free()`             | Luôn `free()` bộ nhớ sau khi dùng |
| **8️⃣ Không cấp phát động trong vòng lặp** | `malloc()` và `free()` mỗi vòng lặp             | Dùng biến tĩnh hoặc cấp phát một lần |
| **9️⃣ Không dùng biến cục bộ quá lớn**     | `int largeArray[10000];`                        | Dùng bộ nhớ toàn cục hoặc heap |
| **1️⃣0️⃣ Sử dụng `volatile` khi cần thiết**   | `int sensorValue;` có thể bị tối ưu hóa sai     | `volatile int sensorValue;` |

### 8. Quy tắc Biểu Thức và Toán Tử

1️⃣ Tránh sử dụng các biểu thức phức tạp

- Biểu thức phức tạp khó đọc, khó hiểu và dễ xảy ra lỗi logic.
- Tách biểu thức giúp tăng độ rõ ràng và dễ bảo trì hơn.

❌ Không tốt (Biểu thức quá phức tạp)
```
if ((x > 10 && y < 5) || (x <= 10 && y >= 5)) {
    // do something
}
```
🚨 Vấn đề:
- Khó đọc, dễ mắc lỗi khi chỉnh sửa hoặc debug.

✔️ Tốt (Tách biểu thức thành các điều kiện rõ ràng)
```
bool isXGreaterThanTen = (x > 10);
bool isYLessThanFive = (y < 5);
bool isXLessThanOrEqualToTen = (x <= 10);
bool isYGreaterThanOrEqualToFive = (y >= 5);

if ((isXGreaterThanTen && isYLessThanFive) || (isXLessThanOrEqualToTen && isYGreaterThanOrEqualToFive)) {
    // do something
}
```

✅ Lợi ích:
- Dễ đọc hơn, dễ bảo trì hơn, ít lỗi hơn.

2️⃣ Hạn chế sử dụng toán tử động (Dynamic Operator)

- Truy cập con trỏ và ép kiểu dữ liệu có thể dẫn đến lỗi runtime khó debug.
- Việc sử dụng con trỏ không hợp lệ có thể gây lỗi bộ nhớ nghiêm trọng.

❌ Không tốt (Dùng toán tử động không an toàn)
```
int *ptr = NULL;
int a = 10;
ptr = &a; 
*ptr = 20; // Có thể gây lỗi nếu không kiểm tra NULL trước

float *fptr = NULL;
fptr = (float *)&a; // Ép kiểu sai, có thể gây lỗi không xác định
*fptr = 3.14; 
```
🚨 Vấn đề:
- ptr chưa được kiểm tra NULL trước khi sử dụng.
- fptr ép kiểu từ int sang float, gây lỗi dữ liệu không xác định.

✔️ Tốt (Kiểm tra con trỏ và tránh ép kiểu nguy hiểm)
```
int *ptr = NULL;
int a = 10;

ptr = &a; 
if (ptr != NULL) {
    *ptr = 20;  // Đảm bảo an toàn khi truy cập con trỏ
}
```
✅ Lợi ích:
- Tránh lỗi bộ nhớ, tránh crash chương trình.

3️⃣ Luôn sử dụng các toán tử an toàn

- Toán tử &&, || hỗ trợ short-circuit evaluation, giúp tối ưu hiệu suất và tránh lỗi không mong muốn.

- Tránh dùng & và | thay cho && và || vì có thể gây lỗi logic.

❌ Không tốt (Dùng toán tử & thay vì &&)
```
if (a > 0 & b < 20) {  // Lỗi logic nếu `a > 0` nhưng `b` không nhỏ hơn 20
    c = a + b;
}
```
🚨 Vấn đề:
- & luôn kiểm tra cả hai điều kiện, dẫn đến lỗi không mong muốn.

✔️ Tốt (Dùng toán tử && để tối ưu logic)
```
if (a > 0 && b < 20) {  
    c = a + b; // Chỉ thực hiện nếu cả hai điều kiện đều đúng
}
```
✅ Lợi ích:
- Tránh lỗi logic, tối ưu hiệu suất chương trình.

4️⃣ Không sử dụng + để nối chuỗi

- Trong C, toán tử + không hỗ trợ nối chuỗi như trong các ngôn ngữ khác (Java, Python).
- Dùng sprintf() hoặc strcat() thay vì + để nối chuỗi.

❌ Không tốt (Dùng toán tử + sai cách)
```
int x = 5;
char str[10] = "hello";
char new_str[20];

sprintf(new_str, "%d" + str); // Lỗi! không thể nối số với chuỗi
```

✔️ Tốt (Dùng sprintf() hoặc strcat())
```
sprintf(new_str, "%d%s", x, str);  // Đúng, nối chuỗi số và chuỗi bằng format string
```
✅ Lợi ích:
- Đúng cú pháp, tránh lỗi runtime.

5️⃣ Hạn chế sử dụng toán tử bit (&, |, ^, ~) trên kiểu không phải số nguyên

- Toán tử bit chỉ hoạt động đúng với kiểu số nguyên (int, unsigned int, char, …).
- Dùng với float, double có thể gây lỗi biên dịch hoặc lỗi không xác định.

❌ Không tốt (Dùng toán tử bit với float)
```
float a = 3.5;
float b = 2.0;
float c = a | b; // Lỗi biên dịch
```
✔️ Tốt (Chỉ dùng toán tử bit với số nguyên)
```
int a = 3;
int b = 2;
int c = a | b; // Đúng, toán tử bit chỉ áp dụng trên số nguyên
```
✅ Lợi ích:
- Tránh lỗi biên dịch, đảm bảo chương trình chạy đúng.

6️⃣ Sử dụng toán tử phù hợp với kiểu dữ liệu

- Khi thực hiện phép toán giữa int và float, cần ép kiểu dữ liệu để tránh lỗi.

❌ Không tốt (Không ép kiểu trước khi cộng)
```
int a = 5;
float b = 2.5;
float sum = a + b;  // Lỗi, kiểu int và float không khớp
```

✔️ Tốt (Ép kiểu trước khi thực hiện phép toán)
```
float sum = (float)a + b;  // Đúng, đảm bảo phép toán được thực hiện chính xác
```
✅ Lợi ích:
- Tránh lỗi ép kiểu, đảm bảo độ chính xác của phép toán.

7️⃣ Tránh sử dụng phép chia số nguyên nếu không cần thiết

- Phép chia số nguyên (/) có thể làm tròn xuống (floor) thay vì giữ phần thập phân, gây sai lệch kết quả.
- Nếu cần độ chính xác cao, nên ép kiểu sang float hoặc double.

❌ Không tốt (Sử dụng phép chia số nguyên mà không ép kiểu)
```
int a = 5;
int b = 2;
float result = a / b;  // Kết quả: 2, không phải 2.5
```
🚨 Vấn đề:
- Vì a và b đều là int, phép chia a / b sẽ bị làm tròn xuống, gây sai số.

✔️ Tốt (Ép kiểu để tránh mất dữ liệu)
```
float result = (float)a / b;  // Kết quả: 2.5
```
✅ Lợi ích:
- Giữ nguyên phần thập phân, đảm bảo tính toán chính xác.

8️⃣ Không sử dụng toán tử tăng/giảm (++, --) trong biểu thức phức tạp

- Toán tử ++ và -- có thể gây khó hiểu khi sử dụng trong các biểu thức phức tạp.
- Hành vi không xác định có thể xảy ra khi sử dụng ++ hoặc -- nhiều lần trong một dòng.

❌ Không tốt (Dùng ++ trong biểu thức phức tạp)
```
int x = 5;
int y = x++ + ++x;  // Không rõ x được tăng trước hay sau, kết quả khó dự đoán
```
🚨 Vấn đề:
- Hành vi không xác định, tùy vào trình biên dịch, có thể cho kết quả khác nhau.

✔️ Tốt (Tách riêng toán tử ++)
```
int x = 5;
int temp1 = x++;
int temp2 = ++x;
int y = temp1 + temp2;  // Rõ ràng hơn, tránh lỗi logic
```
✅ Lợi ích:
- Dễ hiểu, dễ debug, tránh hành vi không mong muốn.

9️⃣ Không sử dụng toán tử gán (=) bên trong điều kiện if

- Dễ gây nhầm lẫn với toán tử so sánh ==, dẫn đến lỗi logic khó phát hiện.
- Một số trình biên dịch có thể không cảnh báo lỗi khi nhầm = với ==.

❌ Không tốt (Gán nhầm thay vì so sánh)
```
if (x = 5) {  // Sai, vì x được gán 5 thay vì so sánh với 5
    printf("x bằng 5\n");
}
```
🚨 Vấn đề:
- x = 5 sẽ luôn trả về true, gây lỗi logic.

✔️ Tốt (Dùng == để so sánh, không gán)
```
if (x == 5) {  // Đúng, vì kiểm tra điều kiện thay vì gán giá trị
    printf("x bằng 5\n");
}
```
✅ Lợi ích:
- Tránh lỗi logic khó phát hiện, đảm bảo đúng điều kiện kiểm tra.

🔟 Tránh sử dụng toán tử ? : (toán tử ba ngôi) trong các biểu thức dài

- Toán tử ba ngôi (condition ? true_value : false_value) có thể gây khó đọc nếu lồng nhau.
- Dễ gây nhầm lẫn, giảm khả năng bảo trì.

❌ Không tốt (Toán tử ba ngôi lồng nhau, khó đọc)
```
int result = (x > 0) ? ((y > 0) ? 1 : -1) : 0;  // Khó hiểu
```

✔️ Tốt (Dùng if-else để rõ ràng hơn)
```
int result;
if (x > 0) {
    if (y > 0) {
        result = 1;
    } else {
        result = -1;
    }
} else {
    result = 0;
}
```
✅ Lợi ích:
- Dễ đọc, dễ hiểu, dễ bảo trì.

1️⃣1️⃣ Hạn chế sử dụng phép toán modulo (%) nếu có thể

- Phép toán % (chia lấy dư) tốn nhiều chu kỳ CPU, có thể ảnh hưởng đến hiệu suất trên vi điều khiển.
- Nếu có thể, nên thay thế bằng phép toán dịch bit hoặc phép toán khác hiệu quả hơn.

❌ Không tốt (Dùng % khi có thể thay thế bằng phép khác)
```
if (x % 2 == 0) {  // Kiểm tra số chẵn
    printf("x là số chẵn\n");
}
```

✔️ Tốt (Dùng phép toán bit nhanh hơn)
```
if ((x & 1) == 0) {  // Kiểm tra số chẵn bằng toán tử bitwise
    printf("x là số chẵn\n");
}
```
✅ Lợi ích:
- Tối ưu tốc độ tính toán, đặc biệt quan trọng trong hệ thống nhúng.

📌 Bảng Tổng Hợp Quy Tắc Biểu Thức và Toán Tử (Autosar C Coding Guidelines)

| STT  | Quy tắc                                      | Không tốt ❌                                    | Tốt ✔️                                       |
|------|---------------------------------------------|-----------------------------------------------|----------------------------------------------|
| **1️⃣**  | Tránh sử dụng biểu thức phức tạp                | `(x > 10 && y < 5) || (x <= 10 && y >= 5)`  | Tách thành **các biến `bool` rõ ràng**       |
| **2️⃣**  | Hạn chế sử dụng toán tử động                     | `float *fptr = (float *)&a;`                 | **Kiểm tra con trỏ NULL trước khi sử dụng**  |
| **3️⃣**  | Sử dụng toán tử logic an toàn (`&&`, `||`)       | `if (a > 0 & b < 20)`                        | `if (a > 0 && b < 20)`                       |
| **4️⃣**  | Không sử dụng `+` để nối chuỗi                  | `sprintf(new_str, "%d" + str);`              | `sprintf(new_str, "%d%s", x, str);`          |
| **5️⃣**  | Hạn chế sử dụng toán tử bit với `float`         | `float c = a | b;`                           | **Chỉ dùng toán tử bit với kiểu số nguyên** |
| **6️⃣**  | Dùng toán tử phù hợp với kiểu dữ liệu           | `float sum = a + b;`                         | `float sum = (float)a + b;`                   |
| **7️⃣**  | Tránh sử dụng phép chia số nguyên không cần thiết | `float result = a / b;`                     | `float result = (float)a / b;`                |
| **8️⃣**  | Không dùng `++/--` trong biểu thức phức tạp      | `y = x++ + ++x;`                           | **Tách riêng từng phép toán** để rõ ràng hơn |
| **9️⃣**  | Không gán (`=`) bên trong `if`                  | `if (x = 5)`                                | `if (x == 5)`                                 |
| **🔟**  | Tránh sử dụng toán tử ba ngôi (`?:`) quá dài      | `result = (x > 0) ? ((y > 0) ? 1 : -1) : 0;`| **Dùng `if-else` để rõ ràng hơn**            |
| **1️⃣1️⃣** | Hạn chế dùng `%` nếu có thể thay thế         | `if (x % 2 == 0)`                          | `if ((x & 1) == 0)`                          |

### 9. Quy tắc về cấu trúc và liên kết

– Tập tin mã nguồn nên được phân chia thành các tập tin nhỏ hơn để dễ dàng quản lý và tái sử dụng mã.

– Tên các tập tin và biến nên được đặt sao cho dễ hiểu và mô tả được chức năng của chúng.

– Các biến và hằng nên được khai báo ở đầu tập tin và được sắp xếp theo thứ tự chữ cái.

– Các hằng số nên được định nghĩa bằng các macro và được đặt tên theo dạng chữ hoa và các từ cách nhau bởi dấu gạch dưới.

– Các hàm nên được sắp xếp theo thứ tự chức năng và tên hàm nên được đặt sao cho mô tả được chức năng của hàm.

– Các hàm nên được định nghĩa trước khi sử dụng và các hàm nên được khai báo ở đầu tập tin.

– Tên tham số của các hàm nên được đặt sao cho mô tả được dữ liệu mà tham số đại diện.

– Các lệnh nên được định dạng sao cho dễ đọc và dễ hiểu.

– Các khối lệnh nên được đặt trong cặp dấu ngoặc nhọn và được thụt đầu dòng sao cho dễ đọc và hiểu.

– Các biến nên được khai báo ở phạm vi nhỏ nhất có thể để tránh lỗi không xác định.

📌 Tóm tắt bảng quy tắc về Cấu trúc và Liên kết

| Quy tắc                              | Không tốt ❌                          | Tốt ✔️                                    |
|--------------------------------------|--------------------------------------|------------------------------------------|
| **Chia nhỏ file mã nguồn**           | Toàn bộ code trong một file          | Chia thành nhiều file `.c` và `.h`      |
| **Đặt tên biến, hàm dễ hiểu**        | `int x, y;`                         | `int vehicleSpeed;`                      |
| **Sắp xếp biến và hằng theo thứ tự chữ cái** | Lộn xộn, khó theo dõi               | `#define MAX_SIZE 100` trước `#define MIN_SIZE 0` |
| **Định nghĩa macro đúng quy tắc**     | `#define MaxSize 100`               | `#define MAX_SIZE 100`                   |
| **Sắp xếp hàm theo chức năng**       | Lộn xộn                             | Nhóm hàm theo từng module               |
| **Thụt đầu dòng, đặt dấu `{}` hợp lý** | Code khó đọc                         | Code rõ ràng, dễ bảo trì                 |
| **Khai báo biến trong phạm vi nhỏ nhất** | Dùng biến toàn cục không cần thiết  | Dùng biến cục bộ khi có thể              |

### 10. Quy tắc về xử lý chuỗi

– Sử dụng hằng số để lưu độ dài tối đa của chuỗi.

– Sử dụng các hàm chuẩn như strncpy() hoặc memcpy() để sao chép chuỗi.

– Kiểm tra kích thước đầu vào của chuỗi để tránh các lỗi tràn bộ đệm.

– Sử dụng các hàm chuẩn như strcmp() hoặc strncmp() để so sánh chuỗi.

– Sử dụng các hàm chuẩn như strcat() hoặc strncat() để nối chuỗi.

– Kiểm tra giá trị trả về của các hàm xử lý chuỗi để xử lý các lỗi.

```
#include <stdio.h>
#include <string.h>

#define MAX_LENGTH 100

void copyString(char *dest, const char *src)
{
    size_t length = strlen(src);
    if (length >= MAX_LENGTH)
    {
        printf("Error: source string is too long\n");
        return;
    }
    strncpy(dest, src, length);
    dest[length] = '\0';
}

int main()
{
    char str1[MAX_LENGTH] = "Hello";
    char str2[MAX_LENGTH] = "World";
    char str3[MAX_LENGTH];

    copyString(str3, str1);
    printf("str3: %s\n", str3);

    strcat(str1, " ");
    strncat(str1, str2, MAX_LENGTH - strlen(str1) - 1);
    printf("str1: %s\n", str1);

    if (strncmp(str1, "Hello World", MAX_LENGTH) == 0)
    {
        printf("The strings match!\n");
    }
    else
    {
        printf("The strings do not match.\n");
    }

    return 0;
}
```
- Trong ví dụ trên, hàm copyString() được sử dụng để sao chép chuỗi từ src sang dest. Hằng số MAX_LENGTH được sử dụng để giới hạn độ dài của chuỗi và tránh lỗi tràn bộ đệm. Hàm strncat() được sử dụng để nối chuỗi str2 vào str1 với số lượng ký tự tối đa được tính toán để tránh lỗi tràn bộ đệm. Hàm strncmp() được sử dụng để so sánh hai chuỗi. Giá trị trả về của các hàm xử lý chuỗi được kiểm tra để xử lý các lỗi.

### 11. Quy tắc về xử lý số học

– Tránh sử dụng toán tử chia (/) với số nguyên: Nếu một biểu thức chứa toán tử chia (/) với số nguyên, kết quả sẽ được chuyển đổi thành số nguyên và làm tròn về phía không gần nhất. Điều này có thể dẫn đến sai sót trong tính toán.

– Sử dụng phép chia hợp lệ cho số thực: Tránh sử dụng toán tử chia (/) với số thực, vì nó có thể dẫn đến sai số trong tính toán. Thay vào đó, sử dụng các phép chia hợp lệ cho số thực như phép chia liên tục (floating-point division) hoặc phép nhân với nghịch đảo (multiply by inverse).

– Tránh tràn số: Khi thực hiện các phép tính số học, cần kiểm tra tràn số để tránh kết quả không xác định hoặc sai sót trong tính toán.

– Sử dụng các hàm toán học chuẩn: Các hàm toán học chuẩn như sqrt(), sin(), cos(), tan() đã được kiểm tra và xác định rằng chúng hoạt động đúng với mọi trường hợp. Vì vậy, nên sử dụng các hàm này thay vì tự viết hàm toán học của riêng mình.

– Đảm bảo độ chính xác của tính toán: Khi thực hiện các tính toán phức tạp hoặc yêu cầu độ chính xác cao, cần sử dụng các thư viện toán học đáng tin cậy hoặc các thuật toán tính toán độ chính xác cao.

    Ví dụ, theo tiêu chuẩn Autosar C Coding Guidelines, nếu ta muốn tính giá trị của sin(x) với x là một số thực, ta nên sử dụng hàm sin() được cung cấp sẵn trong thư viện math.h thay vì tự viết hàm sin() của riêng mình. Nếu muốn kiểm tra độ chính xác của kết quả, ta có thể so sánh giá trị tính toán được với giá trị đã biết của sin(x) trong một số trường hợp cụ thể.

### 12 Quy tắc về hàm và tham số

- Tên hàm và tham số:
    – Tên hàm và tham số nên được đặt sao cho dễ hiểu và mô tả được chức năng của chúng.
    – Tên hàm nên bắt đầu bằng một động từ hoặc chữ viết tắt mô tả chức năng của hàm.
    – Tên tham số nên được đặt sao cho mô tả được dữ liệu mà tham số đại diện.
- Tên hàm: Bắt đầu bằng một động từ mô tả chức năng của hàm, ví dụ như calculate, initialize, validate, set, get, process,…
- Tên tham số: Nên được đặt sao cho mô tả được dữ liệu mà tham số đại diện, ví dụ như input, output, value, pointer, length, index,…
- Nên tránh viết tắt và các ký tự đặc biệt.
```
// Hàm tính giá trị tuyệt đối của số nguyên
int calculateAbsoluteValue(int input);
// Hàm đặt giá trị cho biến
void setVariableValue(int *pointer, int value);
// Hàm xử lý chuỗi
void processString(char *inputString, int length);
// Hàm lấy giá trị từ mảng
int getValueFromArray(int *array, int index);
```
- Định dạng hàm:
    – Hàm nên được định dạng sao cho dễ đọc và dễ hiểu.
    – Hàm nên được phân chia thành các phần rõ ràng như đầu vào, xử lý và đầu ra.
    – Hàm nên được viết theo một chuẩn nhất định để dễ đọc và hiểu.
    - Ví dụ về việc định dạng hàm cho một thuộc tính trong một struct như sau:
```
/**
 * @brief Structure containing the properties of a car
 */
typedef struct {
    uint8_t speed;  /**< Speed of the car in km/h */
    uint8_t fuel_level;  /**< Fuel level of the car in percent */
} car_properties_t;
/**
 * @brief Function to update the speed of a car
 *
 * @param car Pointer to the car properties struct
 * @param new_speed The new speed of the car in km/h
 */
void update_car_speed(car_properties_t *car, uint8_t new_speed)
{
    car->speed = new_speed;
}
```
- Trong ví dụ này, hàm update_car_speed được định dạng rõ ràng với đầu vào là car và new_speed, và không có giá trị trả về. Hàm được chia thành các phần rõ ràng như đầu vào (car và new_speed), xử lý (cập nhật giá trị của thuộc tính speed trong car) và không có đầu ra.

- Ngoài ra, các comment được sử dụng để giải thích chức năng của hàm và các thuộc tính của struct, giúp cho việc đọc code dễ hiểu và dễ bảo trì hơn.

- Các quy tắc về tham số: Tham số đầu vào nên được khai báo là const để bảo vệ chúng khỏi việc thay đổi bất hợp lý.

- void print_array(const int *arr, int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
}

- Trong ví dụ này, tham số đầu vào arr được khai báo là const int *, cho phép hàm sử dụng giá trị của mảng arr nhưng không cho phép thay đổi giá trị của mảng này.

- Lưu ý: Vì sao dùng const int *arr thay vì truyền toàn bộ mảng? Thay vì truyền toàn bộ mảng (dẫn đến sao chép dữ liệu) → tốn bộ nhớ & hiệu suất kém., cách tối ưu hơn là truyền con trỏ đến mảng, nhưng đánh dấu const để tránh thay đổi dữ liệu.

- Tham số nên được truyền theo giá trị nếu không cần thiết phải thay đổi giá trị đó.
```
int sum(int a, int b) {
    return a + b;
}
```
- Trong ví dụ này, tham số a và b được truyền theo giá trị, vì hàm sum không cần phải thay đổi giá trị của a và b.

- Tham số nên được truyền theo con trỏ nếu cần phải thay đổi giá trị đó.
```
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}
```
- Trong ví dụ này, tham số a và b được truyền theo con trỏ, cho phép hàm swap thay đổi giá trị của a và b bằng cách sử dụng con trỏ.

- Các quy tắc về giá trị trả về của hàm:
    – Hàm nên trả về một giá trị duy nhất để tránh lỗi không xác định.
    – Giá trị trả về của hàm nên được xác định trước khi thực hiện hàm và được đưa ra trong tài liệu hướng dẫn sử dụng của hàm.
    – Giá trị trả về của hàm nên được xử lý sao cho phù hợp với mục đích của hàm.

```
/**
 * @brief Tính tổng các phần tử trong một mảng.
 * 
 * @param arr Mảng đầu vào.
 * @param size Kích thước của mảng.
 * @return Tổng các phần tử trong mảng.
 */
int sumArray(const int arr[], int size) {
    int sum = 0;
    for(int i = 0; i < size; i++) {
        sum += arr[i];
    }
    return sum;
}
```

- Trong ví dụ này, hàm sumArray trả về một giá trị nguyên duy nhất – tổng các phần tử trong mảng đầu vào. Giá trị trả về đã được xác định trước khi thực hiện hàm và được đưa ra trong tài liệu hướng dẫn sử dụng của hàm. Ngoài ra, giá trị trả về đã được xử lý sao cho phù hợp với mục đích của hàm – tính tổng các phần tử trong mảng.

- Các quy tắc về việc gọi hàm:
    – Gọi hàm với đúng tên và đúng kiểu trả về của hàm 
    – Gọi hàm với đúng số lượng tham số và kiểu dữ liệu của chúng
    – Gọi hàm với các tham số hợp lệ để tránh lỗi không xác định
    – Gọi hàm với đúng thứ tự của các tham số

```
// Hàm tính diện tích hình chữ nhật
float calculateRectangleArea(float length, float width) {
    return length * width;
}
int main() {
    float length = 4.0;
    float width = 5.0;
    // Gọi hàm tính diện tích hình chữ nhật với các tham số đúng kiểu và đúng thứ tự
    float area = calculateRectangleArea(length, width);
    return 0;
}
```

- Trong ví dụ trên, hàm calculateRectangleArea được gọi với đúng kiểu và đúng thứ tự của các tham số, và sử dụng các biến hợp lệ là length và width để tính toán diện tích của hình chữ nhật.


---
## 📞 Contact
Email: individual.thuongnguyen@gmail.com    
GitHub: [github.com/thuongnvLK](https://github.com/thuongnvLK)