# Análise de Dados -  Cliente: “O Mercado”

### 1. Equipe e Documentação:

Projeto individual - Carla Bruckmann.

- [Documentação](https://www.notion.so/Documenta-o-1c1d4ec7c31680028b48f22b5bb10cc4?pvs=21)
- [Planilha](https://docs.google.com/spreadsheets/d/1MGBgbYxkxNDSzJEIpB21-fjVKRQPNNd6xVdKlGZZ4hk/edit?gid=918502877#gid=918502877)
- [Apresentação](https://docs.google.com/presentation/d/1ohcnpA49ciVYfdtTcZ1p4hTc7BL4ZGN3MdTtNt8mOX4/edit?usp=sharing)
- [Vídeo](loom.com/share/999dfe3550a64a5e9f7a604d26cdd14c)

### 2. Objetivo da Análise:

Esta análise tem como objetivo orientar **"O Mercado"** na adaptação e reconhecimento de perfil de clientes, otimizando estratégias **retenção** e **crescimento**. Para isso, será aplicada a metodologia **RFM (Recência, Frequência e Valor Monetário)**, que permite segmentar os clientes com base em seu comportamento de compra.

Através dessa segmentação, buscamos compreender melhor o perfil dos clientes e identificar oportunidades de ação mais eficientes e personalizadas.

- ❓ **Perguntas Norteadoras**
    
    **🧍‍♂️ Quem são os clientes da loja "O Mercado"?**
    
    - Quais são suas características demográficas (idade, estado civil, filhos)?
    
    **🧩 Como segmentar a base de clientes de forma eficaz?**
    
    - Quais são os segmentos identificados com base nas pontuações RFM?
    - Qual é o perfil e o valor de cada segmento para o negócio?
    
    **🎯 Onde concentrar os esforços da empresa?**
    
    - Quais segmentos são mais valiosos e merecem atenção especial?
        - Quais apresentam maior potencial de crescimento ou risco de abandono?
    
    **📈 Como otimizar as estratégias de marketing e retenção?**
    
    - Quais ações personalizadas são mais eficazes para cada perfil de cliente?
    - Como aumentar a fidelização dos clientes mais valiosos?
    
    **🛒 Qual é o desempenho em volume de vendas?**
    
    - Quais categorias de Produtos geram maior valor monetário?

### 3. Ferramentas e Tecnologias:

- Google Planilhas e Apresentação.
- Notion (Documentação e Gerenciamento de Tempo).

### 4. Fonte dos Dados:

Origem dos dados: 

[Tabela de Clientes.](https://docs.google.com/spreadsheets/d/1NxJ_OwvxcU5m-0XRuKvc4ZZa4btbm4IVN9UWv-TzxfE/edit?gid=0#gid=0)

[Tabela de Transações.](https://docs.google.com/spreadsheets/d/1mLQ7TFStIrCK0Rns5o3OiY_DlMJsaO9Hf4qiyLwxWkg/edit?gid=0#gid=0)

[Tabela Resumo de Compras.](https://docs.google.com/spreadsheets/d/1-s27h_IauM88qjoZ9xxvL4_NPAA_dtQ31RFT3oTslNo/edit?gid=1803379532#gid=1803379532) 

Período de dados: de 30/07/2020 à 31/12/2022.

### 5. Pré-processamento:

- **Importação de dados**
    - Utilização de `IMPORTRANGE` para realização de importação de dados da fonte.
    - Criação de planilhas de database (`db_clientes`, `db_transacoes`, `db_resumo_compras`).
    - Criação de planilhas de verificação e processamento de dados (`clientes`, `transacoes`, `resumo_compras`).
- **Limpeza dos Dados**
    1. **Planilha `clientes` :**
        1. Contagem e identificação de Dados Ausentes:
            - Utilizando a fórmula `COUNTBLANK` , identificou-se 24 dados ausentes na variável `salario_anual_dolar` :
                
                
                | **salario_anual_dolar** |
                | --- |
                | 24 |
                - Solução: Dados mantidos para posterior manutenção e substituição como “*Não informado*”, em junção/ transformação de dados consolidados, utilizando `IF / OR` condicional.
                - Justificativa: Preservação de informações relevantes presentes em outras variáveis e tabelas do banco de dados, que seriam perdidas com a exclusão das linhas com dados ausentes. Sua retirada poderia significar perda de dados interessantes à análise RFM.
        2. Verificação e Remoção de duplicatas:
            - Verificação de itens duplicados em variável `id_cliente`  realizada através de tabela dinâmica, nenhum item encontrado.
        3. Verificação e gerenciamento de dados fora do escopo ou atípicos: 
            - Utilizando tabela dinâmica, identificou-se que a variável `ano_nascimento` apresenta 3 casos de insistência com o período analisado, sendo anos atípicos (abaixo do esperado):
                
                
                | *id_cliente* | *ano_nascimento* |
                | --- | --- |
                | 1150 | 1899 |
                | 7829 | 1900 |
                | 11004 | 1893 |
                - Solução: Dados mantidos para posterior manutenção e imputação por mediana, oferecendo uma estimativa de anos, minimizando o impacto de valores extremos na análise. A imputação será realizada na criação de nova variável ao calcular a `idade` do cliente, utilizando a função `MEDIAN` para calcular a mediana e a função `IF condicional(≤1900)`  para substituir os 3 valores atípicos.
                - Justificativa: Preservação de informações relevantes presentes em outras variáveis e tabelas do banco de dados, que seriam perdidas com a exclusão das linhas com dados atípicos. Sua retirada poderia significar perda de dados interessantes à análise RFM.
            - Variáveis `nivel_educacao` e `estado_civil` seguem padrão consistente, com a mesma variação de grafia ou abreviações.
            - A quantidade de crianças nas colunas `criancasatedez_anos` e `criancasmaisdez_anos` são compatíveis com os anos de nascimento dos clientes.
            - Variável `data_entrada` está compatível com o período de análise **30/07/2020 a 31/12/2022.**
            - Variável `resposta_campanha` está com padrão consistente, com variação 0 e 1.
    2. **Planilha `transacoes`:**
        1. Contagem e identificação de Dados Ausentes:
            
            Utilizando a fórmula `COUNTBLANK` , identificou-se 7 dados ausentes na variável `id_cliente` :
            
            | **id_cliente** |
            | --- |
            | 7 |
            - Solução: Exclusão de dados nulos, utilizando `QUERY` ao importar.
            - Justificativa: Os dados nulos não possuem origem (IDs), sendo impossível identificar a causa. A separação não compromete a representatividade de dados não nulos.
        2. Verificação e Remoção de duplicatas:
            - Verificação de itens duplicados em variável `id_transacao` realizada através de tabela dinâmica, nenhum item encontrado.
        3. Verificação e gerenciamento de dados fora do escopo ou atípicos:  
            - Variável `data_transacao` está compatível com o período de análise **30/07/2020 a 31/12/2022.**
            - Variável `lugar_transacao` segue padrão consistente, com a mesma variação de grafia ou abreviações.
    3. **Planilha `resumo_compras`:**
        1. Contagem e identificação de Dados Ausentes:
            - Verificação de identificação de dados ausentes ou nulos realizada utilizando a fórmula `COUNTBLANK`, nenhum item encontrado.
        2. Verificação e Remoção de duplicatas:
            - Verificação de itens duplicados em variável `id_cliente`  realizada utilizando tabela dinâmica e `FILTER,` itens identificados com a mesma quantidade de totais:
            
            | **id_cliente** | **total_vinho** | **total_frutas** | **total_carnes** | **total_peixes** | **total_doces** | **total_outros** |
            | --- | --- | --- | --- | --- | --- | --- |
            | 1646 | 4 | 9 | 12 | 11 | 2 | 8 |
            | 1646 | 4 | 9 | 12 | 11 | 2 | 8 |
            | 2656 | 23 | 1 | 7 | 0 | 4 | 24 |
            | 2656 | 23 | 1 | 7 | 0 | 4 | 24 |
            | 2666 | 519 | 50 | 167 | 130 | 0 | 41 |
            | 2666 | 519 | 50 | 167 | 130 | 0 | 41 |
            | 4418 | 671 | 47 | 655 | 145 | 111 | 15 |
            | 4418 | 671 | 47 | 655 | 145 | 111 | 15 |
            | 5721 | 611 | 76 | 749 | 59 | 45 | 26 |
            | 5721 | 611 | 76 | 749 | 59 | 45 | 26 |
            | 7990 | 9 | 1 | 2 | 3 | 2 | 0 |
            | 7990 | 9 | 1 | 2 | 3 | 2 | 0 |
            | 8207 | 28 | 0 | 9 | 3 | 0 | 0 |
            | 8207 | 28 | 0 | 9 | 3 | 0 | 0 |
            | 9723 | 530 | 142 | 217 | 62 | 9 | 56 |
            | 9723 | 530 | 142 | 217 | 62 | 9 | 56 |
            | 10652 | 240 | 67 | 500 | 199 | 0 | 163 |
            | 10652 | 240 | 67 | 500 | 199 | 0 | 163 |
            - Solução: Exclusão de dados duplicados, utilizando `UNIQUE` ao importar.
            - Justificativa: Os dados duplicados foram excluídos para garantir a integridade da análise.
        3. Verificação e gerenciamento de dados fora do escopo ou atípicos:  
            - Todas as variáveis seguem padrão consistente, não possuem valores negativos ou dados atípicos.
- **Transformações**
    - Compilações realizadas:
        1. Criação de planilha `dados_consolidados`, com finalidade de junção de dados necessários para análise, importados das tabelas `clientes`, `transacoes`, `resumo_compras` utilizando-se a fórmula `VLOOKUP`.
    - Criação de novas variáveis:
        1. **Planilha `clientes` :**
            - `*total_filhos*`: totalizadora. Soma total de crianças por id_cliente, utilizando a fórmula `SUM`.
            - *idade*: idade do cliente no ano de 2025 calculada com a fórmula `YEAR()`.  Realizada imputação em 3 dados atípicos, utilizando-se a função `MEDIAN` para calcular a mediana e a função `IF condicional(≤1900)`:
                
                
                | *id_cliente* | *idade* |
                | --- | --- |
                | 1150 | 55 |
                | 7829 | 55 |
                | 11004 | 55 |
            - `faixa_etaria`: categorização por idade/ faixa etária, utilizando função `IF`:
                
                
                | **18-39 anos** | Jovens adultos em ascensão profissional, conectados digitalmente, consumo guiado por tendências e experiências |
                | --- | --- |
                
                | **40-54 anos** | Estabilidade profissional e financeira, foco em conforto, qualidade e bem-estar familiar |
                | --- | --- |
                
                | **55-64 anos** | Preparação para aposentadoria, valorização de saúde, segurança e produtos duráveis |
                | --- | --- |
                
                | **65+ anos** | Aposentados ou em transição, preferência por praticidade, saúde, conforto e atendimento personalizado |
                | --- | --- |
        2. **Planilha `transacoes`:**
            - `id_cliente`: id unificada, utilizando tabela dinâmica.
            - `total_transacao`: totalizadora. Soma total de transações por id_cliente, utilizando tabela dinâmica  (valor`COUNT`.)
            - `ultima_data_transacao`: última data de transação efetuada por id_cliente, utilizando tabela dinâmica (valor `MAX`).
            - `total_transacao_loja`: totalizadora. Soma total de transações por id_cliente em loja, utilizando fórmula `COUNTIFS` .
            - `total_transacao_online`*:* totalizadora. Soma total de transações por id_cliente online, utilizando fórmula `COUNTIFS` .
            - `frequencia_transacoes`: categorização conforme o `total_transacao,` utilizando a função `IFS`:
                - "Baixa" (1-3 compras)
                - "Moderada" (4-7 compras)
                - Alta" (8+ compras)
        3. **Planilha `resumo_compras`:**
            - `total_compras`: soma total de valores por id_cliente, utilizando `SUM`.
    - Normalização/Padronização:
        1. Planilha `dados_consolidados:` 
            - Identificados clientes que possuem dados de cadastro (`id_cliente` e demais variáveis) e compras (`total_compras`), porém sem registos de transação (`total_transacao` e demais variáveis). Como solução, os dados foram mantidos como `‘Não encontrado’`. Utilizadas funções `IFERROR` e `VLOOKUP`.
                
                
                | **id_cliente** | **total_compras** | **total_transacao** | **ultima_data_transacao** |
                | --- | --- | --- | --- |
                | 3955 | 6 | Não encontrado | Não encontrado |
                | 4931 | 1730 | Não encontrado | Não encontrado |
                | 5376 | 1730 | Não encontrado | Não encontrado |
                | 5555 | 6 | Não encontrado | Não encontrado |
                | 6862 | 8 | Não encontrado | Não encontrado |
                | 8475 | 1608 | Não encontrado | Não encontrado |
                | 9931 | 9 | Não encontrado | Não encontrado |
                | 10749 | 178 | Não encontrado | Não encontrado |
                | 11110 | 5 | Não encontrado | Não encontrado |
                | 11181 | 8 | Não encontrado | Não encontrado |
            - Em variável `nivel_educacao`, efetuada tradução de categoria utilizando a função `LET/ IFS:`
                - De: *Grado o superior* - Para: *Ensino Superior*
                - De: *Posgrado* - Para: *Pós-Graduação*
                - De: *Secundarista* - Para: *Ensino Médio*
            - Em variável `estado_civil`, efetuada tradução de categoria utilizando a função `LET/ IFS:`
                - De: *Soltero* - Para: *Solteiro*
                - De: *Viuda/Viudo* - Para: *Viúva/Viúvo*
                - De: *Unión de hecho* - Para: *União Estável*
                - De: *Otros* - Para: *Outros*
            - Em variável `resposta_campanha`, efetuada alteração de descrição de valores booleanos, utilizando a função `IF`:
                - De: 0 - Para: Não
                - De: 1 - Para: Sim

### 6. Análise Exploratória:

- Criação de Planilha `analise_exploratoria,` contendo tabelas dinâmicas (gráficos em `dashboard`) correspondentes para avaliação do comportamento do cliente sob diferentes dimensões:
    1. Quantidade de clientes por Faixa Etária.
    2. Total de Compras por Faixa Etária.
    3.  Frequência de Compras por Faixa Etária.
    4. Média de Salário Anual por Nível de Educação.
    5. Média Total de Compras pelo Nível de Educação.
    6. Impacto de Filhos no Total de Compras.
    7. Impacto de Estado Civil no Total de Compras.
    8. Total de Transações em Canais e Resposta de Campanha.
    9. Comparativo de Canais de Venda e Faixa Etária.
    10. Faixa Etária e Resposta à Campanha.
    11. Categorias de Produtos com Maior Valor Monetário.

### 7. Análise RFM:

- **Cálculo de Quartis:**
    - Criada planilha `analise_RFM`, importando de planilha `dados_consolidados` as variáveis necessárias para cálculo de quartis.
    - Criada variável `periodo_dias_ultima_compra`, para calcular o total de dias entre `ultima_data_transacao` e data final do período de dados (31/12/2022), utilizando a função `DATE()`.
    - Identificados clientes com cadastro id e valor de compras, que não possuem transações. A solução aplicada foi considera-los como **recência máxima**, aplicando **999**, utilizando a função `IF OR` .
        
        
        | **id_cliente** | **total_compras** | **total_transacao** | **ultima_data_transacao** |
        | --- | --- | --- | --- |
        | 3955 | $6,00 | Não encontrado | Não encontrado |
        | 4931 | $1.730,00 | Não encontrado | Não encontrado |
        | 5376 | $1.730,00 | Não encontrado | Não encontrado |
        | 5555 | $6,00 | Não encontrado | Não encontrado |
        | 6862 | $8,00 | Não encontrado | Não encontrado |
        | 8475 | $1.608,00 | Não encontrado | Não encontrado |
        | 9931 | $9,00 | Não encontrado | Não encontrado |
        | 10749 | $178,00 | Não encontrado | Não encontrado |
        | 11110 | $5,00 | Não encontrado | Não encontrado |
        | 11181 | $8,00 | Não encontrado | Não encontrado |
- **Recência/ Frequência e Monetário:**
    
    
    | Quartil | Faixa de dados | Posição relativa |
    | --- | --- | --- |
    |  | 0 | 0 |
    | 1 | 25% menores valores | Mais baixo |
    | 2 | 50% (mediana) | Meio |
    | 3 | 75% | Mais alto |
    | 4 | 100% | Topo |
    - **Recência:** Dias desde a última compra ate 31/12/2022 **- Quanto menor, mais distante está a última compra.** Ex: Menos dias significam clientes mais recentes, valores menores ficam em quartis maiores.
    - Frequência: Número de Compras- Quanto menor, menos frequente.
    - Monetário: Soma dos Gastos - Quanto menor, menos gasta.
    
    | Quartil | **Recência** | Quartil | **Frequência** | **Monetário** |
    | --- | --- | --- | --- | --- |
    | 4 | 54 | 1 | 5 | $68,75 |
    | 3 | 134 | 2 | 10 | $396,00 |
    | 2 | 245 | 3 | 14 | $1.045,50 |
    | 1 | 999 | 4 | 27 | $2.525,00 |
- **Score RFM:**
    
    Com base no Score RFM, foram definidos os seguintes segmentos, de forma que cada cliente se enquadra exclusivamente em um deles:
    
    | Segmento | Descrição | RFM Score |
    | --- | --- | --- |
    | Clientes Premium | 📌 Recente, frequente e com bom valor. 
    💎 Grupo mais valioso — fidelizar e manter. | 444, 443, 434, 344, 442, 433 |
    | Clientes Fiéis | 📌 Frequência e gasto bons, recência variável. 
    💼 Valem ouro. | 343, 333, 334, 432, 423 |
    | Clientes em Potencial | 📌 Tendência de crescer, já deram sinais positivos. 
    🌱 Cultivar o relacionamento. | 243, 234, 324, 244, 322, 323, 424 |
    | Clientes em Risco | 📌 Já foram bons, mas esfriaram.
    🧯 Ativar com campanhas específicas. | 233, 232, 223, 224, 212 |
    | Em Recuperação | 📌 Comportamento fraco, mas ainda vivos.
    🔧 Ainda podem ser reativados com incentivo certo. | 123, 122, 132, 143, 133, 134, 121, 124, 144 |
    | Novos Clientes | 📌 Compraram recentemente, mas pouca frequência e valor. 
    🚀 Alta oportunidade de nutrição. | 421, 411, 412, 422, 414 |
    | Inativos ou Fracos | 📌 Inativos ou com comportamento inconsistente.
    🧊 Baixo potencial, foco limitado. | 112, 111, 211, 222, 311, 332, 312, 321, 221, 113, 114, 342 |
- **Tabelas Dinâmicas:**
    
    Criadas tabelas dinâmicas para análise e posterior gráfico em `dashboard`:
    
    1. Distribuição de Clientes po Segmentos.
    
    2. Comparação de Valor Monetário por Segmento.
    
    3. Faixa Etária por Segmento.
    

### 8. Conclusões:

- **Principais descobertas:**
    - **Faixa Etária Relevante:** Clientes entre **55 e 64 anos** apresentam a **maior frequência de compras** e o **maior volume de gastos**, sendo o grupo mais valioso atualmente. Já clientes entre **18 e 39 anos** têm menor participação em volume e frequência.
    - **Escolaridade e Consumo:** Clientes com **Pós-Graduação** destacam-se pelo **maior poder aquisitivo** e maior média de compras, sugerindo que o nível educacional influencia diretamente no comportamento de consumo.
    - **Perfil Familiar:** Clientes **sem filhos** e **casados** tendem a consumir mais, apontando correlações relevantes entre perfil familiar e gasto.
    - **Canal de Compra:** A **loja física** é o principal canal de vendas, mas o **canal online** apresenta maior **engajamento com campanhas de marketing**, sobretudo entre adultos e meia-idade.
    - **Segmentação de Clientes:**
        - O grupo **"Inativos ou Fracos"** representa **quase 30% da base**, sendo o mais numeroso.
        - O segmento **"Clientes em Potencial"** apresenta alto valor monetário e representa a **maior oportunidade de crescimento**
- **Insights gerados:**
    - **Foco em Faixa Etária 40-54 anos:** Estratégias devem priorizar esse grupo, devido à sua alta frequência de compras e contribuição ao faturamento.
    - **Valorização da Escolaridade:** Clientes com maior nível educacional são mais lucrativos — vale adaptar linguagem e produtos para esse público.
    - **Explorar o Canal Online:** Apesar da preferência pela loja física, o maior engajamento digital representa um **potencial de crescimento** não explorado.
    - **Segmentação Inteligente:** Perfis familiares (com/sem filhos, estado civil) influenciam no consumo. Segmentações com base nesses critérios podem melhorar a personalização.
    - **Aproveitar o Potencial dos Segmentos:** Ações específicas para segmentos como **Clientes em Potencial, Premium e em Risco** podem aumentar receita e retenção.
    - **Omnicanalidade como Estratégia:** Garantir uma experiência integrada entre físico e digital é essencial para manter o engajamento e a fidelidade.
- **Recomendações**
    1. **Personalizar campanhas para 40-50 anos**, com foco em praticidade, confiança e qualidade.
    2. **Segmentar ações por escolaridade**, promovendo produtos de maior valor agregado para públicos com maior nível de instrução.
    3. **Melhorar a experiência online**, com navegação intuitiva, frete grátis e promoções exclusivas, para aumentar conversão digital.
    4. **Criar ofertas específicas para perfis familiares**, diferenciando a comunicação para clientes com e sem filhos.
    5. **Aplicar estratégias específicas por segmento:**
        - **Inativos ou Fracos:** Campanhas de reativação com ofertas atrativas. Cupons, campanhas campanhas nostálgicas (“Sentimos sua falta”), pesquisas de satisfação.
        - **Clientes Premium e Fiéis:** Programas de fidelidade e valorização. Programas VIP, benefícios exclusivos, atendimento diferenciado.
        - **Clientes em Potencial:** Nutrição e incentivo à recompra. Combos, upsell, cupons progressivos.
        - **Clientes em Risco:** Monitoramento e retenção proativa. Alertas personalizados, descontos limitados, pesquisas para entender o motivo.
        - **Novos Clientes:** Incentivos para estimular a primeira recompra. Onboarding por e-mail, kit boas-vindas digital, tutoriais ou vídeos.
    6. **Integrar canais com promoções cruzadas:**
        - Loja física: **cupons exclusivos no balcão**.
        - Online: **frete grátis e campanhas no app/site**.
    7. **Entender o apelo da loja física:** Investigar motivos da preferência para reforçar esse canal de forma estratégica.
    8. **Avaliação contínua das ações**, com métricas claras e ajuste constante das estratégias conforme o comportamento dos clientes.

### 9. Limitações:

- **Dados Limitados:** Algumas informações relevantes (ex: renda, origem dos clientes, detalhes de campanhas, salários) não estavam disponíveis ou estavam incompletas.
- **Recorte Temporal:** A análise considera um período específico e pode não refletir mudanças recentes.
- **Perfis Generalizados:** Os perfis por faixa etária, escolaridade e estado civil representam tendências, não comportamentos individuais.
- **Segmentação Simples:** A segmentação RFM usou regras fixas, podendo não captar nuances mais complexas.
- **Ferramentas Utilizadas:** O uso de Google Sheets e Notion impôs limitações em termos de automação e visualização avançada.

### 10. Referências:

- **Referências utilizadas:**
    - [Documentação da Fórmula IMPORTRANGE. Google Support.](https://support.google.com/docs/answer/3093340?hl=pt#)
    - [Learn the COUNTBLANK Function in Google Sheets. Sheets Help.](https://www.youtube.com/watch?v=xnPehRE2GZQ)
    - [Tutorials. SheetsHelp.](https://sheetshelp.com/category/tutorials/)
    - [Recurso Query. UFPEL](https://wp.ufpel.edu.br/planilhasgoogle/modulo-intermediario/aula-3-funcoes-de-busca/query/).
    - [Função UNIQUE. Google Support.](https://support.google.com/docs/answer/10522653?hl=pt-BR)
    - [Outliers. Aquarela Advances Analytics.](https://aquare.la/o-que-sao-outliers-e-como-trata-los-em-uma-analise-de-dados/)
    - [Juntar todas as abas do Google Sheets - Função Query. Canal SGP(youtube).](https://www.youtube.com/watch?v=fXFNPH__nkg)
    - [Documentação da Fórmula PROCV. Google Support.](https://support.google.com/docs/answer/3093318?hl=pt)
    - [Documentação da Fórmula YEAR(). Google Support.](https://support.google.com/docs/answer/3093061?hl=pt-BR&ref_topic=3105385&sjid=3370694051131916819-SA)
    - [Tabela Dinâmica. Academia BePro (Youtube).](https://www.youtube.com/watch?v=jMdKyTiL3Hw)
    - [QUARTIL. Educador Interativo (Youtube).](https://www.youtube.com/watch?v=qlBk00ismYE)
    - [Análise RFM. IBM](https://www.ibm.com/docs/pt-br/spss-statistics/30.0.0?topic=marketing-rfm-analysis).
    - [Análise RFM - A chave para potencializar suas vendas. Canal Ciclo E-commerce (youtube).](https://www.youtube.com/watch?v=kWRAFkKX79I)
    - [Segmentação de Clientes - Eudito Magul (GitHub).](https://eudito.github.io/SegmentacaoClientes.html)
    - [Gemini AI](https://gemini.google.com/app?hl=pt-BR).
    - [Chatgpt AI](https://chatgpt.com/).
    - [Gamma AI](https://gamma.app/) 