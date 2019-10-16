# Configuração de NAT e DHCP

## Ambiente

Ubuntu 16.04

## Ferramentas

* isc-dhcp-server
* vim

### Procedimentos

* Edição do arquivo: <b>/etc/network/interfaces</b>

A interface wlp3s0 irá estabelecer conexão com a rede externa,
recebendo o endereço de ip do servidor DHCP externo.

Já a interface enp2s0f1 é conectada a rede interna.

obs: nos lugares das interfaces wlp3s0 e enp2s0f1 pode-se colocar as interfaces da máquina que você utilizará como gateway.

```
auto wlp3s0
iface wlp3s0 inet dhcp

auto enp2s0f1
iface enp2s0f1 inet static
   address 192.168.100.10 
   netmask 255.255.255.0
   gateway 192.168.100.1

```
* Reiniciar a conexão

```
sudo systemctl restart networking

```

* Instalação do isc-dhcp-server:

```

sudo apt install isc-dhcp-server

```

## Configuração do servidor DHCP

* Acessar o arquivo: <b>/etc/default/iscp-dhcp-server</b>
na seguinte linha de código ela será responsavel por estabelecer qual
interface irá estabelecer conexão para a rede interna

```

    INTERFACES="enp2s0f1"
```

* Editar o seguinte arquivo: /etc/dhcp/dhcpd.conf

```
option subnet-mask 255.255.255.0;
option broadcast-address 192.168.100.255;
subnet 192.168.100.0 netmask 255.255.255.0 {
range 192.168.100.20 192.168.100.100;
option routers 192.168.100.10;
option domain-name-servers 192.168.133.1;
}
```
Adicionando os IPs estáticos 

```
host lds {hardware ethernet 48:4d:7e:fb:f2:d7; fixed-address 192.168.100.21;}
host casa{hardware ethernet 74:27:ea:74:d8:87; fixed-address 192.168.100.27;}

```

* Reiniciar o servidor DHCP

```

sudo service isc-dhcp-server restart

```

## Configuração do NAT

```
sudo vim /etc/sysctl.conf
net.ipv4.ip_forward=1

```

Encaminhamento das regras do nat para a interface que faz conexão com a rede interna

```

sudo iptables -t nat -A POSTROUTING -o enp2s0f1 -j MASQUERADE

```

# Configuração nos cliente dhcp

* Adição da seguinte linha no /etc/network/interfaces

<b>ifconfig para listar as interfaces de redes do host e adicione a seguinte linha de codigo</b>

```
ifconfig 

auto "interface"
iface "interface" inet dhcp

```
* Reiniciar a conexão nos host
```

sudo systemctl restart networking

```
* conferir os leases no servidor
```

cat /var/lib/dhcp/dhcpd.leases

```

# Validação

* ping no ip do servidor dhcp para os hosts
* ping dos hosts para o servidor dhcp
* conexão na internet pelos hosts
