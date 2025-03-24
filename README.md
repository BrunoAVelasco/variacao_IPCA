Análise histórica do IPCA – Últimos 20 anos 

Arquivo: ingestao_IPCA
Objetivo: Fazer a coleta dos dados históricos do IPCA diretamente da API do Banco Central e salvar esses dados brutos em arquivos JSON.
Passos:
1 - Realiza requisição via requests na API oficial do Banco Central.
2 - Converte a resposta para JSON.
3 - Gera um timestamp com data e hora da coleta.
4 - Salva o arquivo JSON na pasta /Volumes/raw/ipca_bronze/ipca_raw/ com o timestamp no nome, garantindo versionamento.

Arquivo: Tabela_IPCA
Objetivo: Ler os dados brutos coletados, tratar a base e transformar em uma tabela estruturada.
Passos:
1 - Leitura do arquivo JSON salvo na camada Bronze.
2 - Conversão da coluna de data para o formato date.
3 - Garantia de que a coluna valor está no formato numérico (double).
4 - Escrita em formato Delta Lake na tabela silver.ipca_silver, garantindo rastreabilidade e eficiência para consultas futuras.

Arquivo:Resultados_IPCA
Objetivo: Realizar o processamento analítico dos dados do IPCA, gerando tabelas agregadas por ano e criando indicadores-chave.
Passos:
1 - Leitura da tabela com os dados previamente tratados.
2 - Criação da coluna de ano e adiciona uma coluna ano extraída a partir da data.
3 - Cálculo do IPCA acumulado anual, agrupa os dados por ano e soma os valores mensais do IPCA, considerando os últimos 20 anos.
4 - Salva a tabela gold.ipca_anual no formato Delta.
5 - Cálculo de KPIs: Média do IPCA dos últimos 5 anos, desconsiderando 2020 e 2021 (período pandêmico); Ano com maior IPCA acumulado; Ano com menor IPCA acumulado.
6 - Criação do dataframe de KPIs e junta os KPIs em um único DataFrame.
7 - Salva a tabela gold.ipca_kpis no formato Delta.
