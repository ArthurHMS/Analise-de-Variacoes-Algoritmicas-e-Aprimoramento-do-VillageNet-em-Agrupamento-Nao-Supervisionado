# Análise de Variações Algorítmicas e Aprimoramento do VillageNet em Agrupamento Não Supervisionado

**Autor:** Arthur Santos  
**Instituição:** Departamento de Ciência da Computação (DCOMP) — Universidade Federal de São João del-Rei (UFSJ)

## 📌 Sobre o Projeto

Este repositório contém a implementação modular e os experimentos da pesquisa voltada à investigação e otimização estrutural do algoritmo de agrupamento não supervisionado **VillageNet** (Ballal et al., 2026).

O objetivo central é avaliar se alterações nos módulos internos do algoritmo (Etapas 1, 3 e 4) conseguem superar a precisão, a estabilidade e o tempo de execução da formulação padrão em bases públicas de *benchmark* e em dados clínicos reais de pacientes com Infarto Agudo do Miocárdio (IAM).

## 🧪 Variações Algorítmicas Investigadas

* **Etapa 1 (Criação das Vilas / Células de Voronoi):**
  * `kmeans_random` (Padrão do artigo: 1 inicialização aleatória)
  * `kmeans++`
  * `minibatch` (MiniBatch K-Means)
  * `kmedoids` (PAM — robustez a *outliers*)
* **Etapa 3 (Ponderação das Arestas do Grafo Inter-Vilas $A_{UV}$):**
  * `discrete` (Contagem simples original)
  * `continuous` (Decaimento Gaussiano contínuo pela distância até a fronteira $\eta$)
* **Etapa 4 (Detecção de Comunidades na Rede de Vilas):**
  * `leiden`
  * `spectral` (Spectral Clustering com estimativa automática de $k$ via *eigengap*)
  * `infomap`
  * `louvain_igraph`

## 📂 Estrutura Principal

* `src/experimentos_villagenet.ipynb`: Notebook principal contendo a classe `ModularVillageNet` e a bateria de experimentos nos *datasets* públicos (*Wine*, *Breast Cancer* e *Pen Digits*).
* `DIARIO_DE_BORDO.md`: Registro semanal do progresso, resultados preliminares e próximos passos.
* `requirements.txt`: Lista de dependências do projeto.

## 🚀 Como Executar Localmente

1. Crie e ative um ambiente virtual Python:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```
2. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```
3. Abra e execute o notebook `experimentos_villagenet.ipynb` no VS Code ou Jupyter Lab.

## 📚 Principais Referências

1. **BALLAL, A. et al.** VillageNet: Graph-based, Easily-interpretable, Unsupervised Clustering for Broad Biomedical Applications. *arXiv preprint arXiv:2501.10471v2*, 2026.
2. **TRAAG, V. A.; WALTMAN, L.; VAN ECK, N. J.** From Louvain to Leiden: guaranteeing well-connected communities. *Scientific Reports*, v. 9, n. 5233, 2019.
3. **DHILLON, I. S.; GUAN, Y.; KULIS, B.** Kernel k-means: Spectral clustering and normalized cuts. *Proceedings of the Tenth ACM SIGKDD*, p. 551–556, 2004.
4. **NG, R. T.; HAN, J.** CLARANS: a method for clustering objects for spatial data mining. *IEEE Transactions on Knowledge and Data Engineering*, v. 14, n. 5, p. 1003–1016, 2002.