# Introdução à Análise de EEG com MNE-Python

Este repositório contém uma série de tutoriais práticos (Jupyter Notebooks) focados na introdução à análise de dados de eletroencefalografia (EEG) utilizando a biblioteca MNE-Python.

O objetivo é guiar o usuário desde o carregamento de dados brutos até o pré-processamento avançado e limpeza de artefatos, seguindo as melhores práticas da neurociência computacional.

---
## Fluxo geral de Estudo

<div align='center'>

Leitura de dados brutos e pré-processamento geral 
<br>
↓
<br>
Organização dos dados em estrutura BIDS
<br>
↓
<br>
Segmentação e divisão de epochs
<br>
↓
<br>
Limpando dados com ICA
<br>
↓
<br>
Estudo de Caso: iEEG e Epilepsia (Filtros e Simulação)
<br>
↓
<br>
Machine Learning: Decodificação Temporal e Validação Cruzada 

</div>

---

## Conteúdo do Repositório

Os notebooks estão organizados de forma sequencial para facilitar o aprendizado dos conceitos e do fluxo de processamento de EEG:

### [01 - Introdução](01-introducao.ipynb)
Apresenta os conceitos fundamentais e primeiros passos com a biblioteca MNE.
* **Leitura de Dados:** Carregamento de dados brutos (Raw) no formato .fif.
* **Visualização:** Plotagem de sinais, uso do modo "Butterfly" para visualização global e manipulação de canais e sensores (EEG, STI).
* **Eventos:** Extração de eventos a partir de canais de estímulo (Trigger channels/STI).
* **Pré-processamento Básico:** Recorte de dados (crop) e filtragem de frequências (Filtro passa-banda) para remover tendências lentas ou ruídos de alta frequência.
* **Análise Espectral:** Cálculo e visualização da Densidade Espectral de Potência (PSD) para identificar ritmos cerebrais (alfa, beta, etc.).

### [02 - BIDS (Brain Imaging Data Structure)](02-BIDS.ipynb)
Aborda a padronização e organização de dados de neuroimagem.
* **Conceito:** Explicação do formato BIDS, estrutura de pastas e metadados obrigatórios (como a frequência da rede elétrica).
* **Conversão:** Utilização do pacote `mne-bids` para converter datasets e criar a estrutura de arquivos (arquivos .json, .tsv e .fif).
* **Anonimização:** Procedimentos para remover informações pessoais e alterar datas de gravação para conformidade com o padrão.
* **Leitura:** Como carregar datasets que já estão formatados em BIDS, convertendo eventos e anotações.

### [03 - Epoching e Respostas Evocadas](03-epoching_e_evoked_responses.ipynb)
Foca na segmentação do sinal de EEG para análise de eventos específicos.
* **Conceitos:** Definição de Epochs (janelas de tempo ao redor de um evento) e Evoked (médias/ERPs).
* **Correção de Offset:** Explicação sobre offset DC e aplicação de Baseline para correção.
* **Criação:** Geração de Epochs baseadas em eventos (estímulos visuais e auditivos) e manipulação de Metadata.
* **Visualização:** Geração de mapas topográficos (Topomaps) da atividade no couro cabeludo e comparação de potenciais evocados.

### [04 - Limpando Dados (Artefatos e ICA)](04-limpando_dados.ipynb)
Detalhamento de técnicas avançadas para remoção de ruídos e componentes indesejados no sinal de EEG.
* **Artefatos:** Definição e distinção entre artefatos fisiológicos (ex: piscadas/EOG, batimentos cardíacos/ECG) e não fisiológicos.
* **Rejeição por Amplitude:** Exclusão de epochs baseada em critérios de amplitude pico-a-pico (ex: limiar de 150 µV para EEG).
* **ICA (Análise de Componentes Independentes):** Separação de sinais misturados em fontes independentes para limpeza de dados.
    * Importância do filtro passa-alta (1.0 Hz) para a performance da ICA.
    * Detecção e remoção automática de artefatos de piscadas (EOG) e batimentos cardíacos.
* **Visualização:** Comparação do sinal antes e depois da limpeza (Overlay) para validar a remoção de ruídos sem perda de sinal neural.


### [05 - EEG Clínico e Epilepsia (iEEG)](05-eeg_epilepsia.ipynb)
Foca nas particularidades do processamento de dados clínicos e intracranianos, com ênfase em epilepsia.
* **Fluxo Padronizado (Scalp EEG):** Consolidação das etapas de limpeza para EEG de superfície (correção de unidades µV/V, remoção de offset DC, re-referência CAR).
* **iEEG e BIDS:** Carregamento e manipulação de dados de EEG intracraniano (iEEG) estruturados em BIDS, destacando diferenças de sinal e frequência.
* **Filtros para Epilepsia:** Estratégia específica de filtragem (Notch + Passa-Banda largo de 1-100Hz) para preservar eventos rápidos como *Spikes* e HFOs, ao contrário dos filtros convencionais.
* **Simulação de Eventos:** Geração de dados sintéticos para visualização didática de morfologias clínicas, como pontas (*Spikes*) e complexos ponta-onda.

### [06 - Machine Learning e Decodificação Temporal](06-decodificacao_temporal_ML.ipynb)
Introdução à aplicação de técnicas de Machine Learning (MVPA) para classificar estados mentais a partir do EEG/MEG.
* **Balanceamento de Classes:** Importância e métodos para equalizar o número de *epochs* entre condições para garantir um nível de chance (chance level) de 50%.
* **Pipelines com Scikit-Learn:** Criação de fluxos de processamento integrando MNE e Scikit-Learn, incluindo vetorização e escalonamento (`StandardScaler`).
* **Classificação Estática:** Treinamento de modelos (Regressão Logística) usando todo o intervalo de tempo como *features*.
* **Decodificação Resolvida no Tempo:** Uso do `SlidingEstimator` para treinar classificadores independentes para cada instante de tempo, revelando a dinâmica temporal do processamento neural ("quando o cérebro diferencia os estímulos?").
* **Validação:** Aplicação de validação cruzada estratificada (*Stratified K-Fold*) e métrica ROC AUC.

---

## Instalação e Requisitos

Para executar os notebooks, recomenda-se o uso do Anaconda para gerenciar o ambiente virtual Python.

### Pré-requisitos
* Python (versão 3.x)
* Anaconda Prompt (ou terminal similar)

### Passo a Passo

1.  Crie o ambiente virtual:
    ```bash
    conda create --name mne python=3.10
    conda activate mne
    ```

2.  Instale as dependências necessárias:
    Os notebooks utilizam `mne`, `mne-bids`, `nibabel`, `pybv`, `matplotlib` e `pandas`.
    ```bash
    pip install mne mne-bids nibabel pybv pandas matplotlib
    ```

    *Nota: Nos notebooks, é configurado o backend `Qt5Agg` para o matplotlib garantir que os gráficos interativos do MNE funcionem corretamente em ambientes locais.*

---

## Dados Utilizados

Os tutoriais utilizam o dataset clássico **MNE-sample-data** (`sample_audvis_raw.fif`).

* Este dataset contém dados neurofisiológicos gravados durante uma tarefa audiovisual.
* Os notebooks contêm código para verificar e baixar automaticamente os dados caso não estejam presentes no computador:
    ```python
    sample_data_dir = mne.datasets.sample.data_path()
    ```

---

## Ferramentas Adicionais


### MNELab
Uma interface gráfica (GUI) para o MNE que permite abrir dados brutos e realizar **processamentos básicos** de EEG sem a necessidade de escrever código.
* Link: [MNELab no GitHub](https://github.com/cbrnr/mnelab)
