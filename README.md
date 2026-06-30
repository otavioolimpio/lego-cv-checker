# Sistema de Visão Computacional para Contagem e Validação de Peças LEGO

Este projeto implementa um sistema de visão computacional desenvolvido em Python para automatizar a contagem e validação de peças LEGO. O algoritmo foi projetado para rodar em ambiente Google Colab ou localmente (via VS Code), processando imagens submetidas no formato JPEG e realizando uma auditoria automática de inventário com base em um manual adaptado às condições reais da bancada de testes.

---

## Etapas do Processo

O pipeline de execução do projeto é estritamente dividido em 5 etapas modulares fundamentadas em conceitos acadêmicos de PDI:

1. **Configuração do Inventário Gabarito e Interface de Seleção:** Define o banco de dados das peças LEGO (ground truth) com suas respectivas regras morfológicas e quantidades esperadas, calibradas dinamicamente com base nas peças reais da bancada.
2. **Carregamento da Imagem e Segmentação por Cor (Espaço HSI):** Converte a imagem capturada para o espaço de cores **HSI** (Matiz, Saturação e Intensidade) para isolar com precisão o canal de Matiz de cada peça-alvo, reduzindo a interferência de iluminação espelular.
3. **Morfologia Matemática (Tratamento de Ruídos e Falhas):** Aplica operações algébricas compostas de Fechamento (`cv2.MORPH_CLOSE`), para unificar contornos e tapar os furos característicos de vigas Technic, seguido por uma Abertura (`cv2.MORPH_OPEN`) utilizando um elemento estruturante retangular ou elíptico para eliminação de ruídos espaciais de fundo.
4. **Extração de Contornos e Classificação por Descritores Geométricos Invariantes:** Extrai as curvas de borda da máscara binária limpa. Substitui a abordagem de caixas delimitadoras simples por retângulos rotacionados de área mínima (`cv2.minAreaRect`). Calcula descritores intrínsecos invariantes à rotação e translação como **Razão de Aspecto (Aspect Ratio)** e **Fator de Compactação (Extent)**, aplicando um filtro de dominância para selecionar estritamente o maior candidato geométrico compatível (eliminando falsos positivos).
5. **Validação do Kit e Exibição do Resultado Visual Final:** Realiza a auditoria de estoque comparando o inventário físico processado contra a meta do manual. Desenha os contornos rotacionados exatos ao redor da peça aprovada e projeta o rótulo descritivo no baricentro geométrico do objeto, gerando o veredito final do Kit.

---

## Base de Dados Utilizada (Subconjunto do Kit Calibrado)

O sistema adota como referência os componentes do **LEGO Education SPIKE Prime Set**, porém com calibrações de assinaturas cromáticas adaptadas para as variações e substituições físicas observadas na bancada real de ensaios:

* **Documento de Referência Original:** [SPIKE Prime Set Element Overview (Classroom Poster)](https://le-www-live-s.legocdn.com/sc/media/files/support/spike-prime/le_spike_prime_set_element_overview_classroom_poster_18x24inch-a7ecd36fbf6d15fd4c7617f4cb882531.pdf)

A tabela abaixo reflete exatamente o dicionário dinâmico configurado na Etapa 1 do ecossistema de software:

| Peça LEGO (Identificador no Código) | Cor Cadastrada | Qtd. Exigida (Gabarito) | Descrição do Componente / Assinatura de Bancada |
| :--- | :--- | :---: | :--- |
| **`cross_axle_2m_red`** | Vermelho | 8 | Eixo em cruz curto de fixação (Vermelho) |
| **`brick_2x4_red`** | Vermelho | 2 | Bloco retangular padrão 2x4 (Vermelho) |
| **`wire_clip_cross_green`** | Verde | 2 | Clipe de conexão rápida (Verde) |
| **`brick_2x4_green`** | Verde | 2 | Bloco retangular padrão 2x4 (Verde) |
| **`wire_clip_cross_magenta`** | Magenta | 2 | Clipe de conexão rápida (Roxo/Magenta) |
| **`technic_5m_beam_magenta`** | Magenta | 4 | Viga técnica intermediária de 5 furos (Magenta) |
| **`biscuit_1x3x3_magenta`** | Magenta | 6 | Bloco estrutural especial vazado em formato L |
| **`technic_5m_beam_blue`** | Azul | 0 | Viga técnica intermediária de 5 furos (Adaptação Azul) |
| **`technic_11m_beam_blue`** | Azul | 4 | Viga técnica longa de 11 furos (Adaptação Ciano/Azul) |

> ⚠️ **Nota de Engenharia e Robustez:** Peças que compartilham o mesmo canal cromático passam pela mesma máscara morfológica na **Etapa 2**. O discernimento definitivo de qual padrão está presente na mesa ocorre estritamente na **Etapa 4**, onde a árvore de decisão heurística analisa as dimensões normalizadas obtidas pelo retângulo rotacionado mínimo para rejeitar objetos concorrentes e ruídos de oclusão.

### 📸 Amostra de Peça para Teste

![Brick 2x4 Verde](imagens/Brick%202x4%20green.jfif)

### 📊 Catálogo de Referência do Kit

![Catálogo de Peças LEGO](imagens/Pecas%20LEGO.PNG)

---

## Tecnologias Utilizadas

* **Python 3.12+**
* **OpenCV** (Processamento Digital de Imagens)
* **NumPy** (Operações de Álgebra Linear e Indexação de Matrizes de Pixels)
* **Matplotlib** (Exibição e Plotagem de Gráficos e Imagens no Espectro RGB)

---

## Como Executar o Projeto

### No Google Colab:

1. Faça o upload do arquivo `.ipynb` deste repositório para o seu Google Drive.
2. Execute a **Etapa 1** e selecione via formulário `@markdown` interativo o modelo exato da peça LEGO que deseja auditar.
3. Execute a **Etapa 2**, clique no botão de upload e selecione a imagem da mesa contendo todas as peças misturadas (formatos `.jpg`, `.jpeg` ou `.jfif`).
4. Execute as células subsequentes em ordem cronológica (`Shift + Enter`) para processar as transformações espaciais e visualizar o relatório de conferência final.

### No VS Code (Local):

1. Certifique-se de instalar as dependências via terminal em conformidade com as versões recentes do NumPy (evitando erros de atributos descontinuados como `int0`):
   ```bash
   pip install opencv-python numpy matplotlib
