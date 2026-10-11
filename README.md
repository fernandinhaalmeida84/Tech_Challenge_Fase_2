# Tech Challenge — Fase 2 | POSTECH Data Analytics

**Predição de risco de inadimplência em pedidos de cartão de crédito**

---

## 1. Identificação

| Campo | Valor |
|---|---|
| Turma | 2º ano – 2DTATBB – 4ª Entrada |
| Grupo | Grupo 33 |
| Data de entrega | 10/10/2026 |

### Integrantes

| Nome completo | RM | E-mail |
|---|---|---|
| Angelica Fonseca de Freitas | RM377836 | angelicafonsecabb@gmail.com |
| Fernanda de Oliveira Almeida | RM377812 | fernandaalmeida84@yahoo.com.br |
| Gabriela Souza e Silva | RM377814 | gsgabizinha@gmail.com |
| Juliana Oliveira Couto Espindola Vaz de Lima | RM377805 | juoliveiracouto@gmail.com |
| William Vladimir Rosales Merida da Silva | RM377758 | wvmerida@gmail.com |

---

## 2. Links da entrega

| Item | Link |
|---|---|
| Repositório | https://github.com/fernandinhaalmeida84/Tech_Challenge_Fase_2 |
| Vídeo executivo (≤ 5 min) | https://bit.ly/video_TC_fase2_grupo33 |
| Apresentação | https://github.com/fernandinhaalmeida84/Tech_Challenge_Fase_2/tree/main/docs/apresentacao_executiva.pdf |

---

## 3. O problema

A concessão de cartão de crédito é uma das atividades centrais de uma instituição financeira, e também uma das que mais expõem o banco ao risco de inadimplência. Atualmente, a decisão depende de regras e da análise de cada solicitação. Com dados históricos de clientes, o Machine Learning pode **ordenar os novos solicitantes por risco** e apoiar a triagem, para que o analista concentre a atenção onde ela mais importa.

**Pergunta que o projeto responde:** o perfil que o cliente declara no cadastro consegue prever quem vai ficar inadimplente?

### Variável alvo

A base de histórico traz, mês a mês, o `STATUS` de pagamento de cada cliente (`0` = atraso de até 29 dias, `1` = 30 a 59, `2` = 60 a 89, `3` = 90 a 119, `4` = 120 a 149, `5` = 150 dias ou mais, `C` = pago no mês, `X` = sem empréstimo). Consolidamos o histórico **por cliente** e criamos dois alvos:

| Alvo | Definição | Papel |
|---|---|---|
| **Inad90** | o cliente teve `STATUS` 3, 4 ou 5 em algum mês (atraso de 90 dias ou mais) | **principal**: é o atraso grave, que de fato gera perda |
| **Inad30** | o cliente teve `STATUS` de 1 a 5 em algum mês (atraso de 30 dias ou mais) | alternativo: mostra o quanto a definição do alvo muda a conclusão |

O atraso leve (`STATUS` 0) **não** conta como inadimplência, pois é comum (36,5% dos meses) e não representa perda. O limiar de 90 dias é uma convenção usual de mercado para inadimplência. Mantivemos os dois alvos porque a escolha do limiar é uma decisão de negócio, e queríamos mostrar o efeito dela.

O problema é **fortemente desbalanceado**: apenas **0,83%** dos clientes são Inad90 (302 de 36.457) e **11,77%** são Inad30. Por isso acurácia não serve como métrica (um modelo que aprova todo mundo já acerta 99%).

### Dataset

| Campo | Valor |
|---|---|
| Fonte | Base fornecida no enunciado do Tech Challenge: [BaseDadosFase2.zip](https://drive.google.com/file/d/1z4yEyiCE_CGCWbvAAZQZSz-5-E5T5eYd/view?usp=sharing) (cópia em `data/Raw/`) |
| `application_record.csv` (cadastro) | 438.557 linhas × 18 colunas, 438.510 IDs distintos (438.463 clientes únicos após o tratamento) |
| `credit_record.csv` (histórico) | 1.048.575 linhas × 3 colunas, 45.985 clientes (1 linha = 1 cliente em 1 mês, de 1 a 61 meses de histórico) |
| Base de modelagem | **36.457 clientes** que têm cadastro **e** histórico (só neles medimos inadimplência) |
| Período / versão | `MONTHS_BALANCE` é relativo ao mês da coleta (0 = mês atual); a base não informa datas absolutas |
| Licença de uso | Dados de uso acadêmico, fornecidos pela POSTECH para esta atividade |

Variáveis do cadastro:

| Variável | Tipo | Descrição |
|---|---|---|
| `ID` | identificador | Código do cliente (não entra no modelo) |
| `CODE_GENDER` | categórica | Sexo |
| `FLAG_OWN_CAR`, `FLAG_OWN_REALTY` | binária | Possui carro / imóvel próprio |
| `CNT_CHILDREN`, `CNT_FAM_MEMBERS` | numérica | Quantidade de filhos / de pessoas na família |
| `AMT_INCOME_TOTAL` | numérica | Renda total declarada |
| `NAME_INCOME_TYPE` | categórica | Tipo de renda (assalariado, servidor, pensionista…) |
| `NAME_EDUCATION_TYPE` | categórica | Escolaridade |
| `NAME_FAMILY_STATUS` | categórica | Estado civil |
| `NAME_HOUSING_TYPE` | categórica | Tipo de moradia |
| `OCCUPATION_TYPE` | categórica | Ocupação (30,6% ausente, tratada como "Unknown") |
| `FLAG_WORK_PHONE`, `FLAG_PHONE`, `FLAG_EMAIL` | binária | Possui telefone do trabalho / telefone / e-mail cadastrado |
| `DAYS_BIRTH`, `DAYS_EMPLOYED` | numérica | Dias desde o nascimento / no emprego atual (viraram `Idade` e `Anos_Trabalhados`) |
| `FLAG_MOBIL` | binária | Possui celular (constante, descartada) |

---

## 4. Como reproduzir

```bash
git clone https://github.com/fernandinhaalmeida84/Tech_Challenge_Fase_2.git
cd Tech_Challenge_Fase_2

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt
jupyter lab
```

Os notebooks também rodam no Google Colab (cada um tem o botão "Open in Colab" no topo). A base bruta está em `data/Raw/BaseDadosFase2.zip`, e as bases tratadas pelo notebook 1 estão em `data/`. Os notebooks procuram a pasta `data/` e, se não a encontram (no Colab, por exemplo), clonam o repositório sozinhos.

Execute os notebooks nesta ordem:

| # | Notebook | O que faz |
|---|---|---|
| 1 | `notebooks/01_Tratamento_e_AED.ipynb` | Limpeza das duas bases, outliers, perfis repetidos e análise exploratória |
| 2 | `notebooks/02_Definição_Target.ipynb` | Construção dos alvos Inad90 e Inad30 e da base de modelagem |
| 3 | `notebooks/03_Modelagem_Preditiva.ipynb` | Divisão por perfil, pré-processamento em `Pipeline`, treino e validação cruzada dos dois modelos |
| 4 | `notebooks/04_Avaliacao_e_Implicacoes_de_Negocio.ipynb` | Avaliação única no teste, limiar por custo, importância das variáveis e implicações de negócio |

**Semente fixa:** `RANDOM_STATE = 42`, declarada na primeira célula de cada notebook.

**Versão do scikit-learn:** fixamos `scikit-learn==1.6.1` (a do Colab). A divisão treino/teste (`StratifiedGroupKFold`) muda de uma versão para outra e, com ela, todos os números da seção 5. Com a versão fixada, rodar os notebooks na ordem acima, a partir de um ambiente limpo, reproduz os números abaixo.

Os gráficos do notebook 4 também ficam salvos em `results/figures/`.

---

## 5. Resultados

Avaliação **única** no conjunto de teste (7.087 clientes de 1.940 perfis; 74 inadimplentes no Inad90), com o corte padrão de 0,5. Entre parênteses, a faixa de 95% (bootstrap por perfil, 1.000 reamostragens). **Alvo principal: Inad90.**

| Modelo | Acurácia | Precisão | Recall | F1 | AUC-ROC | AUC-PR |
|---|---|---|---|---|---|---|
| Aprova todos (referência) | 98,96% | 0,0% | 0,0% | 0,000 | 0,500 | 0,010 |
| **Regressão Logística** | 64,6% (61,7 a 67,3) | 1,2% (0,7 a 1,9) | 41,9% (26,6 a 58,0) | 0,024 (0,013 a 0,036) | **0,599** (0,502 a 0,692) | 0,039 (0,018 a 0,088) |
| Random Forest | 98,9% (98,5 a 99,2) | 23,1% (0,0 a 100,0) | 4,1% (0,0 a 9,5) | 0,069 (0,000 a 0,146) | 0,577 (0,468 a 0,684) | 0,043 (0,016 a 0,097) |

Para o alvo alternativo (Inad30), o sinal é ainda mais fraco: AUC-ROC de 0,554 (Regressão Logística) e 0,545 (Random Forest), contra 0,500 de um sorteio.

**Modelo escolhido:** **Regressão Logística, no alvo Inad90.** Tecnicamente os dois modelos empatam: a diferença entre eles é menor que a incerteza das métricas, e a diferença pareada entre os dois inclui o zero em todos os cortes. Desempatamos pela **interpretabilidade**: na Regressão Logística, cada variável tem um coeficiente que o analista consegue ler e explicar, o que importa quando o modelo apoia uma decisão de crédito.

**Métricas priorizadas:** AUC-PR, recall, precisão e F1, e não a acurácia. Com apenas 1% de inadimplentes, a acurácia premia quem aprova todo mundo (98,96% sem identificar um único inadimplente). A AUC-PR mede a qualidade do ranking na classe rara. O recall mostra quantos inadimplentes o modelo pega, e a precisão mostra quantos bons clientes recusamos junto. O custo dos erros é assimétrico, pois aprovar um inadimplente custa muito mais que recusar um bom cliente. Por isso não fixamos um corte único: mostramos uma **curva de sensibilidade** para razões de custo de 1:1 a 100:1.

---

## 6. Principais conclusões

1. **O cadastro prevê pouco, e é isso que importa dizer ao negócio.** O modelo ordena os clientes um pouco melhor que um sorteio (AUC-ROC de 0,60 no atraso grave), mas o sinal é modesto. A conclusão vale para dois algoritmos bem diferentes, então o limite está na **informação disponível**, e não na técnica.
2. **O modelo serve como apoio de triagem, não como decisor único.** Com um corte que sinaliza os 5% de maior risco, a taxa de inadimplentes entre os sinalizados é cerca de 5 vezes a média (precisão de 5,5% contra 1,0%). Mesmo assim, a maioria dos sinalizados é de bons clientes: em cada 1.000 pedidos novos, 36 são sinalizados, 2 são inadimplentes e 34 são bons pagadores. A sinalização deve levar a uma **análise adicional**, e não a uma recusa automática.
3. **Compensa financeiramente apenas quando o calote custa muito mais que a recusa errada.** Com o calote custando 100 vezes uma recusa errada, a economia sobre "aprovar todos" é de cerca de **17%** (faixa de 2% a 32%). Com 50:1 a faixa já encosta no zero, e abaixo disso nenhum corte compensa. A razão de custo é uma decisão de negócio, e o projeto mostra o que ela muda.
4. **As variáveis que mais pesam são o tempo de trabalho e a idade**, nos dois modelos (coeficientes da Regressão Logística e importância por permutação do Random Forest). Em linguagem de negócio, a estabilidade profissional e a fase da vida do cliente carregam mais informação de risco que renda, estado civil ou sexo. A **renda declarada praticamente não discrimina** o risco.
5. **Dividir por perfil evita um falso sucesso.** 96,5% dos clientes dividem o cadastro com outros. Se misturarmos perfis iguais no treino e no teste, o Random Forest parece prever bem (AUC-ROC de 0,72), mas é uma ilusão; separando por perfil, ele cai para o patamar real, perto de 0,56.

### Limitações e próximos passos

- **Só 8% do cadastro tem histórico de crédito** (36.457 de 438.463 clientes). Medimos inadimplência apenas nesse recorte, então há viés de seleção, e o resultado pode não valer para quem nunca teve histórico.
- **Poucos casos positivos:** 74 inadimplentes no teste. As faixas de incerteza são largas, e por isso as apresentamos sempre junto com os valores.
- **Efeito de exposição:** quem tem mais meses de histórico tem mais chances de aparecer como inadimplente. Por isso o tamanho do histórico não entrou como variável.
- **Probabilidades não calibradas:** o `class_weight` desloca as probabilidades, então expressamos os cortes como "recusar os X% de maior risco".
- **Próximos passos:** incorporar variáveis de comportamento (renda comprovada, consultas a bureaus, uso do produto), calibrar as probabilidades e testar o modelo em uma amostra mais recente, em piloto controlado como apoio à triagem.

---

## 7. Estrutura do repositório

```
.
├── data/          base bruta (Raw) e bases tratadas pelo notebook 1
├── notebooks/     análise em ordem numerada (01 a 04)
├── results/       figuras do notebook 4 (results/figures)
├── src/           reservada para código de apoio
├── docs/          apresentação executiva (PDF)
├── submissao/     dados e script do PDF de submissão
├── README.md
└── requirements.txt
```

Detalhes e convenções em [`ESTRUTURA.md`](ESTRUTURA.md). Antes de enviar, percorremos o [`CHECKLIST.md`](CHECKLIST.md).

---

## 8. Tecnologias

Python 3.10 ou superior, pandas, NumPy, scikit-learn 1.6.1 (Regressão Logística, Random Forest, `Pipeline`, `StratifiedGroupKFold`, importância por permutação), Matplotlib, Seaborn e Jupyter. Versões em [`requirements.txt`](requirements.txt).
