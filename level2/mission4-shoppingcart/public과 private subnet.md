# public과 private subnet

[https://guide-fin.ncloud-docs.com/docs/networking-vpc-vpcuserscenario2](https://guide-fin.ncloud-docs.com/docs/networking-vpc-vpcuserscenario2)

Virtual Private Cloud(VPC)에 public subnet과 private subnet이 존재한다.

private 서버 (예: DB)에 대한 공개적인 접근을 차단하며 Public Web Application을 실행하려는 경우에 권장된다.

![Untitled](public%E1%84%80%E1%85%AA%20private%20subnet%203b70ce1dbe4b4e9f90c3f11fd1ffb3ca/Untitled.png)

1. VPC

- IPv4 CIDR 블록의 크기가 /16(예: 10.0.0.0/16)으로 제공되며 65,536개의 주소 공간을 제공한다.

2. Public Subnet

- Subnet 크기가 최대 /24(예: 10.0.0.0/24)으로 제공되며 256개의 주소 공간을 제공한다.
    - Public Subnet내의 서버는 인터넷과 연결이 필요한 서버입니다. (예 : 웹서버)
    - 각각의 서버는 공인 IP를 하나씩 보유합니다. (1 : 1 NAT)

3. Private Subnet

- Subnet 크기가 최대 /24(예: 10.0.0.0/24)으로 제공되며 256개의 주소 공간을 제공한다.
- Private Subnet내의 서버는 인터넷에서 수신되는 트래픽을 수락할 필요가 없는 서버입니다.(예 : DB)

4. Internet Gateway

- VPC를 인터넷 및 다른 Ncloud 서비스에 연결합니다.

5. NAT Gateway

- Private Subnet 내의 서버가 인터넷 연결 시에 사용하는 게이트웨이 입니다.

6. 라우팅 테이블

- Public Subnet : 서버가 VPC 내의 다른 인스턴스와 통신할 수 있게 하는 항목(local)과 인터넷과 직접 통신할 수 있게 하는 항목(IGW)이 저장됩니다.
- Private Subnet : 서버가 VPC 내의 다른 인스턴스와 통신할 수 있게 하는 항목(local)과 NAT Gateway를 통해 인터넷과 통신할 수 있게 하는 항목이 저장됩니다.

### ****보안****

보안을 위해 ACG와 Network ACL을 제공합니다. ACG는 서버의 Inbound/ Outbound 트래픽을 제어하고, Network ACL은 Subnet의 Inbound/Outbound 의 트레픽을 제어합니다.