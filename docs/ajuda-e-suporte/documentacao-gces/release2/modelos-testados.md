# Modelos de Machine Learning Testados e Escolhidos

A avaliação da qualidade de imagem (IQA) é uma etapa crucial em diversas aplicações de processamento de imagem, e sua importância é particularmente acentuada no campo da imagiologia médica, como na análise de lesões de pele. A qualidade de uma imagem dermatológica ou dermatoscópica pode impactar diretamente a precisão diagnóstica, seja por um especialista humano ou por um sistema de inteligência artificial. Imagens de baixa qualidade podem obscurecer características vitais da lesão, levando a interpretações equivocadas. Neste contexto, foram avaliados modelos de IQA não treinados e pré-treinados, com foco em sua aplicabilidade para garantir que apenas imagens de qualidade adequada sejam utilizadas para análise subsequente.

## Modelo de Classificação de Qualidade de Imagem NIQE (Natural Image Quality Evaluator)

O NIQE é um algoritmo de Avaliação de Qualidade de Imagem Sem Referência (No-Reference Image Quality Assessment - NR-IQA), também conhecido como "blind IQA" (avaliação cega). Isso significa que ele estima a qualidade de uma imagem sem a necessidade de compará-la com uma imagem original "perfeita" ou "sem distorções".

### Princípio Fundamental do NIQE
A ideia central do NIQE é que imagens naturais e de alta qualidade possuem certas regularidades estatísticas que podem ser modeladas. Distorções em uma imagem (como ruído, desfoque, artefatos de compressão, etc.) tendem a perturbar essas estatísticas "naturais". O NIQE quantifica a qualidade de uma imagem medindo o quão distante suas características estatísticas estão de um modelo de "naturalidade" aprendido a partir de um corpus de imagens pristine (sem distorções).

### Como o NIQE Funciona:

1.  **Estatísticas de Cenas Naturais (Natural Scene Statistics - NSS):** O NIQE baseia-se fortemente em modelos de NSS. Estes modelos descrevem as propriedades estatísticas observadas em imagens que retratam cenas naturais. Uma característica comum utilizada são os coeficientes MSCN (Mean Subtracted Contrast Normalized - Média Subtraída e Contraste Normalizado). Os coeficientes MSCN de imagens naturais tendem a seguir distribuições específicas (por exemplo, uma distribuição Gaussiana Generalizada).
2.  **Construção do Modelo de "Qualidade":**
    * Um conjunto de imagens de alta qualidade e sem distorções (imagens "pristine" de cenas naturais) é usado para construir um modelo de referência.
    * Para cada imagem nesse conjunto, são extraídas características baseadas em NSS.
    * Essas características são então modeladas, tipicamente como uma Distribuição Gaussiana Multivariada (MVG). Essa MVG representa o "espaço" das características de imagens naturais de alta qualidade.
3.  **Avaliação de uma Imagem de Teste:**
    * Para uma nova imagem cuja qualidade se deseja avaliar, as mesmas características NSS são extraídas.
    * A "distância" entre o vetor de características da imagem de teste e o modelo MVG é calculada. Essa distância é a pontuação NIQE.
    * **Interpretação da Pontuação:** Uma pontuação NIQE **menor** geralmente indica melhor qualidade perceptual, significando que as estatísticas da imagem de teste estão mais próximas das estatísticas de imagens naturais. Pontuações mais altas indicam um desvio maior da "naturalidade" e, portanto, uma qualidade inferior.

### Características Utilizadas:
O NIQE utiliza um conjunto de características derivadas dos coeficientes MSCN no domínio espacial, incluindo parâmetros de ajuste de distribuições GGD e AGGD a esses coeficientes e seus produtos parciais. Ao todo, são tipicamente 36 características.

### Vantagens do NIQE:

* **Totalmente Sem Referência (Blind).**
* **Não Supervisionado por Opinião Humana (Opinion Unaware):** Não requer scores de qualidade humanos para treinamento, aprendendo apenas com estatísticas de imagens "naturais".
* **Generalidade para Distorções Comuns.**
* **Computacionalmente Mais Leve** que modelos de deep learning.

### Desvantagens e Limitações do NIQE:

* **Correlação com Percepção Humana:** Pode não se correlacionar fortemente com a percepção humana, especialmente para qualidade estética ou tarefas específicas.
* **Foco na "Naturalidade":** Imagens estilizadas ou com conteúdo não fotográfico podem ser mal avaliadas.
* **Sensibilidade ao Conteúdo.**
* **Não Captura Aspectos Estéticos Complexos.**

### Casos de Uso Comuns e Aplicabilidade em Lesões de Pele:

* Monitoramento em tempo real da qualidade de vídeo ou imagem.
* Avaliação automática da qualidade em grandes bancos de dados de imagens genéricas.
* Benchmarking de algoritmos de processamento de imagem.
* **No contexto de lesões de pele:**
    * O NIQE padrão, treinado em cenas naturais, pode ter aplicabilidade limitada, pois as estatísticas de imagens dermatológicas (especialmente dermatoscópicas, com suas texturas e padrões específicos) diferem significativamente das cenas naturais. Uma imagem de lesão de pele pode ser "não natural" segundo o NIQE, mas clinicamente excelente.
    * Poderia, teoricamente, ser usado para detectar distorções muito grosseiras (desfoque extremo, ruído excessivo) que tornariam qualquer imagem inutilizável.
    * Uma adaptação do NIQE, treinando-o com um corpus de imagens de lesões de pele de **alta qualidade clínica** como referência de "naturalidade específica do domínio", poderia ser mais relevante, mas isso foge da sua formulação padrão e propósito original.

Em resumo, para imagens genéricas, o NIQE é uma ferramenta valiosa pela sua natureza não supervisionada. Contudo, para domínios específicos como imagens de lesões de pele, onde a "qualidade" é definida por critérios diagnósticos e não apenas por "naturalidade" estatística geral, suas limitações se tornam mais evidentes.

## Modelo de Classificação de Qualidade de Imagem NIMA (Neural Image Assessment)

O NIMA (Neural Image Assessment) é um modelo de aprendizado profundo projetado para prever a qualidade de imagens de uma forma que se correlaciona fortemente com a percepção humana. Diferentemente dos métodos tradicionais que frequentemente fornecem uma única pontuação média, o NIMA prevê uma **distribuição de pontuações de opinião humana**. Isso significa que ele não apenas avalia a qualidade técnica (ausência de ruído, desfoque, etc.), mas também pode ser treinado para avaliar a qualidade estética ou, crucialmente para o nosso cenário, a **adequação diagnóstica** de uma imagem.

### Como o NIMA Funciona?

O NIMA utiliza Redes Neurais Convolucionais (CNNs):

1.  **Arquitetura da CNN:** Emprega arquiteturas pré-treinadas (VGG, Inception, MobileNet) como extratores de características.
2.  **Camada de Saída Modificada:** Substituída por uma camada que produz uma distribuição de pontuações (e.g., 10 neurônios para uma escala de 1 a 10).
3.  **Função de Perda (Loss Function):** Comumente a Earth Mover's Distance (EMD), que compara a distribuição prevista com a distribuição real das classificações humanas (ou de especialistas).
4.  **Treinamento:** Ajustado (fine-tuned) usando conjuntos de dados de qualidade de imagem com classificações humanas/especialistas. Para lesões de pele, isso envolveria dermatologistas classificando a qualidade das imagens para fins diagnósticos (e.g., nitidez de bordas, visibilidade de estruturas dermatoscópicas, ausência de brilho excessivo).

### Considerações e Limitações do NIMA:

* **Necessidade de Dados Rotulados:** Requer conjuntos de dados com classificações de qualidade, que no caso de lesões de pele, idealmente viriam de dermatologistas.
* **Custo Computacional:** Modelos de aprendizado profundo podem ser mais intensivos.
* **Natureza de "Caixa Preta":** Entender o porquê de uma decisão pode ser menos transparente.

## Avaliação entre modelos pré-treinados: Comparação entre os Modelos Soma_Skin_Cancer_Classifier e Skin_Cancer-Image_Classification

Na busca por modelos eficazes na classificação de lesões de pele, dois modelos disponíveis na plataforma Hugging Face foram analisados: Conn-Cerberus/Soma_Skin_Cancer_Classifier e Anwarkh1/Skin_Cancer-Image_Classification. Ambos demonstram abordagens distintas em termos de arquitetura e desempenho.

### Conn-Cerberus/Soma_Skin_Cancer_Classifier
- **Arquitetura:** Baseado em ResNet-18, uma rede neural convolucional profunda conhecida por sua eficiência em tarefas de classificação de imagens.

- **Tipo de Classificação:** Binária, distinguindo entre lesões benignas e malignas.

- **Conjunto de Dados Utilizado:** HAM10000, que contém imagens dermatoscópicas de diversas lesões de pele.

- **Desempenho:**

    - Acurácia: Aproximadamente 89%.

    - Recall para lesões malignas: Cerca de 78%.

    - Precisão para lesões benignas: Superior a 90%.

#### Considerações:

O modelo foi treinado com pesos ajustados para lidar com o desequilíbrio de classes, dando maior ênfase às lesões malignas.

Embora apresente bom desempenho geral, o recall relativamente mais baixo para lesões malignas indica uma necessidade de cautela, especialmente em aplicações clínicas onde a detecção de casos malignos é crítica.

### Anwarkh1/Skin_Cancer-Image_Classification
   
- **Arquitetura:** Utiliza o Vision Transformer (ViT), uma abordagem baseada em transformadores que tem mostrado resultados promissores em visão computacional.

- **Tipo de Classificação:** Multiclasse, abrangendo sete categorias de lesões de pele, incluindo melanoma, nevos melanocíticos, carcinoma basocelular, entre outros.

- **Conjunto de Dados Utilizado:** Dataset de Skin Cancer disponível na Hugging Face, derivado do conjunto de dados de Marmal88.

- **Desempenho:**

    - Acurácia de Validação: Aproximadamente 96.95% após 5 epochs de treinamento.

#### Considerações:

O modelo demonstra excelente desempenho em tarefas de classificação multiclasse, o que é particularmente útil em cenários clínicos que requerem distinção entre diversos tipos de lesões.

A utilização do ViT permite capturar relações espaciais complexas nas imagens, potencialmente contribuindo para a alta acurácia observada.

### Comparação e Escolha do Modelo

Ao comparar ambos os modelos, observa-se que:

- **Desempenho:** O modelo Anwarkh1/Skin_Cancer-Image_Classification supera o Conn-Cerberus/Soma_Skin_Cancer_Classifier em termos de acurácia geral (96.95% vs. 89%).

- **Capacidade de Classificação:** Enquanto o modelo Conn-Cerberus realiza uma classificação binária, o modelo Anwarkh1 é capaz de distinguir entre múltiplas classes de lesões, oferecendo uma análise mais detalhada.

- **Arquitetura:** O uso do Vision Transformer no modelo Anwarkh1 proporciona vantagens na captura de padrões complexos nas imagens, o que pode ser benéfico para a classificação precisa de lesões de pele.

Diante desses fatores, o modelo Anwarkh1/Skin_Cancer-Image_Classification se destaca como a escolha mais adequada para aplicações que requerem alta precisão e capacidade de distinguir entre diversos tipos de lesões de pele. Sua arquitetura avançada e desempenho superior o tornam uma ferramenta promissora para auxiliar na análise e diagnóstico de lesões dermatológicas.

## Justificativa para a Escolha de um Modelo Pré-Treinado em Detrimento do NIMA

Embora o NIMA (Neural Image Assessment) seja eficaz na avaliação da qualidade de imagens com base na percepção humana, sua aplicação principal é na análise estética ou técnica da imagem, e não na classificação diagnóstica de lesões de pele. Optar por um modelo pré-treinado, como o Anwarkh1/Skin_Cancer-Image_Classification, oferece diversas vantagens:

- **Foco na Classificação Diagnóstica:** Modelos pré-treinados são especificamente ajustados para identificar e classificar diferentes tipos de lesões de pele, proporcionando resultados diretamente aplicáveis ao diagnóstico clínico.

- **Eficiência de Recursos:** Utilizar modelos pré-treinados reduz significativamente o tempo e os recursos computacionais necessários para treinamento, permitindo uma implementação mais rápida e econômica.

- **Desempenho Superior:** Como evidenciado, modelos como o Anwarkh1 alcançam acurácias elevadas (96.95%), superando as capacidades do NIMA em tarefas de classificação de lesões.

- **Aproveitamento de Arquiteturas Avançadas:** Modelos como o Anwarkh1 utilizam arquiteturas modernas, como o Vision Transformer, que capturam relações espaciais complexas nas imagens, melhorando a precisão da classificação.

Portanto, para o objetivo de classificar lesões de pele com alta precisão e eficiência, a escolha por um modelo pré-treinado, como o Anwarkh1/Skin_Cancer-Image_Classification, é mais apropriada do que a utilização do NIMA, que é mais adequado para avaliação geral da qualidade de imagens.

