# 🧠 Postmortem – Projeto DermAlert

## 1. 👥 Equipe

- Esther Sena
- Izabella Alves
- Leticia Martins
- Davi Rodrigues
- Davi Casseb
- Renan Araújo
- Guilherme Brito
- Vitor Carvalho 
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
- Estudo dos repositórios existentes no GitHub.
- Leitura e análise da documentação.
- Configuração dos ambientes (Python, React Native).
- Definição de papéis e responsabilidades.

**Dificuldade inicial:** Falta de instruções claras para rodar o backend e o frontend, e escolha de bibliotecas e dependências.  
**Solução:** Produção de documentação prática com exemplos e passo a passo de instalação, e buscas e testes de ferramentas adequadas para o objetivo do projeto.

---

## 4. 📄 Melhorias na Documentação

- Atualização do `README.md` com instruções claras e objetivas, dentro dos repositórios da Documentação, Backend, Frontend e Machine Learning.
- Adição de diagramas explicativos sobre a arquitetura do projeto em alguns modelos do README.
- Detalhamento dos endpoints da API (métodos, parâmetros e retornos).
- Inclusão de um guia de contribuição e checklist de boas práticas.
- Automação de Issues e Pull-Requests, facilitando e tornando os mais fáceis e seguindo padrões.

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

A equipe implementou uma pipeline MLOps utilizando **GitHub Actions**,  com o objetivo de automatizar e orquestrar etapas críticas do ciclo de vida do modelo, garantindo reprodutibilidade, qualidade e confiabilidade nas entregas.

**Etapas Automatizadas:**
- Validação da qualidade da imagem (com BRISQUE)
- Execução do modelo de classificação
- Geração de logs e alertas em tempo real
- Testes automatizados antes de cada deploy no backend
- Análise estática de código do backend com Pylint.
- Análise estática do código do frontend com Eslint e SonarCloud

**Dificuldades:**  
- Configurar a execução paralela das tarefas
- Eliminar falsos positivos e adaptar as regras do linting à realidade do projeto.
- Manter a consistência entre os ambientes de desenvolvimento local e CI.

**Soluções:**  
- Criação de DAGs bem segmentadas
- Monitoramento com alertas por log
- Padronização de diretórios e volumes no Docker
- Padronização do código-fonte com regras específicas do Pylint e ESLint integradas ao pipeline.
- Integração com SonarCloud

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

- Esther Sena - Aprendi a importância de manter uma documentação bem estruturada e sempre atualizada, refletindo com precisão o que o projeto realmente possui e realiza. Também percebi que nem sempre as soluções encontradas na Internet são corretas ou funcionais. Em alguns casos, se algo só funciona localmente e não pode ser adaptado para outros ambientes, utilizá-lo pode representar perda de tempo e esforço. Além disso, compreendi ainda mais o valor da automação de processos e do uso de bibliotecas de código aberto, que tornam o desenvolvimento mais ágil e eficiente.


- Henrique Galdino - A disciplina foi muito importante para meu aprendizado, principalmente no que diz respeito ao trabalho em equipe. Em outras disciplinas que envolviam projetos em grupo, sempre trabalhei de forma mais “individual”, porém em GCES foi fundamental me manter alinhado com os demais membros da equipe e suas tarefas. Também aprendi bastante sobre tópicos e técnicas que eu nunca havia trabalhado anteriormente, como Machine Learning, conceitos de CI/CD, análise de imagens e documentações técnicas.


- Izabella Alves - Durante a disciplina, minha equipe trabalhou em um projeto que ainda não tinha quase nada do Checklist de Software Livre. Por isso, consegui aprender desde o início como montar uma documentação realmente útil, que pudesse ajudar novos contribuidores. Fizemos isso baseadas em nossa própria experiência ao tentar contribuir com o DermAlert. Já na segunda entrega, nossa equipe conseguiu encontrar bons modelos de machine learning e integrá-los ao código usando somente quatro dependências. Eu considero isso uma vitória e tanto para nós! São dependências confiáveis, e durante a disciplina, aprendi a gerenciar melhor o uso de dependências, evitando usá-las de forma desnecessária.


- Leticia Torres - Nesta disciplina, aprendi bastante sobre gerência de software e como funciona um projeto real que precisa seguir alguns requisitos para ser considerado um software livre, como licenciamento, documentação e abertura para colaboração. Além dos aspectos técnicos, a organização da equipe foi fundamental: cada membro teve seu papel claramente definido, o que garantiu uma colaboração eficiente e estruturada. Essa experiência me mostrou como a gestão tanto do código quanto das pessoas é crucial para que um projeto evolua de forma sustentável.


- Davi Rodrigues - Nessa disciplina, tive a oportunidade de aprender automação de programas, uma área que sempre me interessou. Trabalhar em um projeto com uma grande equipe me proporcionou desafios no quesito de comunicação, mas também me proporcionou uma visão valiosa sobre como gerenciar atividades para um grande número de pessoas através da Izabella. Além disso, aprendi muito ao trabalhar em um projeto já existente, o que me mostrou a complexidade de lidar com bugs e tentar entender códigos preexistentes. Por fim, aprendi também o quão crucial é documentar tudo o que fazemos e estabelecer regras claras para o funcionamento eficaz de uma comunidade.


- Davi Araújo - Ao longo da disciplina pude ver quais eram meus pontos fortes e fracos, além de aprender o funcionamento de um projeto real no dia a dia, vivendo cada etapa dos processos de entrega. Pude viver na pele como é manter um repositório no GitHub que esteja recebendo atualizações constantes, prezando pelo funcionamento integrado do software. Além disso, pude melhorar minhas habilidades para redigir documentos técnicos e descritivos. Também pude aprender muito sobre o funcionamento de APIs e modelos de IA, e também sobre pipelines CI/CD.


- Guilherme Brito - Com essa disciplina consegui aprender e desenvolver minhas habilidades e conhecimentos sobre versionamento, configuração e evolução de um projeto real. Dentre os aspectos aprendidos pode se destacar principalmente como manter um projeto evoluindo pelo GitHub e como o controle de versionamento é importante para que o projeto caminhe bem. Outras habilidades desenvolvidas foram sobre conhecimento de APIs, pipelines CI/CD e modelos de IA, em que nos ajudaram a desenvolver novas habilidades e aprimorar outras.


- Renan Araújo - A disciplina foi muito importante para mim porque mexemos mais com os conceitos de Git/GitHub e isso me fortaleceu bastante, porque tinha conceitos que eu nunca tinha mexido antes, como Pipelines CI/CD, e também desenvolver mais sobre a parte de API e Machine Learning, ao qual eu me aprofundei bastante indo atrás de modelos e implementando, além de mexer com um projeto real e software livre, que eu acho que foi a primeira experiência que aprendi de fato, já tive disciplinas que mexeram com isso, mas a experiência não foi tão aprofundada igual essa.


- Vitor Carvalho – Durante a disciplina, tive a oportunidade de expandir meus conhecimentos técnicos e práticos sobre automação, controle de versão e integração de pipelines em um projeto real. Uma das maiores lições que levo foi a importância da padronização na documentação e na estrutura do repositório, algo essencial quando se trabalha com software livre e colaboração aberta. Além disso, aprendi bastante sobre como gerenciar dependências de forma eficiente e como decisões aparentemente pequenas, como a escolha de uma biblioteca ou framework, podem impactar diretamente na manutenção e escalabilidade do projeto.


- Caio Lucas - Ao longo da disciplina, pude vivenciar de forma prática como é contribuir para um projeto real de software livre. Uma das lições mais marcantes foi entender a importância de uma documentação clara e acessível, especialmente para quem está chegando no projeto agora. Também aprofundei bastante meus conhecimentos sobre a integração de ferramentas como CI/CD, controle de versão com Git e GitHub o que tornou o desenvolvimento mais eficiente. A experiência de trabalhar em equipe ,exclusivamente com a equipe do Dermalet foi muito legal,  precisei me alinhar com os demais membros, dividir responsabilidades e manter uma comunicação constante, algo essencial em projetos colaborativos. Outro ponto importante foi lidar com código feito por outras pessoas: entender e adaptar algo já existente exige paciência, leitura e respeito pelo que já foi feito. Além disso, a vivência com dependências e bibliotecas me ensinou a avaliar melhor o que realmente vale a pena ser incluído no projeto, pensando sempre na manutenção a longo prazo.



---

## 11. 💬 Feedback Pessoal e de Grupo

- Esther Sena - Em relação ao meu desempenho, tanto individualmente quanto do grupo, acredito que fiz o máximo possível, sempre me esforçando para ir além quando surgiam novas demandas. O meu grupo é composto por pessoas dedicadas, com conhecimentos abrangentes, e que estão sempre dispostas a participar ativamente, esclarecer dúvidas e ajudar uns aos outros.

- Henrique Galdino -  Diria que meu desempenho foi bom, consegui efetuar as tarefas sem maiores dificuldades (embora tenha passado do prazo definido pela equipe por duas vezes) e tentei sempre estar disposto a ajudar os demais colegas caso necessário. Em relação ao grupo, todos os membros foram bastante proativos, executando as tarefas com muito esforço e dedicação. A comunicação do grupo também foi impecável, sempre buscando fazer reuniões pontuais para discutir certos tópicos e realizar tarefas em conjunto. Sou muito grato a dedicação de cada um dos membros, em especial a Izabella, que assumiu um papel de liderança no grupo e buscou sempre manter todos a par das tarefas e prazos e aos maintainers, Davi Rodrigues e Davi Araújo, que a todo momento estavam acompanhando o andamento das tarefas e garantindo que os repositórios estavam devidamente organizados.
 
- Izabella Alves - Acredito que tive um bom desempenho, realizando todas as tarefas que me foram designadas e tentando ajudar o grupo ao máximo. Nossa equipe também trabalhou muito bem, mantendo a constância nas entregas e reuniões, o que foi crucial para o desenvolvimento do projeto. Com reuniões e novas tarefas semanais, conseguimos revisar o que havia sido feito e fazer entregas contínuas, chegando ao final do projeto com todos os nossos objetivos alcançados.

- Leticia Torres - O trabalho em grupo foi um dos pontos mais positivos da disciplina para aprender de fato a matéria. Todos contribuíram de forma ativa e competente, sempre dispostos a colaborar e compartilhar ideias. A comunicação entre a gente foi constante e eficiente e estávamos sempre nos falando, alinhando tarefas e resolvendo as coisas juntos. No meu desempenho pessoal, também me mantive bem presente: participei das reuniões, cumpri as tarefas semanais e busquei contribuir com o grupo de forma consistente. Foi uma experiência muito colaborativa, e acredito que nosso comprometimento coletivo fez toda a diferença no sucesso do projeto além de uma boa gerência do grupo feita pela Izabella.


- Davi Rodrigues - Na minha visão, tive um bom desempenho na disciplina, consegui cumprir minhas tarefas e aprender bastante sobre as áreas que me interessavam, como automação. Considero o nosso trabalho em equipe muito bom. Todos se dedicaram e entregaram suas partes dentro do prazo. Sinto que as orientações por parte da Izabella foram fundamentais para a organização e o bom andamento do projeto, garantindo que todos estivéssemos alinhados e conseguíssemos alcançar nossos objetivos. Estou muito satisfeito com o resultado do nosso trabalho em grupo e com o meu próprio desempenho.


- Davi Araújo - Acredito que tive um bom desempenho na matéria, na qual pude contribuir em todas as etapas do projeto, atuando com outros membros e aprendendo as técnicas que nos foi transmitida na sala de aula. A matéria foi uma experiência extremamente positiva. O grupo funcionou durante todo o projeto de maneira totalmente sinérgica, em parte devido ao alto nível demonstrado pelos membros, mas principalmente pelo fato do grupo ter agido em completa sintonia. Fica meus sinceros agradecimentos à todos os integrantes do grupo, e em especial à Izabella por gerir o grupo de maneira primorosa. Ao Henrique por me corrigir e ensinar quando tive dúvidas, ao Davi Rodrigues por me ajudar a manter o repositório, e ao restante do grupo, Esther, Renan, Vitor, Caio, Leticia e Guilherme por também me ajudarem quando precisei e por fazerem cada um sua parte de maneira perfeita.

- Guilherme Brito - Nesta disciplina tive um bom desempenho, realizando as tarefas que foram propostas pelo grupo, trabalhando em equipe, aprendendo novas habilidades e funcionalidades, além da organização para conseguir realizar as tarefas em grupo sempre que eram propostas. O grupo sempre se esforçou ao máximo para atender todas as demandas do projeto, dedicando tempo e esforço para que ninguém ficasse sobrecarregado e que todos pudessem ter uma boa participação e aprendizado do que era exigido na disciplina. Acredito que o ponto chave para que o grupo tivesse um desempenho tão satisfatório foram as reuniões semanais para descrever o que já havia sido feito, o que iria ser feito a partir daquela reunião e tirar e esclarecer as dúvidas.


- Renan Araújo - Na disciplina eu vejo que meu desempenho foi muito bom, participei de todas as reuniões, me disponibilizava para ajudar meus colegas em qualquer questão, corri atrás de soluções para o desenvolvimento do nosso projeto, não creio que tive dificuldades na matéria, os prazos eram bastante grandes, então eu conseguia fazer tudo no meu tempo e com qualidade. Sobre meu grupo eu creio que foram ótimos, todo mundo fez as tarefas que foram designados, participativos, não tivemos nenhum embate e todo mundo se ajudava também, agradeço em especial a Izabella por ter assumido o papel de liderança e ter gerido a equipe muito bem, foi muito importante para a continuidade e organização do nosso projeto.

- Vitor Carvalho – Acredito que tive um bom desempenho ao longo da disciplina, contribuindo com constância para o desenvolvimento do projeto e me mantendo presente nas tarefas e discussões em grupo. Busquei sempre alinhar minhas entregas com os prazos definidos e me comprometi com o avanço técnico do repositório, especialmente nas partes que envolviam automação e organização de código. O grupo como um todo trabalhou de forma muito colaborativa, com apoio mútuo. Destaco a liderança da Izabella, que foi essencial para manter o grupo organizado e engajado. Estou satisfeito com os resultados que alcançamos juntos e com o aprendizado adquirido nessa experiência.

- Caio Lucas -  Acredito que tive um bom desempenho ao longo da disciplina, participando ativamente das discussões, reuniões e execuções das tarefas propostas. Busquei sempre manter minhas entregas em dia e colaborar com os colegas sempre que possível. Em relação ao grupo, foi uma das experiências mais positivas que já tive trabalhando em equipe na FGA. Todos os membros se mostraram extremamente comprometidos, proativos e dispostos a ajudar uns aos outros. A comunicação sempre fluiu de forma clara e constante, o que facilitou muito a organização do projeto e o cumprimento dos prazos. Gostaria de destacar especialmente o papel da Izabella, que assumiu a liderança do grupo de forma muito natural e eficiente. Ela foi essencial para manter todos alinhados, coordenar as tarefas e garantir que não deixássemos nenhum ponto importante de lado. Também sou muito grato ao apoio dos demais membros que acompanharam de perto a organização dos repositórios, e todos os colegas que estiveram sempre prontos a compartilhar conhecimento e colaborar. 


---
