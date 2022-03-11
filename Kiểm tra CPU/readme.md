# Kiểm tra CPU

Lệnh ```CPUID``` trong cấu trúc máy tính trả về thông tin của tiến trình. Khi malware nhận đươc thông tin từ ```CPUID```, nó so sánh với các thông tin có trước, nếu phát hiện thông tin có được là từ máy ảo, malware sẽ tự động đóng tiến trình của bản thân

```CPUID``` nhận vào eax, và trả về 2 thanh ecx, edx thông tin CPU

```C++
#include<iostream>
#incude <string>

int main(){
    bool vb = false;
    
    __asm{
      xor eax, eax
      mov eax, 0x40000000   //Hypervisor CPUID Leaf Range
      cpuid
      cmp ecx, 0x4d566572   // cmp ecx, "reVM"
      jne _nop                                            //
      cmp edx, 0x65726177   // cmp edx, "ware"            // nếu 1 trong 2 giá trị trong ecx, edx bằng với 2 giá trị cho trước thì jump ngay đến lable nop, và không thực hiện đặt vb = 1
      jne _nop                                            //
      mov vb, 0x1           // vb = 1
      _nop:                 
      nop
    }
    cout << vb 
}
```

Kết quả

![3](https://user-images.githubusercontent.com/101321172/157817050-6d88a250-eb53-41b5-8838-0ad2460d8bed.png)

![plant_text](https://www.cynet.com/wp-content/uploads/2020/11/4.png)


cách bypass:

Dùng text editor thay đổi giá trị trong file 
![5](https://user-images.githubusercontent.com/101321172/157817330-911e211a-38b1-47d7-a99a-d92c8a99d77d.png)

Thành 

```cpuid.40000000.ecx=”0000:0000:0000:0000:0000:0000:0000:0000″```

```cpuid.40000000.edx=”0000:0000:0000:0000:0000:0000:0000:0000″```

![plant text](https://www.cynet.com/wp-content/uploads/2020/11/6.png)

Giải thích: khi chuyển như vậy thì giá trị trả về trong ecx, edx luôn == 0 khi gọi ```CPUID``` với eax = 0x4000000, nên 2 lệnh so sánh trên đều thất bại 
