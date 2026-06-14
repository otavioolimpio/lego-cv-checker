# Sistema de Visão Computacional para Contagem e Validação de Peças LEGO

Este projeto implementa um sistema de visão computacional desenvolvido em Python para automatizar a contagem e validação de peças LEGO. O algoritmo foi projetado para rodar em ambiente Google Colab ou localmente (via VS Code), processando imagens submetidas no formato JPEG e realizando uma auditoria automática de inventário com base em um manual pré-definido.

---

## Etapas do Processo

O pipeline de execução do projeto é estritamente dividido em 5 etapas modulares:

1. **Configuração do Inventário Gabarito e Interface de Seleção:** Define o banco de dados das peças LEGO (ground truth) com suas respectivas cores e quantidades esperadas. Permite ao usuário selecionar visualmente qual peça-alvo deseja conferir através de um formulário interativo.

2. **Carregamento da Imagem e Segmentação por Cor (Espaço HSV):** Realiza o upload da imagem JPEG da mesa de trabalho contendo as peças. O sistema converte a imagem do padrão BGR para o espaço de cores HSV para isolar com precisão a peça-alvo através de filtros e máscaras cromáticas ajustadas.

3. **Morfologia Matemática (Tratamento de Ruídos e Falhas):** Aplica operações morfológicas de Fechamento (`cv2.MORPH_CLOSE`) e Abertura (`cv2.MORPH_OPEN`) utilizando um elemento estruturante. Esta etapa limpa a imagem binária gerada, preenchendo buracos internos causados por reflexos ou furos das peças e eliminando poeiras e ruídos do fundo.

4. **Extração de Contornos e Filtragem Geométrica Proporcional:** Rastreia e identifica individualmente cada objeto na imagem limpa. Utilizando algoritmos de extração de contornos, o sistema calcula a área absoluta de cada elemento e aplica um filtro baseado na **área proporcional**, diferenciando peças de tamanhos distintos da mesma cor.

5. **Validação do Kit e Exibição do Resultado Visual Final:** Efetua o cálculo do saldo de peças presentes na mesa em relação ao exigido pelo manual. O programa desenha caixas delimitadoras (bounding boxes) verdes ao redor dos objetos validados e apresenta na tela um relatório textual junto ao veredito final do kit.

---

## Base de Dados Utilizada (Subconjunto do Kit)

O sistema utiliza como referência técnica os componentes oficiais do **LEGO Education SPIKE Prime Set**. Para o escopo atual deste software, foi mapeado um subconjunto estratégico do catálogo contendo as peças principais utilizadas nos testes de validação:

* **Documento de Referência Original:** [SPIKE Prime Set Element Overview (Classroom Poster)](https://le-www-live-s.legocdn.com/sc/media/files/support/spike-prime/le_spike_prime_set_element_overview_classroom_poster_18x24inch-a7ecd36fbf6d15fd4c7617f4cb882531.pdf)

A tabela abaixo reflete exatamente as chaves e propriedades cadastradas no dicionário do código:

| Peça LEGO (Identificador no Código) | Cor Cadastrada | Qtd. Exigida (Gabarito) | Descrição do Componente |
| :--- | :---: | :---: | :--- |
| **`wire_clip_green`** | Verde | 4 | Clipe de conexão rápida (Verde) |
| **`cross_axle_red`** | Vermelho | 6 | Eixo em cruz de fixação (Vermelho) |
| **`brick_2x4_green`** | Verde | 2 | Bloco retangular padrão 2x4 (Verde) |
| **`brick_2x4_red`** | Vermelho | 2 | Bloco retangular padrão 2x4 (Vermelho) |
| **`technic_5m_blue`** | Magenta | 3 | Viga técnica intermediária de 5 furos |
| **`technic_11m_blue`** | Magenta | 1 | Viga técnica longa de 11 furos |
| **`biscuit_green`** | Verde | 1 | Bloco estrutural especial vazado (Verde) |

> ⚠️ **Nota de Engenharia:** Peças que compartilham a mesma cor de filtro (como os três modelos configurados na cor Verde ou os dois modelos configurados no espectro Magenta) passam pela mesma segmentação na **Etapa 2**. O discernimento final de qual objeto está na mesa ocorre estritamente na **Etapa 4**, onde o algoritmo avalia a janela de área proporcional calculada em pixels para aceitar apenas o modelo selecionado.
### 📸 Amostra de Peça para Teste
![Brick 2x4 Verde](imagens/Brick%202x4%20green.jfif)

### 📊 Catálogo de Referência do Kit
![Catálogo de Peças LEGO](imagens/Pecas%20LEGO.PNG)
---

## Tecnologias Utilizadas

* **Python 3**
* **OpenCV** (Processamento Digital de Imagens)
* **NumPy** (Operações de Álgebra Linear e Matrizes)
* **Matplotlib** (Exibição e Plotagem de Gráficos)
* **Tkinter** (Para seleção de arquivos localmente no VS Code)

---

## Como Executar o Projeto

### No Google Colab:
1. Faça o upload do arquivo `.ipynb` deste repositório para o seu Google Drive.
2. Execute a **Etapa 1** e escolha o modelo de peça Lego que deseja inspecionar no formulário interativo.
3. Execute a **Etapa 2**, clique no botão de upload e selecione a sua imagem de teste no formato `.jpg`, `.jpeg` ou `.jfif`.
4. Execute as etapas seguintes em sequência (`Shift + Enter`) para processar os filtros e gerar o relatório visual com o veredito final do Kit.

### No VS Code (Local):
1. Certifique-se de instalar as dependências via terminal: `pip install opencv-python numpy matplotlib`
2. Execute o arquivo de script principal.
3. Escolha o número da peça diretamente pelo menu interativo exibido no Terminal.
4. Uma janela nativa do sistema operacional se abrirá para que você selecione o arquivo de imagem da mesa.
