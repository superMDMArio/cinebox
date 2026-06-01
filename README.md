# cinebox
Documentação técnica, pesquisas e alternativas para receptores Cinebox após as mudanças de 2025. Inclui informações sobre downgrade de firmware, recuperação de funcionalidades removidas, atualização via USB, análise de firmware e preservação do controle do equipamento pelos seus proprietários.

[01-APRESENTAÇÃO](01-APRESENTAÇÃO.md).

[02-COMEÇO](02-COMEÇO.md).

[03-ANALISE](03-ANALISE.md).

[04-DOWNGRADE](04-DOWNGRADE.md).

[05-QRCODE](05-QRCODE.md).

# 01-APRESENTAÇÃO
### Cinebox

Este repositório foi criado para documentar informações técnicas, pesquisas, métodos de análise e alternativas relacionadas aos receptores Cinebox após as alterações implementadas no final de 2025.

As atualizações mais recentes removeram funcionalidades que sempre fizeram parte da experiência do usuário, como a atualização de firmware via USB e a seleção manual de serviços disponíveis no equipamento. Em seu lugar, foi introduzido um sistema de validação por código vinculado a um QR Code permanente na tela.

O objetivo deste projeto é reunir conhecimento, preservar informações técnicas, compartilhar descobertas da comunidade e registrar alternativas que permitam aos proprietários desses equipamentos manter o controle sobre dispositivos que adquiriram legitimamente.

Neste repositório também serão apresentados métodos para restaurar funcionalidades removidas pelas atualizações recentes, incluindo procedimentos de downgrade para versões anteriores de firmware que ainda permitem atualizações locais via USB. Serão documentadas as técnicas utilizadas, os requisitos necessários e as limitações conhecidas de cada método.

Além disso, serão abordadas alternativas para utilização das versões mais recentes do firmware, permitindo que os usuários avaliem diferentes opções de configuração e recuperação do equipamento de acordo com suas necessidades e possibilidades técnicas.

Este projeto tem caráter educacional e de pesquisa, com foco na documentação, transparência, preservação de funcionalidades e liberdade de escolha dos usuários sobre equipamentos que adquiriram legalmente.

# 02-COMEÇO
### Informações Básicas

Antes de iniciar qualquer procedimento, existem algumas informações úteis que podem ser obtidas diretamente do receptor através da interface web interna.

### Descobrir a senha de proteção do menu

A senha de proteção do menu pode ser consultada acessando a seguinte URL no navegador:
```url
http://IP_DO_SEU_CINEBOX:9981/check_access
```
O retorno da página exibirá informações relacionadas ao controle de acesso do equipamento.
```
{
 "result": 0,
 "message": "Connected!",
 "password": 0000
}
```
### Listar os serviços OnDemand conectados ao equipamento

Para visualizar os serviços e servidores atualmente utilizados pelo receptor, execute o seguinte comando em um terminal:
```terminal
curl http://IP_DO_SEU_CINEBOX:9981/check_main
```
O resultado contém diversas informações de diagnóstico e pode revelar endereços IP e serviços associados ao funcionamento do equipamento.

Importante: essas informações podem ser úteis para análise da comunicação de rede do receptor e para identificar servidores utilizados durante processos de atualização e recuperação de firmware.

# 03-ANALISE
### Extraindo uma firmware Cinebox com Binwalk

Partindo do princípio que você já possui o Binwalk instalado.

Abra o Terminal do macOS e entre na pasta onde costuma trabalhar com o Binwalk:
```terminal
cd binwalk
```
Agora execute a extração da firmware:
```terminal
binwalk -e 20260501_cinebox_p_normal.abs
```
Você não precisa digitar o caminho completo manualmente. Basta escrever:
```
binwalk -e
```
e arrastar o arquivo .abs para dentro da janela do Terminal. O macOS preencherá o caminho automaticamente.

Após alguns segundos o Binwalk irá extrair o conteúdo da firmware. O resultado normalmente ficará dentro da pasta:
```
binwalk/extractions
```
Lá você encontrará diversos arquivos extraídos, incluindo alguns chamados:
```
decompressed.bin
```
Escolha o maior deles e abra no seu editor hexadecimal favorito.

A partir daí começa a parte interessante.

Utilize a função de busca do editor para procurar por strings que possam revelar informações úteis. Algumas buscas que costumam trazer resultados interessantes:
```
http
.txt
xml
iptv
server
host
login
password
```
Mas a busca mais importante é:
```
UpdateList.txt
```
Respeite exatamente as letras maiúsculas e minúsculas.

Dependendo da firmware, você poderá encontrar URLs, servidores, listas de atualização, referências a serviços online, arquivos de configuração e outras informações que ajudam a entender como o equipamento funciona.

Daqui para frente é basicamente exploração e curiosidade. Quanto mais você procurar por palavras-chave diferentes, maiores as chances de encontrar algo interessante escondido dentro da firmware.

Aqui começa a diversão.

# 04-DOWNGRADE
