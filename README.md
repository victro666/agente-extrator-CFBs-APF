# Projeto de Agente Autônomo para Extração e Contagem de Pontos de Função Estimativa NESMA

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/victro666/agente-extrator-CFBs-APF/blob/main/AgenteIA_APF_estimativa_NESMA.ipynb)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![LLM](https://img.shields.io/badge/LLM-Qwen2.5--7B--Instruct-purple)
![IFPUG](https://img.shields.io/badge/M%C3%A9tricas-IFPUG%20CPM%204.3.1-green)

Versão aprimorada do meu projeto de TCC em Ciência da Computação para automatização da identificação, classificação e estimativa de Componentes Funcionais Básicos (**EE, SE, CE, ALI e AIE**) a partir de documentos de especificação de requisitos de software (`.pdf` e `.txt`), utilizando modelos de linguagem pré-treinados baseados na arquitetura transformer rodando **100% localmente / on-premise**, para medição de tamanho funcional.

---

## O Problema
A Análise de Pontos de Função (APF) exige leitura minuciosa de documentos extensos para separar Requisitos Funcionais (RFs) de Requisitos Não Funcionais (RNFs), evitar a fragmentação de sub-passos de interface e aplicar rigorosamente as regras de intenção primária do Manual de Práticas de Contagem (CPM) do IFPUG. Além disso, o uso de APIs comerciais em nuvem muitas vezes esbarra em restrições de segurança e privacidade de projetos corporativos e governamentais.

## Arquitetura da Solução
O pipeline combina modelos de linguagem abertos quantizados com regras determinísticas de Engenharia de Software:

1. **Sumarização de Contexto Global:** Extração prévia do objetivo do sistema, atores e principais entidades de domínio.
2. **Agente Triador Autônomo (Chain-of-Thought + Tool Calling):** Filtra blocos puramente técnicos/não funcionais (SLAs, hardware, login/senha, formatação ASCII) e aciona sob demanda a ferramenta `consultar_manual_cpm` no **ChromaDB** quando há ambiguidades de classificação.
3. **Agente Extrator Estruturado (Few-Shot + Pydantic):** Extrai Funções de Dados (`ALI`, `AIE`) e Funções de Transação (`EE`, `SE`, `CE`) aplicando a *Regra da Dualidade* e o *Teste de Transação Completa*.
4. **Pós-Processamento Neuro-Simbólico:** Desdobra deterministicamente verbos guarda-chuva de persistência (*Registrar, Manter, Cadastrar, Gerenciar*) nas transações elementares de **Inclusão** e **Alteração**, eliminando duplicatas semânticas e aplicando os pesos da contagem estimativa **NESMA** (`EE=4`, `SE=5`, `CE=4`, `ALI=7`, `AIE=5`).

---

## 🚀 Como Executar (1 Clique no Google Colab)

Você pode testar tanto o processamento em lote quanto a interface web interativa (**Gradio**) diretamente no navegador usando a GPU gratuita do Google Colab:

1. Clique no botão **[Open in Colab](https://colab.research.google.com/github/victro666/agente-extrator-CFBs-APF/blob/main/AgenteIA_APF_estimativa_NESMA.ipynb)** no topo deste README.
2. No menu superior do Colab, certifique-se de que uma GPU está ativa (`Ambiente de execução > Alterar o tipo de ambiente de execução > GPU T4 ou L4`).
3. Clique em **`Ambiente de execução > Executar tudo`**.
4. O script baixará automaticamente a base vetorial (`regras_CPM.txt`) e o documento de estudo de caso traduzido (`0000_cctns_pt.txt`) do dataset PURE, exibindo a tabela consolidada e gerando um link público do **Gradio** na última célula.

---

## 💻 Execução Local (On-Premise)

Para executar em sua própria estação de trabalho (requer GPU NVIDIA com suporte a CUDA, MÍNIMO 4GB VRAM, 16GB RAM):

```bash
git clone https://github.com/victro666/agente-extrator-CFBs-APF.git
cd agente-extrator-CFBs-APF
pip install -r requirements.txt
jupyter notebook Agente_Extrator_APF.ipynb
