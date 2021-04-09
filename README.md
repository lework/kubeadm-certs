# kubeadm-certs

kubeadm-certs 将 kubeadm 的证书有效期设置为10年，未对源码做任何修改，编译脚本见 [.travis.yml](.travis.yml) , CI 见 [![Build Status](https://travis-ci.com/lework/kubeadm-certs.svg?branch=master)](https://travis-ci.com/lework/kubeadm-certs)


## go 版本

> 最低版本 https://github.com/kubernetes/kubernetes/blob/830055379b811f69f6f4cd8622bdcf8a08307c9a/hack/lib/golang.sh#L477

```bash
v1.15.12 go1.12.1
v1.16.15 go1.13.4
v1.17.12 go1.13.4
v1.18.9  go1.13.4
v1.19.3  go1.15.0
v1.20.0  go1.15.0
v1.21.0  go1.16.0
```

## 使用

项目release版本与kubernetes发布版本一致，可以在 [releases](https://github.com/lework/kubeadm-certs/releases) 页面直接查看

```bash
[ -f /usr/bin/kubeadm ] && mv /usr/bin/kubeadm{,_src}
wget https://github.com/lework/kubeadm-certs/releases/download/v1.19.3/kubeadm-linux-amd64 -O /usr/bin/kubeadm
chmod +x /usr/bin/kubeadm
```

## LICENSE

MIT