# Chapter 01 - Virtual Box 및 Vagrant를 이용해 3개의 VM을 Cluster로 연결

(출처)
```
1. Kubernetes의 설치는 Webmakaka님의 작업 내용을 참고하였습니다. 
    https://github.com/webmakaka/vagrant-kubernetes-3-node-cluster-ubuntu-20.04

2. Jenkins Deploy의 경우 '두발로 걷는 개의 발자국' 블로그를 참고하였습니다.
    https://twofootdog.tistory.com/11?category=845779
```

Chapter 01은 다음의 Contents를 포함하고 있습니다.

1. VirtualBox 및 Vagrant 설치
2. vagrant up
3. kubectl을 통한 Node 동작상태 확인

# 0. 사전 준비 
  Kubernetes 환경을 Baremetal에 설치 할 수도 있지만, Office 환경에 물리장비를 여러대 운용할 수 있는 사람이 얼마나 되겠습니까? 따라서 Oracle에서 제공하는 Virtual Box를 통해 IaaS를 구성하고 해당 VM을 Host로 Kuberneter Node들을 설치할 것입니다.

- Vagrant, Virtual box 를 통한 Kubenetes Cluster 구축

## 0.1 VirtualBox 설치
- Local PC에 Viirtual Box를 설치 합니다.  
https://www.virtualbox.org/wiki/Downloads

## 0.2 Vagrant 설치
- Local PC에 Vagrant를 설치 합니다.  
https://www.vagrantup.com/downloads


# 1. vagrant up

## 1.1 Vagrantfile

 Virtual Box와 Vagrant가 설치 되었다면 vagrant up 명령을 통해 Vagrantfile을 싱핼 할 수 있습니다.

- vagrant up은 Command Line 명령으로 Vagrantfile를 실행하므로 Vagrantfile이 있는 디렉토리에서 실행해야 합니다.
  ```sh
  > vagrant up
  ```


- Vagrantfile에 포함하고 있습니다.
  ```sh
  # -*- mode: ruby -*-
  # vi: set ft=ruby :

  ENV['VAGRANT_NO_PARALLEL'] = 'yes'

  Vagrant.configure(2) do |config|

    config.vm.provision "shell", path: "bootstrap.sh"

    # Kubernetes Master Server
    config.vm.define "master.k8s" do |node|
    
      node.vm.box = "ubuntu/focal64"
      node.vm.box_check_update = false
      node.vm.hostname = "master.k8s"
      node.vm.network "private_network", ip: "172.16.0.10"
    
      node.vm.provider "virtualbox" do |v|
        v.name = "master"
        v.memory = 2048
        v.cpus = 2
      end

      node.vm.provision "shell", path: "bootstrap_master.sh"
    
    end


    # Kubernetes Worker Nodes
    NodeCount = 2

    (1..NodeCount).each do |i|

      config.vm.define "node#{i}.k8s" do |node|

        node.vm.box = "ubuntu/focal64"
        node.vm.box_check_update = false
        node.vm.hostname = "node#{i}.k8s"
        node.vm.network "private_network", ip: "172.16.0.1#{i}"

        node.vm.provider "virtualbox" do |v|
          v.name = "node#{i}"
          v.memory = 2048
          v.cpus = 2
        end

        node.vm.provision "shell", path: "bootstrap_node.sh"
      end
    end
  end

  ```
  - 자세히 설명하진 않겠지만 크게 vagrant가 설치할 Image는 "ubuntu/focal64" 이고 private_network을 통해 IP를 할당하고 있음을 알수 있습니다.
  - H/W 구성을 CPU Core 수와 Memory의 크기를 살표 볼 수 있습니다.
  vagrant의 경우 VM기동 후 bootstrap.sh를 먼저 실행시키고 이후 vm.provision의 내용을 실행시킵니다.

  ## 1.2 Bootstrap
   kubernetes의 설치 방법은 하나하나 따라가보면 좋겠지만, 자세한 내용은 (https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)를 참고하여 주십시오.
  
  ### bootstrap.sh
  - 어쩃든 bootstrap.sh은 kubecadm, kubelet, kubectl을 자동으로 설치 합니다.
  - bootstrap_master.sh 은 apiserver, calico network등을 추가적으로 설정 및 설치하며, 
  - bootstrap_node.sh 은 master node에 Join합니다.

  이제 Kubernetes Cluster 구성이 완료 되었습니다.
  
## 1.2 로컬환경의 Kubectl 연결 설정

### 1.1.1 로컬 환경의 Kubectl 설치
- Vagrant를 통해 설치된 VM 내부에는 Kubectl이 설치 되어있지만, 로컬 환경에서는 Kubectl이 설치 되어있지 않습니다.
- Kubectl이란 Kubernetes Cluster와 통신 할 수 있도록 명령을 제공합니다.
- 설치 방법은 다음 링크를 참고하여 주십시오.  
  (https://kubernetes.io/ko/docs/tasks/tools/)
- Kubectl은 VM 이 아닌 LocalPC에 설치 하여 주십시오.

### 1.2.1  Kubectl을 통한 Kubernetes 동작 확인

- kubectl을 설치 완료 했다면, VM에 생성한 Master Node와 연결을 설정해야합니다. 기본설정파일은 Master Node에 있으며, 해당 파일을 Local 환경으로 복사 해야합니다.

- Linux나 MACOS의 경우는 scp를 이용해 Config 파일을 Master로 부터 가져올 수 있습니다.  
  (root의 password는 kubeadmin입니다. bootstrap.sh의 Task9 에서 확인할 수 있습니다.)
  ```sh
  ❯ scp root@172.16.0.10:/etc/kubernetes/admin.conf ~/.kube/config
  The authenticity of host '172.16.0.10 (172.16.0.10)' can't be established.
  ECDSA key fingerprint is SHA256:umvTkuzcEhPS731sVF/svh0rkbMtsLFYmhiNO0yWh4I.
  Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
  Warning: Permanently added '172.16.0.10' (ECDSA) to the list of known hosts.
  root@172.16.0.10's password: 
  admin.conf                  
  ```

- Windows의 경우도 별도의 Path가 있을 것으로 생각되지만, 찾지 못한 경우 아래의 옵션으로 지정할 수 있다.
  ```sh
  > kubectl.exe --kubeconfig=<CONFIG_PATH>
  ```


- 설정이 완료 되었다면 다음의 명령어를 통해 node들의 동작 상태를 확인할 수 있습니다.
  ```sh
  > kubectl get nodes -o wide
  NAME     STATUS   ROLES                  AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
  master   Ready    control-plane,master   14m   v1.21.0   172.16.0.10   <none>        Ubuntu 20.04.2 LTS   5.4.0-70-generic   containerd://1.3.3-0ubuntu2.3
  node1    Ready    <none>                 12m   v1.21.0   172.16.0.11   <none>        Ubuntu 20.04.2 LTS   5.4.0-70-generic   containerd://1.3.3-0ubuntu2.3
  node2    Ready    <none>                 10m   v1.21.0   172.16.0.12   <none>        Ubuntu 20.04.2 LTS   5.4.0-70-generic   containerd://1.3.3-0ubuntu2.3
  ```