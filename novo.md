# TreasureHunt

> Ferramenta de geração automática de problemas e competições do tipo desafio (CTF) para ensino de Segurança Computacional

---

## Tabela de Conteúdos

- [Introdução](#Introdução)
- [Instalação](#Instalação)
- [Como Jogar](#Como-Jogar)
- [Autores](#Autores)
- [Funcionalidades Futuras](#Funcionalidades-Futuras)
- [Contribuição](#Contribuição)
- [Licença](#Licença)

---

## Introdução

Ainda que seja uma área significativa da computação, a Segurança Computacional apresenta um nítido contraste: se por um lado vivemos uma expansão do mundo tecnológico, refletindo na importância e grande demanda da cibersegurança, por outro, existe uma carência por profissionais e pouco espaço dedicado a ela em cursos de Tecnologia da Informação.

Assim, um dos artifícios usados para incentivar essa área é o uso de **jogos** e **competições de segurança**, dentre os quais, destaca-se o chamado desafio, ou _capture the flag_. Nele, o objetivo dos jogadores é obter os textos secretos, também chamados de _flags_ ao aplicar técnicas se Segurança.

Apesar de importantes, tais competições não são infalíveis, pois apresentam algumas dificuldades:

- Compartilhamento de resposta: costumam ser idêntica a todos os competidores, abrindo brecha para trapaça.
- Elaboração de problemas: uma tarefa normalmente trabalhosa e manual, necessitando de conhecimento técnico.
- Reaproveitamento de problemas: os problemas são rapidamente divulgados na _web_, eliminando o fator surpresa, imprescendível para esse tipo de jogo.

Dessa forma, **TreasureHunt Cybersecurity Game** é, como o nome indica, um jogo de caça ao tesouro que se diferencia ao aplicar soluções para minimizar os problemas supracitados por meio da geração de problemas e competições de forma aleatória, automática e completa. Seu desenvolvimento fez parte do projeto de Mestrado do Ricardo de la Rocha Ladeira.

Os códigos presentes no repositório representam o gerador de competições (em `TreasureHunt/Jogo`) e a interface web do jogo (em `TreasureHunt/TreasureHunt`).

---

## Instalação

Para jogar o TreasureHunt é necessário seguir os seguintes passos:

1. Clonagem do repositório
2. Execução do script de instalação de requisistos
3. Execução do script de geração de competições
4. Configuração e inicialização do servidor web

Vale lembrar que alguns passos adicionais podem ser necessários, como a instalação das ferramentas indicadas para a resolução dos problemas, caso estas não estejam disponíveis para os jogadores.

O restante dessa seção descreverá detalhadamente cada um dos passos sugeridos: 

### Clonagem

Para ter uma cópia do repositório em sua máquina, basta executar o seguinte comando [Git](https://git-scm.com/) em um terminal:

```sh
 git clone https://github.com/TreasureHuntGame/TreasureHunt.git
```

Para prosseguir a instalação, certifique-se de que você encontra no diretório do TreasureHunt:

```sh
cd TreasureHunt
```

### Instalação de Requisitos e Ferramentas 

Após baixar o repositório para sua máquina local, o próximo passo é fazer a instalação dos requisitos do projeto. Para isto, basta executar o script [`instalador.sh`](./instalador/instalador.sh), localizado em [`TreasureHunt/instalador`](./instalador), que se encarrega de instalar todos os pacotes, que por padrão se encontram no arquivo [`requisitos.txt`](./instalador/requisitos.txt).

```sh
cd instalador
chmod +x instalador.sh
./instalador.sh
```

Além dos requisitos para rodar o TreasureHunt, certifique-se de ter pelo menos as seguintes ferramentas (ou equivalentes) Para solucionar os desafios do jogo:

- awk
- base32
- base64
- caesar
- outguess
- sed
- sh
- strings
- Editor de texto
- Navegador web

### Geração da Competição

As competições de TreasureHunt precisam ser criadas pelo organizador. Para tanto, este deve escolher a quantidade de participantes, bem como quantos e quais desafios estarão presentes, podendo inclusive adicionar um problema composto, onde duas técnicas são aplicadas em conjunto.

Para gerar a competição, basta executar o script [`jogo.sh`](./jogo/problemas/jogo.sh) do diretório [TreasureHunt/jogo/problemas/](./jogo/problemas).

```sh
cd jogo/problemas
chmod +x jogo.sh
./jogo.sh
```

Com o fim desses passos, o esquema e as tabelas também terão sido criadas no Bando de Dados, graças ao script `ConfiguraBD.sh` que é executado automaticamente.

### Configuração e Inicialização do Servidor Web

Para iniciar a interface web basta seguir os seguintes passos:

1. Copiar o diretório [/TreasureHunt](./TreasureHunt) para o diretório do servido web. Dependendo do sistema operacional e do servidor web este se localizará em locais diferentes. Exemplo: `C:\xampp\htdocs` com o Xampp no Windows ou `/var/www/html/TreasureHunt/` em certas distribuições Linux:

```sh
cp TreasureHunt /var/www/html/TreasureHunt/
```

2. Iniciar o servidor web, que pode ser o apache como no exemplo a seguir: 

```sh
sudo service apache2 start
```

3. Forneça seu IP aos jogadores (verifique em sua interface de rede, por exemplo, digitando ``ifconfig``)

4. Forneça nome de usuário (ID) e senha aos jogadores. Eles usarão essas informações para se autenticar na interface web posteriormente.

5. Quando desejar finalizar a competição, desligue o servidor. No caso do apache, por exemplo:

```sh
sudo service apache2 stop
```

Ao finalizar o jogo, as submissões estarão armazenadas na base de dados ``TreasureHunt`` no MySQL.

### Problemas Frequentes na Instalação

Pressupõe-se algumas condições para que a instalação ocorra com sucesso. Essas indicações, bem como alguns dos erros comuns serão destacados a seguir:

- *Nota 1*: Necessário obter privilégio de administrador no diretorio do servidor web (por exemplo: ``/var/www/html/TreasureHunt/``).

- *Nota 2*: O script considera que o MySQL será utilizado com usuário ``root`` e sem senha. O organizador pode alterar isso manipulando a chamada ao script ``ConfiguraBD.sh`` no arquivo ``Jogo.sh``.

- *Nota 3*: O script considera que o MySQL será utilizado sem a diretiva ``NO_ZERO_DATE``. Para removê-la, insira no arquivo de configuração: 
[mysqld]  
``sql_mode = "ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"``
Depois, reinicie o mysql (``sudo service mysql restart``) e tente novamente. Você tambm pode verificar se a diretiva foi removida entrando no mysql e digitando no console: ``SHOW VARIABLES LIKE 'sql_mode';``

- *Nota 4*: O arquivo ``apache2.conf``, disponível no diretório ``TreasureHunt/TreasureHunt``, serve apenas como exemplo de configuração do servidor web. O organizador pode configurá-lo de maneira diferente, a seu critério.

- *Nota 5*: Arquivos de texto podem apresentar problemas se codificados com iso 8859-1. Prefira utf-8 ou us-ascii.

- *Nota 6*: Se você obtiver a mensagem `ERROR 1698 (28000): Access denied for user 'root'@'localhost'`ao final do script, verifique o valor default de autenticação para o seu usuário e altere-o. Isso pode ser feito seguindo os passos abaixo (exemplo para o usuário `root`):

```SQL
> sudo mysql -u root
mysql> USE mysql;
mysql> SELECT User, Host, plugin FROM mysql.user;
mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';
mysql> FLUSH PRIVILEGES;
mysql> exit;
> service mysql restart
```

---

## Como Jogar

### Descrição do jogo

Por meio da interface web, os jogadores tem acesso as seguintes funcionalidades: 

- Ler as intruções do jogo e o contato dos desenvolvedores
- Baixar o arquivo com os seus respectivos problemas 
- Enviar respostas (flags) dos problemsa 
- Exibir placar individual 
- Exibir placar geral

Apesar disso, para visualizar o placar, baixar os problemas e submeter as respostas é preciso estar autenticado, ou seja, é preciso fazer _login_ com o ID e senha fornecidos pelo organizardor da competição.

Dessa forma, os passos necessários para jogar o TreasureHunt, são:

1. Se autenticar no _site_ com ID e senha. 
2. Baixar o arquivo compactado com os problemas: cada diretório dentro do arquivo possui um número e representa cada um dos problemas (desafios) do CTF e dentro dele um ou mais arquivos que precisam ser analisados para se obter a flag do respectivo problema.
3. Utilizar as ferramentas adequadas e/ou encontrar as flags nos arquivos, que apresentam o formato ``TreasureHunt{TEXTO_ALEATÓRIO}``
4. Submeter a flag juntamente do número identificador do diretório (número da questão) no qual a flag foi encontrada.

Após submeter uma flag, um das seguintes respostas poderá ser visualizada:

- **Problema com ID inválido**: refere-se ao número do problema, o mesmo que nomeia o diretório. Este erro indica que o número informado foi negativo ou maior que a quantidade de questões da competição. 
- **Errou!**: a flag está com o formato correto, porém a resposta está incorreta
- **Errou! Considere submeter a flag no seguinte formato: TreasureHunt{texto-aleatorio}**: indica que o formato da flag está incorreto
- **Você já acertou a questão ID_problema!**: essa questão já foi respondida corretamente anteriormente (um acerto não é contado duas vezes)
- **Acertou! n/m**: a questão foi respondida corretamente. É indicada quantas questões do total já foram acertadas.

Ao longo da competição o usuário tem a opção de visualizar quais problemas faltam ser resolvidos no placar indivual e sua colocação em relação aos demais jogadores. 

Para ganhar a partida, o jogador precisa ter mais pontos que os adversários, e como cada questão acertada garante 1 ponto, é preciso acertar mais questões que os demais. Caso ocorra um empate no número de pontos, o critério de desempate é o tempo levado para resolver a última questão, sendo vitorioso aquele que a submeteu mais rápido.

Vale lembrar que para a execução dos problemas recomenda-se um conjunto mínimo de ferramentas, descritas no final da seção [Instalação de Requisitos e Ferramentas](#Instalação-de-Requisitos-e-Ferramentas). Apesar disso, o jogador é livre para escolher a forma como resolverá os problemas, incluindo ferramentas alternativas ou scripts automatizados para a resolução dos problemas.

### Tipos de problemas

Existem alguns tipos de problemas que podem ser gerados em uma competição do TreasureHunt. Estes são definidos pelo organizador e são classificados de acordo com a ferramenta da Segurança Computacional precisa ser aplicada para solucioná-lo:

- Comentário em código-fonte de página HTML (HyperText Markup Language)
- Comentário no arquivo robots.txt
- (De)codificação de arquivo em base64
- (De)codificação de arquivo em base32
- (Des)criptografia de Cifra de César
- (De)codificação de caractere ASCII (American Standart Code for Information Interchange)
- Descompilador binário e obter fonte Java
- Descompilador binário e obter fonte Python
- Esteganografia em imagens

Vale lembrar que estes problemas podem ser combinados em problemas compostos, ainda que nem todas as combinações são possíveis. Além disso, a ordem em que são aplicadas altera o problema como um todo, pois as ferramentas adequadas para resolver esses problemas também deverão ser aplicados na ordem correta. 

---

## Autores

### Equipe atual

- Ricardo de La Rocha Ladeira: [ricardo.ladeira@ifc.edu.br](mailto:ricardo.ladeira@ifc.edu.br)
- Vítor Augusto Ueno Otto: [vitoruenootto@gmail.com](mailto:vitoruenootto@gmail.com)

### Contribuidores

- Rafael Rodrigues Obelheiro: [rafael.obelheiro@udesc.br](mailto:rafael.obelheiro@udesc.br)
- Richard Robert Dias Custódio: [richardsoncusto@gmail.com](mailto:richardsoncusto@gmail.com)
- Vinícius Manuel Martins: [Vínivinicius.m.m.2002@gmail.com](mailto:Vínivinicius.m.m.2002@gmail.com)

---

## Funcionalidades Futuras

Este projeto se mantém em constante atualização, o que significa que novas funcionalidades e aperfeiçoamentos continuarão a ser feitos ao longo do tempo. Algumas dessas modificações possuem um caráter duradouro como é o caso da criação de novas possibilidades de problemas no gerador de desafios. Dentre todas as funcionalidades planejadas, pretende-se:

- Criar novos problemas no gerador de desafios, em especial da classe de problemas "web"
- Permitir que mais de duas técnicas componham um problema (por exemplo, esteganograa com outguess e f5.jar)
- Implementar um recurso para inserção de dicas
- Adicionar identificadores às competições, permitindo melhor acompanhamento da evolução dos jogadores, por exemplo.
- Fornecer uma interface de treinamento inteligente para analisar as respostas e o tempo levado pelos jogadores para recomendar problemas de dificuldade adequada.

---

## Contribuição

Sua contribuição é bem vinda nesse projeto, portanto sinta-se livre para relatar bugs, impressões ou sugerir mudanças, tanto por meio da criação de **issues** e **pull requests** quanto pelo contato direto com algum dos [autores do projeto](#Autores). 

Quanto ao formato das contribuições pedimos que utilize uma linguagem clara, descrevendo o seu ambiente e o passo a passo para reproduzir o bug. Seja breve e objetivo, mostrando prints se possível. 

---

## Licença

<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Licença Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">TreasureHunt{Security}</span> de <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/TreasureHuntGame/TreasureHunt" property="cc:attributionName" rel="cc:attributionURL">Ricardo de la Rocha Ladeira</a> está licenciado com uma Licença <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">Creative Commons - Atribuição-NãoComercial 4.0 Internacional</a>.
<br />Baseado no trabalho disponível em <a xmlns:dct="http://purl.org/dc/terms/" href="https://github.com/TreasureHuntGame/TreasureHunt" rel="dct:source">https://github.com/TreasureHuntGame/TreasureHunt</a>.

---
