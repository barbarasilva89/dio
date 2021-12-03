# Introdução ao Git e ao Github - DIO

### O que é o Git?

Wikipédia:

> Git é um sistema de controle de versões distribuído, usado principalmente no desenvolvimento de software, mas pode ser usado para registrar o histórico de edições de qualquer tipo de arquivo (Exemplo: alguns livros digitais são disponibilizados no GitHub e escrito aos poucos publicamente). O Git foi inicialmente projetado e desenvolvido por Linus Torvalds para o desenvolvimento do kernel Linux, mas foi adotado por muitos outros projetos.
> 
> Cada diretório de trabalho do Git é um repositório com um histórico completo e habilidade total de acompanhamento das revisões, não dependente de acesso a uma rede ou a um servidor central. O Git também facilita a reprodutibilidade científica em uma ampla gama de disciplinas, da ecologia à bioinformática, arqueologia à zoologia.

--- 

### Instalação do Git - particularidades

Particularidades na instalação do Git incluem:

* Na página Select Components lembrar de marcar os componentes 
  
  *[]* Git Bash Here
  
  *[]* Git GUI Here

* Configuring extra options, marcar além das default:
  
  *[]* Enable symbolic links

* Nenhuma das demais opções precisa ser alterada na versão 2.34    

---

### Tópicos fundamentais do GIT

#### SHA1

Conjunto de funções hash de criptografia. Gera um conjunto de caracteres de 40 dígitos identificadores de um arquivo, ou seja, uma forma curta de identificar um arquivo.

` echo "ola mundo" | openssl sha1`

No código acima, echo apenas reenvia a string "ola mundo" de volta ao terminal. O comando | por sua vez pega esse trecho inicial e envia para o comando sha1 dentro do programa openssl.

O comando para criar um novo arquivo com a string acima é:

`echo "ola mundo" > nomedoarquivo.extensao` 

#### Objetos

##### Blobs

Comparando os códigos 

`echo 'conteudo'|git hash-object --stdin`

`echo 'conteudo' | openssl sha1` 

obtem-se códigos de encriptação diferentes. Isso ocorre porque o Git, além dos caracteres 'conteudo', armazena o tipo do objeto 'blob', tamanho do arquivo e '\0' no início do arquivo no objeto Blob que é a forma do Git armazenar o arquivo (são metadados do objeto).

##### Trees

As trees armazenam os blobs e outras árvores. Elas apontam para esses objetos, montando a estrutura de localização dos arquivos. Em seu SHA1, elas armazenam seu tipo de objeto ('tree'), seu tamanho, '\0', o tipo do objeto que ela está apontando, o SHA1 desse objeto e o nome desse objeto. Elas possuem seu SHA1.

##### Commit

Os commit encapsulam todo o conteúdo, dando significado à toda à estrutura e armazenando informações sobre seu tipo (commit), seu tamanho, a árvore principal que aponta, o SHA1 dessa, o commit anterior a esse (parente), o autor do commit, uma mensagem inicial e o momento que o commit foi criado (timestamp). Possui seu próprio SHA1.

--- 

### Chave SSH e Token

Atualmente, para enviar um repositório local ao Github é necessário umas das autenticações acima.

A chave SSH informa ao Github que a máquina a enviar o código é confiável, dispensando a autenticação por senha.

#### Gerando a chave SSH no Git

Utilizando o Git Bash, deve-se utilizar os seguintes comandos:

`ssh-keygen -t ed25519 -C email.do.usuario@nogithub..com`

Confirma a geração da frase inserindo uma senha

A chave gerada no terminal inicia com SHA256:...

Duas chaves são geradas, uma pública (final .pub) e outra privada. A pública é a utilizada em sites como o Github

Para navegar à pasta onde as chaves foram criadas e verificar utiliza-se os seguintes códigos:

`cd /c/User/usuario/.ssh/ ls` 

O conteúdo da chave pública que é inserido no Github é acessado usando:

`cat nome-dachave-publica.pub` 

Copia-se a chave e no Github > Setinha no Perfil > Settings > SSH e GPG keys > New SSH key, insira um nome para a chave. Cole a chave gerada e confirme a criação da chave inserindo sua senha de login.

Posteriormente deve-se iniciar o SSH agent para rodar em plano de fundo, enviando a chave ssh quando solicitada pelo servidor. Para isso utiliza-se os seguintes códigos:

`eval $(ssh-agent -s)` - inicializa o agent

`ssh-add nome_dachave_privada`  - passa a chave ao agent inicializado

Insira a senha utilizada na geração da chave e pronto.

Para clonar repositórios no Github com uma chave configurada, deve-se copiar o caminho SSH do repositório, nao o HTML, e usando no Git, dentro do diretório onde será clonado o original:

`git clone chave-ssh-do-repositiorio-a-serclonado` 

##### Token de acesso pessoal

Substitui a utilização da senha. Um arquivo com uma sequencia de caracteres é gerado e deve ser armazenado para autenticação futura. Para gerá-lo, deve-se no Github > Setinha no Perfil > Settings > Developer Settings > New personal access token

Insira uma nota para o token, uma data de expiração e o escopo (em geral a opção _repo_ satisfaz todas as necessidades no Github).

Copia-se a chave gerada e armazena-se em um local seguro.

Para clonar um diretório utilizando o token, no terminal do Git utiliza-se o endereço HTML do diretório:

`git clone html-dodiretorio-a-ser-clonado` 

Um popup aparecerá para a inserção do token gerado.

---

### Iniciando o Git e criando um commit

#### Criando um repositório

Para criar pastas utilizando o prompt de comando do Git usa-se o comando `mkdir`  no local desejado. Para mover o local de ação do Git até a pasta desejada, utiliza-se o comando `cd` e para mover um arquivo da pasta local para outra utiliza-se:

`mv nome-do-arquivo /local/de/destino` 

No local desejado, inicializa-se um repositório do Git com o comando `git init`, gerando uma pasta oculta `.git` que pode ser visualizada com o comando `ls -a`. 

Para utilizar o Git pela primeira vez, é necessário configurar um apelido e um email que serão gravados nos commits gerados. Para configura-los utiliza-se:

`git config --global user.email "email@mail..com"`

`git config --global user.name NomeDaConta` 

`git config --global user.nickname Apelido` 

Para checar as configurações utilize:

`git config --list`  e `q` para sair.

#### Criando um commit

Gere um arquivo, de preferência do tipo markdown (.md) utilizando um software como o *Mark Text* ou *Zettlr*. Esse tipo de arquivo permite a visualização amistosa de uma formatação HTML sem a necessidade de programar nela, usando para isso, caracteres formatadores especiais apenas. Também é possível inserir tabelas e emoticons (no windows o atalho para isso ´w Win + . ).😀

Com o arquivo criado dentro do diretório de trabalho, adiciona-se o arquivo ao repositório do Git e gera-se um commit com o:

`git add *` - adiciona todos os arquivos para serem commitados

`git commit -m "mensagem do commit com o significado dele no projeto"` 

O terminal retorna com a confirmação da criação e o início dos caracteres do SHA1 criado.

#### Como o Git funciona

O funcionamento do Git baseia-se na comparação das chaves SHA1 dos objetos apresentados a ele. Com o Git iniciado, quando um arquivo/pasta é criado no diretório de trabalho, mas não é adicionado ao Git, ele é caracterizado como **Untracked**. Depois de apresentado pela primeira vez ao git com o comando git add, o arquivo está pronto para ser apresentado de fato, passando para o estado de **Staged**, preparado para ter seu commit gerado. Depois do commit gerado, quando uma "fotografia" do arquivo é gerado, ele torna-se **Unmodified**, mas caso ele sofra alterações, o que fará com que seu SHA1 seja alterado, ele passa a ser caracterizado pelo Git como **Modified**, devendo ser apresentado novamente para gerar um novo commit antes de ser entregue a um repositório na nuvem.

As alterações no repositório remoto não acontecem imediatamente, a menos que os arquivos com commit gerado no repositório local sejam *empurrados* para a nuvem.

O estado dos arquivos pode ser verificado com o:

`git status` 

#### Enviando o repositório local para o Github

Para empurrar um repositório local para o Github, primeiro é necessário criar um repositório no Github. Depois de criado, esse repositório possui um link https que deve ser inserido no Git iniciado no diretório de trabalho. Para isso utiliza-se o código:

`git remote add origin https://github.linkcopiado.com...`

`origin` é apenas um apelido para esse repositório, mas pode ser utilizado outro.

Os repositórios da nuvem cadastrados podem ser listados com o comando 

`git remote -v`

O estado do repositório deve ser verificado antes de empurrar para a nuvem com o comando `git status` mencionado anteriormente.

Para, enfim, enviar o repositório para o Github, utiliza-se:

`git push origin master`



### Conflitos no Github

##### Conflitos de Merge

Um dos motivos para a ocorrência de conflitos no Github é o chamado conflito de merge. Existindo um código em um repositório de um perfil no Github, se esse código for atualizado por um terceito, empurrado para substituir o já existente enquanto outra pessoa ou o autor alteram a versão inicial em seus repositórios local, quando eles tentarem empurrar o código para substituir o código que já foi substituído, o Github informará que esse código é originado de uma versão desatualizada e tentará fazer o merge. Caso haja alterações em uma linha comum entre as versões 2 e 3 desse código exemplificado, o Github não irá fazer a atualização desse código para a versão 3, solicitando ao usuário que faça manualmente a atualização do código.

Para fazer um merge das versões do repositório do Github e local, usa-se:

`git pull origin master`

`origin` é o apelido do repositório remoto.

Havendo alterações em conflito, o Git alterará o arquivo local em conflito, destacando os pontos em conflito com marcações especiais (i.e. `>>>> head`, `**** código estranho` ). Esses pontos devem ser solucionados pelo usuário e depois reempurado para o Github usando:

`git add nome-do-arquivo`

`git commit -m "mensagem"`

`git push origin master`

##### Clonando repositórios do Github

Para clonar um repositório do Github pelo Git simplesmente copie o link html dele no Github e cole junto ao código no Git Bash dentro da pasta que o diretório irá ficar:

`git clone linkhtml`

A diferença entre clonar o repositório dessa forma para baixar o .zip, é que ele contém, além dos arquivos, a pasta .git com as informações Git desse repositório e o link remoto para onde ele está apontando, acessável pelo `git remote -v` 
