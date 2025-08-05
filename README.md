# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: [DevJoaoVitorB](https://github.com/DevJoa)

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arquivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

### 1. Alocação Inicial com Best-Fit
**Configuração**:  
- Memória RAM: 64 KB contígua (inicialmente vazia)
- Processos: P1 (20 KB), P2 (15 KB), P3 (30 KB), P4 (10 KB), P5 (18 KB)

**Passos de Alocação**:
1. **P1 (20 KB)**
   - Alocado em `[0KB-20KB]`
   - Espaço livre: `[20KB-64KB]` (44 KB)

2. **P2 (15 KB)**
   - Best-fit: `[20KB-35KB]`
   - Espaço livre: `[35KB-64KB]` (29 KB)

3. **P4 (10 KB)**
   - Best-fit: `[35KB-45KB]`
   - Espaço livre: `[45KB-64KB]` (19 KB)

4. **P5 (18 KB)**
   - Best-fit: `[45KB-63KB]`
   - Espaço livre: `[63KB-64KB]` (1 KB)

**Resultado Final**:
| Processo | Alocação    | Status  |
|----------|-------------|---------|
| P1       | [0KB-20KB]  | RAM     |
| P2       | [20KB-35KB] | RAM     |
| P4       | [35KB-45KB] | RAM     |
| P5       | [45KB-63KB] | RAM     |
| P3       | -           | Disco   |

> **Observação**: P3 (30 KB) não pôde ser alocado por falta de espaço contíguo.

<br>

### 2. Simular Memória Virtual (Paginação)
**Tabela de Páginas**:
| Processo | Faixa       | RAM/Disco | Localização       |
|----------|-------------|-----------|-------------------|
| P1       | 0-20 KB     | RAM       | [0KB-20KB]        |
| P2       | 20-35 KB    | RAM       | [20KB-35KB]       |
| P4       | 35-45 KB    | RAM       | [35KB-45KB]       |
| P5       | 45-63 KB    | RAM       | [45KB-63KB]       |
| P3       | 0-30 KB     | Disco     | Área de Swap      |

**Mecanismo de Paginação**:
- P3 foi movido para o disco por insuficiência de RAM
- Acesso a P3 ocorre via sistema de paginação (mais lento)

<br>

### 3. Desfragmentação da RAM
**Estado Pré-Desfragmentação**:
- Fragmentação: 1 KB livre em `[63KB-64KB]`
- Processos alocados: P1, P2, P4, P5

**Operação de Compactação**:
1. Todos os processos já estão contíguos
2. Espaço livre consolidado permanece: `[63KB-64KB]` (1 KB)

**Resultado**:
- **Não houve melhoria** na capacidade de alocação
- P3 continua impossibilitado de ser carregado na RAM

**Alternativa**:
- Swapping: Remover P4 ou P5 para liberar espaço para P3

<br>

### 4. Questões para Reflexão

#### 1. Eficiência do Best-Fit
- **Neste cenário**:  
  - **Desvantagem**: Deixou 1 KB inutilizável e falhou ao alocar P3
  - **Comparativo**:
    - *Worst-fit*: Alocaria P5 em `[0KB-18KB]`, liberando `[18KB-64KB]` (46 KB) - suficiente para P3
    - *First-fit*: Similar ao best-fit, mas com menor overhead computacional

#### 2. Memória Virtual e Deadlock
- **Prevenção**:  
  Permitiu que P3 fosse executado via disco, mesmo sem espaço na RAM, evitando:
  - Bloqueio permanente de processos
  - Espera circular por recursos

#### 3. Impacto da Desfragmentação
- **Custos**:
  - Tempo de CPU para reorganização
  - Suspensão temporária de processos durante a compactação
- **Benefícios**:
  - Neste caso específico: **nenhum** (fragmentação mínima)
  - Em geral: Redução de fragmentação externa

**Conclusão**:  
O Best-fit mostrou-se ineficiente para alocar processos grandes em memórias com fragmentação significativa, enquanto a memória virtual atuou como mecanismo crítico de resiliência.
