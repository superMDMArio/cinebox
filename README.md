# cinebox
Documentação técnica, pesquisas e alternativas para receptores Cinebox após as mudanças de 2025. Inclui informações sobre downgrade de firmware, recuperação de funcionalidades removidas, atualização via USB, análise de firmware e preservação do controle do equipamento pelos seus proprietários.

Cinebox

Este repositório foi criado para documentar informações técnicas, pesquisas, métodos de análise e alternativas relacionadas aos receptores Cinebox após as alterações implementadas no final de 2025.

As atualizações mais recentes removeram funcionalidades que sempre fizeram parte da experiência do usuário, como a atualização de firmware via USB e a seleção manual de serviços disponíveis no equipamento. Em seu lugar, foi introduzido um sistema de validação por código vinculado a um QR Code permanente na tela.

O objetivo deste projeto é reunir conhecimento, preservar informações técnicas, compartilhar descobertas da comunidade e registrar alternativas que permitam aos proprietários desses equipamentos manter o controle sobre dispositivos que adquiriram legitimamente.

Neste repositório também serão apresentados métodos para restaurar funcionalidades removidas pelas atualizações recentes, incluindo procedimentos de downgrade para versões anteriores de firmware que ainda permitem atualizações locais via USB. Serão documentadas as técnicas utilizadas, os requisitos necessários e as limitações conhecidas de cada método.

Além disso, serão abordadas alternativas para utilização das versões mais recentes do firmware, permitindo que os usuários avaliem diferentes opções de configuração e recuperação do equipamento de acordo com suas necessidades e possibilidades técnicas.

Este projeto tem caráter educacional e de pesquisa, com foco na documentação, transparência, preservação de funcionalidades e liberdade de escolha dos usuários sobre equipamentos que adquiriram legalmente.

Informações Básicas

Antes de iniciar qualquer procedimento, existem algumas informações úteis que podem ser obtidas diretamente do receptor através da interface web interna.

Descobrir a senha de proteção do menu

A senha de proteção do menu pode ser consultada acessando a seguinte URL no navegador:

http://IP_DO_SEU_CINEBOX:9981/check_access

O retorno da página exibirá informações relacionadas ao controle de acesso do equipamento.

Listar os serviços OnDemand conectados ao equipamento

Para visualizar os serviços e servidores atualmente utilizados pelo receptor, execute o seguinte comando em um terminal:

curl http://IP_DO_SEU_CINEBOX:9981/check_main

O resultado contém diversas informações de diagnóstico e pode revelar endereços IP e serviços associados ao funcionamento do equipamento.

Importante: essas informações podem ser úteis para análise da comunicação de rede do receptor e para identificar servidores utilizados durante processos de atualização e recuperação de firmware.

