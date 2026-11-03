# Avaliação da viabilidade de tecnologias de estimativa de idade em consonância com a Lei 15.211/2025 (ECA Digital)

**Trabalho de Conclusão de Curso – Engenharia de Software**  
**Universidade Estadual de Maringá (UEM)**

**Autor:** Gabriel Saraiva de Gouveia  
**Orientador:** Prof. Dr. Alisson Renan Svaigen  
**Ano:** 2026

---

## 1. Sobre o projeto

Este repositório reúne os códigos, notebooks, arquivos de processamento e resultados experimentais desenvolvidos durante o Trabalho de Conclusão de Curso (TCC), cujo objetivo é avaliar a viabilidade de tecnologias de estimativa de idade por reconhecimento facial, considerando os requisitos estabelecidos pela Lei nº 15.211/2025, conhecida como Estatuto Digital da Criança e do Adolescente (ECA Digital).

A pesquisa investiga o desempenho de ferramentas de código aberto utilizadas para estimar a idade de indivíduos a partir de imagens, considerando aspectos como acurácia, confiabilidade, precisão das estimativas e comportamento em decisões baseadas em limiares etários.

O estudo experimental concentra-se na comparação de duas tecnologias: **DeepFace e MiVOLO**, utilizando a base de dados pública UTKFace.

## 2. Objetivos

### Objetivo geral

Avaliar a eficácia e a viabilidade de mecanismos de estimativa de idade, com ênfase em tecnologias de código aberto, fornecendo subsídios técnicos para a discussão sobre mecanismos de aferição de idade no contexto do ECA Digital.

### Objetivos específicos

- Investigar tecnologias de estimativa de idade baseadas em reconhecimento facial.
- Comparar o desempenho dos modelos DeepFace e MiVOLO.
- Avaliar os erros das estimativas em diferentes faixas etárias.
- Analisar o comportamento dos modelos diante de limiares etários específicos.
- Identificar possíveis limitações das tecnologias avaliadas para aplicações de aferição de idade.

## 3. Tecnologias utilizadas

| Tecnologia | Aplicação no projeto |
|---|---|
| Python | Linguagem de programação utilizada nos experimentos. |
| Google Colab | Ambiente de execução dos notebooks. |
| DeepFace | Framework utilizado para estimativa de idade por análise facial. |
| MiVOLO | Modelo utilizado para estimativa de idade considerando informações faciais e corporais. |
| YOLOv8 | Detector de pessoas e rostos utilizado na infraestrutura do MiVOLO. |
| RetinaFace | Backend de detecção facial utilizado no DeepFace. |
| OpenCV | Leitura e processamento das imagens. |
| Pandas | Organização, processamento e análise dos resultados. |
| PyTorch | Infraestrutura de execução dos modelos de aprendizado profundo. |
| Scikit-learn | Geração das matrizes de confusão. |
| Matplotlib | Visualização dos resultados experimentais. |

### 3.1 DeepFace

O DeepFace é um framework de reconhecimento facial desenvolvido em Python, utilizado neste trabalho para estimar a idade dos indivíduos presentes nas imagens da base UTKFace.

A execução utiliza o método `DeepFace.analyze()`, com a configuração de detecção facial RetinaFace.

Repositório oficial: https://github.com/serengil/deepface

### 3.2 MiVOLO

O MiVOLO é um modelo de estimativa de idade que utiliza informações faciais e corporais por meio de uma arquitetura de múltiplas entradas.

Sua infraestrutura utiliza o detector YOLOv8 para identificar pessoas e rostos, permitindo que o modelo realize estimativas mesmo em situações nas quais a informação facial esteja parcialmente comprometida.

Repositório oficial: https://github.com/WildChlamydia/MiVOLO

## 4. Base de dados

### UTKFace Dataset

A base UTKFace é utilizada como conjunto de dados legítimos para a avaliação das tecnologias.

Ela contém mais de 20 mil imagens faciais, com informações de idade, gênero e etnia, abrangendo indivíduos de 0 a 116 anos.

Os arquivos seguem um padrão de nomenclatura no qual as informações são separadas pelo caractere `_`:

`idade_genero_etnia_timestamp.jpg`

A base utilizada nos experimentos está dividida em três partes:

- `part1`
- `part2`
- `part3`

Essa divisão permite executar o processamento em lotes e organizar os resultados de cada etapa experimental.

**Observação:** os arquivos originais de imagens da UTKFace não estão incluídos neste repositório. É necessário obter a base separadamente e respeitar suas condições de utilização.

## 5. Metodologia experimental

O processamento das imagens segue uma sequência de etapas:

1. **Coleta de dados:** carregamento das imagens da base UTKFace.
2. **Pré-processamento:** leitura e preparação das imagens para processamento pelos modelos.
3. **Estimativa de idade:** execução dos modelos DeepFace e MiVOLO.
4. **Cálculo dos erros:** comparação entre a idade real e a idade estimada.
5. **Armazenamento:** geração de arquivos CSV contendo os resultados individuais.
6. **Análise estatística:** consolidação dos resultados, avaliação por limiares e geração das matrizes de confusão.

Os processamentos possuem mecanismos de salvamento periódico (*checkpoints*), permitindo preservar os resultados obtidos durante a execução.

## 6. Métricas de avaliação

As tecnologias são avaliadas por meio de diferentes indicadores quantitativos.

### 6.1 Erro Médio Absoluto (MAE)

O MAE (*Mean Absolute Error*) representa a média das diferenças absolutas entre as idades reais e estimadas.

`MAE = média(|idade real - idade estimada|)`

Quanto menor o valor, menor o erro médio das estimativas.

Para o cálculo do MAE global, devem ser considerados todos os registros válidos das três partes da UTKFace, evitando utilizar uma média simples dos MAEs parciais quando os lotes possuem quantidades diferentes de imagens.

### 6.2 Acurácia com tolerância de ±1 ano

Avalia a proporção de estimativas cuja diferença em relação à idade real não ultrapassa um ano.

São considerados acertos os registros que atendem à condição:

`|idade real - idade estimada| <= 1`

### 6.3 Avaliação por limiares etários

São utilizados três limiares para avaliar o comportamento dos modelos:

- 14 anos;
- 16 anos;
- 18 anos.

A idade real e a idade estimada são convertidas em classificações binárias, considerando:

- Classe 0: idade abaixo do limiar.
- Classe 1: idade igual ou superior ao limiar.

### 6.4 Métricas de classificação

A avaliação por limiares considera:

- **TP (True Positive):** indivíduos classificados corretamente na classe igual ou superior ao limiar.
- **TN (True Negative):** indivíduos classificados corretamente na classe abaixo do limiar.
- **FP (False Positive):** indivíduos abaixo do limiar classificados incorretamente como pertencentes à classe igual ou superior.
- **FN (False Negative):** indivíduos iguais ou superiores ao limiar classificados incorretamente como pertencentes à classe abaixo.

Também são calculadas:

- Acurácia (*Accuracy*);
- Taxa de Verdadeiros Positivos (TPR);
- Taxa de Falsos Positivos (FPR);
- Taxa de Falsos Negativos (FNR).

Esses indicadores permitem analisar não apenas a precisão numérica das estimativas, mas também os erros envolvidos nas decisões de classificação por idade.

## 7. Organização do repositório

A estrutura recomendada para organização dos arquivos no GitHub é:

```text
TCC-Estimativa-Idade/
│
├── README.md
│
├── notebooks/
│   ├── 01_deepface_utkface.ipynb
│   ├── 02_mivolo_utkface.ipynb
│   ├── 03_consolidacao_resultados.ipynb
│   ├── 04_calculo_idades_criticas.ipynb
│   └── 05_matrizes_confusao_limiares.ipynb
│
├── resultados/
│   │
│   ├── deepface/
│   │   ├── resultados_deepface_utkface_part1.csv
│   │   ├── resultados_deepface_utkface_part2.csv
│   │   ├── resultados_deepface_utkface_part3.csv
│   │
│   └── mivolo/
│       ├── resultados_mivolo_utkface_part1.csv
│       ├── resultados_mivolo_utkface_part2.csv
│       └── resultados_mivolo_utkface_part3.csv
│
└── .gitignore
```

## 8. Execução do projeto

Os experimentos foram desenvolvidos para execução no Google Colab, utilizando o Google Drive para armazenamento da base de imagens e dos resultados.

### 8.1 Requisitos

- Conta Google com acesso ao Google Colab.
- Google Drive para armazenamento dos arquivos.
- Python.
- Base de dados UTKFace.
- Dependências específicas de cada tecnologia.

### 8.2 Execução do DeepFace

Instalação das bibliotecas necessárias:

```bash
pip install deepface opencv-python pandas
```

Após a instalação:

1. Abra o notebook `01_deepface_utkface.ipynb`.
2. Monte o Google Drive.
3. Configure o caminho da parte da UTKFace que será processada.
4. Configure o diretório de armazenamento dos resultados.
5. Execute as células do notebook.

O processamento gera arquivos CSV com as idades reais, idades estimadas, erros absolutos e status de execução.

### 8.3 Execução do MiVOLO

O MiVOLO utiliza uma infraestrutura própria, que deve ser configurada antes da execução dos experimentos.

Clone o repositório oficial:

```bash
git clone https://github.com/WildChlamydia/MiVOLO.git /content/MiVOLO
```

Instale as dependências:

```bash
cd /content/MiVOLO

pip install -r requirements.txt
pip install .
```

O ambiente utilizado também contempla versões específicas de algumas dependências:

```bash
pip install "timm==0.8.13.dev0"
pip install "ultralytics==8.1.0"
pip install transformers gdown
```

Os pesos necessários incluem:

- `yolov8x_person_face.pt`: detector de pessoas e rostos.
- Checkpoint do MiVOLO: pesos utilizados para estimativa de idade.

Os arquivos devem ser armazenados no diretório:

```text
/content/MiVOLO/models/
```

**Atenção:** o notebook contém referências a diferentes nomes para o checkpoint do MiVOLO. Antes da execução, confirme que o nome do arquivo baixado corresponde ao caminho definido no parâmetro `checkpoint` da configuração do `Predictor`.

Após a configuração:

1. Abra o notebook `02_mivolo_utkface.ipynb`.
2. Configure os caminhos da base UTKFace.
3. Verifique a disponibilidade dos modelos.
4. Execute inicialmente o teste com uma imagem.
5. Execute o processamento das partes da base.
6. Verifique os arquivos CSV gerados.

### 8.4 Consolidação e análise dos resultados

Depois de executar os experimentos, utilize os notebooks de análise:

| Notebook | Finalidade |
|---|---|
| `03_consolidacao_resultados.ipynb` | Consolidação dos resultados e comparação do MAE por tecnologia e parte da base. |
| `04_calculo_idades_criticas.ipynb` | Análise das idades de 14, 16 e 18 anos e cálculo da acurácia com tolerância de ±1 ano. |
| `05_matrizes_confusao_limiares.ipynb` | Geração das matrizes de confusão e cálculo das métricas de classificação. |

Os notebooks de análise devem ser executados após a geração dos respectivos arquivos CSV.

## 9. Estrutura dos arquivos de resultados

Os arquivos CSV individuais possuem as seguintes colunas:

| Coluna | Descrição |
|---|---|
| `Arquivo` | Nome da imagem processada. |
| `Idade Real` | Idade identificada no nome do arquivo da UTKFace. |
| `Idade Estimada` | Idade retornada pelo modelo. |
| `Erro Absoluto` | Diferença absoluta entre a idade real e estimada. |
| `Status` | Situação do processamento da imagem. |

Registros sem estimativa válida devem ser identificados pelo campo `Status` e tratados separadamente nas análises por limiar, evitando que falhas de processamento sejam interpretadas automaticamente como classificações de indivíduos abaixo da idade de corte.

## 10. Limitações do estudo

Os resultados devem ser interpretados considerando as características e limitações das tecnologias e da base utilizada.

- A estimativa de idade não representa, isoladamente, um mecanismo completo de verificação de identidade ou de aferição de idade.
- O desempenho pode variar conforme iluminação, resolução, posicionamento, oclusão e características individuais.
- A base UTKFace não representa integralmente as condições de utilização de sistemas reais.
- Os erros de classificação podem produzir consequências diferentes conforme o limiar utilizado.
- A estimativa de idade, por si só, não garante resistência a ataques de apresentação, imagens reproduzidas ou deepfakes.

No estado atual dos artefatos disponibilizados, os experimentos reproduzíveis concentram-se nas tecnologias DeepFace e MiVOLO utilizando a UTKFace. DEX, OULU-NPU e testes específicos com deepfakes são assuntos abordados na pesquisa, mas não possuem resultados experimentais correspondentes nos arquivos deste repositório.

## 11. Privacidade e uso responsável

Este projeto possui finalidade exclusivamente acadêmica e experimental.

Por envolver imagens faciais e informações biométricas, recomenda-se observar os princípios de privacidade, segurança da informação e proteção de dados pessoais durante o armazenamento, processamento e compartilhamento dos arquivos.

As imagens originais e outros dados pessoais não devem ser publicados no repositório sem a devida autorização e observância das condições de utilização da base.

Os resultados apresentados não devem ser utilizados isoladamente para decisões reais de concessão ou restrição de acesso de usuários a serviços digitais.

## 12. Referência acadêmica

GOUVEIA, Gabriel Saraiva de. **Avaliação da viabilidade de tecnologias de estimativa de idade em consonância com a Lei 15.211/2025 (ECA Digital)**. 2026. Trabalho de Conclusão de Curso (Bacharelado em Engenharia de Software) – Universidade Estadual de Maringá, Maringá, 2026.

---

**Universidade Estadual de Maringá – UEM**  
**Departamento de Informática**  
**Curso de Engenharia de Software – 2026**
