# Avaliação de tecnologias de estimativa de idade em consonância com a Lei nº 15.211/2025.

## 📌 Sobre o Projeto
Este repositório contém o código-fonte e o ambiente de simulação desenvolvidos para o Trabalho de Conclusão de Curso (TCC) em Engenharia de Software pela Universidade Estadual de Maringá (UEM). 

O objetivo principal é avaliar a viabilidade técnica e a segurança de modelos de Inteligência Artificial de código aberto (*open-source*) voltados para a **Estimativa de Idade Facial (Age Estimation)**. O projeto busca oferecer uma alternativa para que empresas brasileiras possam se adequar às exigências do **ECA Digital (Lei nº 15.211/2025)**, reduzindo custos com APIs na nuvem e garantindo a privacidade dos dados (*privacy by design*).

## ⚙️ Tecnologias e Modelos Avaliados
O projeto realiza testes de estresse e acurácia comparando três abordagens arquiteturais distintas:
* **DeepFace:** Framework baseado em Redes Neurais Convolucionais (CNNs) clássicas utilizando *transfer learning* sobre o modelo VGG-Face.
* **MiVOLO:** Arquitetura de ponta baseada em *Vision Transformers* (ViTs) com abordagem *multi-input*, processando simultaneamente a face (*FaceCrop*) e o corpo (*BodyCrop*).
* **DEX (Deep EXpectation):** Linha de base (*baseline*) acadêmica que formula a estimativa de idade através da expectativa matemática sobre probabilidades de classificação profunda.

## 🗄️ Bases de Dados
Os testes foram segmentados em duas fases experimentais utilizando *datasets* específicos:
* **Base Legítima (UTKFace):** Conjunto público com mais de 20.000 imagens faciais anotadas, utilizado para mensurar a acurácia bruta e o erro estatístico em condições ideais de alinhamento e iluminação.
* **Base de Ataque (OULU-NPU):** Conjunto de dados acadêmico referencial especializado na avaliação de Detecção de Ataques de Apresentação (Presentation Attack Detection - PAD) em dispositivos móveis. Em vez de manipulações sintéticas complexas, a base foca em ameaças do mundo real altamente prováveis no uso cotidiano da população geral, contendo capturas de fraudes como fotografias impressas e ataques de repetição em telas (replay attacks via smartphones ou tablets). No escopo deste projeto, amostras estáticas (frames) desta base serão injetadas na pipeline para simular tentativas de burla (spoofing) e mensurar o Índice de Vulnerabilidade estrutural dos modelos, avaliando se as ferramentas possuem robustez suficiente para atender aos requisitos de segurança estipulados pelo ECA Digital contra fraudes de identidade visual.

## 🧪 Pipeline Experimental
A metodologia de testes segue as diretrizes biométricas da **ISO/IEC FDIS 27566-1** e do Radar Tecnológico nº 5 da ANPD, dividida em 5 etapas sequenciais:

1. **Coleta de Dados:** Ingestão estruturada das imagens das bases UTKFace e Celeb-DF v2.
2. **Pré-processamento:** Localização da face via *backends* de detecção (RetinaFace e MTCNN), alinhamento facial e normalização da escala de *pixels*.
3. **Processamento Preditivo:** Execução dos algoritmos de regressão e expectativa (DeepFace, MiVOLO, DEX) para extrair a idade estimada contínua.
4. **Controle de Integridade:** Injeção das amostras de fraude da base Celeb-DF v2 para mensurar a taxa de falha dos modelos perante ataques.
5. **Emissão de Evidência (Decisão por Limiar):** Conversão da estimativa fracionária contínua em uma decisão lógica binária para verificação da maioridade legal ($\ge 18$ anos), conforme especificado pelo ECA Digital.

## 📊 Métricas de Avaliação
O desempenho das ferramentas é calculado a partir de três métricas primárias:
* **Acurácia de Classificação por Limiares:** Percentual de acerto binário na faixa de corte de 18 anos.
* **Índice de Vulnerabilidade (IV):** Taxa de Falsos Positivos perante ataques de fraude (injeção de *deepfakes*).
* **Erro Médio Absoluto (MAE):** 
  $$	ext{MAE} = rac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$$
  
  *Onde $n$ representa o total de imagens testadas, $y_i$ indica a idade cronológica real no dataset e $\hat{y}_i$ corresponde à idade estimada pelo modelo.*

## 📂 Estrutura do Repositório e Execução
Devido a restrições de processamento local, a infraestrutura deste projeto foi modulada para execução em nuvem através do **Google Colab** integrado ao Google Drive. 

O código está estruturado em *Notebooks* independentes para garantir o isolamento das variáveis de ambiente de cada tecnologia:
* `/notebooks/01_DeepFace_UTK.ipynb` - Experimento de acurácia base do DeepFace.
* `/notebooks/02_MiVOLO_UTK.ipynb` - Experimento de acurácia base do MiVOLO.
* `/notebooks/03_DEX_UTK.ipynb` - Experimento de acurácia base do modelo DEX.
* `/notebooks/04_Testes_Seguranca_CelebDF.ipynb` - Injeção de ataques sintéticos (*deepfakes*).

**Como executar:**
Importe os arquivos `.ipynb` diretamente para o seu ambiente do Google Colab. Certifique-se de realizar o download prévio dos *datasets*. Aqui, cabe ressaltar que a base de dados OULU-NPU deve ser solicitada formalmente e autorizada pelos pesquisadores que a desenvolveram.  alocando-os em uma pasta conectada ao seu Google Drive ou enviando diretamente para a sessão do Colab. O ambiente requer a instalação do Python 3.10+, PyTorch e dependências do OpenCV.

---
*Projeto acadêmico desenvolvido por Gabriel Saraiva de Gouveia. Orientação: Prof. Dr. Alisson Renan Svaigen.*
*Universidade Estadual de Maringá (UEM) - Departamento de Informática (DIN).*
