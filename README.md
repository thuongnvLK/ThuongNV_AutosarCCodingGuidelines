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

📌 Tổng kết Quy tắc xử lý lỗi

| **Quy tắc xử lý lỗi**      | **Không tốt ❌**                  | **Tốt ✔️**                                |
|---------------------------|--------------------------------|--------------------------------|
| **Tránh dùng `goto`**     | Xử lý lỗi bằng `goto error;`  | Sử dụng `if-else` để xử lý lỗi rõ ràng |
| **Sử dụng hằng số lỗi**    | Trả về số cứng (`-1`, `0`, `1`) | Định nghĩa mã lỗi bằng `#define` |
| **Dùng hàm xử lý lỗi chuẩn** | `printf("Error occurred");`  | `perror("Error");` để in lỗi chi tiết |


---
## 📞 Contact
Email: individual.thuongnguyen@gmail.com    
GitHub: [github.com/thuongnvLK](https://github.com/thuongnvLK)