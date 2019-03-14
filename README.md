# SoapUI Jenkins

Este tutorial trará como pode ser utilizada a ferramenta Jenkins para promover a execução automatizada dos testes criados na ferramenta SoapUI.


# Pré-requisitos

Para execução deste projeto você deverá ter em sua máquina:

 - Jenkins (war ou instalado).
 - Um projeto de testes SoapUI com testes criados e xml gerado.

## TestRunner

Primeiramente encontre o xml do projeto de testes SoapUI e importe o mesmo na ferramenta.

![xml do projeto](https://i.imgur.com/klFl3nk.png)

Após este processo, encontre alguma suíte de testes ou caso de teste e clique com o botão direito do mouse na opção "Launch TestRunner":

![Launch TestRunner menu](https://i.imgur.com/Y3F2wIs.png)

O recurso TestRunner é o responsável da execução automática dos testes criados. Escolha a opção "Launch" e em seguida ao iniciar a execução, clique em "Cancel" e mantenha a tela aberta:

![Launch botão clicar](https://i.imgur.com/E79kNzH.png)

![Clicar em cancel](https://i.imgur.com/zDUs2G9.png)

Perceba que ao iniciar a execução  dos testes existem dois comandos na tela que fazem a execução dos testes.  
O comando `C:\Program Files (x86)\SmartBear\SoapUI-5.5.0\bin\` demonstra onde está instalada a nossa ferramenta SoapUI. Esse caminho será utilizado pelo Jenkins para encontrar os binários de execução.
O próximo comando `command: cmd.exe /C testrunner.bat -s"Configuracao Concurso" -cBuscarConcurso C:\API-s-Projeto-Stefanini-readyapi-project.xml` é similar á ferramentas como Nunit, Junit, Xunit, no qual é utilizado um programa/bath para execução dos testes juntamente com argumentos e o binário do projeto de automação de testes que será executado, exemplo: 

    testrunner.bat -s"SuiteDeTeste" -c"CasoDeTeste" xml_que_sera_executado

**As tags -s (suite de testes que quero executar) e a tag -c (caso de teste que quero executar) não são obrigatórias!** 

Agora que já encontramos a ferramenta que executará os testes, podemos testar via prompt de comando a execução. Basta acessar a pasta do binário do TestRunner e executar o seu xml: `testrunner.bat -s"Configuracao Concurso" -cBuscarConcurso C:\API-s-Projeto-Stefanini-readyapi-project.xml`

![enter image description here](https://i.imgur.com/ge1YpEz.png)

Na imagem abaixo podemos verificar a execução do teste "BuscarConcurso". O qual apresentou:
 1. Retorno 200 feito com sucesso.
 2. Verificação de retorno específico.
 3. Verificação de retorno com sucesso.
 4. Informação que a execução foi finalizada.

![enter image description here](https://i.imgur.com/gQYUAwb.png)

Feito tal processo, agora é migrar a execução para o Jenkins conforme próximo passo.


## Instalação Jenkins

Para instalar e executar o Jenkins local, baixe o [Jenkins War no site oficial](http://mirrors.jenkins.io/war-stable/latest/jenkins.war).
Após baixar, abra um prompt de comando em modo administrador no local do download e execute o comando: `java -jar jenkins.war --httpPort=9999`. No seu navegador preferido, acesse `localhost:9999` e faça as configurações inicias de usuário, pacotes e serviços desejados.


## Jenkins + SoapUI

Com o Jenkins instalado e o SoapUI também, vamos realizar a integração destes recursos com os seguintes passos:

Acessar o Jenkins e criar um **Novo Job**:
![enter image description here](https://i.imgur.com/F9P0frX.png)


Criar um job com o nome que deseja e do tipo **"Construir um projeto de software free-style"**:

![enter image description here](https://i.imgur.com/jKqC2hJ.png)

O nosso job irá executar semelhante ao prompt de comando, portanto selecione a opção para o item **Build > Executar no comando do Windows**

![enter image description here](https://i.imgur.com/Vq3QKyC.png)

O próximo passo será inserir o script que havíamos utilizado no prompt. Devemos acessar o diretório do binário do SoapUI e posteriormente executar os testes:

    cd C:\Program Files (x86)\SmartBear\SoapUI-5.5.0\bin

    testrunner.bat -s"Configuracao Concurso" -cBuscarConcurso C:\API-s-Projeto-Stefanini-readyapi-project.xml

![enter image description here](https://i.imgur.com/jbsSQ9v.png)

Agora basta salvar e construir o projeto! 

![enter image description here](https://i.imgur.com/4wr1i1p.png)

No console da execução dos testes automatizados é possível verificar o status. 
Lembrando que se algum teste falhar, ao finalizar o job, o status do mesmo será FAILURE e no caso de todos os testes com sucesso, será SUCCESS.

![enter image description here](https://i.imgur.com/DxBBfgQ.png)

## Próximas implementações nesta doc

Este material será complementado com os seguintes tópicos:

 - Mudança de environment automática via ansible ou powershell.
 - Configurar trigger de push no repositório e uma nova execução se iniciará (integração contínua).
 - Uso de datadriven.
 - Configurações de Events (SetUp e TearDown).


**~~Guarde~~ Compartilhe ideias (=**
