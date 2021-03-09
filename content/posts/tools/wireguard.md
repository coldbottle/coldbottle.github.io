---
author: "coldbottle"
title: "Wireguard"
date: 2021-03-09
draft: false
tags: ["devops", "vpn", "wireguard"]
categories: ["tools"]
series: ["tools"]
ShowToc: true
TocOpen: true
weight: 8001
---

### VPN을 구성해야 하는 이유?

공용 ip를 하나 더 받아서 활용하고 싶은 needs가 생겼다.

그래서 oracle에서 무료 인스턴스를 띄워서 vpn을 구성하고, reverse proxy로 사용하면 이 needs를 충족시킬 수 있다.



### Oracle Cloud에서 무료 인스턴스 받기

잘 설명된 링크로 설명을 대체합니다.

* [오라클 클라우드 가입하기](https://www.wsgvet.com/cloud/1)
* [오라클 클라우드 구획, 가상 네트워크, 방화벽, 공용IP 설정하기](https://www.wsgvet.com/cloud/4)
* [오라클 클라우드 인스턴스 생성 및 SSH 접속하기](https://www.wsgvet.com/cloud/5)
* [오라클 클라우드 Ubuntu 20.04 인스턴스 기본 설정하기](https://www.wsgvet.com/cloud/6)

### iptable 초기화

```bash
sudo iptables -F && sudo iptables -X && sudo netfilter-persistent save && sudo netfilter-persistent reload
```

### PiVPN Wireguard 설치

```bash
curl -L https://install.pivpn.io | bash
```

### PiVPN Wireguard 관리

[List of commands](https://docs.pivpn.io/wireguard/)에 더 자세한 내용을 확인할 수 있습니다.

#### 새로운 client 추가

```bash
pivpn add
Enter a Name for the Client: einstein
::: Client Keys generated
::: Client config generated
::: Updated server config
::: WireGuard reloaded
======================================================================
::: Done! einstein.conf successfully created!
::: einstein.conf was copied to /home/opc/configs for easy transfer.
::: Please use this profile only on one device and create additional
::: profiles for other devices. You can also use pivpn -qr
::: to generate a QR Code you can scan with the mobile app.
======================================================================
```

#### client list 확인

```bash
pivpn list
::: Clients Summary :::
Client        Public key                                        Creation date
einstein      zHkwFwDlvYJmT4aOboaNn3YDgzDTaQCCIka08b8qc3E=      09 Mar 2021, 12:32, UTC
gaudi         ozILnCC/QbWvB/MLZkOYmonDB4gWpuokYkx7MlYsQzU=      09 Mar 2021, 12:32, UTC
::: Disabled clients :::
```
