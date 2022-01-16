# kubespary-with-vagrant
* VirtualBox Extension Pack 설치 필수
* 백신을 사용할 경우 https 연결 검사 기능을 중지 또는 아래의 주소를 신뢰할 수 있는 주소로 설정 필요
    * vagrantcloud.com, app.vagrantup.com
    * Kaspersky Free 버전의 경우, https 연결 검사를 중지해야 정상적으로 vagrant를 통해 설치가 가능

## Controlplane 작업
```
swapoff -a
sed -i '/swap/d' /etc/fstab
systemctl stop ufw
systemctl disable ufw
sysctl -w net.ipv4.ip_forward=1
sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/g' /etc/sysctl.conf
sysctl -p /etc/sysctl.conf
```
---

## Worker 작업
```
swapoff -a
sed -i '/swap/d' /etc/fstab
systemctl stop ufw
systemctl disable ufw
sysctl -w net.ipv4.ip_forward=1
sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/g' /etc/sysctl.conf
sysctl -p /etc/sysctl.conf
```
---

## Bootstrap 작업
```
sudo apt install python3-pip python3-setuptools virtualenv -y
virtualenv --python=python3 venv
. venv/bin/activate
git clone https://github.com/kubernetes-sigs/kubespray
cd kubespray
pip install -r requirements.txt
```
---

## 설치
* Bootstrap 서버, venv 환경에서 실행
```
ansible-playbook -i inventory/mycluster/inventory.ini --become --become-user=root cluster.yml
```

## References
* https://github.com/choisungwook-vagrant/kubespray-onpremise