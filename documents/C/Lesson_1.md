### Quy chuẩn đặt tên thông dụng

* Hằng số phải viết in hoa. 

  ```c
  #define UART_TXD_PINNUM (10)
  ```

* Tên biến, tên hàm viết thường.

  ```c
  void robot_response(int id_command, char *message);
  ```

* Nếu tên gồm nhiều từ thì cách nhau bởi dấu "_".

* Thêm "_t" nếu là biến typedef.

  ```c
  typedef struct {
      double scale;
      double bias;
      double under_limit;
      double upper_limit;
  } servo_channel_calib_t;
  ```

* Thêm "_e" nếu là tên enum.

* Thêm "_st" nếu là tên struct.

### Kiểu dữ liệu trong C

​    Bao gồm 3 nhóm chính:

* Kiểu số nguyên:

  Số thập phân được lưu dưới dạng nhị phân tương ứng. Các phép toán được thực hiện như số bình thường.

  ![](D:\workspace\git\pif_c_cpp_sharing\documents\C\soures\1.1.png)

* Kiểu số thực dấu chấm động

  Được lưu trongbộ nhớ dưới dạng:

  ![](D:\workspace\git\pif_c_cpp_sharing\documents\C\soures\1.2.png)

  

  ![](D:\workspace\git\pif_c_cpp_sharing\documents\C\soures\1.3.png)

* Kiểu chuỗi

  Có bản chất là kiểu int kích thước 1 byte, mỗi giá trị số nguyên tương ứng với một ký tự trong bảng mã ASCII.

### Phép gán (assignment)

Phép gán khác kiểu được chia làm 2 loại

#### 		Implicit assignment

Phép gán do chương trình ngầm thực hiện. Trong trường hợp gán biến size nhỏ cho biến size lớn. Hoặc từ int sang float. Do code C có quy định sẵn cho trường hợp này.

```c
int a = 10;
float b;
b = a; //Implicit assignment
```

#### 		Explicit assignment

Phép gán thông qua việc _ép kiểu_ của người viết. Dùng trong trường hợp gán biến size lớn cho biến size nhỏ hơn. Hoặc từ các kiểu dữ liệu khác nhau.

```c
int a = 65;
float b = 9.5;
char c;
c = (char)a; //Explicit assignment
a = (int)b;  //Explicit assignment
```

### Declare và Define

**Declare** một biến hay một hàm là chỉ cung cấp thông tin về tên, kiểu dữ liệu cho compiller. Mà không cho biết giá trị cụ thể:

```c
int add(int arg1, int arg2);
```

**Define** sẽ cung cấp rõ giá trị cho biến hay mô tả cụ thể cho hàm. Define có thể thực hiện cùng lúc hoặc sau khi declare.

```c
int add(int arg1, int arg2) {
return arg1 + arg2;
}
```

### Number literal

Phần giá trị trong lệnh define một biến, được chương trình ngầm hiểu theo một số kiểu nhất định.  Những kiểu đó gọi là _non-suffix_, vd: int, double,...

```c
int int_var = 8; // 8 thuộc kiểu int
float pi = 3.14 // 3.14 thuộc kiểu double
```

Nên nếu chúng ta muốn thay đổi kiểu của các giá trị đó, ta phải thêm suffix:

```c
long a = 3210123456789L;
unsigned long b = 1234567890123456789ul; //ul hay lu đều được.
float pi = 3.14f;
// suffix không phân biệt chữ hoa, chữ thường
```

### Signed và unsigned number

Xem đoạn code sau, và trả lời câu hỏi

```c
unsigned int unsign_var = -1;
printf("%u", unsign_var); //Giá trị mà biến unsign_var in ra là gì??
```

Ta cần biết cách chương trình lưu một số âm: _dạng bù 2_

![](D:\workspace\git\pif_c_cpp_sharing\documents\C\soures\2.1.png)

Và vì biến **unsign_var** là biến không dấu, nên nó sẽ hiểu số đó theo cách chuyển cơ số thông thường. Vì vậy giá trị in ra màn hình sẽ là **4.294.967.295**. số lớn nhất của kiểu _unsigned int_.

