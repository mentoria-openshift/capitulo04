<p align="center"><a href="../aula01">❮ Aula anterior</a> | <a href="../aula03">Próxima aula ❯</a></p>
<br/>

# Aula 2 - Gerenciamento de compilação
Nesta aula, aprenderemos mais sobre como gerenciar compilações no OpenShift. Vamos aprender mais sobre a linha de comando e definições de BuildConfigs.

## Gerenciando compilações com o CLI
Quando trabalhamos com recursos do OpenShift, usamos a linha de comando para criar ou modifica-los. Também é possível modificar e criar alguns recursos através da interface web. Mas quando tratamos de configurações de compilação, a forma mais comum de definir uma é através do comando `oc new-app`. Ao fornecer como parâmetro um caminho de código, a aplicação será compilada, e uma BuildConfig é necessária para definir como o processo ocorrerá. A partir desta BuildConfig serão gerados recursos de Build, que são as compilações em si. 

É possível também criar BuildConfigs através de declarações de recursos em YAML ou JSON, conforme vimos na aula anterior. Além da definição de recurso em si, é possível também inclui-la num template, que criará, além da BuildConfig, outros recursos e até mesmo a aplicação em si. Com o arquivo de recurso ou template salvo, você pode usar o comando `oc create -f <caminho do arquivo>`, e a definição será importada e criada para o OpenShift.

### Comandos
Levando em consideração que Build e BuildConfig são recursos diferentes, alguns comandos podem ser usados em ambos, mas gerenciar ou verificar as diferentes características de cada recurso.

### `oc start-build`
Este comando inicia uma compilação manualmente. Ele recebe como parâmetro o nome de uma configuração de compilação, e cria um recurso de Build. Uma compilação bem-sucedida gera uma imagem de container no ImageStream (fluxo de imagem) da aplicação. Caso a configuração de compilação tenha um gatilho vinculado ao ImageStream em questão, um processo de implantação se inicia automaticamente.

Exemplo: `oc start-build bc/nome_da_buildconfig`

### `oc cancel-build`
Cancela uma compilação em andamento. Caso uma compilação da versão errada da aplicação tenha sido iniciada ou a versão do código sendo compilada tenha erros, você pode querer cancelá-la antes de que a compilação falhe. É apenas possível cancelar compilações em andamento ou em estado pendente. O parâmetro recebido é a configuração de compilação.

Exemplo: `oc cancel-build bc/nome_da_buildconfig`

### `oc delete`
Este comando serve para remover um recurso do OpenShift, e, como tal, pode ser usado para ambos Builds e BuildConfigs. Ao remover uma configuração de compilação, ela deixa de existir no cluster e precisa ser _recriada_, portanto o comando `start-build` deixa de funcionar até que outra seja criada.

Exemplo (BuildConfig): `oc delete bc/nome_da_buildconfig`

Exemplo (Build): `oc delete build/nome_da_build`

### `oc describe`
Este comando descreve um recurso do OpenShift, e imprime detalhes do recurso solicitado. Os detalhes impressos variam de recurso para recurso.

Exemplo (BuildConfig): `oc describe bc/nome_da_buildconfig`

Exemplo (Build): `oc describe build/nome_da_build`

### `oc logs`
Este comando exibe logs vinculados ao recurso fornecido, caso o recurso fornecido suporte logs. O flag `-f` mantém os logs no buffer, e atualiza automaticamente com linhas novas.

Exemplo (BuildConfig): `oc describe bc/nome_da_buildconfig`

Exemplo (Build): `oc describe build/nome_da_build`

### `oc edit`
Este comando edita um recurso do OpenShift. A definição do recurso é aberta numa versão em buffer do editor de texto vim, e pode ser salva.

Exemplo: `oc edit bc/nome_da_buildconfig`

### Limite de exibição
Por padrão, compilações bem-sucedidas são mantidas nos registros por tempo indeterminado. Mas é possível alterar o limite de compilações mantidas através da sua BuildConfig. Para isso, usa-se os campos `successfulBuildsHistoryLimit` e `failedBuildsHistoryLimit`.

```yaml
apiVersion: "v1"
kind: "BuildConfig"
metadata:
 name: "minha-compilacao"
spec:
 successfulBuildsHistoryLimit: 5
 failedBuildsHistoryLimit: 2
```

Este tipo de limitação eleva a performance do cluster, pois menos coisas serão armazenadas em disco. Consequentemente, o etcd lida com menos informações. O comando `oc delete` mostrado acima também pode ser usado para remover compilações antigas.

### Verbosidade
Existem dois parâmetros de verbosidade de logs no OpenShift. O primeiro é global, definido pelo administrador do cluster, e é válido para todos os usuários e projetos do OpenShift. O segundo é feito pelo próprio desenvolvedor no momento de definir a BuildConfig.

A variável de ambiente `BUILD_LOGLEVEL` pode ser definida na sua BuildConfig, diretamente na definição dela, ou através do CLI. O valor desta variável varia de 0 a 5, e tem resultados diferentes. 

<center>

|Valor|Efeito|
|--|--|
|0|Valor padrão. Exibe logs de informação e erros.|
|1|Fornece informações básicas do processo.|
|2|Fornece informações detalhadas do processo.|
|3|Fornece informações detalhadas do processo e lista o conteúdo do arquivo.|
|4|Fornece as mesmas informações do nível 3.|
|5|Fornece todas as informações disponíveis, inclusive logs de push da imagem.|

</center>

## Exercícios
Isso conclui a aula prática da seção de gerenciamento de compilações no OpenShift. Para praticar o conteúdo aprendido, vamos fazer um exercício prático.

* [Questionário](questionario.md)
* [Exercício prático](exercicio-pratico.md)

## Referências
* [Documentação do OpenShift](https://docs.openshift.com/)

----
<p align="center"><a href="../aula01">❮ Aula anterior</a> | <a href="../aula03">Próxima aula ❯</a></p>