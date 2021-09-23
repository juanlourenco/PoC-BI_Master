# PoC-BI_Master
Repositório criado para a entrega da monografia do curso de BI_Master da PUC-Rio
===================================================

# Previsão da necessidade de revisão em pedidos de compras de bens

#### Aluno: [Juan Lourenço](https://github.com/juanlourenco)
#### Aluno: [Rodolfo de Oliveira](https://github.com/Rodolfo-de-Oliveira)
#### Orientadora: [Manoela Kohler](https://github.com/manoelakohler)


Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Link para o código - Repositório Juan](https://github.com/juanlourenco/PoC-BI_Master).
- [Link para o código - Repositório Rodolfo](https://github.com/Rodolfo-de-Oliveira/PoC-BI_Master).

-------


---

### Resumo

Uma empresa (nome da empresa omitido) emite, diligencia e recebe dezenas de milhares de pedidos de compras de bens mensalmente. Parte desses pedidos, atualmente cerca de 40%, passa por algum tipo de revisão entre o momento de sua criação e a efetiva chegada do produto ao armazém, seja por falha no preenchimento, mudança tributária, erro do fornecedor, etc. Tais revisões não são desejadas, pois geram dispêndio de tempo trabalhado e potencial atraso na entrega da mercadoria.

O fluxo dessa atividade funciona da seguinte maneira: uma área da Cia. percebe a necessidade de aquisição de algum bem. Munido dessa necessidade, um empregado cria uma requisição de compra em uma ferramenta de Enterprise Resourcing Planning (ERP). O formulário possui uma série de campos e informações que podem ser preenchidas em desacordo com o esperado, um exemplo é o erro de preenchimento do Número do Material (NM), que é um identificador numérico único de algum material, outro é o código IVA (identificador do grupo de impostos aplicáveis ao bem em questão). Essa requisição é processada pelo ERP que gera um pedido de compras a ser enviado ao fornecedor.

O pedido, refletindo informações da requsição de compras, pode transportar erros na criação deste, bem como podem ocorrer erros na entrada de informações manuais do pedido, quando criado de forma não automatizada. Tanto o cliente interno solicitante do material, o emissor da requisição, o do pedido, a área de diligenciamento dos pedidos e o fornecedor podem identificar uma falha no pedido. Nesse caso é solicitada a correção, seja para o criador do pedido ou para a área de diligenciamento, que deverá efetuar o ajuste necessário. Vale destacar que estamos falando de dezenas de milhares de pedidos por mês, e pequenas informações como as do exemplo são de díficil percepção, além de que desde a criação do pedido até a sua correção muito tempo se passou, e a necessidade do bem ainda não foi atendida.

O presente projeto visa a criação de uma regra a ser aplicada sempre que um novo pedido de compras for criado, que retorne "Sim"/"Não" para uma previsão de necessidade de revisão, isto é, aponte no momento da geração do pedido, se aquele pedido potencialmente necessitará de revisão com base em suas características comparadas ao histórico de pedidos da Cia. revisados e não revisados, ou apresentar as dificuldades encontradas nesse modelo e o porque de seu funcionamento não ser confiável.

### 1. Introdução

Analisando as características dos pedidos de compra, foi identificado que uma boa opção para a solução a que se propunha seria a utilização da ferramenta RapidMiner. Foi então realizada uma extração aleatória de itens de pedido entregues no mês anterior, a partir do ERP. Tal arquivo foi importado para a mencionada ferramenta e diversas análises foram realizadas.

### 2. Modelagem

Os dados extraídos foram: Valor Líquido, Organização de Compras (o grupo interno da Cia. que emitiu aquele pedido), Tipo de Documento (se pedido com referência a contrato ou pedido spot, e se compra nacional ou internacional), o Centro (local de destino daquele material), o nome do criador do pedido, a gerência setorial em que o criador do pedido está lotado, a gerência de nível 1 (nível acima da gerência setorial) em que o criador do pedido está lotado, o tipo de contrato (codificação interna 1 e 2), uma outra variável que não detalharemos por medida de anonimicidade, chamada Ptnct), o grupo de mercadorias (código que agrupa diversos materiais com características semelhantes) e finalmente a coluna "itens não alterados" que tem valores 1 (não revisado) e 0 (revisado).

Para a modelagem dos dados foi primeiramente realizada uma anonimização ainda em Excel nos dados categóricos através de simples substituição de itens de mesmo nome para uma nomenclatura com numeração sequencial, por exemplo, os criadores de pedido "José", "Maria" e "João", passaram a ser "Criador1", "Criador2" e "Criador3".

Importada tal base *comma-separated values* (CSV) para o RapidMiner foi realizada sua divisão entre treino e teste através da ação *"Split Data"* na proporção 80%/20%. Sobre os dados de treino foi realizada normalização, posteriormente os campos nominais foram transformados em numéricos através da ação *"Nominal to Numerical"*, finalmente aplicamos o método *"Random Forest"*. Aplicamos também as saídas de cada ação à base de teste e ligamos os resultados à saída para avaliação dos resultados.

Após uma primeira rodada foram identificados *outliers* que estavam afetando o modelo em virtude de seus valores líquidos extremamente altos. Tais itens foram removidos da base e uma nova rodada foi realizada. Diversas tentativas foram feitas de remoção de campos através da ação *"Select Atributes"* e, analisando os resultados, foi definido que a melhor estratégia seria a remoção do campo "Gerência de Nível 1 do criador do Pedido" (primeiro.getPessoa.Gerência). Também foram realizados diversos testes nos métodos de normalização e nos tipo de codificação do conversor *"nominal to numerical"*, sempre visando o melhor resultado ao final do processamento.

Antes de utilizar a ação *"Random Forest"* também foram realizadas tentativas com *"Decision Tree"* (arquivo "poc_decision_tree.rmp"), *" k-nearest neighbors"* (KNN) (arquivo "poc_knn.rmp") e *Deep Learning* (arquivo poc_deep.rmp). Para todas as tentativas foram realizados diversos testes nas variáveis buscando os melhores resultados - no caso do "Random Forest" buscamos otimizações no número de árvores, tipo de critério, profundidade máxima, etc. 

### 3. Resultados

Considerando a saída do processo binária ("sim/não"), sabemos que até mesmo uma resposta do tipo "100% sim" ou "100% não" já teria 50% de acertos (caso a base esteja balanceada), portanto este é nosso ganho zero. Nota-se assim que o método *"Decision Tree"* com 55,24% de acurácia praticamente não avança em relação ao nosso ponto de partida. Já o método *"KNN"* alcançando 68% se mostra uma solução plausível - de modo grosseiro podemos dizer que ele acerta mais de 2/3 das afirmações (~66,6%). Enquanto o método *"Deep Learning"* atingiu 65 a 67% (sem fixação de semente os resultados variam), estando em patamar similar. Em fim, com um ganho marginalmente superior de acurácia, o método *"Random Forest"* se mostrou o mais indicado, alcançando 69,54%, além de uma velocidade de execução consideravalmente menor se comparada ao método *KNN*.

### 4. Conclusões

Apesar de não alcançar resultados realmente altos, considerando tratar-se de um problema real, com dados reais, a acurácia atingida foi um avanço e um sucesso para o uso real da ferramenta. Com 69,54% de acurácia os profissionais da Cia. poderão focar sua atenção com maior assertividade aos itens que potencialmente necessitarão de correções ainda nas etapas iniciais, reduzindo o risco de atraso na entrega dos materiais.

---

Matrícula Juan Lourenço: **192.167.049**

Matrícula Rodolfo de Oliveira: **192.671.027**

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
