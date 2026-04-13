# 📊 Desafio DIO - Diagrama Dimensional (Star Schema)

![Status](https://img.shields.io/badge/status-active-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
![Build](https://img.shields.io/badge/build-passing-success)
![Database](https://img.shields.io/badge/database-MySQL-orange)

## 🎯 Objetivo
Criar um **modelo dimensional (Star Schema)** para análise de dados relacionados aos **professores**, cursos ministrados, disciplinas e departamentos.  
O foco é a **tabela fato Fato_Atuacao_Professor**, que reflete o evento de um professor ministrando uma disciplina para um curso em uma determinada data.

> **Observação:** Dados de alunos e matrículas foram descartados para manter o escopo no corpo docente.

---

## 📐 Estrutura do Projeto

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

## ⚙️ Requisitos

- **Banco de Dados:** MySQL 8+  
- **Ferramentas de BI:** Power BI (opcional para visualização)  
- **Modelo:** Star Schema puro (sem snowflake)  

---

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

| Etapa                          | Status     |
|--------------------------------|------------|
| Modelagem Dimensional           | ✅ Concluído |
| Criação das Tabelas             | ✅ Concluído |

---

## 📜 Script SQL (Resumo)

```sql
CREATE DATABASE IF NOT EXISTS dw_universidade;
USE dw_universidade;

-- Exemplo de criação de dimensão
CREATE TABLE Dim_Professor (
  sk_professor INT AUTO_INCREMENT PRIMARY KEY,
  nk_idProfessor INT NOT NULL,
  nome_professor VARCHAR(100),
  titulacao VARCHAR(45),
  is_coordenador TINYINT(1) DEFAULT 0
);

-- Exemplo de criação da tabela fato
CREATE TABLE Fato_Atuacao_Professor (
  sk_professor INT,
  sk_departamento INT,
  sk_disciplina INT,
  sk_curso INT,
  sk_data_oferta INT,
  carga_horaria_lecionada INT,
  qtd_ofertas INT DEFAULT 1,
  CONSTRAINT fk_fato_professor FOREIGN KEY (sk_professor) REFERENCES Dim_Professor(sk_professor)
);

