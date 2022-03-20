# How to Use Socket CAN

리눅스에서는 SocketCAN을 지원하고, 다음과 같은 특징을 가진다
 1. Pysical CAN 설정 및 네트워크 프로토콜 지원   'can0', 'can1', ...
 2. Virtual CAN 생성하고 Pysical CAN에 할당 가능  'vcan0', 'vcan1', ...
 3. Serial CAN틍로 사용가가능  'slcan0', 'slcan1', ...


## pysical can0 상태 확인
'ip addr ls dev'
```
$ ip addr ls dev can0
```
또는
'ifcofnig'
```
$ ifcofnig can0
```

## pysical can0 설정 및 연결
SocketCAN을 사용하기 앞서 HW 설정을 해줘야 한다.
'modprobe' 'ip link set' 'type' 'bitrate'
```
$ modpobe can
$ ip link set can0 type can bitrate 500000 mtu
```
연결
```
$ ip link set up can0
```

## virtucal vcan0 추가 
'modprobe' 'ip link add dev' 'type'
```
$ modprobe vcan
$ ip link add dev vcan0 type vcan
```


## can-utils를 이용한 데이터 송수신
'can-utils'를 사용하여 CLI로 CAN data를 송수신할 수 있다.
```
$ install apt can-utils
```
CAN 데이터 보내기 'cansend'
 - can0 노드에 ID 0x012로 데이터 0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77 8개 보내기
```
$ cansend can0 012#0011223344556677
```
CAN 데이터 확인하기 'candump'
```
$ candump can0
```

# bash 셀실행시 자동으로 can 설정하기

init_can.sh 만들기

```bash
#!/bin/bash
[ "$UID" -eq 0 ] || exec sudo bash "$0" "$@"
modprobe vcan
ip link add dev vcan0 type vcan
ip link set up vcan0
```
[참조 사이트](https://www.pragmaticlinux.com/2021/10/how-to-create-a-virtual-can-interface-on-linux/?msclkid=556e5514a77911ec86426dc010880a5f)

[Python.can](https://python-can.readthedocs.io/en/master/interfaces/socketcan.html)
