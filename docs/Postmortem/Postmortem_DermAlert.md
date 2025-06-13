# 🧠 Postmortem – Projeto DermAlert

## 1. 👥 Equipe

- Esther Sena Martins  
- Izabella Alves Pereira
- Leticia Martins
- Davi Rodrigues
- Davi Casseb
- Renan Araújo
- Guilherme
- Vitor
- Caio Lelis
- Henrique Galdino

---

## 2. 🌐 Contextualização

O **DermAlert** é um aplicativo mobile que auxilia na detecção de possíveis doenças de pele (como acne, manchas e tumores) a partir de imagens tiradas pelo próprio usuário. O sistema utiliza técnicas de MLOps para avaliar a qualidade da imagem, classificá-la e retornar um **pré-diagnóstico** com recomendações.

### Módulos do Projeto:
- **Frontend:** Interface do aplicativo para o envio de imagens e visualização dos resultados (modais).
- **Backend:** Processamento das imagens, avaliação da qualidade e classificação via modelo de machine learning.
- **Documentação:** Estruturação do projeto, instruções de instalação, guias de uso e contribuição.

---

## 3. 🧭 Ambientação da Equipe

A ambientação incluiu:
- Estudo dos repositórios existentes no GitHub
- Leitura e análise da documentação
- Configuração dos ambientes (Python, React Native)
- Definição de papéis e responsabilidades

**Dificuldade inicial:** Falta de instruções claras para rodar o backend e o frontend.  
**Solução:** Produção de documentação prática com exemplos e passo a passo de instalação.

---

## 4. 📄 Melhorias na Documentação

- Atualização do `README.md` com instruções claras e objetivas
- Adição de diagramas explicativos sobre a arquitetura do projeto
- Detalhamento dos endpoints da API (métodos, parâmetros e retornos)
- Inclusão de um guia de contribuição e checklist de boas práticas
- Automação de Issues e Pull-Requests

---

## 5. 🧠 Aprimoramentos e Novas Funcionalidades no Backend

### 5.1 Avaliação de Qualidade de Imagem com BRISQUE
- **Implementação**: Integração do algoritmo BRISQUE (Blind/Referenceless Image Spatial Quality Evaluator) para avaliação automática da qualidade de imagens dermatoscópicas.
- **Como funciona**: O BRISQUE utiliza Estatísticas de Cenas Naturais (NSS) e Regressão de Vetores de Suporte (SVR) para atribuir uma pontuação de 0 a 100, onde valores menores indicam maior qualidade perceptual. Não requer imagem de referência, sendo ideal para aplicações em tempo real.
- **Benefícios**:
  - Garante a análise de imagens nítidas, reduzindo falsos positivos/negativos em diagnósticos.
  - Eficiente computacionalmente, com processamento rápido e sem transformações complexas.
  - Quantifica distorções (ruído, desfoque, compressão) para assegurar a confiabilidade das imagens.

### 5.2 Integração de Modelos de Classificação de Lesões de Pele
Dois modelos de aprendizado profundo foram incorporados para análise de imagens dermatoscópicas, oferecendo maior precisão na detecção de condições cutâneas:

- **Soma_Skin_Cancer_Classifier**:
  - **Descrição**: Modelo baseado na arquitetura ResNet-18, ajustado com o conjunto de dados HAM10000 para classificação binária de lesões (benigna ou maligna).
  - **Características**:
    - Entrada: Imagens RGB (224x224 pixels).
    - Saída: Probabilidade de malignidade (0: Benigna, 1: Maligna).
    - Acurácia: 89% em testes.
  - **Impacto**: Identificação confiável de lesões potencialmente malignas, com foco em simplicidade e eficiência.

- **Skin_Cancer-Image_Classification**:
  - **Descrição**: Modelo baseado em Vision Transformer (ViT), treinado no Skin Cancer Dataset para classificação multiclasse de lesões, incluindo melanoma, carcinoma basocelular, queratose actínica, entre outros.
  - **Características**:
    - Entrada: Imagens processadas com patch size de 16x16.
    - Saída: Classificação em 7 categorias de lesões cutâneas.
    - Acurácia: Até 96% em validação.
    - Treinamento: 5 épocas, otimizador Adam, função de perda Cross-Entropy.
  - **Impacto**: Diagnósticos mais detalhados, permitindo a identificação de tipos específicos de lesões com alta precisão.

### 5.3 Endpoint para Alertas de Casos Suspeitos
- **Funcionalidade**: Novo endpoint que gera alertas automáticos para casos suspeitos de lesões malignas, com base nas predições dos modelos de classificação.
- **Exemplo de Resposta**:
  ```json
  {
    "message": "Lesão e imagens cadastradas com sucesso",
    "lesao": {
      "id": 14,
      "local_lesao_id": 1,
      "local_lesao_nome": "Cabeça",
      "descricao_lesao": "Teste de lesão suspeita"
    },
    "imagens": [
      "imagens-lesoes/imagens-lesoes_20250525165547_80daf1a5.jpg"
    ],
    "tipos": ["benigno"],
    "prediagnosticos": ["Nevo melanocítico"],
    "descricoes-lesao": [
      "Pintas benignas formadas por aglomerados de células produtoras de pigmento."
    ]
  }
  ```
- **Benefícios**:
  - Notifica usuários e profissionais de saúde em tempo real sobre casos que requerem atenção.
  - Integração fluida com sistemas de notificação para agilizar o acompanhamento médico.

### 5.4 Refatoração e Padronização do Código
- **Qualidade do Código**:
  - **Verificação com Pylint**: Garantia de pontuação mínima de 8.0, seguindo as melhores práticas de desenvolvimento.
  - **Formatação com Black**: Código formatado segundo o padrão PEP 8, assegurando legibilidade e consistência.
- **Benefícios**:
  - Maior manutenibilidade e escalabilidade do backend.
  - Facilidade de colaboração entre desenvolvedores, com código claro e padronizado.
  - Redução de erros e melhoria na robustez do sistema.

---

### Principais Melhorias
1. **Qualidade de Imagem**: O BRISQUE assegura que apenas imagens de alta qualidade sejam processadas, aumentando a confiabilidade dos diagnósticos.
2. **Classificação Avançada**: A integração de modelos ResNet-18 e ViT permite detectar e classificar lesões com alta precisão, abrangendo desde casos binários até multiclasses.
3. **Alertas Automatizados**: O novo endpoint facilita a identificação rápida de casos suspeitos, melhorando a resposta a possíveis riscos.
4. **Código Otimizado**: A refatoração com Pylint e Black garante um backend robusto, legível e preparado para futuras expansões.

---

## 6. 📱 Melhorias e Adições no Frontend

- Integração com novos endpoints
- Criação de um novo modal de pré-diagnóstico com visual menos alarmante
- Uso de cores suaves e mensagens explicativas
- Ajustes de layout e acessibilidade

---
## 7. 🔄 Pipeline do Projeto

A equipe implementou uma pipeline MLOps utilizando **Airflow** e **GitHub Actions**, visando automatizar etapas críticas como:

- Validação da qualidade da imagem (com BRISQUE)
- Execução do modelo de classificação
- Geração de logs e alertas em tempo real
- Testes automatizados antes de cada deploy no backend

**Dificuldades:**  
Configurar a execução paralela das tarefas e lidar com erros silenciosos no Airflow.

**Soluções:**  
- Criação de DAGs bem segmentadas
- Monitoramento com alertas por log
- Padronização de diretórios e volumes no Docker

---

## 8. 🧩 Desenvolvimento da Nova Funcionalidade

**Funcionalidade desenvolvida:** Classificação da imagem enviada pelo usuário.

### Principais Dificuldades:
- Escolher uma métrica eficaz de qualidade de imagem que funcionasse bem sem ter que ter uma imagem de referência.
- Ajustar a sensibilidade do modelo para evitar falsos alarmes
- Tornar a comunicação dos resultados clara e tranquila para o usuário

**Soluções Encontradas:**
- Uso da métrica BRISQUE para validar a imagem
- Testes iterativos com imagens reais
- Reformulação do modal de pré-diagnóstico com linguagem cuidadosa
- Utilização de pipeline automatizada para validação e inferência do modelo
- Pipeline MLOps com etapas de verificação de qualidade, classificação e geração de alerta


---

## 9. ⚠️ Problemas Encontrados e Soluções

| Problema | Solução |
|---------|---------|
| Backend mal documentado | Atualização completa do `README.md` e adição de documentação relacionada ao projeto e automoção de issue e pullrequests |
| Resultado alarmante para usuário final | Modal com linguagem moderada e ícones visuais |
| Falta de versionamento da API | Padronização de rotas (`/api/v1/`) |
| Ambiente local difícil de configurar | Criação de script de setup automatizado |

---

## 10. 🎓 Lições Aprendidas

- 

---

## 11. 💬 Feedback Pessoal e de Grupo

- 

---
