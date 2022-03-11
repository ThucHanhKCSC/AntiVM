Nguồn: https://www.cynet.com/evasion-techniques/malware-anti-vm-techniques/

Hầu hết các máy ảo đều có tính năng tùy chỉnh card mạng

Nếu dùng máy ảo với NIC tự nhiên thì có thể liệt kê thông tin của nó bằng ```wmic nic list```

Và trong các thông tin nhận được sẽ có thông tin về địa chỉ MAC

Nếu đang dùng VMware thì sẽ nhận được địa chỉ MAC như sau:

00:05:69 (Vmware)

00:0C:29 (Vmware)

00:1C:14 (Vmware)

00:50:56 (Vmware)

Có thể khai thác các địa chỉ này để phát hiện máy ảo.

Bypass: không dùng NIC, hay tạo 1 custom NAT
