# Sistema de Visão Computacional para Contagem e Validação de Peças LEGO

Este projeto implementa um sistema de visão computacional desenvolvido em Python para automatizar a contagem e validação de peças LEGO. O algoritmo foi projetado para rodar em ambiente Google Colab, processando imagens submetidas no formato JPEG e realizando uma auditoria automática de inventário com base em um manual pré-definido.

---

## Etapas do Processo

O pipeline de execução do projeto é estritamente dividido em 5 etapas modulares:

1. **Configuração do Inventário Gabarito e Interface de Seleção:** Define o banco de dados das peças LEGO (ground truth) com suas respectivas cores e quantidades esperadas. Permite ao usuário selecionar visualmente qual peça-alvo deseja conferir através de um formulário interativo.

2. **Carregamento da Imagem e Segmentação por Cor (Espaço HSV):** Realiza o upload da imagem JPEG da mesa de trabalho contendo as peças. O sistema converte a imagem do padrão BGR para o espaço de cores HSV para isolar com precisão a peça-alvo através de filtros e máscaras cromáticas ajustadas.

3. **Morfologia Matemática (Tratamento de Ruídos e Falhas):** Aplica operações morfológicas de Fechamento (`cv2.MORPH_CLOSE`) e Abertura (`cv2.MORPH_OPEN`) utilizando um elemento estruturante. Esta etapa limpa a imagem binária gerada, preenchendo buracos internos causados por reflexos ou furos das peças e eliminando poeiras e ruídos do fundo.

4. **Extração de Contornos e Filtragem Geométrica Proporcional:** Rastreia e identifica individualmente cada objeto na imagem limpa. Utilizando algoritmos de extração de contornos, o sistema calcula a área absoluta de cada elemento e aplica um filtro baseado na **área proporcional**, diferenciando peças de tamanhos distintos da mesma cor.

5. **Validação do Kit e Exibição do Resultado Visual Final:** Efetua o cálculo do saldo de peças presentes na mesa em relação ao exigido pelo manual. O programa desenha caixas delimitadoras (bounding boxes) verdes ao redor dos objetos validados e apresenta na tela um relatório textual junto ao veredito final do kit.

---

## Tecnologias Utilizadas

* **Python 3**
* **OpenCV** (Processamento Digital de Imagens)
* **NumPy** (Operações de Álgebra Linear e Matrizes)
* **Matplotlib** (Exibição e Plotagem de Gráficos)
* **Google Colab Forms** (Interface de Usuário)

---

## Como Executar o Projeto

1. Crie um novo notebook no **Google Colab** ou faça o upload do arquivo `.ipynb` deste repositório.
2. Copie e cole os blocos de código correspondentes a cada etapa em células separadas.
3. Execute a **Etapa 1** e escolha o modelo de peça Lego que deseja inspecionar na barra lateral de parâmetros.
4. Execute a **Etapa 2**, clique no botão de upload e selecione a sua imagem no formato `.jpg` ou `.jpeg`.
5. Execute as etapas seguintes em sequência (`Shift + Enter`) para processar os filtros e gerar o relatório visual com o veredito final do Kit.
