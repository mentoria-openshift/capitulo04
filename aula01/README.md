<p align="center"><a href="../aula02">Próxima aula ❯</a></p>
<br/>

# Aula 1 - O processo de compilação
Bem-vindo ao capítulo 3 do curso de OpenShift! Nesta aula, implantaremos uma aplicação simples no OpenShift usando uma imagem pré-compilada e hospedada num repositório.

## O processo de compilação
No capítulo 2 do curso, falamos um pouco sobre recursos do OpenShift, e, entre eles, falamos sobre Builds e BuildConfigs e como elas se aplicam no uso do OpenShift. Vimos a anatomia de uma BuildConfig definida em YAML, que é o formato padrão do OpenShift, apesar de recursos do OCP também poderem ser declarados em JSON. 

O processo de compilação de uma aplicação é aquele entre o término da edição de código e o início da implantação. É o processo onde dependências são baixadas, instruções executadas e a versão de fato executável da sua aplicação é gerada. Mas existem diversas formas de fazer isso no seu projeto, e já vimos algumas dessas formas em prática. Mas vamos entrar em mais detalhes sobre o que são compilações no OpenShift.

## Estratégias de compilação
Existem diversas estratégias de compilação no OpenShift, e no capítulo 3 trabalhamos com três delas: a estratégia Source, a estratégia Custom e a estratégia Docker. Além destas três formas, que são integralmente gerenciadas por OpenShift, podemos também utilizar a estratégia Pipeline, que nos permite integrar com ferramentas de CI/CD como o Jenkins para gerenciar o ciclo de vida da nossa aplicação. 

### Estratégia Source
A estratégia Source cria uma nova imagem de container baseada no código fonte da aplicação, ou até mesmo dos binários da aplicação. O código é clonado para uma imagem de compilação compatível, e então uma nova imagem de container é gerada, pronta para implantação. Esta estratégia facilita o processo para o desenvolvedor pois não é necessário saber comandos de sistemas, basta apenas ter a imagem necessária e o código da aplicação, e o resto do processo é aplicado pela própria plataforma do OpenShift. A estratégia Source é baseada no processo S2I, que vimos no capítulo 3.

### Estratégia Docker
A estratégia Docker usa comandos do Buildah, bem como aprendemos no capítulo 1, para compilar uma imagem fornecida a partir de um Dockerfile. É fornecido o caminho de um repositório Git, de onde o OpenShift clona o Dockerfile e o compila. A partir da imagem gerada, então, são iniciados pods dentro do cluster. Note: pode ser necessária uma permissão especial para executar compilações Docker, e isso depende dos administradores do cluster.

### Estratégia JenkinsPipeline
Esta estratégia usa a integração com o Jenkins para criar uma imagem de container, que então são iniciadas em pods. Apesar de o processo de gerenciado e executado através do Jenkins, por conta do plugin de integração, é possível iniciar, monitorar e gerenciar todo o processo de compilação diretamente pelo OpenShift. A BuildConfig da aplicação deve referenciar um Jenkinsfile contendo as configurações da pipeline que será usada. É possível usá-lo do repositório Git ou até mesmo referenciá-lo na configuração de compilação. 

### Estratégia Custom
A estratégia Custom especifica uma imagem de compilação específica para o processo. Esta estratégia permite que o desenvolvedor customize e modifique o processo de compilação. 

## Configuração de compilação (bc)
A configuração de compilação (BuildConfig ou BC) é uma definição de como as aplicações serão compiladas, e é composta de por gatilhos para que uma nova compilação seja iniciada. É caracterizada por uma estratégia para compilação vinda de uma ou mais fontes de recursos, e o processo é determinado usando as fontes como entrada.

Configurações de compilação são geradas automaticamente para alguns tipos de aplicação quando elas são criadas, e podem ser editadas a qualquer momento como os demais recursos do OCP.

```yaml
kind: BuildConfig
apiVersion: build.openshift.io/v1
metadata:
  name: "ruby-sample-build" 
spec:
  runPolicy: "Serial" 
  triggers: 
    -
      type: "GitHub"
      github:
        secret: "secret101"
    - type: "Generic"
      generic:
        secret: "secret101"
    -
      type: "ImageChange"
  source: 
    git:
      uri: "https://github.com/openshift/ruby-hello-world"
  strategy: 
    sourceStrategy:
      from:
        kind: "ImageStreamTag"
        name: "ruby-20-centos7:latest"
  output: 
    to:
      kind: "ImageStreamTag"
      name: "origin-ruby-sample:latest"
```

A definição de `runPolicy` controle se compilações criadas a partir desta definição podem rodar simultaneamente, e `triggers` definem gatilhos para as compilações, isto é, em quais ocasiões elas deverão ser executadas automaticamente pelo OCP. 

O campo `source` define a fonte usada para a compilação, normalmente um repositório git contendo o código da aplicação. Mas o OCP também permite que aplicações sejam compiladas a partir de arquivos binários ou até mesmo de um `Dockerfile` contendo informações de uma imagem.

A tag `strategy` define a estratégia de compilação usada. É possível definir uma estratégia própria, mas as comuns são `Docker` e `Source`, para usar registros de imagens ou fluxos de imagens.

Em `output` temos a definição do que será feito no término da compilação da imagem de aplicação. Neste caso, uma tag de fluxo de imagem será gerada para nossa aplicação.

## Exercícios
Isso conclui a aula dos básicos de uma implantação no OpenShift. Para praticar o conteúdo aprendido, vamos fazer um exercício prático.

* [Questionário](questionario.md)

## Referências
* [Documentação do OpenShift](https://docs.openshift.com/)
* [Entradas de compilação](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.5/html/builds/creating-build-inputs)
* [Fluxos S2I binários e por fonte](https://developers.redhat.com/blog/2018/09/26/source-versus-binary-s2i-workflows-with-red-hat-openshift-application-runtimes/)


----
<p align="center"><a href="../aula02">Próxima aula ❯</a></p>
