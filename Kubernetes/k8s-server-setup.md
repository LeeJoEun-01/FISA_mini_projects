# Kubernetes Cluster 구축 및 테스트
<div align="center">
  <img width="500" alt="image" src="https://github.com/user-attachments/assets/7d6f8d77-2f4e-4b6d-bc35-58237828c979" />
</div>

> Ubuntu 환경에서 Kubernetes 클러스터를 구축하고, ClusterIP / Ingress를 통한 트래픽 라우팅을 실습한 과정을 정리한 것입니다.
<br>
##  1. 서버 사전 준비
### 📌 1-1. Netplan으로 고정 IP 설정
- VM을 여러 대 클러스터로 묶으려면 각 서버에 고정 IP가 필요합니다.
-  VM 간 네트워크 연결을 위해 NAT Network를 선택했으며, 최신 Ubuntu에서는 Netplan이 기본 네트워크 설정 툴이므로 이를 사용했습니다.
    
#### 🛜 Netplan 설치
  - `sudo apt install net-tools`
- 네트워크 인터페이스 확인
  - `ifconfig`
    - 인터페이스 이름: enp0s3
    <img width="400" src="https://github.com/user-attachments/assets/3e113832-0ffc-4247-84dd-a6aa06d2f7f9" />

#### 📄 Netplan 설정 파일 (/etc/netplan/01-netcfg.yaml)
``` bash
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 10.0.2.15/24
      routes:
        - to: default
          via: 10.0.2.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```

#### 📇 root 전용 권한을 주고 네트워크 적용
``` bash
sudo chmod 600 /etc/netplan/01-netcfg.yaml
sudo netplan apply
```

---

### 📌 1-2. VM 복제 및 Host 설정

> Kubernetes 클러스터는 <br>최소 1개의 마스터 노드와 2개 이상의 워커 노드로 구성되므로 VM을 복제하여 총 3개로 만들어서 사용합니다.

#### VM 복제후 설정
- **Hostname 변경**
  - `sudo hostnamectl set-hostname <hostname>`

- **/etc/hosts 파일에 각 서버 추가**
  ``` bash
  10.0.2.15 myserver01
  10.0.2.20 myserver02
  10.0.2.25 myserver03
  ```

### 📌 1-3. SSH 키 기반 인증
> 비밀번호 입력 없이 scp / ssh 가능하도록 설정 → 클러스터 관리 및 파일 전송 자동화에 필수이기 때문에 설정해줍니다.

- **키 생성**
  - `ssh-keygen -t rsa -b 4096`
  - 키 생성 후 확인 명령어
    ``` bash
    ls -l .ssh

    cat .ssh/authorized_keys
    cat .ssh/id_rsa
    cat .ssh/id_rsa.pub
    ```

- **공개키 공유**
  ``` bash
  # myserver02에 key 생성 직후 myserver01에 key 파일에 추가 
  ssh-copy-id ubuntu@myserver01

  ssh ubuntu@myserver01    #  myserver02 입장에선 myserver01에 비번 없이 접속 성공
  ```
- **접속 성공**
  - <img width="500" alt="image" src="https://github.com/user-attachments/assets/7f472df4-c434-4fdc-9347-c2c130a299bd" />
