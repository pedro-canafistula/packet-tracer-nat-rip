# Abrir em versões antigas do Packet Tracer (ex.: 8.2.2)

O arquivo `topologia_nat_rip.pkt` foi salvo no **Packet Tracer 9.0.1**. Versões anteriores
(como a 8.2.2) não abrem arquivos de versões mais novas, e o PT não salva em formato antigo.

## Opção 1: atualizar o Packet Tracer (recomendado)
Baixe o Packet Tracer 9 gratuitamente em https://www.netacad.com (login Cisco/NetAcad →
Resources → Download Packet Tracer) e abra o `.pkt` normalmente.

## Opção 2: remontar no 8.2.2 em ~5 minutos
Todos os equipamentos abaixo existem no 8.2.2.

**1. Equipamentos** (arraste para o canvas e renomeie em Config → Display Name):

| Nome | Modelo |
|---|---|
| PC1, PC2, PC3 | PC-PT (End Devices) |
| SW-EMPRESA | 2960 (Switches) |
| R-EMPRESA, R-ISP | 2911 (Routers) |
| SERVIDOR-WEB | Server-PT (End Devices) |

**2. Cabos**

| De | Porta | Para | Porta | Cabo |
|---|---|---|---|---|
| PC1 | FastEthernet0 | SW-EMPRESA | Fa0/1 | Cobre direto |
| PC2 | FastEthernet0 | SW-EMPRESA | Fa0/2 | Cobre direto |
| PC3 | FastEthernet0 | SW-EMPRESA | Fa0/3 | Cobre direto |
| SW-EMPRESA | Gi0/1 | R-EMPRESA | Gi0/0 | Cobre direto |
| R-EMPRESA | Gi0/1 | R-ISP | Gi0/0 | Cobre cruzado |
| R-ISP | Gi0/1 | SERVIDOR-WEB | FastEthernet0 | Cobre cruzado |

**3. Roteadores**: abra a aba CLI, responda `no` à pergunta inicial e cole o conteúdo de
[`configs-para-colar/R-EMPRESA.txt`](configs-para-colar/R-EMPRESA.txt) e
[`configs-para-colar/R-ISP.txt`](configs-para-colar/R-ISP.txt).

**4. PCs e servidor** (Desktop → IP Configuration, modo Static):

| Host | IP | Máscara | Gateway |
|---|---|---|---|
| PC1 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC2 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 |
| PC3 | 192.168.10.13 | 255.255.255.0 | 192.168.10.1 |
| SERVIDOR-WEB | 193.10.10.1 | 255.255.255.0 | 193.10.10.254 |

**5. Servidor Web**: SERVIDOR-WEB → Services → HTTP: deixe HTTP **On** e (opcional) edite o
`index.html`.

**6. Teste**: aguarde ~30 s (RIP e STP convergirem), abra o Web Browser de um PC e acesse
`http://193.10.10.1`. No R-EMPRESA, `show ip route` deve mostrar
`R 193.10.10.0/24 [120/1] via 201.1.1.1`, e `show ip nat translations` mostra as conexões.

Depois disso, salve: o arquivo ficará no formato da sua versão (8.2.2).
