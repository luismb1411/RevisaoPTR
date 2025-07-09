<img src="https://github.com/luismb1411/RevisaoPTR/blob/main/Lab_Revisao1.png" alt="Lab1" align="center" />

## VLAM + OSPF 

**Configuração ips das interfaces de R3** 
```bash
enable
conf t
int loopback 0 
ip add 3.3.3.3 255.255.255.255 
int e0/0
ip add 172.17.0.5 255.255.255.252
no shut
int e0/1
ip add 172.17.0.1 255.255.255.252
no shut 
```
**Faça o mesmo processo pra R4** 

**Configurando o SW1** 
```bash
enable
conf t
int e0/0
no switchport
ip add 172.17.0.2 255.255.255.252
no shut 
```
**Configurando as VLANS** 
```bash
vlan 10 // Cria a vlan 10
vlan 20 // Cria a vlan 20 
int vlan 10
ip add  192.168.10.1 255.255.255.0 // Atribui um ip a vlan 10 
int vlan 20
ip add  192.168.10.1 255.255.255.0 // Atribui um ip a vlan 20
int e0/1
switchport mode access
switchport access vlan 10 // A int e0/1 vai estar em access passando a vlan 10 
int e0/2
switchport mode access
switchport access vlan 20 // A int e0/1 vai estar em access passando a vlan 20 
int vlan 10
no shut
int vlan 20
no shut 
```
**Configurando Ip nos VPCS**
```bash
ip 192.168.x.x/24 192.168.x.1 // IP/Mask Gateway
```
Fazer o mesmo para o switch e os VPCS do outro lado 

**Configurando o OSPF no R3**
```bash
router ospf 1  // Ativa o roteamento OSPF 
router-id 3.3.3.3 // Id roteador
network 172.17.0.0 0.0.0.255 area 0 // Informas as redes daquele roteador junto com a sua area 
```
Fazer o mesmo para R4

**Configurar rotas estasticas e rota default para que tudo tenha comunicação** 
**No SW** 
```bash
ip route 0.0.0.0 0.0.0.0 172.17.0.1  // Ip destino / Mask / Proximo Salto 
```
**No R3**
```bash
ip route 172.17.0.2  255.255.255.0 192.168.10.0
ip route 172.17.0.2  255.255.255.0 192.168.10.0
router ospf 1
redistribute static subnets  // Comando para que o OSPF redistribua rotas estaticas da
tabela de roteamneto para outros roteadores no mesmo OSPF 
```
Fazer o mesmo para o R4 para SW2 e para os VPCs 

