# Docker for Beginners

![alt tag](https://github.com/bugfactory/posts/blob/master/docker-for-beginners/imgs/docker.png)

Hoje em dia muito se fala em docker no mundo nerd, de fato essa é uma das tecnologias que estão em alta no mercado, todos os dias novas empresas estão aderindo ao docker. Esta ferramenta se tornou um conhecimento quase obrigatório para desenvolvedores e systems adminstrators.

Pensando nisso, resolvemos criar uma sequência de posts para tentar cobrir esse assunto da melhor forma possível. Nesse primeiro post tentaremos tirar a maioria das dúvidas, apresentando os principais conceitos, como instalar e administrar um ambiente docker.

Espero que vocês aproveitem o conteúdo deste post e em caso de dúvidas, sugestões ou reclamações você será ignorado, hehehe brincadeira, deixe um comentário que tentaremos responder o mais rápido possível.

## Um pouco da história

Docker começou a ser desenvolvido por Solomon Hykes e outros engenheiros da empresa dotCloud. Inicialmente ele era apenas um projeto interno para manipular Linux Containers (LXC), até os desenvolvedores perceberem o poder que eles tinham em mãos e tornar o projeto publico. Em março de 2013 a primeira release foi liberada. Mais tarde a empresa veio mudar o nome para Docker.

Eh muito comum artigos sobre docker fazerem associacao com outro conceito, Linux Container (LXC). Caso você não saiba, LXC é um maneira de virtualização a nível do sistema operacional e é utilizado para executar vários sistemas Linux isoladamente, para isso ele faz uso de features do kernel tais como cgroups e namespaces. Esses ambientes isolados são os conhecidos containers. O cgroups é uma feature do kernel que possibilita o compartilhamento de recursos tais como: CPU, memória do sistema e rede. O namespace cria um isolamento para os containers, em outras palavras ele  garante que um container não pode ver ou afetar outro.

O conceito de cgroups e namespaces é muito mais complexo do que foi citado acima. Porém, o nosso propósito não é entrar afundo nesses conceitos, mas sim, mostrar a ferramenta docker, que usa ambas features. Caso você tenha curiosidade de conhecer mais sobre essas features, uma rápida pesquisa na internet te trará ótimos papers.
Desde a versão 0.9 o docker utiliza a biblioteca libcontainer para fazer acesso as features de virtualização do kernel, porem ele também é compatível com a libvirt, LXC ou systemd-nspwan.

->![alt tag](https://github.com/bugfactory/posts/blob/master/docker-for-beginners/imgs/lxc-docker.png)<-

A figura acima deixa claro que o Docker consegue utilizar as features de virtualização do kernel utilizando diferentes interfaces.

## Docker Container

Todo Docker Container é criado a partir de uma Imagem, e o seu ciclo de vida consiste na execução de um processo, ou seja, enquanto esse processo estiver em execução o container também estará. Quando esse processo for finalizado o container também será. 

Um container pode executar mais de um processo, porém isso não é considerado uma boa prática. Esse conceito é importante para entender como “Dockerizar” nossa aplicação. 

“In almost all cases, you should only run a single process in a single container. Decoupling applications into multiple containers makes it much easier to scale horizontally and reuse containers. If that service depends on another service, make use of container linking.” (https://docs.docker.com/articles/dockerfile_best-practices/)

## Docker Image

São templates “somente-leitura”, usados para criar os containers. Um exemplo de imagem seria o sistema operacional ubuntu com um servidor web instalado. A imagem é caracterizada “somente-leitura” porque a partir do momento que você a altera, esta se torna outra imagem.  Esse conceito ficará mais claro quando começarmos a utilizar o docker em linha de comando.

## Docker Registry and Docker Hub

Imagine o seguinte cenário: Você trabalha como sysadmin em uma empresa que nesse exato momento esta adotando a tecnologia docker. Dentro desse ambiente os desenvolvedores querem alterar e compartilhar várias imagens, baseadas em distribuições diferentes e rodando aplicações diferentes. A todo momento esses desenvolvedores estão criando novas imagens e atualizando as existentes. E também essas imagens devem ser privadas. Como gerenciar tudo isso?

Para isso existe o docker registry que é um projeto opensource que permite você armazenar, compartilhar e gerenciar suas imagens docker (in-house registry). 

A empresa Docker fornece um serviço de registro público para os usuários poderem armazenar suas imagens, conhecido como Docker Hub. Mais adiante iremos demonstrar como fazer o download, alterar e publicar uma imagem no docker hub. Caso você ainda não tenha uma cadastro aproveite o momento e se registre.

## Como instalar

O processo de instalacao não é nada complicado e é possível achar diversos artigos ensinando como instalar o docker na sua distribuição preferida. Apenas fique atento aos pré-requisitos, tais como:

Instalação 64 bits independente da sua distribuição;
Kernel 3.10 ou superior;

O ambiente que faremos os testes para esse post será Ubuntu 14.10. Caso você esteja usando uma versão antiga do Kernel eu aconselho dar uma lida no site do (docker)[https://docs.docker.com/installation/].

Para instalar o docker iremos precisar do pacote curl.

```
$ sudo apt-get install curl
```

Agora podemos instalar o docker.

```
$ curl -sSL https://get.docker.com/ |sh
```

Look this message when we finished docker installation. “If you would like to use Docker as a non-root user, you should now consider adding your user to the "docker" group with something like”.

```
$ usermod -aG docker <YOUR-USERNAME>
Remember that you will have to log out and back in for this to take effect!
```

O docker disponibiliza uma imagem de teste que podemos utilizar para verificar se a instalação foi bem sucedida. O nome da imagem não poderia ser diferente, hello-world. Executando o comando abaixo você irá fazer o download da imagem e executá-la em um container.

```
$ docker run hello-world
```

Mais fácil que empurrar bêbado na descida, concordam? Agora só precisamos aprender como usá-lo :D.

Os primeiros passos com Docker

Agora que o docker já esta instalado em nossa máquina podemos começar a brincadeira. Provavelmente nesse momento você tem somente a imagem de teste hello-world instalada no seu computador. Com o comando docker images você pode visualizar as suas imagens.

```
$ docker images

REPOSITORY        TAG        IMAGE ID          CREATED       VIRTUAL SIZE
hello-world     latest     af340544ed62      2 weeks ago       960 B
```

Caso você deseja baixar uma nova imagem o comando pull ira nos ajudar no processo.

```
$ docker pull centos

Using default tag: latest
latest: Pulling from library/centos
f1b10cd84249: Pull complete 
c852f6d61e65: Pull complete 
7322fbe74aa5: Pull complete 
Digest: sha256:90305c9112250c7e3746425477f1c4ef112b03b4abe78c612e092037bfecc3b7
Status: Downloaded newer image for centos:latest
```

Agora se você executar o comando docker images novamente, sera possível visualizar  a nova imagem que baixamos do centos.

```
$ docker images

REPOSITORY        TAG        IMAGE ID          CREATED       VIRTUAL SIZE
hello-world     latest     af340544ed62      2 weeks ago       960 B
centos          latest     7322fbe74aa5      9 weeks ago       172.2 MB
```

Para executar um container com uma imagem é necessário utilizar o comando run e se essa imagem não existir localmente o docker irá efetuar o download automaticamente da imagem, caso ela exista.

O comando abaixo irá criar e executar um container com a imagem do debian. As opções “i” e “t” geralmente são combinadas para executar um processo de forma interativa, como se fosse uma shell.

```
$ docker run -i -t debian:8 /bin/bash
```

É possível ver todos os containers que estão em execução com o comando docker ps.

```
$ docker ps

CONTAINER ID    IMAGE     COMMAND      CREATED   STATUS   PORTS      NAMES
cf52245c4c32   debian:8  "/bin/bash"  5 min ago  Up 5 min      desperate_mccarthy  
```

Eh possível pararmos a execucao de um container com o comando stop, para isso devemos passar o ID do container. Repare no comando docker ps que existe o campo chamado CONTAINER ID. Esse campo é um identificador único do container. 

```
$ docker stop cf52245c4c32
cf52245c4c32
```

Se existe o comando stop, provavelmente exista o comando start, right? :D

```
$ docker start cf52245c4c32
cf52245c4c32
```

Agora que seu container esta Up novamente é possível conectar nele com o comando docker attach.

```
$ docker attach cf52245c4c32
root@cf52245c4c32:/# 
```

Caso você não queira mais uma determinada imagem em sua maquina local é possível realizar a remocao com o comando rmi.

```
$ docker rmi 91e54dfb1179

Error response from daemon: Conflict, cannot delete 91e54dfb1179 because the container 216c436397ca is using it, use -f to force
Error: failed to remove images: [91e54dfb1179]
```

Se o erro acima acontecer, temos duas opcoes. A primeira eh utilizar o comando rmi com a opcao -f, --force, acredito que esta seja a melhor opcao. A segunda seria remover os containers associado a imagem, para isso utilizariamos o comando rm. Veja como ficariam as opcoes

Primeiro removemos o containter que esta executando a imagem. Preste atencao que para remover o container temos que utilizar o comando docker rm e para remover a imagem utilizamos o comando docker rmi.

Removendo o container.

```
$ docker rm 216c436397ca
```

Para removermos uma imagem, tempos que passar como parametro o ID da imagem. Para ver suas imagens e os respectivos ID’s utilize o comando docker images.

```
$ docker rmi 91e54dfb1179
```

Lembre-se que o seu melhor amigo sempre sera o comando help. 

```
$ docker <COMMAND> help
```

Publicando uma imagem no Docker Hub

Até o momento nós apenas utilizamos várias imagens disponíveis no docker hub, que tal agora criarmos a nossa imagem e disponibilizar para os outros usuários? Volto a lembrar que você precisara ser cadastrado ao site.

Vamos baixar a imagem do ubuntu 14.10 e instalar o nginx.

```
$ docker run -i -t ubuntu:14.10 /bin/bash
root@281b3fcf24f2:/# apt-get update
root@281b3fcf24f2:/# apt-get install nginx
```

Agora que instalamos o nginx vamos verificar as modificacoes que fizemos na nossa imagem. Para você sair do container sem mata-lo pressione as teclas: ctrl + p + q. Com o comando docker ps você pode verificar que seu container ainda esta rodando.

Agora vamos utilizar o comando docker diff para verificar quais foram as alteracoes feitas por nos na imagem.

```
$ docker diff 281b3fcf24f2
```

A saida do comando ira mostrar tudo o que nos adicionamos, alteramos ou removemos. Para salvar as nossas modicacoes na imagem precisamos usar o comando commit que sera explicado a seguir. Lembre-se que pressionando as teclas ctrl + p + q eh possivel sair do container sem para-lo.

Antes de mostrar o comando commit vamos fazer o login no docker hub. O processo eh muito rapido e simples.

```
$ docker login
Username: 
Password: 
Email: benatto@gmail.com
WARNING: login credentials saved in /home/patito/.docker/config.json
Login Succeeded
```

O comando abaixo ira comitar as nossas alteracoes. Repare que estamos criando uma nova imagem no suario patito, e o nome dessa imagem eh ubuntu-nginx.

```
$ docker commit b15765f0448b patito/ubuntu-nginx
6b5522a3b94dcd25d3b4afb84b5069008bc69643fc69fae9ea6254be909c904e
```

Agora nossas alteracoes estao salvas na nossa nova imagem patito/ubuntu-nginx. Vamos executar essa nova imagem e vamos exportar a porta 80. A porta 8080 estara escutando no host e a porta 80 no container. Lembre-se de iniciar o nginx.

```
$ docker run -i -t -p 8080:80 patito/ubuntu-nginx /bin/bash
root@68246cddc002:/# service nginx start
root@68246cddc002:/# ps aux
```

Com o comando ps aux dentro do container você ira verificar se o nginx esta rodando. Se tudo ocorreu normalmente você pode acessar o seu browser http://<IP-ADDRESS>:8080. Você deveria acessar a pagina de boas vindas do nginx.

Agora que já instalamos e brincamos bastante com o nginx podemos publicar a nossa imagem, para isso fazemos uso do comando push.

```
$ docker push patito/ubuntu-nginx
```

Se você fizer o login no docker hub você podera ver a sua nova imagem.

O comando search eh utilizado para procurar as imagens que estao disponiveis no docker hub. Digite o comando abaixo e veja o resultado.

```
$ docker search centos
```

Para realizar o logout do docker hub bastar utilizar o comando docker logout.

```
$ docker logout
Remove login credentials for https://index.docker.io/v1/
```

## Authors

Luciano Borguetti trabalha como BlackOps at Neoway e também é um dos fundadores do meetup Docker Florianópolis, Brazil. Trabalha há mais de 10 anos com projetos opensource e administracao de ambientes Linux e BSD.

Paulo Leonardo Benatto é formado em Ciência da Computação pela Universidade Estadual do Oeste do Paraná. Muitos o chamam de hobbit e/ou anão, mas ele gostaria de deixar claro que é apenas uma pessoa compactada com o algoritmo LZO. Atualmente trabalha na empresa Brandwatch como Jr. Linux Sysadmin, mas em seu tempo livre gosta de fabricar bugs nas linguagens C, Python e Go.
