# Usando e desenvolvendo com Docker

## Índice:

* [Entendendo o que é Docker](https://github.com/mahnetti/como-usar-docker/edit/main/README.md#entendendo-o-que-%C3%A9-o-docker)
* [Instalando Docker Engine](https://github.com/mahnetti/como-usar-docker/edit/main/README.md#instalando)
* [Desinstalando](https://github.com/mahnetti/como-usar-docker/edit/main/README.md#desinstalando)
* [Referências](https://github.com/mahnetti/como-usar-docker/edit/main/README.md#refer%C3%AAncias)


  ## Entendendo o que é Docker
  
De forma direta o *Docker Engine* é uma tecnologia de código aberto, que permite criar independência e a execução de múltiplas aplicações e processos de forma separada.
Imagina uma pessoa desenvolvedora usando o *Mac* e outra uma distro *'X' de Linux*, isso poderia causar diversos problemas, certo? Esse tecnologia garante a funcionalidade em ambas máquinas e a funcionabilidade dos servidores no processo de deploy.

__MAS COMO ISSO?__

Por mais que se assemelhe com uma máquina virtual(VM), não é. Ele usa um sistema de containers e imagens, não necessita de múltiplos sistemas operacionais(OS) ele usa um único sistema operacional para todas as aplicações. Caso queira saber mais sobre máquinas virtuais, acesse <a href="https://azure.microsoft.com/pt-br/overview/what-is-a-virtual-machine/#overview">aqui</a>.

O __container__ é um ambiente isolado que usa os recursos do mesmo sistema operacional do hospedeiro e é executado à partir de uma imagem (o container roda a imagem).

A __imagem__ é um modelo apenas para leitura para subir no container e pode ser utilizada em vários containers, são configurações únicas que sua aplicação precisa. Para achar essas imagens temos o principal repositório <a href='https://hub.docker.com/'>Docker Hub</a>.

Resumindo esse software fornece containers virtuais que empacotam sua aplicação e suas dependências, à partir desse momento o container se torna portável para rodar em qualquer máquina (que também tenha *Docker*), dessa forma sua aplicação vai se portar sempre da mesma forma.


Aqui vai um esquema para você entender melhor:

![image](https://user-images.githubusercontent.com/85520887/168180989-646fe888-3ee8-49b5-be13-55f525fc8837.png)
(fonte: https://www.weave.works/blog/a-practical-guide-to-choosing-between-docker-containers-and-vms )

O *Docker* acaba sendo um recurso interessante por:
* inicializar mais rápido
* utilizar menos memória e armazenamento
* maior disponibilidade do sistema
* facilidade do sistema
* padronização e replicação
* acesso á comunidade
* entre outros...


*Nossa!!!! Que coisa maravilhosa, já quero... como que começo a usar?*

## Instalando 
Estou usando como base o *Ubuntu LTS - 64-bit*. Para outros sistemas operacionais(OS) acesse a <a href="https://docs.docker.com/engine/install/">documentação oficial</a> e se você está no *Ubuntu* e ocorra algo que não deveria, não se preocupe ~~(confia)~~, <a href="https://docs.docker.com/engine/install/ubuntu/">essa documentação(também oficial)</a> te salva!

#### Pré-requisitos
Para a instalação do *Docker Enginer* você precisa da versão de algum desses *Ubuntu*:
* *Ubuntu Jammy 22.04 (LTS)*
* *Ubuntu Impish 21.10*
* *Ubuntu Focal 20.04 (LTS)*
* *Ubuntu Bionic 18.04 (LTS)*

*Docker Enginer* é suportado pelas seguintes arquiteturas: *x86_64 (ou amd64), armhf, arm64 e s390x*

1. Primeiro passo é o de sempre!! Bora de ```crlt + alt + t``` para abrir o terminal.

2. Agora, caso você já use o *Docker* e esteja com uma versão antiga ou com problemas vamos precisar desintalá-lo.

Rode o seguinte comando: 
~~~
sudo apt-get remove docker docker-engine docker.io containerd runc
~~~

Se você não possui nenhum pacote instalado vai aparecer uma mensagem de erro `E: Impossível encontrar o <nome-do-pacote>`, sendo assim só seguir os passos.


*Obs:* O Docker *mantém as informações sobre imagens, conteiners, volumes e redes na pasta `/var/lib/docker/`, nesse passo esses arquivos permanecem intactos.*

*Caso queira saber como desinstalar completamente, desce logo [ali](https://github.com/mahnetti/como-usar-docker/edit/main/README.md#desinstalando) que te explico!*

3. Ainda no terminal vamos rodar:
~~~
sudo apt-get update
~~~
*Os pacotes apt são atualizados dessa forma!*


4. Vamos instalar alguns pacotes necessários que permitem o apt usar pacotes pelo HTTPS:

  ~~~
    sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
   ~~~
5. Então, adicionamos a chave GPG para o repositório oficial do *Docker* no seu sistema:

~~~
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
~~~

*Se tudo ocorrer conforme o plano, não haverá nenhum retorno visual.*

6. Agora temos que adicionar o repositório *(oficial)* ás fontes do apt:

~~~
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
~~~

Foi adicionado o repositório ```stable``` (no ```$(lsb_release -cs) stable``` ) para termos apenas as versões estáveis do *Docker*

7. Vamos garantir que os pacotes estão atualizados...
~~~
sudo apt-get update
~~~

*Tudo certo? Então bora pro próximo passo.*

8.Instalando o *Docker Engine*:
 ~~~
 sudo apt-get install docker-ce docker-ce-cli containerd.io
 ~~~
 
9.Adicionando permissão de usuário ao *Docker*(sem o uso de `sudo`):

De forma padrão, todo comando que formos rodar vai ser exigido o inicio como `root`
 ou `sudo` para qualquer usuário. Para evitarmos o uso do `sudo` nos comandos, temos que dar as permissões necessárias.
 
 Faremos da seguinte forma, criamos um grupo:
 ~~~
 sudo groupadd docker
 ~~~
 
 *Se aparecer a mensagem: `groupadd: grupo 'docker' já existe` é só seguir pro passo 10.*
 
 Agora vamos adicionar o usuário ao grupo.
 
 ~~~
 sudo usermod -aG docker $USER
 ~~~
 
 Temos que ativar as alterações realizadas, pode ser feita fazendo logout e login na sessão ou rodando o comando:
 
 ~~~
 newgrp docker
 ~~~
 
10. Agora temos que iniciar o *Daemon*

Vamos consultar o estado que o *daemon* está:
~~~
sudo systemctl status docker
~~~
Deve ser retornado algo similar:
~~~
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Thu 2021-09-23 11:55:11 -03; 2s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
    Process: 2034 ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock (code=exited, status=0>
   Main PID: 2034 (code=exited, status=0/SUCCESS
~~~

Preste atenção do paramêtro `Active`, se estiver como *stop/waiting* ou *inactive*, devemos inicia-lo:

~~~
sudo systemctl start docker
~~~
Agora o status deve aparecer como *start/running/active*.
Então vamos habilitar o *daemon* de forma que seja iniciado durante o boot:
~~~
sudo systemctl enable docker
~~~

11.Por último ~~(UFA!)~~ vamos validar a instalação;

Pra isso vamos executar um *hello world*
~~~
docker run hello-world
~~~
Esse comando faz um pedido ao repositório de uma imagem chamada *hello world*, é um exemplo simples de container.

### Se agora você quer saber como usar o *Docker* e seus comandos corre aqui(inserir link)

## Desinstalando

Bom pra desinstalar o processo é bem mais simples;

Para remover totalmente o motor do *Docker* de sua máquina, só usar:
~~~
sudo apt-get purge docker-ce docker-ce-cli containerd.io
~~~

Agora se você quiser remover containers, volumes e configurações personalizadas(lembra que vimos que esses não são excluidos automaticamente?Então...), os comando são esses:

~~~
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
~~~

#### Referências:
* https://www.globalmind.com.br/vantagens-da-utilizacao-do-docker-container/
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-pt
* https://www.hostinger.com.br/tutoriais/install-docker-ubuntu
* https://www.youtube.com/watch?v=Kzcz-EVKBEQ
* https://docs.docker.com/engine/install/ubuntu/
* Documentação das aulas da Trybe
