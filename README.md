# PoC-BI_Master
Repositório criado para a entrega da monografia do curso de BI_Master da PUC-Rio
===================================================

<!-- antes de enviar a versão final, solicitamos que todos os comentários, colocados para orientação ao aluno, sejam removidos do arquivo -->

# Título do Trabalho

#### Aluno: [Rodolfo de Oliveira](https://github.com/Rodolfo-de-Oliveira/PoC-BI_Master)
#### Aluno: [Juan Lourenço](https://github.com/ xpto) ==> Ajustar o link para o github do Juan
#### Orientadora: [Manoela Kohler](https://github.com/manoelakohler)


Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".
-------
- [Link para o código](https://github.com/link_do_repositorio/nome_do_arquivo_de_codigo). <!-- caso não aplicável, remover esta linha -->

---

### Resumo

Este trabalho visa criar uma análise preditiva da necessidade de se fazer correções em pedidos de aquisições de bens com base nos pedidos anteriores.

Uma empresa (nome da empresa omitido) emite, diligencia e recebe dezenas de milhares de pedidos de compras de bens mensalmente. Parte desses pedidos, atualmente cerca de 50%,  passa por algum tipo de revisão entre o momento de sua criação e a efetiva chegada do produto no armazém, porém essa revisão não é desejada, geralmente se trata de uma correção e significa atraso na entrega. 

O fluxo dessa atividade funciona da seguinte maneira: uma área da cia percebe a necessidade de aquisição de algum bem, munido dessa necessidade, um empregado cria o pedido em uma ferramenta de ERP (Enterprise Resourcing Planning), o formulário possui uma série de campos e informações, que em geral não são preenchidas de acordo com o esperado, um exemplo de equivoco que pode ocorrer é o mal preenchimento do Número do Material (**NM**). Esse pedido é então enviado a uma central que processa todos os pedidos e pequenas informações como a do exemplo são de díficil percepção. A central, munida do pedido cria uma requisição de compras para ser enviada a algum fornecedor, após o fornecedor receber o pedido, ele pode perceber que alguma informação não está coerente, pois o NM descrito pode ser de algo que ele não fornece, então é necessário que todo o processo retorne até o cliente inicial para que as correções sejam feitas e novamente a requisição de compras seja criada com as informações corretas, no entanto até a percepção do equívoco, muito tempo se passo 

Então o projeto é criar uma regra para ser aplicada sempre que um novo pedido de compras for criado, que retorne "Sim"/"Não" para a necessidade de revisão, ou então mostrar as dificuldades encontradas nesse modelo e o porque seu funcionamento não é confiável.

de necessidade de revisão, assim poderíamos gerar um alerta do tipo "historicamente itens com essas características necessitaram de revisão, verifique com cautela o preenchimento", entende?

---

Matrícula: 192.671.027

Matrícula: 123.456.789 ==> ***Ajustar a matrícula do Juan

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
