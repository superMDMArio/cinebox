# Extraindo uma firmware Cinebox com Binwalk

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

binwalk -e

e arrastar o arquivo .abs para dentro da janela do Terminal. O macOS preencherá o caminho automaticamente.

Após alguns segundos o Binwalk irá extrair o conteúdo da firmware. O resultado normalmente ficará dentro da pasta:

binwalk/extractions

Lá você encontrará diversos arquivos extraídos, incluindo alguns chamados:

decompressed.bin

Escolha o maior deles e abra no seu editor hexadecimal favorito.

A partir daí começa a parte interessante.

Utilize a função de busca do editor para procurar por strings que possam revelar informações úteis. Algumas buscas que costumam trazer resultados interessantes:

http
.txt
xml
iptv
server
host
login
password

Mas a busca mais importante é:

UpdateList.txt

Respeite exatamente as letras maiúsculas e minúsculas.

Dependendo da firmware, você poderá encontrar URLs, servidores, listas de atualização, referências a serviços online, arquivos de configuração e outras informações que ajudam a entender como o equipamento funciona.

Daqui para frente é basicamente exploração e curiosidade. Quanto mais você procurar por palavras-chave diferentes, maiores as chances de encontrar algo interessante escondido dentro da firmware.

Aqui começa a diversão.
