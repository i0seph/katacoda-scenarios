# 알파인 리눅스에 쿠버네티스 설치하는 방법

```sh
echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories
echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories
apk update
echo "br_netfilter" > /etc/modules-load.d/k8s.conf
modprobe br_netfilter
sysctl net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
echo "net.bridge.bridge-nf-call-iptables=1" >> /etc/sysctl.conf
sysctl net.bridge.bridge-nf-call-iptables=1
apk add cni-plugin-flannel
apk add cni-plugins
# apk add flannel -- aarch64에서는 아직 패키지 없음
curl -OLJ https://github.com/flannel-io/flannel/releases/download/v0.26.4/flannel-v0.26.4-linux-arm64.tar.gz
tar xf flannel-v0.26.4-linux-arm64.tar.gz
curl -OLJ 'https://gitlab.alpinelinux.org/alpine/aports/-/raw/master/testing/flannel/flanneld.confd?inline=false'
curl -OLJ 'https://gitlab.alpinelinux.org/alpine/aports/-/raw/master/testing/flannel/flanneld.initd?inline=false'
curl -OLJ 'https://gitlab.alpinelinux.org/alpine/aports/-/raw/master/testing/flannel/flanneld.logrotated?inline=false'
mkdir out
mv flanneld out
curl -OLJ https://raw.githubusercontent.com/flannel-io/flannel/refs/heads/master/Documentation/kube-flannel.yml
curl -OLJ https://raw.githubusercontent.com/flannel-io/flannel/refs/heads/master/Documentation/k8s-old-manifests/kube-flannel-legacy.yml

pkgname=flannel
_pkgname=flanneld
srcdir=.

install -Dm0755 out/$_pkgname "$pkgdir"/usr/bin/$_pkgname
install -Dm755 "$srcdir"/$_pkgname.initd \
		"$pkgdir"/etc/init.d/$_pkgname
install -Dm755 "$srcdir"/$_pkgname.confd \
		"$pkgdir"/etc/conf.d/$_pkgname
install -Dm644 "$srcdir"/$_pkgname.logrotated \
		"$pkgdir"/etc/logrotate.d/$_pkgname
install -d "$pkgdir"/var/log/$pkgname
install -d "$pkgdir"/var/run/$pkgname

mkdir -p "$subpkgdir"/etc/cni/net.d
install -Dm0755 mk-docker-opts.sh "$subpkgdir"/usr/bin/mk-docker-opts.sh
install -Dm0644 kube-flannel.yml \
		"$subpkgdir"/etc/kube-flannel/kube-flannel.yml
install -Dm0644 kube-flannel-legacy.yml \
		"$subpkgdir"/etc/kube-flannel/kube-flannel-legacy.tml

apk add kubelet
apk add kubeadm
apk add kubectl
apk add containerd
apk add uuidgen
apk add nfs-utils
sed -i '/swap/s/^/#/' /etc/fstab
swapoff -a
echo "#!/bin/sh" > /etc/local.d/sharemetrics.start
echo "mount --make-rshared /" >> /etc/local.d/sharemetrics.start
chmod +x /etc/local.d/sharemetrics.start
rc-update add local
uuidgen | sed 's/\-//g' > /etc/machine-id
rc-update add containerd
rc-update add kubelet
rc-service containerd start
apk add 'kubelet=~1.32'
apk add 'kubeadm=~1.32'
apk add 'kubectl=~1.32'
kubeadm init --pod-network-cidr=10.244.0.0/16 --node-name=$(hostname)
# set user profile
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
# on master
kubeadm token create --print-join-command
# on worker
```
