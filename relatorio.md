# Configuração de NAT e DHCP

## Ambiente

Ubuntu 16.04

## Ferramentas

* isc-dhcp-server
* vim

### Procedimentos

* Edição do arquivo: <b>/etc/network/interfaces</b>

```
auto 'interface interna'
iface 'interface interna' inet dhcp

auto 'interface externa'
iface 'interface externa' inet static
   address 
   netmask 
   gateway 

```

* Instalação do isc-dhcp-server:
```
sudo apt install isc-dhcp-server
```

* Acessar o arquivo:

```
    /etc/default/iscp-dhcp-server

    e setar a seguinte linha de comando:

    INTERFACES="interface de rede"
```

* Editar o seguinte arquivo: /etc/dhcp/dhcpd.conf

```
option subnet-mask ;
option broadcast-address ;
subnet  netmask  {
range ;
option routers ;
option domain-name-servers ;
}
```

* Reiniciar o servidor

```
sudo service isc-dhcp-server restart

```

## Configuração do NAT

´´´
sudo vim /etc/sysctl.conf
net.ipv4.ip_forward=1
´´´

# iptables

´´´
sudo iptables -t nat -A POSTROUTING -o "interface" -j MASQUERADE
´´´
