# 🍽️ Análise do Setor de Alimentação no Brasil: Explorando Mercado para Expansão no Power BI
<p align="left">
  <img src="https://img.shields.io/badge/power_bi-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
  <img src="https://img.shields.io/badge/status-concluído-brightgreen?style=for-the-badge" alt="Status: Concluído"/>
</p>

Este repositório contém o relatório desenvolvido em **Power BI** com foco no setor de alimentação no Brasil. O objetivo principal é identificar oportunidades de expansão para novos negócios, analisando dados públicos e transformando‐os em insights

🔗 **Acesse o Dashboard:**  
👉 https://sites.google.com/view/analise-setor-alimentacao-br?usp=sharing  (clique com o botão direito → Abrir em nova aba)

---

## Contextualização

O setor de alimentação é altamente competitivo e entender o mercado é fundamental para decisões relativas à abertura de novos negócios. Durante minha experiência como consultor em uma empresa júnior, percebi os desafios enfrentados por pequenos e médios empreendedores, especialmente a ausência de dados claros e estruturados para orientar estratégias de crescimento.

Este projeto transforma dados públicos da Receita Federal em inteligência de mercado, analisando:

- Distribuição territorial dos estabelecimentos ativos;
- Oportunidades geográficas;
- Riscos de mortalidade empresarial.

Além disso, dados demográficos complementam o estudo, oferecendo uma visão ampla sobre o comportamento do setor no Brasil.

---

## Perguntas Norteadoras da Análise

Antes do tratamento e modelagem dos dados, foram definidas questões-chave para direcionar o projeto e garantir objetividade nas métricas construídas:

- Como está distribuída geograficamente a rede de empresas ativas?
- Quais estados e cidades apresentam maior concentração?
- Quais CNAEs predominam no setor?
- Como evoluiu a criação de empresas ao longo dos anos?
- Quais motivos explicam a inatividade ou baixa?
- Quais regiões e municípios apresentam maior potencial de expansão?

---
## Fontes de Dados

Para a construção do dashboad, foram utilizadas bases públicas da Receita Federal e dados demográficos do IBGE, complementadas por tabelas auxiliares criadas para transformação e organização do modelo.

### Tabelas do Modelo

| Tabela | Descrição | Carregada no Modelo? |
|--------|-----------|---------------------|
| _Medidas | Tabela técnica utilizada para centralizar e organizar todas as medidas DAX; não contém dados brutos. | SIM |
| CALENDARIO | Tabela de datas completa com ano, mês, trimestre e outras colunas auxiliares para cálculos temporais. | SIM |
| ESTABELECIMENTO | Tabela principal, composta por dados da Receita Federal contendo CNPJ, CNAE, situação cadastral e localização geográfica. | SIM |
| REGIAO | Tabela de referência com nomes e códigos das regiões brasileiras, usada para enriquecimento dos dados. | Apenas Power Query |
| POPULAÇÃO POR MUNICIPIO | Base demográfica do IBGE com estimativas populacionais municipais, utilizada para cálculo de densidade e índices de oportunidade. | Apenas Power Query |
| MUNICIPIOS | Tabela de correspondência entre códigos IBGE, municípios e estados, utilizada para padronização no processo de ETL. | Apenas Power Query |
| MOTIVOS | Lista de motivos de situação cadastral (como encerramento voluntário ou omissão), utilizada para categorização. | Apenas Power Query |
| CNAE | Tabela auxiliar contendo códigos e descrições das atividades econômicas. | Apenas Power Query |

> **Nota:** As tabelas marcadas como *Apenas Power Query* foram utilizadas no processo de transformação e enriquecimento dos dados, mas não foram carregadas para o modelo tabular final.

---

## Limpeza e Transformação (Power Query)

Principais funções utilizadas:

- `Table.TransformColumnTypes` - Ajuste dos tipos de dados para garantir consistência entre colunas numéricas, de texto e datas.
- `Text.Trim`, `Text.Upper`, `Text.Select`  - Padronização do CNPJ e de outros campos textuais, removendo caracteres inválidos e unificando formatação.
- `Table.SelectRows`- Filtragem inicial para manter apenas estabelecimentos pertencentes ao setor de alimentação
- `Table.RemoveColumns` - Exclusão de campos irrelevantes, deixando o modelo mais limpo e performático.
- `Table.Distinct` - Tratamento das tabelas auxiliares para evitar duplicidades
- `Table.AddColumn` - Criação de uma coluna derivada para estimar o tempo de vida das empresas
- `Table.NestedJoin` - Integração da base principal com tabelas auxiliares


Além disso:

- criação de coluna para estimar tempo de vida das empresas;
- função customizada para remoção de acentos e padronizar textos para garantir consistência nos joins entre tabelas.

---

##  Modelagem e Relacionamentos

###  Relacionamentos utilizados

| From Table | From Column | To Table | To Column | Cardinalidade |
|------------|-------------|----------|-----------|---------------|
| ESTABELECIMENTO | DATA DE INÍCIO ATIVIDADE | CALENDARIO | Data | Many-to-One |

---

## Medidas em DAX

Medidas criadas para:

- calcular tamanho real do mercado
- medir risco e mortalidade
- identificar oportunidades de expansão

Principais medidas/funções utilizadas:

- **Empresas Ativas**
  - `CALCULATE`, `COUNTROWS`
- **Empresas Inativas / Baixadas**
  - `CALCULATE`, `IN`
- **Mortalidade (%)**
  - cálculo de proporção
- **Densidade de Mercado**
  - `DIVIDE`, `SUM`
- **Índice de Oportunidade**
  - `SWITCH(TRUE())`, `MAXX`, `ALL`, `DIVIDE`
- **Pareto de Motivos de Inatividade**
  - `SUMMARIZE`, `FILTER`, `ALLSELECTED`, `SUMX`

---

## Visualizações Criadas

- Cartões de KPIs -  Exibição da quantidade de empresas ativas, empresas inativas/baixadas e da taxa de mortalidade calculada em DAX.
- Mapa de Distribuição Geográfica por município - Mapa do brasil destacando a concentração de empresas ativas por município, variando a intensidade de cor conforme o volume.
- Gráfico de Rosca por CNAE - Representação da fatia das principais atividades econômicas do setor com base na classificação CNAE
- Gráfico de Linhas: evolução temporal - Visualização da quantidade de novas empresas abertas ao longo dos anos, com linhas diferentes por região do país.
- Pareto de motivos de inatividade - Gráfico para ordenar e acumular os principais motivos de baixa ou inativação de empresas .
- Matriz de Oportunidades (Quadrante) - Gráfico de quadrante posicionando regiões de acordo com dois indicadores: Mortalidade e Densidade de Mercado.
- Tabela Resumo - Tabela consolidando para cada cidade os indicadores de Mortalidade, Empresas Ativas, Densidade de Mercado e o Índice de Oportunidade 


---

## Principais Insights

- Forte concentração de empresas no Sudeste — especialmente SP, RJ e MG.
- CNAEs predominantes:
  - Lanchonetes (30%)
  - Restaurantes (27%)
  - Delivery e refeições domiciliares (23%)
- Aberturas cresceram a partir de 2007, com pico em 2024.
- 53% das baixas → extinção por encerramento ou liquidação voluntária.
- 29% → omissão de declaração.
- Mortalidade acumulada: 68%.

Esses resultados indicam que a inatividade no setor está fortemente associada a decisões internas de encerramento ( falta de rentabilidade, mudança de estratégia) e à falta de conformidade fiscal, reforçando a importância da gestão administrativa e contábil para a sobrevivência das empresas. 

A análise de quadrantes demonstra:

Quadrante Superior Esquerdo – Alta densidade, baixa mortalidade: Representa estados com poucas empresas em relação à população e baixo risco de fechamento.: Oportunidade ideal para expansão: mercado com demanda potencial alta e risco baixo. 

Quadrante Superior Direito – Alta densidade, alta mortalidade: Estado com poucas empresas, mas mercado arriscado. •Requer análise cautelosa: pode haver oportunidade, mas o risco de insucesso é maior; é necessário avaliar fatores locais antes de investir.

Quadrante Inferior Esquerdo – Baixa densidade, baixa mortalidade: Mercados saturados (muitas empresas para a população) mas seguros. Expansão pode ser menos prioritária, pois a competição é intensa, embora o risco seja menor. 

Quadrante Inferior Direito – Baixa densidade, alta mortalidade :Áreas com muitas empresas e alto risco de fechamento. Pouco atraente para expansão; mercado competitivo e instável, exige estratégias diferenciadas ou investimento mínimo 


Índice de Oportunidade:
 As três cidades com melhor desempenho são: Americas/SP, Ananindeua/PA e Anápolis/GO, considerando critérios como densidade de mercado e taxa de mortalidade;

### Iniciativas recomendadas 

- **Pesquisa de Mercado Regional** -Realizar estudos locais de comportamento do consumidor e análise competitiva
- **Estratégia de Entrada Piloto** - Adotar modelo piloto em uma cidade de cada região (SP, PA e GO) para validar o formato de negócio antes da escalabilidade. 
- **Fortalecimento de Gestão Financeira e Compliance** -Criar programas internos de governança e capacitação voltados a controladoria, contabilidade e gestão fiscal, reduzindo riscos de inatividade por omissão

---

##  Conclusões

O setor de alimentação no Brasil apresenta um cenário ambíguo: ao mesmo tempo em que evidencia forte  potencial de expansão  impulsionado pelo aumento do consumo fora do lar, pela consolidação do delivery , também revela fragilidades estruturais que comprometem a longevidade dos negócios.

A elevada taxa de mortalidade (68%), aliada ao fato de que 53% dos encerramentos são voluntários e 29% decorrem de omissão de declarações, indica que boa parte dos empreendimentos não quebram  por exclusivamente a alta concorrência ou baixa demanda no mercado, mas por deficiências internas. Isso sugere que gestão financeira, governança e conformidade fiscal são gargalos mais críticos.

Esse contraste revela o verdadeiro ponto estratégico: o mercado não está saturado, ele está mal gerido. Dessa forma, o risco não reside na falta de consumidores, mas na falta de gestão financeira e contábil, principalmente entre micro e pequenos negócios.

Ainda assim, os dados indicam que a expansão não deve ocorrer sem critério de seleção. A análise de densidade populacional versus taxa de mortalidade demonstra que o caminho ideal é uma expansão seletiva associando cidades com alta densidade de mercado (população / empresas ativas) e  baixa mortalidade, como por exemplo Americana (SP), Ananindeua (PA) e Anápolis (GO)  representam ambientes favoráveis com menor risco.

Portanto, o setor possui oportunidade real de crescimento, desde que sustentada por três pilares:

- Escolha criteriosa de localização,
- Profissionalização da gestão e compliance,
- E adequação do modelo de negócio às novas dinâmicas de consumo, especialmente delivery e consumo rápido.



## Como Executar o Relatório

1. Baixe o arquivo `.pbix` e as fontes de dados  deste repositório.
2. Abra no Power BI Desktop.
3. Atualize o endereço da fonte de dados baixada .

---

## Tecnologias Utilizadas

- Power BI Desktop
- Power Query (M)
- DAX
---

## 📬 Contato

🔗 LinkedIn: [David Nunes](https://www.linkedin.com/in/davidnunes9/) (clique com o botão direito → Abrir em nova aba)

---

