# Kiểm tra Registry Keys


Resistry key là mạng lưới của hệ thống Window, và thông tin về việc sử dụng máy ảo có thể tìm thấy ở đây

Việc cài đặt VMtool có thể tạo thêm các registry key

Việc đơn giản nhất là kiểm tra sự tồn tại của registry key bằng phương thức ```Get```

```C
expected_val = 0x1;
lResult = RegOpenKeyEx (hKeyRoot, lpSubKey, 0, KEY_READ, &hKey);

if(hKey == expected_val){
  // Đây là máy ảo
}
else{
  // Máy vật lý
}
```
