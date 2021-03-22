### Kiểu dữ liệu trong C

​    Bao gồm 3 nhóm chính:

* Kiểu số nguyên:

  Số thập phân được lưu dưới dạng nhị phân tương ứng. Các phép toán được thực hiện như số bình thường.

  ![1.1](D:\code c\project\1.1.png)

* Kiểu số thực dấu chấm động

  Được lưu trongbộ nhớ dưới dạng:

  ![](D:\code c\project\1.2.png)

  

  ![](D:\code c\project\1.3.png)

* Kiểu chuỗi

  Có bản chất là kiểu int kích thước 1 byte, mỗi giá trị số nguyên tương ứng với một ký tự trong bảng mã ASCII.

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

### Suffix



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

### Phép gán (assignment)

#### 	Implixit assignment

	#### 	Explixit assignment









