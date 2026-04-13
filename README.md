# 📊 Desafio DIO - Diagrama Dimensional (Star Schema)

![Database](https://img.shields.io/badge/database-MySQL-orange)
![Build](https://img.shields.io/badge/build-success-success)
![Status](https://img.shields.io/badge/status-finalizado-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
[![GitHub Repo Size](https://img.shields.io/github/repo-size/cristianorr38/desafio-dio-diagrama-dimensional-star-schema)](https://github.com/cristianorr38/desafio-dio-diagrama-dimensional-star-schema)
[![GitHub Stars](https://img.shields.io/github/stars/cristianorr38/desafio-dio-diagrama-dimensional-star-schema?style=social)](https://github.com/cristianorr38/desafio-dio-diagrama-dimensional-star-schema)

## 🎯 Objetivo
Criar um **modelo dimensional (Star Schema)** para análise de dados relacionados aos **professores**, cursos ministrados, disciplinas e departamentos.  
O foco é a **tabela fato Fato_Atuacao_Professor**, que reflete o evento de um professor ministrando uma disciplina para um curso em uma determinada data.

> **Observação:** Dados de alunos e matrículas foram descartados para manter o escopo no corpo docente.

**Diagrama Entidade Relacionamento**

<img width="1465" height="859" alt="Diagrama_exemplo" src="https://github.com/user-attachments/assets/f0c5df1a-0d62-42a3-8a14-c9f976da51a8" />


---

## 📐 Estrutura do Modelo Dimensilnal

### 🔹 Tabela Fato
**Fato_Atuacao_Professor**  
Granularidade: oferta de uma disciplina por um professor, para um curso específico, em um determinado período.

- `sk_professor` (FK → Dim_Professor)  
- `sk_departamento` (FK → Dim_Departamento)  
- `sk_disciplina` (FK → Dim_Disciplina)  
- `sk_curso` (FK → Dim_Curso)  
- `sk_data_oferta` (FK → Dim_Data)  
- `carga_horaria_lecionada` (Métrica)  
- `qtd_ofertas` (Métrica, valor fixo = 1)

---

### 🔹 Tabelas Dimensão

**Dim_Professor**  
- Nome, Titulação, Coordenador (booleano)

**Dim_Departamento**  
- Nome, Campus

**Dim_Disciplina**  
- Nome, Carga Horária Padrão

**Dim_Curso**  
- Nome do Curso

**Dim_Data**  
- Data Completa, Ano, Semestre, Trimestre, Mês (número e nome)

---

**Diagrama Modelo Dimensional (Star Schema)**

<img width="890" height="640" alt="Modelo_Dimensional_Atuacao_Professor" src="https://github.com/user-attachments/assets/37cec4a7-9ad2-435b-be87-f1d727934891" />

## ⚙️ Requisitos

- **Banco de Dados:** MySQL 8+
- **Modelo:** Star Schema puro (sem snowflake)  

---

## 📂 Estrutura do Projeto
```plaintext
📂     desafio-dio-diagrama-dimensional-star-schema
├── 📂 data
│   ├── 📄 
│   └── 📄 
│
└── 📄 README.md
```

## 🚀 Roadmap

- [x] Definição da granularidade da tabela fato  
- [x] Criação das dimensões principais (Professor, Departamento, Disciplina, Curso)  
- [x] Inclusão da dimensão de tempo (Data)

---

## 📦 Release

**Versão 1.0.0**  
- Estrutura inicial do **Data Warehouse** criada  
- Script SQL para criação das tabelas incluído  
- Modelo dimensional documentado  

---

## 📊 Barra de Progresso

Progresso atual:  
`█████████████████████` **100% Concluído**

---

## 📜 Script SQL (Resumo)

```sql
-- 1. Criar o Schema/Banco de Dados

CREATE DATABASE IF NOT EXISTS star_schema_atuacao_professor;
USE star_schema_atuacao_professor;

-- 2. Criar Tabelas de Dimensão

CREATE TABLE Dim_Professor (
    sk_professor INT AUTO_INCREMENT PRIMARY KEY,
    nk_idProfessor INT NOT NULL,
    nome_professor VARCHAR(100),
    titulacao VARCHAR(45),
    is_coordenador TINYINT(1) DEFAULT 0
);

CREATE TABLE Dim_Departamento (
    sk_departamento INT AUTO_INCREMENT PRIMARY KEY,
    nk_idDepartamento INT NOT NULL,
    nome_departamento VARCHAR(100),
    campus VARCHAR(45)
);

CREATE TABLE Dim_Disciplina (
    sk_disciplina INT AUTO_INCREMENT PRIMARY KEY,
    nk_idDisciplina INT NOT NULL,
    nome_disciplina VARCHAR(100),
    carga_horaria_padrao INT
);

CREATE TABLE Dim_Curso (
    sk_curso INT AUTO_INCREMENT PRIMARY KEY,
    nk_idCurso INT NOT NULL,
    nome_curso VARCHAR(100)
);

CREATE TABLE Dim_Data (
    sk_data INT PRIMARY KEY, -- Formato YYYYMMDD
    data_completa DATE NOT NULL,
    ano INT,
    semestre TINYINT,
    trimestre TINYINT,
    mes_numero TINYINT,
    nome_mes VARCHAR(20)
);

-- 3. Criar Tabela Fato

CREATE TABLE Fato_Atuacao_Professor (
    sk_professor INT,
    sk_departamento INT,
    sk_disciplina INT,
    sk_curso INT,
    sk_data_oferta INT,
    carga_horaria_lecionada INT,
    qtd_ofertas INT DEFAULT 1,
    
    -- Chaves Estrangeiras
    CONSTRAINT fk_fato_professor FOREIGN KEY (sk_professor) REFERENCES Dim_Professor(sk_professor),
    CONSTRAINT fk_fato_depto FOREIGN KEY (sk_departamento) REFERENCES Dim_Departamento(sk_departamento),
    CONSTRAINT fk_fato_disciplina FOREIGN KEY (sk_disciplina) REFERENCES Dim_Disciplina(sk_disciplina),
    CONSTRAINT fk_fato_curso FOREIGN KEY (sk_curso) REFERENCES Dim_Curso(sk_curso),
    CONSTRAINT fk_fato_data FOREIGN KEY (sk_data_oferta) REFERENCES Dim_Data(sk_data)
);
