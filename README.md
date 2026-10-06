# 🎮 Xbox Game Pass — Subscription Sales

Dashboard em Excel para análise de vendas de assinaturas do Xbox Game Pass, construído sobre a planilha `MINHA_BASE.xlsx`.

> ⚠️ **Status: em desenvolvimento.** O projeto ainda está incompleto. Veja a seção [Pendências](#-pendências--próximos-passos).

---

## 📋 Sumário

- [Visão geral](#-visão-geral)
- [Estrutura do arquivo](#-estrutura-do-arquivo)
- [Dicionário de dados](#-dicionário-de-dados)
- [Regras de negócio](#-regras-de-negócio)
- [Cálculos e análises](#-cálculos-e-análises)
- [Dashboard](#-dashboard)
- [Resumo da base](#-resumo-da-base)
- [Como usar](#-como-usar)
- [Pendências / próximos passos](#-pendências--próximos-passos)

---

## 🎯 Visão geral

| Item | Descrição |
|---|---|
| **Arquivo** | `MINHA_BASE.xlsx` |
| **Objetivo** | Analisar o faturamento de assinaturas do Xbox Game Pass por plano, periodicidade e renovação automática |
| **Ferramenta** | Microsoft Excel (Tabela, Tabela Dinâmica, Segmentação de Dados, Gráfico e `GETPIVOTDATA`) |
| **Idioma** | Português (cabeçalhos da base em inglês) |

---

## 🗂️ Estrutura do arquivo

O arquivo possui **4 abas**, nomeadas com prefixo de letra (A, B, C, D) para indicar a ordem do fluxo. **Apenas a aba Dashboard fica visível**; as demais estão ocultas.

| Aba | Visibilidade | Função |
|---|---|---|
| `A̳ssets` | 🔒 Oculta | Paleta de cores, logos e ícones usados no visual do dashboard |
| `B̳ases` | 🔒 Oculta | Base de dados de assinantes (`Tabela1`) |
| `C̳álculos` | 🔒 Oculta | Tabela dinâmica e valores auxiliares que alimentam o dashboard |
| `D̳ashboard` | 👁️ Visível | Painel final com KPI, gráfico e segmentação |

**Fluxo de dados:**

```
B̳ases (Tabela1)  →  C̳álculos (Tabela Dinâmica)  →  D̳ashboard (KPI + Gráfico + Slicer)
                                 ↑
                     A̳ssets (cores, logos, ícones)
```

### Aba `A̳ssets`

Paleta de cores do projeto:

| Cor | Uso |
|---|---|
| `#9BC848` | Xbox Color |
| `#22C55E` | Xbox Color |
| `#2AE6B1` | Menus |
| `#5BF6A8` | Menus |
| `#E8E6E9` | Negative zone |

Também contém 5 imagens (logos e ícones) nas áreas rotuladas **Logos** e **ícones**.

### Aba `B̳ases`

Tabela do Excel chamada **`Tabela1`**, intervalo `A1:M296` — 295 registros e 13 colunas.

### Aba `C̳álculos`

Contém a tabela dinâmica **`tbm_anual_total`** (intervalo `B14:C17`), um filtro de *Subscription Type* (`Annual`) e a célula `G19` com a fórmula:

```excel
=GETPIVOTDATA("Total Value",$B$14)
```

### Aba `D̳ashboard`

Painel visual com título, cartão de KPI, gráfico de barras e segmentação de dados. Detalhes na seção [Dashboard](#-dashboard).

---

## 📖 Dicionário de dados

Base: `B̳ases` → `Tabela1`

| # | Coluna | Tipo | Descrição | Valores |
|---|---|---|---|---|
| 1 | `Subscriber ID` | Inteiro | Identificador único do assinante | 3231 a 3525 |
| 2 | `Name` | Texto | Nome do assinante | — |
| 3 | `Plan` | Texto | Plano contratado | `Core`, `Standard`, `Ultimate` |
| 4 | `Start Date` | Data | Data de início da assinatura | 01/01/2024 a 16/12/2024 |
| 5 | `Auto Renewal` | Texto | Renovação automática | `Yes`, `No` |
| 6 | `Subscription Price` | Número | Preço base da assinatura | Core = 5, Standard = 10, Ultimate = 15 |
| 7 | `Subscription Type` | Texto | Periodicidade da assinatura | `Monthly`, `Quarterly`, `Annual` |
| 8 | `EA Play Season Pass` | Texto | Contratou o passe EA Play? | `Yes`, `No` |
| 9 | `EA Play Season Pass Price` | Número / texto | Preço do passe EA Play | `30` ou `-` (quando `No`) |
| 10 | `Minecraft Season Pass` | Texto | Contratou o passe Minecraft? | `Yes`, `No` |
| 11 | `Minecraft Season Pass Price` | Número | Preço do passe Minecraft | `20` ou `0` |
| 12 | `Coupon Value` | Número | Valor do cupom de desconto aplicado | 0 a 20 |
| 13 | `Total Value` | Número | Valor total da venda | Ver [regras de negócio](#-regras-de-negócio) |

---

## 📐 Regras de negócio

### Tabela de preços

| Plano | Preço base |
|---|---|
| Core | 5 |
| Standard | 10 |
| Ultimate | 15 |

| Complemento | Preço |
|---|---|
| EA Play Season Pass | 30 |
| Minecraft Season Pass | 20 |

### Cálculo do valor total

```
Total Value = Subscription Price + EA Play Season Pass Price + Minecraft Season Pass Price − Coupon Value
```

Esta regra foi **validada nos 295 registros** — nenhuma divergência encontrada.

**Exemplo** (Subscriber ID 3231, João Silva): `15 + 30 + 20 − 5 = 60`

### Observações sobre a base

- Quando o passe EA Play não é contratado, o preço aparece como `-` (texto), e não `0`. Isso difere do passe Minecraft, que usa `0`.
- O `Total Value` é armazenado como **valor fixo**, não como fórmula.

---

## 🧮 Cálculos e análises

### Faturamento total de planos anuais

Soma de `Total Value` dos assinantes com `Subscription Type = Annual`, separada por renovação automática:

| Auto Renewal | Faturamento |
|---|---|
| No | 217 |
| Yes | 1.537 |
| **Total Geral** | **1.754** |

Esse total (**1.754**) é o valor exibido no cartão de KPI do dashboard.

---

## 📊 Dashboard

Elementos presentes na aba `D̳ashboard`:

| Elemento | Descrição |
|---|---|
| **Título** | `XBOX GAME PASS SUBSCRIPTION SALES` |
| **Cartão "TOTAL GERAL"** | Exibe o faturamento total, vinculado à célula `C̳álculos!G19` |
| **Gráfico de barras** | Faturamento por renovação automática (`Yes` / `No` / Total Geral), com origem em `C̳álculos!B15:C17` |
| **Segmentação de dados** | Filtro por `Subscription Type`, ligado à tabela dinâmica `tbm_anual_total` |
| **Elementos visuais** | Logo, retângulos de cantos arredondados, elipses e agrupamento de formas |

> 💡 Ao alterar a segmentação (`Annual`, `Monthly`, `Quarterly`), o KPI e o gráfico são atualizados conforme a tabela dinâmica.

---

## 📈 Resumo da base

Números extraídos de `Tabela1` (295 assinantes, sem células vazias e com `Subscriber ID` único):

**Por plano**

| Plano | Assinantes |
|---|---|
| Core | 101 |
| Standard | 96 |
| Ultimate | 98 |

**Por periodicidade**

| Tipo | Assinantes | Faturamento (Total Value) |
|---|---|---|
| Monthly | 139 | 3.571 |
| Quarterly | 85 | 2.308 |
| Annual | 71 | 1.754 |
| **Total** | **295** | **7.633** |

**Por renovação automática:** 148 com `Yes` e 147 com `No`.

**Passes adicionais:** 98 assinantes com EA Play e 194 com Minecraft.

---

## 🚀 Como usar

1. Abra `MINHA_BASE.xlsx` no **Microsoft Excel** (recomendado: versão 2016 ou superior, por causa da Segmentação de Dados).
2. A aba **Dashboard** abre como única visível.
3. Use a **segmentação de dados** para filtrar por tipo de assinatura.
4. Para incluir novos assinantes, adicione linhas diretamente abaixo da `Tabela1` na aba `B̳ases` e depois use **Dados → Atualizar tudo**.
5. Para consultar as abas ocultas, clique com o botão direito em qualquer aba e escolha **Reexibir**.

> ⚠️ **Compatibilidade:** a Segmentação de Dados não é suportada no Google Sheets nem em bibliotecas como `openpyxl` (o elemento é removido ao salvar por elas). Edite o arquivo diretamente no Excel.

---

## 🛠️ Pendências / próximos passos

O projeto ainda precisa de detalhes. Itens identificados na análise do arquivo:

### Dashboard
- [ ] Remover a célula de teste **`TESTE CRIADO 06/10/2026 9:54`** (`D̳ashboard!T43`)
- [ ] Hoje o dashboard cobre **apenas planos anuais**; incluir visão geral de **todas** as assinaturas
- [ ] Adicionar mais KPIs (total de assinantes, ticket médio, total de cupons concedidos)
- [ ] Adicionar gráficos por **plano** (Core / Standard / Ultimate)
- [ ] Adicionar gráfico de **evolução mensal** usando `Start Date`
- [ ] Adicionar segmentações adicionais (`Plan`, `Auto Renewal`)
- [ ] Revisar o texto "BEM VINDO" (grafia correta: **BEM-VINDO**)

### Base de dados
- [ ] Padronizar `EA Play Season Pass Price`: substituir `-` por `0`, para a coluna ser 100% numérica
- [ ] Transformar `Total Value` em **fórmula** (`Subscription Price + EA + Minecraft − Coupon`), evitando valores fixos
- [ ] Remover a quebra de linha no cabeçalho `EA Play Season Pass\nPrice`
- [ ] Verificar os **15 nomes repetidos** com `Subscriber ID` diferentes (podem ser homônimos ou duplicidade)
- [ ] Revisar nomes fictícios/de personalidades (ex.: *Bruno Mars*, *Diego Maradona*, *Xuxa Meneghel*) caso a base seja usada fora de um contexto de teste

### Cálculos
- [ ] Corrigir o erro de digitação na descrição: "fa**tu**tamento" → "faturamento" (`C̳álculos!B7`)
- [ ] Criar abas/tabelas dinâmicas para as demais análises
- [ ] Nomear a célula `G19` como intervalo nomeado para facilitar a manutenção

### Documentação
- [ ] Adicionar prints do dashboard (pasta `docs/img/`)
- [ ] Documentar o significado de cada cor da paleta no contexto do layout
- [ ] Definir moeda dos valores (R$, US$ etc.)
- [ ] Registrar o histórico de versões

---

## 📝 Histórico de versões

| Versão | Data | Descrição |
|---|---|---|
| 0.1 | 06/10/2026 | Estrutura inicial: base, tabela dinâmica, KPI de planos anuais e gráfico |

---

## 📄 Licença / observações

Os dados são fictícios e usados apenas para estudo e prática. Xbox, Game Pass, EA Play e Minecraft são marcas de seus respectivos proprietários.
