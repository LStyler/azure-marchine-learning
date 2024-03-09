# Criando um modelo de previsão no Machine Learning Azure

Durante este laboratório sobre IA, irei criar um serviço de aprendizado de máquina no Azure simulando dados de uma empresa de aluguel de bicicleta.

Para utilizar o serviço de aprendizado de máquina do Azure, é necessário realizar uma assinatura dos serviços Microsoft Azure.

Após realizar a assinatura, deve-se se dirigir até a opção "Criar um recurso" e pesquisar pelo serviço "Azure Machine learning"

Agora é necessário configurar o Workspace, selecionando a qual assinatura ele será atrelado e selecionar um grupo de recursos, que basicamente é uma coleção de recursos que compartilham o mesmo ciclo de vida e as mesmas permissões.  <br>  Também utilizei as seguintes configurações:

<ul>
    <li>Nome: LaboratorioIA002</li>
    <li>Servidor: Us East</li>
    <li>Conta de armazenamento: LABIA02</li>
</ul>

As opções Cofre de chaves, **Application Insights** e Registro de contêiner serão geradas automaticamente, logo, não senti necessidade de altera-las.  <br>  Após finalizar as configurações, basta clicar em Examinar + criar e se direcionar ao Azure estúdio.

Dentro do Workspace, iremos nos dirigir até a aba ML automatizado e depois em Novo trabalho de ML automatizado.

Agora realizando as configurações básicas, utilizaremos o nome: *mslearn-bike-rental*  <br>  O nome do experimento: *mslearn-bike-rental*  <br>  em descrição foi adicionado o seguinte trecho "Aprendizado de máquina sobre aluguel de bicicleta"

Na aba "Tipo de tarefa e dado" foi selecionado o tipo de tarefa Regressão.  <br>  Na hora de criar a base de dados, adicionei o nome de *bike-rentals* com a descrição "Dados de aluguel de bicicletas", no tipo Tabular.  <br>  Em fonte de dados, selecionei De arquivo web e o link "https://aka.ms/bike-rentals" e esperei a validação.  <br>  Após a validação, Selecionei as seguintes configurações: 

<ul>
    <li>Formato do arquivo: Delimitado</li>
    <li>Delimitador: Vírgula</li>
    <li>Codificação: UTF-8</li>
    <li>Cabeçalhos de coluna: Somente o primeiro arquivo tem cabeçalhos</li>
    <li>Ignorar linhas: Nenhuma</li>
    <li>Conjunto de dados com dados de várias linhas: Desmarcado</li>
</ul>
 
Em esquemas não foi necessário alterar nada, pois já havia sido detectado automáticamente, apenas chequei se estava tudo correto e realizei a criação.  <br>  

Em configurações de tafera, devemos alterar a Coluna de destino para *rentals (Integer)* e editar as definições de configuração adicionais, alternando a métrica primária para *"NormalizedRootMeanSquaredError"*, desmarque as 3 opções e alterne Modelos permitidos para *"RandomForest"* e *"LightGBM"*.

Depois vá em Limites e configure as seguintes opções:
<ul>
    <li>Máximo de avaliações: 3</li>
    <li>Máximo de avaliações simultâneas: 3</li>
    <li>Máximo de nós: 3</li>
    <li>Limite de pontuação da métrica: 0.085</li>
    <li>Tempo limite do experimento (minutos): 15</li>
    <li>Tempo limite de iteração (minutos): 15</li>
    <li>Habilitar encerramento antecipado: marcado</li>
</ul>

As configurações de Validar e testar é:
<ul>
    <li>Tipo de validação: Divisão de validação de treinamento</li>
    <li>Validação de percentual de dados: 10</li>
    <li>Dados de teste: Divisão de teste de treinamento</li>
    <li>Teste percentual de dados: 10</li>
</ul>

Para finalizar a aba de Trabalho de treinamento, vamos terminei de configurar a aba de computação deixando da seguinte forma:

<ul>
    <li>Selecione o tipo de computação: Sem servidor</li>
    <li>Tipo de máquina virtual: CPU</li>
    <li>Tipo de máquina virtual: Dedicado</li>
    <li>Tamanho da máquina virtual: Standard_DS3_V2</li>
    <li>Número de Instâncias: 1</li>
</ul>

Após isso é só verificar se estar tudo correto em examinar e enviar o trabalho de treinamento e após aguardar alguns minutos (aqui foi 9 minutos) vai abrir uma nova janela mostrando o mostrando o status do processo e suas propriedades.

Para criar o ponto de extremidade, vamos ter que criar um modelo.  <br>  enquanto estamos no ML automatizado na visão geral, clicamos no hyperlink que está embaixo do "Nome do algoritmo", o meu como por exemplo, foi VotingEnsemble.

Após entrar no modelo, vamos em implementar, serviços web e adicionar as seguintes configurações:

<ul>
    <li>Nome: predict-rentals</li>
    <li>Descrição: previsão de ciclo de aluguel.</li>
    <li>Tipo de computador: Instância de Contêiner do Azure</li>
    <li>Habilitar autenticação: habilitado</li>
</ul>

Após configurar tudo corretamente, basta esperar que o Deploy status mude para *Succeeded*, o que demora em torno de 5 a 10 minutos

Após finalizar corretamente e sem problemas, basta ir em Pontos de extremidade e selecionar o predict-rentals.  <br>  após isto, basta ir clicar em teste e alterar o código Json pelo código que está localizado no [ponto-de-extremidade.json](https://github.com/LStyler/azure-marchine-learning/blob/main/ponto-de-extremidade.json)

O resultado deve ser similiar a:

<code>
 {
   "Results": [
     444.27799000000000
   ]
 }
</code>

