# Usando e desenvolvendo com Docker

  ## Entendendo o que é o Docker
  
De forma direta o Docker é uma tecnologia de código aberto, que permite criar independência que permite a execução de múltiplas aplicações e processos de forma separada.
Imagina uma pessoa desenvolvedora usando o Mac e outra uma distro 'X' de Linux, isso poderia causar diversos problemas, certo? Esse tecnologia garante a funcionalidade em ambas máquinas e garantir a funcionabilidade dos servidores no processo de deploy.

__MAS COMO ISSO?__

Por mais que se assemelhe com uma máquina virtual(VM), não é. Ele usa um sistema de containers e imagens, não necessita de múltiplos sistemas operacionais convidados(OS) sendo o sistema operacional usado para todas as aplicações.

![image](https://user-images.githubusercontent.com/85520887/168180989-646fe888-3ee8-49b5-be13-55f525fc8837.png)
(fonte: https://www.weave.works/blog/a-practical-guide-to-choosing-between-docker-containers-and-vms )

O __container__ é um ambiente isolado que usa os recursos do mesmo sistema operacional do hospedeiro e é executado à partir de uma imagem (o container roda a imagem).

A __imagem__ é um modelo apenas para leitura para subir no container, para achar essa imagens temos o o principal repositório <a href='https://hub.docker.com/'>Docker Hub</a>

