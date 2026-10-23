# 📊 Inteligência Preditiva: Projeção de Faturamento

Painel executivo com algoritmos de previsão de séries temporais aplicados diretamente na camada de dados.

```sql dados_historicos
-- Base de dados com o histórico de faturamento real
WITH historico AS (
    SELECT 1 as id_mes, '2026-01' as mes, 12000 as faturamento, 'Realizado' as tipo
    UNION ALL SELECT 2, '2026-02', 14500, 'Realizado'
    UNION ALL SELECT 3, '2026-03', 13900, 'Realizado'
    UNION ALL SELECT 4, '2026-04', 16200, 'Realizado'
    UNION ALL SELECT 5, '2026-05', 18000, 'Realizado'
    UNION ALL SELECT 6, '2026-06', 21000, 'Realizado'
),
-- Cálculo dos coeficientes da linha de tendência (Regressão Linear: y = mx + b)
estatisticas AS (
    SELECT 
        regr_slope(faturamento, id_mes) as m,
        regr_intercept(faturamento, id_mes) as b
    FROM historico
),
-- Geração dos meses futuros para a previsão
proximos_meses AS (
    SELECT 7 as id_mes, '2026-07' as mes
    UNION ALL SELECT 8, '2026-08'
    UNION ALL SELECT 9, '2026-09'
)
-- União do passado real com a projeção preditiva futura
SELECT mes, faturamento, tipo FROM historico
UNION ALL
SELECT 
    p.mes, 
    ROUND(e.m * p.id_mes + e.b, 2) as faturamento, 
    'Projetado (Algoritmo IA/SQL)' as tipo
FROM proximos_meses p, estatisticas e
ORDER BY mes
```

### 📈 Gráfico de Tendência e Previsão de Receita
<LineChart 
    data={dados_historicos} 
    x=mes 
    y=faturamento 
    series=tipo 
    lineType=cardinal
/>

### 📋 Tabela Dimensional Descritiva
<DataTable data={dados_historicos}/>
