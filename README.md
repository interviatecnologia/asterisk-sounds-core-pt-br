Instalar os sons pt-br no Asterisk

Para instalar os sons você tem que se conectar ao servidor Asterisk através de um cliente SSH com o usuário root.

Uma vez dentro do servidor, criar o diretório de destino para os arquivos de áudio.

mkdir /var/lib/asterisk/sounds/pt-br

Baixar o pacote core e extra

cd /var/lib/asterisk/sounds/pt-br

chown -R asterisk.asterisk /var/lib/asterisk/sounds/pt-br

find /var/lib/asterisk/sounds/pt-br -type d -exec chmod 0775 {} \;
ou chmod 0775 /var/lib/asterisk/sounds/pt-br

Agora você vai ter que configurar a nova língua no ramal que deseje ou no contexto geral do protocolo (SIP, IAX2, etc.) no qual utilize os novos sons.



Converter arquivos de som para outros formatos

cd /var/lib/asterisk/sounds/pt-br

vi convert

#!/bin/bash

for a in $(find . -name '*.sln16'); do

  sox -t raw -e signed-integer -b 16 -c 1 -r 16k $a -t gsm -r 8k `echo $a|sed "s/.sln16/.gsm/"`;\
  
  sox -t raw -e signed-integer -b 16 -c 1 -r 16k $a -t raw -r 8k -e a-law `echo $a|sed "s/.sln16/.alaw/"`;\
  
  sox -t raw -e signed-integer -b 16 -c 1 -r 16k $a -t raw -r 8k -e mu-law `echo $a|sed "s/.sln16/.ulaw/"`;\
  
done

chmod +x convert

############################

./convert

Configuração 
Para ativar os sons utilizando FreePBX, basta alterar o parâmetro da variável language dentro de Asterisk SIP Settings. Neste caso, colocando es. Se existirem ramais ou linhas IAX2, será necessário configurar no parâmetro language acessando o menu Asterisk IAX Settings.

#language=pt-br
