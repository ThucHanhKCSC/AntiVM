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

Kết quả

![7](https://user-images.githubusercontent.com/101321172/157817759-e1cdd005-ee2e-47f7-8963-3e15c4b78816.png)

# CPUId với eax = 1

sau khi gọi, CPUID trả về 31 byte chứa thông tin hệ thống. Trên máy vật lý, byte cuối sẽ là 0, còn trên máy ảo sẽ là 1

```C
#include <iostream>

using namespace std;

int main(){
    bool Vm = false;
    __asm{
        xor eax, eax                //reset eax
        mov eax, 1                  // eax = 1
        cpuid
        bt ecx, 0x1f //Bit test     // test bit cuối trong ecx
        jc _VM                      // Nếu bit cuối được set (CF = 1) thì jump đến _VM
        
        _notVM:
            jump _nop
        
        _VM:
            mov VM, 1
        _nop:
            nop
    }
    return 0;
}
```

![alt_text](https://www.cynet.com/wp-content/uploads/2020/11/9.png)

![10](https://user-images.githubusercontent.com/101321172/157818867-c4986ede-c7dd-484e-9d10-30043d21ae94.png)


Bypass cũng như trên, thay đổi file ![11](https://user-images.githubusercontent.com/101321172/157818925-69106243-677b-47e1-8169-de4d13a68e54.png)

Thành ```cpuid.1.ecx=”0—:—-:—-:—-:—-:—-:—-:—-”```

![alt_text](https://www.cynet.com/wp-content/uploads/2020/11/12.png)


![13](https://user-images.githubusercontent.com/101321172/157819022-e3f92fea-4c71-4ac1-990f-f95f9cd9eb09.png)



