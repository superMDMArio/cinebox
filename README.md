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
### Guia de Interceptação de Atualizações via Servidor Local

Este procedimento demonstra como redirecionar as requisições de atualização de um receptor para um servidor HTTP hospedado localmente em um Mac, permitindo fornecer arquivos personalizados de firmware e listas de canais.

### 1. Identificando a Interface de Rede Correta no macOS (SE VC USA OUTRO SISTEMA PROCURE)

Antes de iniciar, é necessário descobrir qual interface de rede está conectada à mesma rede do receptor.

Abra o Terminal e execute:
```terminal
networksetup -listallhardwareports
```
O sistema exibirá todas as interfaces disponíveis. Localize a seção correspondente à conexão em uso (Wi-Fi ou Ethernet).

Exemplo:
```
Hardware Port: Wi-Fi
Device: en0
```
Neste caso, a interface utilizada será en0.

Para descobrir o endereço IP atual dessa interface:
```terminal
ipconfig getifaddr en0
```
Substitua en0 pela interface identificada anteriormente.

Exemplo de resultado:
```
192.168.15.10 (ESSE SERÁ O IP DO SEU COMPUTADOR)
```
Esse endereço será utilizado posteriormente como gateway do receptor.

### 2. Criando a Estrutura de Diretórios (NESSE CASO PARA UM FANTASIA PLUS)

Crie a estrutura de pastas esperada pelo equipamento:
```terminal
mkdir -p ~/Sites/AllNewVOD/UPDATE_PLUS/Fantasia
```
### 3. Criando o Arquivo de Atualização

Crie o arquivo:
```terminal
~/Sites/AllNewVOD/UPDATE_PLUS/Fantasia/UpdateList.txt
```
Conteúdo de exemplo: (USAR BINWALK NA SUA FIRMWARE PARA DESCOBRIR O SEU CAMINHO)
```
W_TYPE:1
W_TITLE:FIRMWARE V1.2(24.05.23)
W_URL:http://IP_QUE_VAI_ENCONTRAR_NO_BINWALK/AllNewVOD/UPDATE_PLUS/Fantasia/20240523_1_fantasia_plus.abs
W_IMG:http://IP_QUE_VAI_ENCONTRAR_NO_BINWALK/AllNewVOD/UPDATE_PLUS/Fantasia/cinebox_plus.jpg
W_MD5:b95b41ba1ec4fda84edb57df58f877b1
```
Coloque também o arquivo de firmware correspondente na mesma pasta: (CADA FIRMWARE TEM SEU MD5, VAI PRECISAR DESCOBRIR)
```
~/Sites/AllNewVOD/UPDATE_PLUS/Fantasia/
```
Estrutura final:
```
Sites/
└── AllNewVOD/
    └── UPDATE_PLUS/
        └── Fantasia/
            ├── UpdateList.txt
            ├── 20240523_1_fantasia_plus.abs
```
### 4. Iniciando o Servidor HTTP

Acesse o diretório raiz:
```terminal
cd ~/Sites
```
Inicie um servidor HTTP simples na porta 80:
```terminal
sudo python3 -m http.server 80
```
Mantenha esta janela aberta durante todo o processo.

### 5. Clonando o Endereço IP do Servidor Remoto

Abra uma nova aba do Terminal e adicione um endereço IP secundário à interface de rede:
```terminal
sudo ifconfig en0 alias IP_QUE_VAI_ENCONTRAR_NO_BINWALK netmask 255.255.255.255
```
Substitua en0 pela interface identificada anteriormente, caso seja diferente.

Após esse comando, o Mac responderá localmente pelas requisições destinadas ao endereço IP especificado.

### 6. Configurando a Rede do Receptor

Configure manualmente os parâmetros de rede do aparelho cinebox:
```
Configuração	Valor
IP	192.168.15.2
Máscara	255.255.255.0
Gateway	IP do Mac
DNS	8.8.8.8
```
Exemplo:
```
IP:       192.168.15.2
Máscara:  255.255.255.0
Gateway:  192.168.15.10
DNS:      8.8.8.8
```
O gateway deve ser o endereço IP real do Mac obtido anteriormente.

### 7. Executando a Atualização

Com o servidor ativo e a rede configurada:

Acesse o menu de atualização do receptor.
Solicite a leitura da lista de atualizações.
Verifique no terminal se as requisições HTTP estão sendo recebidas.
Se tudo estiver correto, o equipamento deverá baixar os arquivos disponibilizados pelo servidor local. (PODE DEMORAR UM POUCO)

### 8. Limpeza e Restauração

Após concluir o procedimento:

Pare o servidor HTTP:
```terminal
Ctrl + C
```
Remova o endereço IP secundário:
```terminal
sudo ifconfig en0 -alias IP_QUE_VAI_ENCONTRAR_NO_BINWALK
```
Retorne a configuração de rede do receptor para:
```
DHCP / Automático
```
Isso restaura o funcionamento normal da rede.

### Observações
O IP utilizado no exemplo (IP_QUE_VAI_ENCONTRAR_NO_BINWALK) pode variar conforme a versão do equipamento e os servidores consultados.
O servidor Python integrado ao macOS é suficiente para testes e desenvolvimento.
Sempre mantenha cópias de segurança dos firmwares originais antes de qualquer procedimento.
Verifique a integridade dos arquivos utilizando os hashes MD5 informados pelo equipamento.

# 05-QRCODE
