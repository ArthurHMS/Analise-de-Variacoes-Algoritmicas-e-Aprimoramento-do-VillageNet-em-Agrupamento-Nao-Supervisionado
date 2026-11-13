**Pesquisador:** Arthur Santos (DCOMP - UFSJ)  
**Repositório:** [Analise-de-Variacoes-Algoritmicas-e-Aprimoramento-do-VillageNet-em-Agrupamento-Nao-Supervisionado](https://github.com/ArthurHMS/Analise-de-Variacoes-Algoritmicas-e-Aprimoramento-do-VillageNet-em-Agrupamento-Nao-Supervisionado)

---

## Semana 1 — 7 de Novembro de 2026

### 1. O que foi feito nesta semana
* **Definição do Escopo e Documentação Inicial:** Elaboração dos slides de apresentação (5 minutos) e do documento formal da proposta de pesquisa em LaTeX, definindo os gargalos das Etapas 1, 3 e 4 do algoritmo VillageNet original (Ballal et al., 2026).
* **Configuração do Ambiente e Repositório:** Criação do repositório no GitHub e configuração do ambiente local em Python (`venv`) integrado ao VS Code (`experimentos_villagenet.ipynb`).
* **Implementação da Classe `ModularVillageNet`:** Desenvolvimento completo das 5 etapas do VillageNet de forma modular e desacoplada:
  * **Etapa 1 (Criação de Vilas / Voronoi):** Implementação das opções `kmeans_random` (padrão do artigo com 1 inicialização), `kmeans++`, `minibatch` e `kmedoids` (implementação própria via PAM para robustez a *outliers*).
  * **Etapas 2 e 3 (Fronteira $\eta$ e Grafo Inter-Vilas):** Cálculo vetorizado da projeção geométrica na linha entre centroides, seleção dos $\eta = 30$ pontos externos via `np.argpartition` e duas opções de peso para a matriz de adjacência $A_{UV}$: `discrete` (contagem simples original) e `continuous` (decaimento Gaussiano pela distância até a fronteira).
  * **Etapa 4 (Detecção de Comunidades):** Integração dos detectores `leiden`, `louvain_igraph`, `infomap` e `spectral` (com estimativa automática do número de grupos $k$ via heurística do *eigengap* da matriz Laplaciana).
* **Primeira Bateria de Testes (Benchmarks Públicos):** Execução comparativa das 8 configurações iniciais (Experimentos 1 e 2) nas bases públicas *Wine* ($m^*=3$), *Breast Cancer* ($m^*=2$) e *Pen Digits* ($m^*=10$), coletando NMI, ARI, Silhueta, erro no número de clusters $|m - m^*|$ e tempo de execução.

---

### 2. Principais Resultados e Achados Preliminares

| Dataset | Melhor Configuração Testada | NMI (Artigo Original) | NMI (Nosso Teste) | ARI (Artigo Original) | ARI (Nosso Teste) | $m$ Inferido ($m^*$ Real) | Tempo (s) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Wine** ($m^*=3$) | `KMeans-Random + Discreto + Leiden/Infomap` | $0.8400$ | **$0.8920$** | $0.8600$ | **$0.9134$** | $3$ ($3$) | $0.0023\text{s}$ |
| **Breast Cancer** ($m^*=2$) | `KMeans-Random + Discreto + Infomap` | $0.5300$ | **$0.6225$** | $0.5400$ | **$0.6971$** | $3$ ($2$) | $0.0461\text{s}$ |
| **Breast Cancer** ($m^*=2$) | `KMeans-Random + Discreto + Spectral` | $0.5300$ | $0.5662$ | $0.5400$ | $0.6092$ | **$2$ ($2$)** | $0.0631\text{s}$ |
| **Breast Cancer** ($m^*=2$) | `KMeans++ + Contínuo + Leiden` | $0.5300$ | $0.5313$ | $0.5400$ | **$0.6447$** | $3$ ($2$) | $0.0954\text{s}$ |
| **Pen Digits** ($m^*=10$) | `KMeans-Random + Discreto + Infomap` | $0.8300$ | **$0.8450$** | $0.7300$ | **$0.8099$** | $12$ ($10$) | $0.1248\text{s}$ |

1. **Superioridade do Infomap na Etapa 4 (Experimento 1):** O algoritmo *Infomap* (que assim como o WLCF utiliza *Random Walks*, mas otimiza a compressão de fluxo de informação) superou as métricas reportadas no artigo original nas três bases testadas, com destaque para a base médica *Breast Cancer* (salto de $\text{ARI}$ de $0.54$ para $0.6971$).
2. **Impacto da Ponderação Contínua e K-Means++ (Experimento 2):** Na base *Breast Cancer*, substituir a inicialização aleatória única por `K-Means++` e aplicar o peso contínuo na fronteira (`continuous`) fez o $\text{ARI}$ do detector *Leiden* saltar de $0.3296$ para $0.6447$, confirmando a hipótese de que a contagem discreta simples na fronteira sofre com ruído.
3. **Acerto exato de $k$ pelo Spectral Clustering:** O *Spectral Clustering* combinado à heurística do *eigengap* foi o único método capaz de descobrir o número exato de classes ($m=2$, erro zero) na base *Breast Cancer*.

