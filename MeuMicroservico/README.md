## Instale o SDK do .NET

Para começar a criar aplicativos .NET, baixe e instale o .NET SDK (Software Development Kit).

[Instale as instruções do SDK do .NET 7](https://learn.microsoft.com/dotnet/core/install/linux?WT.mc_id=dotnet-35129-website)

### Verifique tudo instalado corretamente

Depois de instalado, abra um novo terminal e execute o seguinte comando:

```bash
$ dotnet
```

Se a instalação for bem-sucedida, você deverá ver uma saída semelhante à seguinte:

```bash
Usage: dotnet [options]
Usage: dotnet [path-to-application]

Options:
-h|--help         Display help.
--info            Display .NET information.
--list-sdks       Display the installed SDKs.
--list-runtimes   Display the installed runtimes.

path-to-application:
The path to an application .dll file to execute.
```
Se tudo estiver bem, desça abaixo para a próxima etapa.

### Ocorreu um erro?

Se você receber um erro dotnet: command not found, certifique-se de abrir uma nova janela de terminal. Se você não conseguir resolver o problema, use o botão Encontrei um problema para obter ajuda para resolvê-lo.

## Crie seu serviço

No seu terminal, execute o seguinte comando para criar seu aplicativo:

```bash
$ dotnet new webapi -o MeuMicroservico --no-https -f net7.0
```

Em seguida, navegue até o novo diretório criado pelo comando anterior:

```bash
$ cd MeuMicroservico
```

## O que esses comandos significam?

O comando dotnet cria um novo aplicativo do tipo webapi (que é um endpoint da API REST).

- O parâmetro -o cria um diretório chamado MyMicroservice onde seu aplicativo é armazenado.
- O sinalizador --no-https cria um aplicativo que será executado sem um certificado HTTPS, para simplificar a implantação.
- O parâmetro -f indica que você está criando um aplicativo .NET 7.

O comando cd MyMicroservice coloca você no diretório de aplicativos recém-criado.

## O código gerado

Vários arquivos foram criados no diretório MyMicroservice, para fornecer a você um serviço simples e pronto para ser executado.

- MyMicroservice.csproj define quais bibliotecas o projeto referencia, etc.
- Program.cs contém todas as definições e configurações que são carregadas quando o aplicativo é iniciado.
- Controllers/WeatherForecastController.cs tem código para uma API simples que retorna a previsão do tempo para os próximos cinco dias.
- O arquivo launchSettings.json dentro do diretório Properties define diferentes configurações de perfil para o ambiente de desenvolvimento local. Um número de porta variando entre 5000-5300 é atribuído automaticamente na criação do projeto e salvo neste arquivo.

O código que está no repositório mostra o conteúdo do arquivo WeatherForecastController.cs localizado no diretório Controllers:

Continuar abaixo para ir para a próxima etapa.

## Execute seu serviço

No seu terminal, execute o seguinte comando:

```bash
$ dotnet run
```

Você deve ver uma saída semelhante à seguinte:

```bash
Building...
info: Microsoft.Hosting.Lifetime[14]
Now listening on: http://localhost:5020
info: Microsoft.Hosting.Lifetime[0]
Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
Content root path: C:\Users\Ana\MyMicroservice\
```
Aguarde até que o aplicativo mostre que está escutando em http://localhost:<número da porta> e, em seguida, abra um navegador e navegue até http://localhost:<número da porta>/WeatherForecast.

Neste exemplo, ele mostrou que estava escutando na porta 5020, então a imagem a seguir mostra a URL http://localhost:5020/WeatherForecast.

Parabéns, você tem um serviço simples em execução!

Pressione CTRL+C em seu terminal para encerrar o comando dotnet run que está executando o serviço localmente.

# Instalar Docker

O Docker é uma plataforma que permite combinar um aplicativo com sua configuração e dependências em uma única unidade implementável de forma independente chamada contêiner.

> Se você já possui o Docker instalado, verifique se é a versão 20.10 ou superior.

## Baixar e instalar

Você será solicitado a se registrar na Docker Store antes de poder baixar o instalador.

[Obtenha o Docker para Linux](https://hub.docker.com/search?offering=community&operating_system=linux&type=edition)

Depois de instalar o Docker, você pode ser solicitado a sair para finalizar a instalação.

## Verifique se o Docker está pronto para uso

Depois de instalado, abra um novo terminal e execute o seguinte comando:

```bash
$ docker --version
```
Se o comando for executado, exibindo algumas informações de versão, o Docker foi instalado com sucesso.

## Adicionar metadados do Docker

Para executar com uma imagem do Docker, você precisa de um Dockerfile — um arquivo de texto que contém instruções sobre como criar seu aplicativo como uma imagem do Docker. Uma imagem do Docker contém tudo o que é necessário para executar seu aplicativo como um contêiner do Docker.

## Retornar ao diretório do aplicativo

Como você abriu um novo terminal na etapa anterior, precisará retornar ao diretório em que criou seu serviço.

```bash
$ cd MeuMicroservico
```

## Adicionar um DockerFile

Crie um arquivo chamado Dockerfile com este comando:

```bash
$ touch Dockerfile
```
Você pode abri-lo em seu editor de texto favorito.

Substitua o conteúdo do Dockerfile pelo seguinte no editor de texto:

Nota: Certifique-se de nomear o arquivo como Dockerfile e não Dockerfile.txt ou algum outro nome.

## Opcional: adicione um arquivo .dockerignore

Um arquivo .dockerignore reduz o conjunto de arquivos que são usados ​​como parte do `docker build`. Menos arquivos resultarão em compilações mais rápidas.

Crie um arquivo chamado arquivo .dockerignore (semelhante a um arquivo .gitignore, se você estiver familiarizado com eles) com este comando:

```bash
$ touch .dockerignore
```
Você pode abri-lo em seu editor de texto favorito.

Substitua o conteúdo de .dockerignore pelo seguinte no editor de texto:

## Criar imagem do Docker

Execute o seguinte comando:

```bash
$ docker build -t mymicroservice .
```
O comando docker build usa o Dockerfile para criar uma imagem do Docker.

- O parâmetro -t mymicroservice informa para marcar (ou nomear) a imagem como mymicroservice.
- O parâmetro final informa qual diretório usar para localizar o Dockerfile (. especifica o diretório atual).
- Este comando fará o download e construirá todas as dependências para criar uma imagem do Docker e pode levar algum tempo.

Você pode executar o seguinte comando para ver uma lista de todas as imagens disponíveis em sua máquina, incluindo a que você acabou de criar.

```bash
$ docker images
```

## Executar imagem do Docker

Você pode executar seu aplicativo em um contêiner usando o seguinte comando:

```bash
$ docker run -it --rm -p 3000:80 --name meumicroservicocontainer meumicroservico
```
You can browse to the following URL to access your application running in a container: http://localhost:3000/WeatherForecast

Opcionalmente, você pode visualizar seu contêiner em execução em uma janela de terminal separada usando o seguinte comando:
```bash
$ docker ps
```
Pressione CTRL+C no seu terminal para encerrar o comando docker run que está executando o serviço em um contêiner.

Parabéns! Você criou com sucesso um serviço pequeno e independente que pode ser implantado e dimensionado usando contêineres do Docker.

Esses são os blocos de construção fundamentais dos microsserviços.

# Próximos passos

Parabéns! Você criou um serviço simples e o executou em um contêiner do Docker.

Agora você pode aprender como implantar seu microsserviço na nuvem com nosso próximo tutorial.

[Tutorial: implantar microsserviço no Azure](https://dotnet.microsoft.com/en-us/learn/aspnet/deploy-microservice-tutorial/intro)