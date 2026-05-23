========================================================================
PROJETO: PCG - Uso de Algoritmos para Solução de Problemas de Engenharia
TEMA: Otimização de Rotas de Entrega de Materiais na Engenharia Civil
ABORDAGEM: Heurística Construtiva do Vizinho Mais Próximo (Nearest Neighbor)
DISCIPLINA: Algoritmos e Programação de Computadores
COMPLEXIDADE APROXIMADA: O(n²)
========================================================================

Algoritmo Otimizacao_Rotas_Entrega
Variáveis
    // Matriz de adjacência bidimensional para armazenar a malha logística de distâncias
    distancias : Matriz de Real [1..8, 1..8] 
    
    // Vetor booleano de controle de estado (indica se a obra já foi visitada)
    // CRÍTICO: Evita loops infinitos e redundâncias na rota logístca
    visitado : Vetor de Lógico [1..8]       
    
    // Vetor que armazena a sequência final do Circuito Hamiltoniano gerado
    rota_final : Vetor de Inteiro [1..8]    
    
    // Variáveis de indexação, ponteiros de nós e contadores de loops aninhados
    ponto_atual, proximo_ponto, i, j : Inteiro
    
    // Variáveis de cálculo métrico e minimização local
    menor_distancia, distancia_total : Real
    
    // Define dinamicamente o tamanho do espaço amostral conforme o teste (3, 5 ou 8)
    num_pontos : Inteiro                    

Início
    // ====================================================================
    // PASSO 1: INICIALIZAÇÃO DO SISTEMA E ALOCAÇÃO DE ESTADOS LÓGICOS
    // ====================================================================
    distancia_total <- 0
    num_pontos <- 8 // Definir aqui o cenário de teste: 3, 5 ou 8 pontos

    // Inicializa todas as localidades como "Não Visitadas" (Falso)
    Para i de 1 até num_pontos Faça
// A primeira posição já foi ocupada pelo ponto inicial
        visitado[i] <- Falso
    FimPara

    // Definição do Nó Raiz (Ex: Centro de Distribuição / Ponto Inicial)
    ponto_atual <- 1
    visitado[ponto_atual] <- Verdadeiro
    rota_final[1] <- ponto_atual

    // ====================================================================
    // PASSO 2: LOOP EXTERNO - ESTRUTURA DE REPETIÇÃO PRINCIPAL
    // ====================================================================
    // O loop corre a partir do segundo ponto até preencher todas as paragens
    Para i de 2 até num_pontos Faça
        
        // Define um valor lógico arbitrariamente alto simulando o "Infinito" 
        // para permitir a correta comparação de minimização na primeira iteração
        menor_distancia <- 99999.9 
        proximo_ponto <- -1

        // ----------------------------------------------------------------
        // LOOP INTERNO: VARREDURA DA MATRIZ E TOMADA DE DECISÃO (MÍOPE)
        // ----------------------------------------------------------------
        // Varre a linha correspondente ao 'ponto_atual' na matriz de adjacência
        Para j de 1 até num_pontos Faça
            
            // Condicional estruturada complexa: avalia se o destino NÃO foi visitado 
            // E se o arco/distância atual é menor do que a menor distância registada
            Se (Não visitado[j]) E (distancias[ponto_atual, j] < menor_distancia) Então
                menor_distancia <- distancias[ponto_atual, j]
                proximo_ponto <- j // Atualiza o ponteiro para o vizinho mais próximo local
            FimSe
            
        FimPara
        // ----------------------------------------------------------------

        // ====================================================================
        // PASSO 3: ATUALIZAÇÃO PARAMÉTRICA DE ESTADOS
        // ====================================================================
        // Se um vizinho válido foi encontrado, consolida a deslocação
        Se proximo_ponto <> -1 Então
            visitado[proximo_ponto] <- Verdadeiro // Altera o estado para visitado
            rota_final[i] <- proximo_ponto        // Insere o ponto na sequência da rota
            distancia_total <- distancia_total + menor_distancia // Acumula a métrica de custo
            ponto_atual <- proximo_ponto // Transpõe o ponteiro: o destino passa a ser a nova origem
        FimSe
        
    FimPara

    // ====================================================================
    // PASSO 4: FECHAMENTO DO CIRCUITO (RETORNO OBRIGATÓRIO À RAIZ)
    // ====================================================================
    // Conecta a última obra visitada de volta ao ponto inicial (Nó 1)
    distancia_total <- distancia_total + distancias[ponto_atual, 1]
    
    // ====================================================================
    // SAÍDA DE DADOS: OUTPUT ESTRUTURADO DO RELATÓRIO LOGÍSTICO
    // ====================================================================
    Escreval("--- RELATÓRIO DE ROTEIRIZAÇÃO AUTOMATIZADA ---")
    Escreval("Sequência final de visitação dos canteiros de obras: ")
    
    Para i de 1 até num_pontos Faça
        Escrever(rota_final[i], " -> ")
    FimPara
    Escreval(1) // Output gráfico do retorno à origem
    
    Escreval("Desempenho da Heurística (Distância Total): ", distancia_total, " km")
    Escreval("--------------------------------------------------")
Fim
