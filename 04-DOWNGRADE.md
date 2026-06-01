# Guia de Interceptação de Atualizações via Servidor Local

Este procedimento demonstra como redirecionar as requisições de atualização de um receptor para um servidor HTTP hospedado localmente em um Mac, permitindo fornecer arquivos personalizados de firmware e listas de canais.

## 1. Identificando a Interface de Rede Correta no macOS (SE VC USA OUTRO SISTEMA PROCURE)

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

## 2. Criando a Estrutura de Diretórios (NESSE CASO PARA UM FANTASIA PLUS)

Crie a estrutura de pastas esperada pelo equipamento:
```terminal
mkdir -p ~/Sites/AllNewVOD/UPDATE_PLUS/Fantasia
```
## 3. Criando o Arquivo de Atualização

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
## 4. Iniciando o Servidor HTTP

Acesse o diretório raiz:
```terminal
cd ~/Sites
```
Inicie um servidor HTTP simples na porta 80:
```terminal
sudo python3 -m http.server 80
```
Mantenha esta janela aberta durante todo o processo.

## 5. Clonando o Endereço IP do Servidor Remoto

Abra uma nova aba do Terminal e adicione um endereço IP secundário à interface de rede:
```terminal
sudo ifconfig en0 alias IP_QUE_VAI_ENCONTRAR_NO_BINWALK netmask 255.255.255.255
```
Substitua en0 pela interface identificada anteriormente, caso seja diferente.

Após esse comando, o Mac responderá localmente pelas requisições destinadas ao endereço IP especificado.

## 6. Configurando a Rede do Receptor

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

## 7. Executando a Atualização

Com o servidor ativo e a rede configurada:

Acesse o menu de atualização do receptor.
Solicite a leitura da lista de atualizações.
Verifique no terminal se as requisições HTTP estão sendo recebidas.
Se tudo estiver correto, o equipamento deverá baixar os arquivos disponibilizados pelo servidor local. (PODE DEMORAR UM POUCO)

## 8. Limpeza e Restauração

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

## Observações
O IP utilizado no exemplo (IP_QUE_VAI_ENCONTRAR_NO_BINWALK) pode variar conforme a versão do equipamento e os servidores consultados.
O servidor Python integrado ao macOS é suficiente para testes e desenvolvimento.
Sempre mantenha cópias de segurança dos firmwares originais antes de qualquer procedimento.
Verifique a integridade dos arquivos utilizando os hashes MD5 informados pelo equipamento.
