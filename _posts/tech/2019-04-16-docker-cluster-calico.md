---
layout: post
title: calico容器集群搭建
category: 技术
tags: docker cluster calico
keywords: docker cluster calico
---

## 容器集群搭建

## 环境

###关闭防火墙，若开启iptables防火墙，则需要打开2380端口通信
systemctl disable firewalld
systemctl stop firewalld
iptables -F
firewall-cmd --state

###打开ip转发功能
echo 1 > /proc/sys/net/ipv4/ip_forward
echo "net.ipv4.conf.all.rp_filter=1" >> /etc/sysctl.conf
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p

## etcd

## calico

curl -L https://github.com/projectcalico/calicoctl/releases/download/v1.1.0/calicoctl -o /usr/local/bin/calicoctl
curl -L https://github.com/projectcalico/calicoctl/releases/download/v1.6.1/calicoctl -o /usr/local/bin/calicoctl
chmod +x /usr/local/bin/calicoctl

curl -L -o /var/lib/calico/calico https://github.com/projectcalico/cni-plugin/releases/download/v1.11.0/calico
curl -L -o/var/lib/calico/calico-ipam https://github.com/projectcalico/cni-plugin/releases/download/v1.11.0/calico-ipam
chmod +x /var/lib/calico/calico
chmod +x /var/lib/calico/calico-ipam

cat << EOF | calicoctl create -f -
- apiVersion: v1
  kind: calicoApiConfig
  metadata:
  spec:
    datastoreType: "etcdv2"
    etcdEndpoints: "http://172.16.5.221:2379,http://172.16.5.222:2379,http://172.16.5.225:2379,http://172.16.5.226:2379"
EOF

cat << EOF | calicoctl create -f -
- apiVersion: v1
  kind: ipPool
  metadata:
    cidr: 172.18.0.0/16
  spec:
    ipip:
      enabled: true
    nat-outgoing:  true
    disabled: false
EOF

## flannel

## 测试

## consul
