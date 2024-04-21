# HOTLINE CISCO 4321
EXEMPLO DE CONFIGURAÇÃO DE CIRCUITO DE VOZ HOTLINE

- ROUTER 1
> CONFIGURAÇÕES DE ENDEREÇAMENTO E ROTEAMENTO

 
```
enable
configure-terminal
hostname ROUTER-1

interface GigabitEthernet 0/0/0.100
encapsulation dot1q 100
description WAN
ip address 100.126.100.2 255.255.255.252
exit

interface GigabitEthernet 0/0/0
no shutdown
exit

route-map PERMIT-ALL permit 999
quit

router bgp 65055
neighbor 100.126.100.1 remote-as 111
neighbor 100.126.100.1 route-map PERMIT-ALL in
neighbor 100.126.100.1 route-map PERMIT-ALL out 
quit
```
> CONFIGURAÇÃO DE SIP E ROTAS

```
sip-ua
registrar ipv4: 100.126.100.2 expires 3600
exit

dial-peer 4108 voice voice
description OUTGOING
destination-pattern 4108
session protocol sipv2
session target ipv4: 100.64.200.2
dtfm-relay h264 alphanumeric
codec g711alaw
exit

dial-peer 4008 voice pots
description HOTLINE INCOMING
destination-pattern 4008
port 0/1/0
exit

voice-port 0/1/0
connection-plar 4108
exit
```
---

- ROUTER 2
> CONFIGRAÇÕES DE ENDEREÇAMENTO E ROTEAMENTO

```
enable
configure-terminal
hostname ROUTER-2

interface GigabitEthernet 0/0/0.200
encapsulation dot1q 200
description WAN
ip address 100.126.200.6 255.255.255.252
exit

interface GigabitEthernet 0/0/0
no shutdown
exit

route-map PERMIT-ALL permit 999
exit

router bgp 65055
neighbor 100.126.200.5 remote-as 65000
neighbot 100.126.200.5 route-map PERMIT-ALL in
neighbot 100.126.200.5 route-map PERMIT-ALL out
exit

```
> CONNFIGURAÇÃO DE SIP E ROTAS

```
sip-ua
registrar ipv4: 100.126.200.6 expires 3600
exit

dial-peer 4008 voice voip
description OUTGOING
destination-pattern 4108
session protocol sipv2
session target ipv4: 100.126.100.2
dtfm-relay hh245 alphanumeric
codec g711alaw

dial-peer 4108 voice pots
description INCOMING
destination pattern 4108
port 0/1/0
exit

voice-port 0/1/0
connection-plar 4008
exit
```

---

> PARA VISUALIZAR CHAMADAS CORRENTES

```
show voice call status
```
