---
author: "coldbottle"
title: "Intro"
date: 2021-03-04
draft: true
tags: []
categories: []
series: []
ShowToc: true
TocOpen: true
weight: 99999
---

### 준비물

```bash
brew install tmux ipcalc multipass cfssl kubectl
```

### multipass를 이용한 machine 3개 만들기

```bash
for i in 'master-k8s' 'worker-1-k8s' 'worker-2-k8s'; do multipass launch --name "${i}" --cpus 2 --mem 2048M --disk 5G 20.04; done
```

### 인증서 만들기

```bash
cat > ca-config.json << EOF
{
  "signing": {
    "default": {
      "expiry": "8760h"
    },
    "profiles": {
      "kubernetes": {
        "usages": [
          "signing",
          "key encipherment",
          "server auth",
          "client auth"
        ],
        "expiry": "8760h"
      }
    }
  }
}
EOF
```

```bash
cat > ca-csr.json <<EOF
{
  "CN": "Kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "KR",
      "ST": "Seoul",
      "L": "Seoul",
      "O": "Kubernetes",
      "OU": "CA"
    }
  ]
}
EOF
```

```bash
cfssl gencert -initca ca-csr.json | cfssljson -bare ca
# result
ca.pem
ca-key.pem
```

### admin client 인증서
### kubelet client 인증서
### kube controller manager client 인증서
### kube proxy client 인증서
### scheduler client 인증서
### kubernetes api server 인증서
