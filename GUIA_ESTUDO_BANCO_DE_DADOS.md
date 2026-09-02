# 🗄️ GUIA DEFINITIVO DE REVISÃO: BANCO DE DADOS & SGBD (SGBD 01)
**Professor:** Prof. Flávio Mania | **Curso:** Tecnologia em Sistemas para Internet — IFSP Itapetininga  
**Material Consolidado:** Aulas 01, 02, 03, 04, Atividades Práticas 1, 2, 3 e 4 (Avaliação de Banco de Dados 1).

---

## 🧭 MAPA MENTAL RÁPIDO PARA A PROVA
* **Dado vs Informação vs Conhecimento:**
  * **Dado:** Elemento bruto sem contexto (ex: `37.5`, `"Vermelho"`).
  * **Informação:** Dado processado e contextualizado com significado (ex: `Temperatura de 37.5°C`).
  * **Conhecimento:** Informação compreendida que gera padrão, insight ou tendência (ex: `Paciente pode estar com febre`, `Hábitos de compra dos clientes`).
  * **Decisão:** Ação tomada a partir do conhecimento (ex: `Acionar protocolo médico`).
* **SGBD (Sistema de Gerenciamento de Banco de Dados):** Software que gerencia armazenamento, segurança, concorrência e integridade dos dados (ex: PostgreSQL, MySQL, Oracle).
* **Álgebra Relacional (Operadores Fundamentais):**
  * **Seleção ($\\sigma$ - Sigma):** Filtra **linhas / tuplas** por uma condição ($\\sigma_{\\text{condição}}(Tabela)$).
  * **Projeção ($\\pi$ - Pi):** Filtra **colunas / atributos** desejadas ($\\pi_{\\text{colunas}}(Tabela)$).
  * **Junção ($\\bowtie$ ou $|X|$):** Combina tuplas de tabelas relacionadas baseadas em chaves.
  * **União ($\\cup$):** Junta tuplas de duas tabelas compatíveis.
  * **Diferença ($-$)**: Elementos que estão na 1ª tabela mas NÃO na 2ª ($\\pi_{RA}(Aluno) - \\pi_{RA}(Matricula)$ = Alunos sem matrícula).
  * **Operadores Lógicos:** $\\wedge$ (E / AND), $\\vee$ (OU / OR), $\\neg$ (NÃO / NOT).
* **Modelo Conceitual (MER / DER de Peter Chen):**
  * **Retângulo:** Entidade (Forte: simples; Fraca: duplo).
  * **Elipse / Oval:** Atributo (Chave Primária/Identificador: texto sublinhado $\\underline{\\text{id}}$ ou preenchida; Multivalorado: elipse dupla; Derivado: elipse tracejada; Composto: ramificado).
  * **Losango:** Relacionamento (associação entre entidades).
  * **Cardinalidade:** Razão de ocorrências: `(1,1)`, `(0,N)`, `(1,N)`, `(0,1)` ou simplificada `1:1`, `1:N`, `N:N`.
* **Modelo Lógico Relacional (Tabelas e Regras de Mapeamento):**
  * **Chave Primária (PK):** Identifica unicamente cada registro (`NOT NULL` e único).
  * **Chave Estrangeira (FK):** Campo que faz referência à Chave Primária de outra tabela para estabelecer o relacionamento.
  * **Regra 1:N:** A PK do lado `1` migra como **FK** para o lado `N`.
  * **Regra N:N:** Cria-se uma **nova tabela intermediária** (associativa) contendo as FKs das duas entidades como PK composta.

---

## 1. DADO, INFORMAÇÃO, CONHECIMENTO E SGBD (AULA 01)

### 📌 A Pirâmide do Conhecimento:

```
        /\
       /  \    DECISÃO: Acionar protocolo médico / Estratégia de vendas
      /    \
     / CONHE- \   CONHECIMENTO: "Paciente pode estar com febre" / Rotina de compras
    / CIMENTO \
   /------------\
  /  INFORMAÇÃO  \  INFORMAÇÃO: "Temperatura corporal de 37.5°C" (Dado com contexto)
 /----------------\
/   DADO BRUTO     \ DADO: "37.5", "F", "Vermelho", "10" (Fato isolado sem significado)
--------------------
```

### 📌 Comparativo Conceitual (QUESTÃO CERTA NO MOODLE):
| Conceito | Definição | Exemplo Prático |
| :--- | :--- | :--- |
| **Dado** | Símbolo, número ou caractere isolado, sem tratamento ou contexto. | `37.5`, `Sorocaba`, `F` |
| **Informação** | Dado estruturado, processado e contextualizado com significado útil. | `Temperatura de 37.5°C`, `Cliente reside em Sorocaba` |
| **Conhecimento** | Compreensão, análise de padrões e tendências obtidas da informação. | `Paciente apresenta quadro febril`, `Tendências de consumo por cidade` |
| **Metadados** | "Dados sobre os dados": especificações de tipo, tamanho, formato e regras. | `codigo INT (PK)`, `nome VARCHAR(100) NOT NULL` |

### 📌 Escala de Unidades de Armazenamento:
$$\\text{Bit (b)} \\rightarrow \\text{Byte (B)} \\rightarrow \\text{KB} (10^3) \\rightarrow \\text{MB} (10^6) \\rightarrow \\text{GB} (10^9) \\rightarrow \\text{TB} (10^{12}) \\rightarrow \\text{PB} (10^{15}) \\rightarrow \\text{EB} (10^{18}) \\rightarrow \\text{ZB} (10^{21}) \\rightarrow \\text{YB} (10^{24})$$

---

## 2. ÁLGEBRA RELACIONAL (AULA 02 & ATIVIDADE 1)

A Álgebra Relacional é uma linguagem formal de consulta baseada em operações sobre relações (tabelas).

### 📌 1. Seleção ($\\sigma$ - Sigma grego)
* **Objetivo:** Filtrar **linhas (tuplas)** que atendem a um critério lógico.
* **Sintaxe:** $\\sigma_{\\text{condição}}(Relação)$
* **Exemplos Clássicos:**
  * Clientes do sexo feminino: $\\sigma_{sexo = \x27F\x27}(CLIENTE)$
  * Clientes do sexo feminino que gostam de azul: $\\sigma_{(sexo = \x27F\x27 \\wedge cor\_preferida = \x27Azul\x27)}(CLIENTE)$
  * Alunos de Sistemas: $\\sigma_{Curso = \x27Sistemas\x27}(Aluno)$

### 📌 2. Projeção ($\\pi$ - Pi grego)
* **Objetivo:** Filtrar **colunas (atributos)** específicas, descartando as demais e eliminando linhas duplicadas.
* **Sintaxe:** $\\pi_{\\text{lista\_de\_atributos}}(Relação)$
* **Exemplos Clássicos:**
  * Apenas os nomes dos alunos: $\\pi_{Nome}(Aluno)$
  * Nome e cidade de todos os clientes: $\\pi_{(nome, cidade)}(CLIENTE)$
  * RA e Nome dos alunos: $\\pi_{RA, Nome}(Aluno)$

### 📌 3. Junção ($\\bowtie$ ou $|X|$)
* **Objetivo:** Combinar tuplas de duas tabelas com base em igualdade de chaves (PK = FK).
* **Exemplo:** $\\pi_{Nome, NomeDisc}(Aluno \\bowtie Matricula \\bowtie Disciplina)$

### 📌 4. Operações de Conjuntos ($\\cup$, $-$, $\\cap$):
* **União ($\\cup$):** Combina registros de tabelas com mesma estrutura (ex: $AlunoPresencial \\cup AlunoEAD$).
* **Diferença ($-$)**: Retorna registros que existem na primeira tabela mas NÃO na segunda.
  * *Exemplo clássico de prova:* Alunos não matriculados: $\\pi_{RA}(Aluno) - \\pi_{RA}(Matricula)$.

---

## 3. MODELO CONCEITUAL: MER / DER (AULA 03 & ATIVIDADE 3)

O Diagrama Entidade-Relacionamento (DER) é a representação visual de mais alto nível do banco de dados.

### 📌 Símbolos Oficiais de Peter Chen:

| Símbolo Gráfico | Elemento | Função e Significado | Exemplo |
| :---: | :---: | :--- | :--- |
| **Retângulo** | **Entidade** | Objeto ou conceito do mundo real sobre o qual guardamos dados. | `Paciente`, `Medico`, `Consulta`, `Cliente` |
| **Elipse / Oval** | **Atributo** | Característica ou propriedade que descreve a entidade. | `dtNascimento`, `Nome`, `CRM`, `valorTotal` |
| **Elipse com Texto Sublinhado** | **Chave Primária (PK)** | Identificador único e exclusivo de cada ocorrência da entidade. | $\\underline{\\text{codPaciente}}$, $\\underline{\\text{CRM}}$, $\\underline{\\text{idCliente}}$ |
| **Elipse Dupla** | **Atributo Multivalorado** | Atributo que pode ter vários valores para o mesmo registro. | `Telefones`, `Emails` |
| **Elipse Tracejada** | **Atributo Derivado** | Atributo calculado a partir de outro dado existente. | `Idade` (calculada de `dtNascimento`) |
| **Losango** | **Relacionamento** | Associação ou ação que conecta duas ou mais entidades. | `tem`, `atende`, `realiza`, `possui` |
| **(min, max)** ou `1:N` | **Cardinalidade** | Quantidade mínima e máxima de ocorrências associadas. | `(1,1)`, `(0,N)`, `(1,N)` |

---

## 4. MODELO LÓGICO RELACIONAL & MAPEAMENTO (AULA 04)

O Modelo Lógico traduz o DER em um **Esquema Relacional de Tabelas**, definindo Chaves Primárias (PK) e Chaves Estrangeiras (FK).

### 📌 Regras de Ouro de Mapeamento:

#### 1. Mapeamento 1:N (Um para Muitos):
* A Chave Primária (PK) da entidade do lado **1** é inserida como **Chave Estrangeira (FK)** na tabela do lado **N**.
* *Exemplo da Consulta Médica:*
  * `Paciente (1) ---- tem ---- (N) Consulta`
  * `Medico (1) ---- atende ---- (N) Consulta`
  * **Tabela Gerada:** `Consulta(<u>codConsulta</u>, dtConsulta, horario, codPaciente_FK, codMedico_FK)`

#### 2. Mapeamento N:N (Muitos para Muitos):
* Gera-se uma **nova tabela associativa** cuja Chave Primária é composta pelas Chaves Primárias das duas entidades originais (ambas atuando como FKs).
* *Exemplo da Biblioteca (Atividade 4):*
  * `Livro (N) ---- escreve ---- (N) Autor`
  * **Tabela Gerada:** `Livro_Autor(<u>ISBN_FK</u>, <u>codAutor_FK</u>)`

---

## 5. RESOLUÇÃO DETALHADA DAS 5 QUESTÕES DO TESTE MOODLE

### 🎯 Questão 1 (Álgebra Relacional - Seleção com Condição Composta):
* **Enunciado:** *"Agora a expressão para todos os clientes do sexo Feminino que gostam da cor azul"*
* **Tabela:** `CLIENTE (codigo, nome, sexo, cidade, cor_preferida)`
* **Montagem dos Slots:**
  $$\\sigma_{(sexo = \x27F\x27 \\wedge cor\_preferida = \x27Azul\x27)}(CLIENTE)$$
* **Ordem das Peças:**
  1. Operador: $\\sigma$ (Sigma)
  2. Subscrito / Condição: `(` $\\rightarrow$ `sexo` $\\rightarrow$ `=` $\\rightarrow$ `\x27F\x27` $\\rightarrow$ `^` $\\rightarrow$ `cor_preferida` $\\rightarrow$ `=` $\\rightarrow$ `\x27Azul\x27` $\\rightarrow$ `)`
  3. Alvo: `(CLIENTE)`

---

### 🎯 Questão 2 (Álgebra Relacional - Projeção de Atributos):
* **Enunciado:** *"Construir a expressão algébrica que faça: Apresentar o nome e a cidade de todos os cliente."*
* **Tabela:** `CLIENTE`
* **Montagem dos Slots:**
  $$\\pi_{(nome, cidade)}(Cliente)$$ ou $$\\pi_{nome, cidade}(Cliente)$$
* **Ordem das Peças:**
  1. Operador: $\\pi$ (Pi)
  2. Parênteses/Atributos: `(` $\\rightarrow$ `nome` $\\rightarrow$ `,` $\\rightarrow$ `cidade` $\\rightarrow$ `)`
  3. Alvo: `Cliente`

---

### 🎯 Questão 3 (Modelo Conceitual - Completar o DER da Clínica Médica):
* **Enunciado:** *"Completar o DER?"*
* **Mapeamento de cada Slot:**
  * **Entidade Esquerda (Superior):** `Paciente` | Atributo Chave: `codPaciente`
  * **Entidade Direita (Superior):** `Medico` | Atributo Chave: `codMedico`
  * **Entidade Central (Inferior):** `Consulta` | Atributo Chave: `codConsulta` | FKs: `codPaciente`, `codMedico`
  * **Relacionamento da Esquerda:** `tem` (Cardinalidade: `1` em Paciente, `N` em Consulta)
  * **Relacionamento da Direita:** `atende` (Cardinalidade: `1` em Medico, `N` em Consulta)

---

### 🎯 Questão 4 (Classificação: Dado, Informação e Conhecimento):
* `"37.5"` ou `"Sorocaba"` ➔ **Dado**
* `"Temperatura de 37.5°C"` ou `"Cliente reside em Sorocaba"` ➔ **Informação**
* `"Paciente pode estar com febre"` ou `"Padrão de consumo por região"` ➔ **Conhecimento**

---

### 🎯 Questão 5 (Classificação dos Símbolos do DER):
* **Retângulo** ➔ **Entidade**
* **Elipse / Oval** ➔ **Atributo**
* **Losango** ➔ **Relacionamento**
* **`(1,1)`, `(0,N)`, `1:N`** ➔ **Cardinalidade**
