Nguồn: https://www.cynet.com/evasion-techniques/malware-anti-vm-techniques/

Máy ảo chứa các file, tính năng và driver đặc biệt hỗ trợ hệ điều hành hoạt động

Malware có thể tìm kiểm những file này dựa trên các thư viện DLL trong C:\windows\System32\Drivers\, và trong đó có thể chứa các driver chỉ có ở máy ảo

Code:

```C
 _OFSTRUCT Of;

char *path = "C:\windows\System32\Drivers\Filenaodo.dll"

HFILE f = Openfile(path, &Of, OF_EXIST);

if(f == HFILE_ERROR){
      //Không phải máy ảo
}
else{
 // Máy ảo
}

```
