# Bypassing de Validação de Firmware via Bloqueio NTP e Telemetria

Este guia demonstra como explorar uma falha de validação temporal em firmwares específicos de receptores para manter o acesso aos serviços On Demand de forma gratuita.

O método baseia-se em impedir que o dispositivo atualize sua data interna após um reset de fábrica, mantendo-o permanentemente no ano padrão de fabricação (2016), período no qual a licença do firmware é considerada válida pelo sistema.

## Mecanismo de Funcionamento
O Bug: O firmware libera os serviços On Demand se a data do aparelho for anterior à data de criação da firmware atual.

O Reset: Ao restaurar o aparelho para os padrões de fábrica, o relógio interno retrocede para o ano de 2016.

O Problema: Ao conectar o aparelho à internet, ele sincroniza automaticamente com servidores NTP, atualiza a data e bloqueia os serviços, exibindo a tela de QR Code.

A Solução: Bloquear as requisições de sincronização de horário (Porta 123) e os servidores de auditoria do fabricante (Porta 5002) diretamente no roteador/modem da rede.

## Configuração do Firewall (iptables)
Para aplicar o bloqueio, acesse o terminal (SSH/Telnet) do seu modem ou roteador e execute os comandos abaixo. Substitua o IP_DO_SEU_CINEBOX pelo endereço IP estático ou reservado do seu receptor.

## 1. Bloqueio do Protocolo NTP (Relógio)
Impede que o dispositivo consulte a hora certa na internet e descubra que o firmware expirou.

Bash
iptables -I FORWARD -s IP_DO_SEU_CINEBOX -p udp --dport 123 -j DROP
iptables -I FORWARD -s IP_DO_SEU_CINEBOX -p tcp --dport 123 -j DROP

## 2. Bloqueio de Telemetria e Auditoria
Bloqueia as requisições constantes que o aparelho envia para os servidores de validação do fabricante.

Bash
iptables -I FORWARD -s IP_DO_SEU_CINEBOX -p udp --dport 5002 -j DROP
iptables -I FORWARD -s IP_DO_SEU_CINEBOX -p tcp --dport 5002 -j DROP

## Verificação das Regras
Após inserir os comandos no modem, você pode certificar-se de que o tráfego está sendo devidamente interceptado rodando o seguinte comando:

Bash
iptables -L -n -v | grep IP_DO_SEU_CINEBOX
Certifique-se de que as regras apareçam na lista com o status de DROP para o IP do seu aparelho.

## Passo a Passo para Ativação no Receptor
Com as regras de firewall ativas e rodando no seu roteador, siga o procedimento abaixo no aparelho receptor:

Reset de Fábrica: No menu do aparelho, realize a restauração para os padrões de fábrica.

Confirmação: Garanta que, após reiniciar, a data exibida no sistema do aparelho esteja travada no ano de 2016.

Conexão de Rede: Conecte o aparelho à sua rede local (via Wi-Fi ou cabo de rede) mantendo o DHCP ativado.

Pronto: O aparelho terá acesso à internet para carregar as mídias, mas não conseguirá atualizar o relógio nem validar a telemetria, mantendo o serviço On Demand liberado.

Obs: Não é possível utilizar o serviço de satélite pois o mesmo atualiza a data e horário do equipamento.
