
Nguồn: https://www.cynet.com/evasion-techniques/malware-anti-vm-techniques/

Sử dụng command để cho ra thông tin của tiến trình đang chạy, Malware có thể thực hiện các thuật toán với các tiến trình liên quan tới máy ảo từ các string nhận được qua command

Code API có thể display các tiến trình đang chạy

```C
void _displayProcess(){

	HANDLE hSnap = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);

	if(hSnap != INVALID_HANDLE_VALUE){

		PROCESSENTRY32 procEntry;
		procEntry.dwSize = sizeof(PROCESSENTRY32);

		if(Process32First(hSnap, &procEntry)){
			do{
				#undef UNICODE
				std::wcout << procEntry.szExeFile << " ";
				
			}
			while(Process32Next(hSnap, &procEntry));
		}
			
	}
	CloseHandle(hSnap);

}
```

![New Bitmap Image](https://user-images.githubusercontent.com/101321172/157825385-24cdeaf7-4138-44af-92aa-b738e1f0ce0a.jpg)


Một số các process sau xuất hiện ở trong máy ảo:

-Vmtoolsd.exe

-Vmwaretrat.exe

-Vmwareuser.exe

-Vmacthlp.exe

