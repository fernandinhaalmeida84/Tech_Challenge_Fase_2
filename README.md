# TECH CHALLENGE - FASE 2

# Predição de Aprovação de Pedidos De Cartão De Crédito

## 1. Identificação

| Campo | Valor |
|---|---|
| Turma | 2º ano – 2DTATBB – 4ª Entrada |
| Grupo | Grupo 33 |
| Data de entrega | 10/10/2026 |

## Integrantes

| Nome completo | RM | E-mail |
|---|---|---|
| Angelica Fonseca de Freitas | RM377836 | angelicafonsecabb@gmail.com |
| Fernanda de Oliveira Almeida | RM377812 | fernandaalmeida84@yahoo.com.br |
| Gabriela Souza e Silva | RM377814 | gsgabizinha@gmail.com |
| Juliana Oliveira Couto Espindola Vaz de Lima | RM377805 | juoliveiracouto@gmail.com |
| William Vladimir Rosales Merida da Silva | RM377758 | wvmerida@gmail.com |

## 2. Links da entrega

|Repositório| xxxx|	
|Vídeo executivo (≤ 5 min)| xxxx|
|Apresentação| xxx|

## 3. Objetivo
Desenvolver um modelo de Machine Learning capaz de prever se um solicitante de cartão de crédito possui perfil de “bom” ou “mau pagador”, apoiando o processo de análise de crédito e mitigação de riscos de inadimplência.

## 4. Problema de Negócio
A concessão de crédito é uma atividade essencial para as instituições financeiras, mas envolve riscos relacionados à inadimplência.
O objetivo deste projeto é utilizar dados históricos dos clientes para identificar padrões de comportamento e construir um modelo preditivo capaz de apoiar a decisão de aprovação de novas solicitações de cartão de crédito.

## 5. Bases utilizadas 
https://drive.google.com/file/d/1z4yEyiCE_CGCWbvAAZQZSz-5-E5T5eYd/view?usp=sharing
- applocation_record.csv: dados cadastrais e características dos clientes.
- credit_record.csv: histórico mensal de pagamentos dos clientes.

## 6. Tecnologicas utilizadas
-	Python
-	Google Colab
-	Pandas
-	Matplotlib
-	Seanborn
-	NumPy
-	Scikit-Learn
-	Git e Github

## 7. Estrtutura do repositório
├── data/ : bases de dados

├── docs: Apresentação executiva

├── notebooks/ : análises realizadas no python

├── README.md : documentação do projeto

└── requirements.txt : bibliotecas utilizadas


## 8. Principais Análises Exploratórias
- Distribuição das variáveis
- Perfil dos clientes
- Valores ausentes
- Duplicidades
- Histórico de pagamentos
- Relações entre características cadastrais e inadimplências

## 9. As modelagens utilizadas foram:
-	Regressão Logística
-	Árvore de Decisão
-	Random Forest
-	XGBoost

## 10. Avaliação
As métricas utilizadas foram:
-	Accuracy
-	Precision
-	Recall
-	F1-Score
-	Matriz de Confusão
- ROC-AUC

## 11. Resultados
Partimos de uma pergunta de negócio: dá para prever, só com o que o cliente declara no cadastro, quem vai ficar inadimplente? Tratamos os dados e entendemos a base (notebook 1), definimos a inadimplência com dois critérios e provamos que o histórico mais longo não podia entrar no modelo (notebook 2), comparamos dois modelos sem vazamento, separando perfis idênticos (notebook 3), e agora confirmamos tudo em clientes que os modelos nunca viram (notebook 4).

A resposta é: um pouco, e não o bastante para decidir sozinho. O cadastro tem um sinal fraco no conjunto, com valor concentrado no topo do ranking (os 5% de maior risco têm cerca de 5 vezes a taxa média de inadimplência), e quase nenhum sinal para o atraso leve. Isso vale para dois algoritmos bem diferentes, então o limite está na informação disponível, não na técnica. Um modelo assim serve como apoio de triagem para organizar a fila de análise, não como juiz de aprovação ou recusa. Se a instituição quiser crédito automatizado de verdade, o investimento certo é em mais e melhores dados.

  
