# kubeadm-certs

kubeadm-certs 只将 kubeadm 的证书有效期设置为10年，未对源码做任何其他修改。编译脚本见 [.travis.yml](.travis.yml) , CI 见 [![Build Status](https://travis-ci.com/lework/kubeadm-certs.svg?branch=master)](https://travis-ci.com/lework/kubeadm-certs)


## go 版本

> [编译最低版本源文件](https://github.com/kubernetes/kubernetes/blob/v1.24.0/hack/lib/golang.sh#L483) 

```bash
v1.15.12 go1.12.1
v1.16.15 go1.13.4
v1.17.12 go1.13.4
v1.18.9  go1.13.4
v1.19.3  go1.15.0
v1.20.0  go1.15.0
v1.21.0  go1.16.0
v1.22.0  go1.16.0
v1.23.0  go1.17.0
v1.24.0  go1.18.1
v1.25.0  go1.19
```

## 证书有效期文件

CA 证书

- https://github.com/kubernetes/kubernetes/blob/v1.21.0/staging/src/k8s.io/client-go/util/cert/cert.go#L38
- https://github.com/kubernetes/kubernetes/blob/v1.21.0/staging/src/k8s.io/client-go/util/cert/cert.go#L66

签发证书

- https://github.com/kubernetes/kubernetes/blob/v1.21.0/cmd/kubeadm/app/constants/constants.go#L49
- https://github.com/kubernetes/kubernetes/blob/v1.21.0/cmd/kubeadm/app/util/pkiutil/pki_helpers.go#L659

## 使用

项目release版本与kubernetes发布版本一致，可以在 [releases](https://github.com/lework/kubeadm-certs/releases) 页面直接查看

```bash
[ -f /usr/bin/kubeadm ] && mv /usr/bin/kubeadm{,_src}
wget https://github.com/lework/kubeadm-certs/releases/download/v1.21.0/kubeadm-linux-amd64 -O /usr/bin/kubeadm
chmod +x /usr/bin/kubeadm
```

版本信息差异

```bash
# 文件大小差异
$ls -al /usr/bin/kubeadm*
-rwxr-xr-x 1 root root 44613632 4月  12 17:30 /usr/bin/kubeadm
-rwxr-xr-x 1 root root 44625920 4月   9 01:54 /usr/bin/kubeadm_src

# sha256sum
$ sha256sum /usr/bin/kubeadm* 
3994974fdf4ab235d15151dee18caf9224e910d83dcc4606263c5129d89ea9bd  /usr/bin/kubeadm
7bdaf0d58f0d286538376bc40b50d7e3ab60a3fe7a0709194f53f1605129550f  /usr/bin/kubeadm_src

# 官方发布的版本
$ kubeadm_src version
kubeadm version: &version.Info{Major:"1", Minor:"21", GitVersion:"v1.21.0", GitCommit:"cb303e613a121a29364f75cc67d3d580833a7479", GitTreeState:"clean", BuildDate:"2021-04-08T16:30:03Z", GoVersion:"go1.16.1", Compiler:"gc", Platform:"linux/amd64"}

# 修改后的版本
$ kubeadm version
kubeadm version: &version.Info{Major:"1", Minor:"21+", GitVersion:"v1.21.0-dirty", GitCommit:"cb303e613a121a29364f75cc67d3d580833a7479", GitTreeState:"dirty", BuildDate:"2021-04-12T09:23:51Z", GoVersion:"go1.16.1", Compiler:"gc", Platform:"linux/amd64"}
```

> `dirty` 标记代表 `GitCommit` 版本后有修改源代码。

生成证书

```bash
kubeadm init phase certs all
kubeadm init phase kubeconfig all
```

检查证书过期时间

```bash
$ kubeadm certs check-expiration
[check-expiration] Reading configuration from the cluster...
[check-expiration] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[check-expiration] Error reading configuration from the Cluster. Falling back to default configuration

CERTIFICATE                EXPIRES                  RESIDUAL TIME   CERTIFICATE AUTHORITY   EXTERNALLY MANAGED
admin.conf                 Apr 10, 2031 10:02 UTC   9y                                      no      
apiserver                  Apr 10, 2031 10:02 UTC   9y              ca                      no      
apiserver-etcd-client      Apr 10, 2031 10:02 UTC   9y              etcd-ca                 no      
apiserver-kubelet-client   Apr 10, 2031 10:02 UTC   9y              ca                      no      
controller-manager.conf    Apr 10, 2031 10:02 UTC   9y                                      no      
etcd-healthcheck-client    Apr 10, 2031 10:02 UTC   9y              etcd-ca                 no      
etcd-peer                  Apr 10, 2031 10:02 UTC   9y              etcd-ca                 no      
etcd-server                Apr 10, 2031 10:02 UTC   9y              etcd-ca                 no      
front-proxy-client         Apr 10, 2031 10:02 UTC   9y              front-proxy-ca          no      
scheduler.conf             Apr 10, 2031 10:02 UTC   9y                                      no      

CERTIFICATE AUTHORITY   EXPIRES                  RESIDUAL TIME   EXTERNALLY MANAGED
ca                      Apr 10, 2031 10:02 UTC   9y              no      
etcd-ca                 Apr 10, 2031 10:02 UTC   9y              no      
front-proxy-ca          Apr 10, 2031 10:02 UTC   9y              no      
```

## LICENSE

MIT

