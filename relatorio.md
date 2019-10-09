# Configuração de NAT e DHCP

## Ambiente

Ubuntu 16.04

## Ferramentas

* isc-dhcp-server
* vim

### Procedimentos

* Edição do arquivo: <b>/etc/network/interfaces</b>

```
auto wlp3s0
iface wlp3s0 inet dhcp

auto enp2s0f1
iface enp2s0f1 inet static
   address 192.168.100.10 
   netmask 255.255.255.0
   gateway 192.168.100.1

```

* Instalação do isc-dhcp-server:

```

sudo apt install isc-dhcp-server

```

* Acessar o arquivo:

```
    /etc/default/iscp-dhcp-server

    e setar a seguinte linha de comando:

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

* Reiniciar o servidor

```

sudo service isc-dhcp-server restart

```

## Configuração do NAT

```
sudo vim /etc/sysctl.conf
net.ipv4.ip_forward=1

```

# iptables

```

sudo iptables -t nat -A POSTROUTING -o enp2s0f1 -j MASQUERADE

```

# Configuração nos cliente dhcp

* Adição da seguinte linha no /etc/network/interfaces

<b>ifconfig para listar as interfaces de redes do host</b>
```
ifconfig 

auto enps20
iface enp2s0 inet dhcp

```
* conferir os leases

```

cat /var/lib/dhcp/dhcpd.leases

```
