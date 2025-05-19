CRIAÇÃO DO BANCO DE DADOS

CREATE DATABASE IF NOT EXISTS voleiDB;
USE voleiDB;

CREATE TABLE Teams (
    team_id INT AUTO_INCREMENT PRIMARY KEY,
    team_name VARCHAR(150) NOT NULL,
    city VARCHAR(100) NOT NULL
);


CREATE TABLE Players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    player_name VARCHAR(100) NOT NULL,
    team_id INT,
    position VARCHAR(50),
    number INT,
    age INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Coaches (
    coach_id INT AUTO_INCREMENT PRIMARY KEY,
    coach_name VARCHAR(100) NOT NULL,
    team_id INT,
    experience_years INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Tournaments (
    tournament_id INT AUTO_INCREMENT PRIMARY KEY,
    tournament_name VARCHAR(150) NOT NULL,
    location VARCHAR(100),
    edition INT,
    start_date DATE,
    end_date DATE
);


CREATE TABLE TeamTournament (
    team_id INT,
    tournament_id INT,
    PRIMARY KEY (team_id, tournament_id),
    FOREIGN KEY (team_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


CREATE TABLE Matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE,
    team_a_id INT,
    team_b_id INT,
    score_a INT,
    score_b INT,
    tournament_id INT,
    FOREIGN KEY (team_a_id) REFERENCES Teams(team_id),
    FOREIGN KEY (team_b_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


INSERT INTO Teams (team_name, city) VALUES
('Dentil/Praia Clube', 'Uberlândia'),
('Sesc-RJ/Flamengo', 'Rio de Janeiro'),
('Gerdau Minas', 'Belo Horizonte'),
('São Cristovão Saúde – Osasco', 'Osasco'),
('Sesi Vôlei Bauru', 'Bauru'),
('Fluminense', 'Rio de Janeiro'),
('Esporte Clube Pinheiros', 'São Paulo'),
('Barueri', 'Barueri'),
('Mackenzie Cia. do Terno', 'Campinas'),
('Brasília Vôlei', 'Brasília');


INSERT INTO Players (player_name, team_id, position, number, age) VALUES
('Carlos Silva', 1, 'Levantador', 7, 28),
('Maria Santos', 2, 'Ponta', 12, 26),
('João Lima', 3, 'Central', 5, 30),
('Ana Costa', 4, 'Oposta', 8, 24),
('Pedro Souza', 5, 'Líbero', 11, 27),
('Renato Mota', 6, 'Levantador', 10, 29),
('Beatriz Gomes', 7, 'Ponta', 3, 22),
('Eduardo Nunes', 8, 'Central', 6, 31),
('Fernanda Prado', 9, 'Oposta', 9, 25),
('Lucas Alves', 10, 'Líbero', 4, 23);


INSERT INTO Coaches (coach_name, team_id, experience_years) VALUES
('Ricardo Oliveira', 1, 15),
('Sandra Pereira', 2, 12),
('Fabiano Costa', 3, 10),
('Marta Souza', 4, 8),
('Bruno Fernandes', 5, 14),
('Claudia Matos', 6, 9),
('Julio Mendes', 7, 11),
('Camila Rocha', 8, 13),
('Rafael Dias', 9, 7),
('Patrícia Silva', 10, 10);


INSERT INTO Tournaments (tournament_name, location, edition, start_date, end_date) VALUES
('Copa do Vôlei Recife', 'Recife', 5, '2025-06-01', '2025-06-05'),
('Torneio Fortaleza de Vôlei', 'Fortaleza', 3, '2025-07-10', '2025-07-15'),
('Campeonato Olinda Vôlei', 'Olinda', 4, '2025-08-05', '2025-08-10'),
('Liga Maceió de Vôlei', 'Maceió', 2, '2025-09-15', '2025-09-20'),
('Circuito Natal Vôlei', 'Natal', 6, '2025-10-01', '2025-10-06'),
('Desafio João Pessoa Vôlei', 'João Pessoa', 1, '2025-11-10', '2025-11-15'),
('Festival Campina Vôlei', 'Campina Grande', 4, '2025-12-01', '2025-12-05'),
('Confronto São Paulo de Vôlei', 'São Paulo', 7, '2026-01-20', '2026-01-25'),
('Torneio Rio Vôlei', 'Rio de Janeiro', 2, '2026-02-10', '2026-02-15'),
('Desafio Belo Horizonte Vôlei', 'Belo Horizonte', 3, '2026-03-05', '2026-03-10');


INSERT INTO TeamTournament (team_id, tournament_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);


INSERT INTO Matches (match_date, team_a_id, team_b_id, score_a, score_b, tournament_id) VALUES
('2025-06-02', 1, 2, 3, 2, 1),
('2025-07-12', 3, 4, 0, 3, 2),
('2025-08-06', 5, 6, 3, 1, 3),
('2025-09-16', 7, 8, 2, 3, 4),
('2025-10-02', 9, 10, 3, 0, 5),
('2025-11-12', 1, 3, 1, 3, 6),
('2025-12-03', 2, 4, 3, 2, 7),
('2026-01-22', 5, 7, 2, 3, 8),
('2026-02-12', 6, 8, 3, 1, 9),
('2026-03-06', 9, 10, 0, 3, 10);


SELECT p.player_name, t.team_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
WHERE p.player_name LIKE '%a%';



SELECT c.coach_name, t.team_name
FROM Coaches c
INNER JOIN Teams t ON c.team_id = t.team_id
WHERE c.coach_name LIKE '%Silva%';



SELECT t.team_name, tr.tournament_name
FROM Teams t
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE t.city LIKE '%a%';



SELECT m.match_date, t1.team_name AS Team_A, t2.team_name AS Team_B, m.score_a, m.score_b
FROM Matches m
INNER JOIN Teams t1 ON m.team_a_id = t1.team_id
INNER JOIN Teams t2 ON m.team_b_id = t2.team_id
WHERE t1.team_name LIKE 'Sesc%';



SELECT p.player_name, t.team_name, tr.tournament_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE p.player_name LIKE '%a' AND tr.tournament_name LIKE '%Copa%';CRIAÇÃO DO BANCO DE DADOS

CREATE DATABASE IF NOT EXISTS voleiDB;
USE voleiDB;

CREATE TABLE Teams (
    team_id INT AUTO_INCREMENT PRIMARY KEY,
    team_name VARCHAR(150) NOT NULL,
    city VARCHAR(100) NOT NULL
);


CREATE TABLE Players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    player_name VARCHAR(100) NOT NULL,
    team_id INT,
    position VARCHAR(50),
    number INT,
    age INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Coaches (
    coach_id INT AUTO_INCREMENT PRIMARY KEY,
    coach_name VARCHAR(100) NOT NULL,
    team_id INT,
    experience_years INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Tournaments (
    tournament_id INT AUTO_INCREMENT PRIMARY KEY,
    tournament_name VARCHAR(150) NOT NULL,
    location VARCHAR(100),
    edition INT,
    start_date DATE,
    end_date DATE
);


CREATE TABLE TeamTournament (
    team_id INT,
    tournament_id INT,
    PRIMARY KEY (team_id, tournament_id),
    FOREIGN KEY (team_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


CREATE TABLE Matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE,
    team_a_id INT,
    team_b_id INT,
    score_a INT,
    score_b INT,
    tournament_id INT,
    FOREIGN KEY (team_a_id) REFERENCES Teams(team_id),
    FOREIGN KEY (team_b_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


INSERT INTO Teams (team_name, city) VALUES
('Dentil/Praia Clube', 'Uberlândia'),
('Sesc-RJ/Flamengo', 'Rio de Janeiro'),
('Gerdau Minas', 'Belo Horizonte'),
('São Cristovão Saúde – Osasco', 'Osasco'),
('Sesi Vôlei Bauru', 'Bauru'),
('Fluminense', 'Rio de Janeiro'),
('Esporte Clube Pinheiros', 'São Paulo'),
('Barueri', 'Barueri'),
('Mackenzie Cia. do Terno', 'Campinas'),
('Brasília Vôlei', 'Brasília');


INSERT INTO Players (player_name, team_id, position, number, age) VALUES
('Carlos Silva', 1, 'Levantador', 7, 28),
('Maria Santos', 2, 'Ponta', 12, 26),
('João Lima', 3, 'Central', 5, 30),
('Ana Costa', 4, 'Oposta', 8, 24),
('Pedro Souza', 5, 'Líbero', 11, 27),
('Renato Mota', 6, 'Levantador', 10, 29),
('Beatriz Gomes', 7, 'Ponta', 3, 22),
('Eduardo Nunes', 8, 'Central', 6, 31),
('Fernanda Prado', 9, 'Oposta', 9, 25),
('Lucas Alves', 10, 'Líbero', 4, 23);


INSERT INTO Coaches (coach_name, team_id, experience_years) VALUES
('Ricardo Oliveira', 1, 15),
('Sandra Pereira', 2, 12),
('Fabiano Costa', 3, 10),
('Marta Souza', 4, 8),
('Bruno Fernandes', 5, 14),
('Claudia Matos', 6, 9),
('Julio Mendes', 7, 11),
('Camila Rocha', 8, 13),
('Rafael Dias', 9, 7),
('Patrícia Silva', 10, 10);


INSERT INTO Tournaments (tournament_name, location, edition, start_date, end_date) VALUES
('Copa do Vôlei Recife', 'Recife', 5, '2025-06-01', '2025-06-05'),
('Torneio Fortaleza de Vôlei', 'Fortaleza', 3, '2025-07-10', '2025-07-15'),
('Campeonato Olinda Vôlei', 'Olinda', 4, '2025-08-05', '2025-08-10'),
('Liga Maceió de Vôlei', 'Maceió', 2, '2025-09-15', '2025-09-20'),
('Circuito Natal Vôlei', 'Natal', 6, '2025-10-01', '2025-10-06'),
('Desafio João Pessoa Vôlei', 'João Pessoa', 1, '2025-11-10', '2025-11-15'),
('Festival Campina Vôlei', 'Campina Grande', 4, '2025-12-01', '2025-12-05'),
('Confronto São Paulo de Vôlei', 'São Paulo', 7, '2026-01-20', '2026-01-25'),
('Torneio Rio Vôlei', 'Rio de Janeiro', 2, '2026-02-10', '2026-02-15'),
('Desafio Belo Horizonte Vôlei', 'Belo Horizonte', 3, '2026-03-05', '2026-03-10');


INSERT INTO TeamTournament (team_id, tournament_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);


INSERT INTO Matches (match_date, team_a_id, team_b_id, score_a, score_b, tournament_id) VALUES
('2025-06-02', 1, 2, 3, 2, 1),
('2025-07-12', 3, 4, 0, 3, 2),
('2025-08-06', 5, 6, 3, 1, 3),
('2025-09-16', 7, 8, 2, 3, 4),
('2025-10-02', 9, 10, 3, 0, 5),
('2025-11-12', 1, 3, 1, 3, 6),
('2025-12-03', 2, 4, 3, 2, 7),
('2026-01-22', 5, 7, 2, 3, 8),
('2026-02-12', 6, 8, 3, 1, 9),
('2026-03-06', 9, 10, 0, 3, 10);


SELECT p.player_name, t.team_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
WHERE p.player_name LIKE '%a%';



SELECT c.coach_name, t.team_name
FROM Coaches c
INNER JOIN Teams t ON c.team_id = t.team_id
WHERE c.coach_name LIKE '%Silva%';



SELECT t.team_name, tr.tournament_name
FROM Teams t
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE t.city LIKE '%a%';



SELECT m.match_date, t1.team_name AS Team_A, t2.team_name AS Team_B, m.score_a, m.score_b
FROM Matches m
INNER JOIN Teams t1 ON m.team_a_id = t1.team_id
INNER JOIN Teams t2 ON m.team_b_id = t2.team_id
WHERE t1.team_name LIKE 'Sesc%';



SELECT p.player_name, t.team_name, tr.tournament_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE p.player_name LIKE '%a' AND tr.tournament_name LIKE '%Copa%';CRIAÇÃO DO BANCO DE DADOS

CREATE DATABASE IF NOT EXISTS voleiDB;
USE voleiDB;

CREATE TABLE Teams (
    team_id INT AUTO_INCREMENT PRIMARY KEY,
    team_name VARCHAR(150) NOT NULL,
    city VARCHAR(100) NOT NULL
);


CREATE TABLE Players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    player_name VARCHAR(100) NOT NULL,
    team_id INT,
    position VARCHAR(50),
    number INT,
    age INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Coaches (
    coach_id INT AUTO_INCREMENT PRIMARY KEY,
    coach_name VARCHAR(100) NOT NULL,
    team_id INT,
    experience_years INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Tournaments (
    tournament_id INT AUTO_INCREMENT PRIMARY KEY,
    tournament_name VARCHAR(150) NOT NULL,
    location VARCHAR(100),
    edition INT,
    start_date DATE,
    end_date DATE
);


CREATE TABLE TeamTournament (
    team_id INT,
    tournament_id INT,
    PRIMARY KEY (team_id, tournament_id),
    FOREIGN KEY (team_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


CREATE TABLE Matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE,
    team_a_id INT,
    team_b_id INT,
    score_a INT,
    score_b INT,
    tournament_id INT,
    FOREIGN KEY (team_a_id) REFERENCES Teams(team_id),
    FOREIGN KEY (team_b_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


INSERT INTO Teams (team_name, city) VALUES
('Dentil/Praia Clube', 'Uberlândia'),
('Sesc-RJ/Flamengo', 'Rio de Janeiro'),
('Gerdau Minas', 'Belo Horizonte'),
('São Cristovão Saúde – Osasco', 'Osasco'),
('Sesi Vôlei Bauru', 'Bauru'),
('Fluminense', 'Rio de Janeiro'),
('Esporte Clube Pinheiros', 'São Paulo'),
('Barueri', 'Barueri'),
('Mackenzie Cia. do Terno', 'Campinas'),
('Brasília Vôlei', 'Brasília');


INSERT INTO Players (player_name, team_id, position, number, age) VALUES
('Carlos Silva', 1, 'Levantador', 7, 28),
('Maria Santos', 2, 'Ponta', 12, 26),
('João Lima', 3, 'Central', 5, 30),
('Ana Costa', 4, 'Oposta', 8, 24),
('Pedro Souza', 5, 'Líbero', 11, 27),
('Renato Mota', 6, 'Levantador', 10, 29),
('Beatriz Gomes', 7, 'Ponta', 3, 22),
('Eduardo Nunes', 8, 'Central', 6, 31),
('Fernanda Prado', 9, 'Oposta', 9, 25),
('Lucas Alves', 10, 'Líbero', 4, 23);


INSERT INTO Coaches (coach_name, team_id, experience_years) VALUES
('Ricardo Oliveira', 1, 15),
('Sandra Pereira', 2, 12),
('Fabiano Costa', 3, 10),
('Marta Souza', 4, 8),
('Bruno Fernandes', 5, 14),
('Claudia Matos', 6, 9),
('Julio Mendes', 7, 11),
('Camila Rocha', 8, 13),
('Rafael Dias', 9, 7),
('Patrícia Silva', 10, 10);


INSERT INTO Tournaments (tournament_name, location, edition, start_date, end_date) VALUES
('Copa do Vôlei Recife', 'Recife', 5, '2025-06-01', '2025-06-05'),
('Torneio Fortaleza de Vôlei', 'Fortaleza', 3, '2025-07-10', '2025-07-15'),
('Campeonato Olinda Vôlei', 'Olinda', 4, '2025-08-05', '2025-08-10'),
('Liga Maceió de Vôlei', 'Maceió', 2, '2025-09-15', '2025-09-20'),
('Circuito Natal Vôlei', 'Natal', 6, '2025-10-01', '2025-10-06'),
('Desafio João Pessoa Vôlei', 'João Pessoa', 1, '2025-11-10', '2025-11-15'),
('Festival Campina Vôlei', 'Campina Grande', 4, '2025-12-01', '2025-12-05'),
('Confronto São Paulo de Vôlei', 'São Paulo', 7, '2026-01-20', '2026-01-25'),
('Torneio Rio Vôlei', 'Rio de Janeiro', 2, '2026-02-10', '2026-02-15'),
('Desafio Belo Horizonte Vôlei', 'Belo Horizonte', 3, '2026-03-05', '2026-03-10');


INSERT INTO TeamTournament (team_id, tournament_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);


INSERT INTO Matches (match_date, team_a_id, team_b_id, score_a, score_b, tournament_id) VALUES
('2025-06-02', 1, 2, 3, 2, 1),
('2025-07-12', 3, 4, 0, 3, 2),
('2025-08-06', 5, 6, 3, 1, 3),
('2025-09-16', 7, 8, 2, 3, 4),
('2025-10-02', 9, 10, 3, 0, 5),
('2025-11-12', 1, 3, 1, 3, 6),
('2025-12-03', 2, 4, 3, 2, 7),
('2026-01-22', 5, 7, 2, 3, 8),
('2026-02-12', 6, 8, 3, 1, 9),
('2026-03-06', 9, 10, 0, 3, 10);


SELECT p.player_name, t.team_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
WHERE p.player_name LIKE '%a%';



SELECT c.coach_name, t.team_name
FROM Coaches c
INNER JOIN Teams t ON c.team_id = t.team_id
WHERE c.coach_name LIKE '%Silva%';



SELECT t.team_name, tr.tournament_name
FROM Teams t
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE t.city LIKE '%a%';



SELECT m.match_date, t1.team_name AS Team_A, t2.team_name AS Team_B, m.score_a, m.score_b
FROM Matches m
INNER JOIN Teams t1 ON m.team_a_id = t1.team_id
INNER JOIN Teams t2 ON m.team_b_id = t2.team_id
WHERE t1.team_name LIKE 'Sesc%';



SELECT p.player_name, t.team_name, tr.tournament_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE p.player_name LIKE '%a' AND tr.tournament_name LIKE '%Copa%';CRIAÇÃO DO BANCO DE DADOS

CREATE DATABASE IF NOT EXISTS voleiDB;
USE voleiDB;

CREATE TABLE Teams (
    team_id INT AUTO_INCREMENT PRIMARY KEY,
    team_name VARCHAR(150) NOT NULL,
    city VARCHAR(100) NOT NULL
);


CREATE TABLE Players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    player_name VARCHAR(100) NOT NULL,
    team_id INT,
    position VARCHAR(50),
    number INT,
    age INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Coaches (
    coach_id INT AUTO_INCREMENT PRIMARY KEY,
    coach_name VARCHAR(100) NOT NULL,
    team_id INT,
    experience_years INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Tournaments (
    tournament_id INT AUTO_INCREMENT PRIMARY KEY,
    tournament_name VARCHAR(150) NOT NULL,
    location VARCHAR(100),
    edition INT,
    start_date DATE,
    end_date DATE
);


CREATE TABLE TeamTournament (
    team_id INT,
    tournament_id INT,
    PRIMARY KEY (team_id, tournament_id),
    FOREIGN KEY (team_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


CREATE TABLE Matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE,
    team_a_id INT,
    team_b_id INT,
    score_a INT,
    score_b INT,
    tournament_id INT,
    FOREIGN KEY (team_a_id) REFERENCES Teams(team_id),
    FOREIGN KEY (team_b_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


INSERT INTO Teams (team_name, city) VALUES
('Dentil/Praia Clube', 'Uberlândia'),
('Sesc-RJ/Flamengo', 'Rio de Janeiro'),
('Gerdau Minas', 'Belo Horizonte'),
('São Cristovão Saúde – Osasco', 'Osasco'),
('Sesi Vôlei Bauru', 'Bauru'),
('Fluminense', 'Rio de Janeiro'),
('Esporte Clube Pinheiros', 'São Paulo'),
('Barueri', 'Barueri'),
('Mackenzie Cia. do Terno', 'Campinas'),
('Brasília Vôlei', 'Brasília');


INSERT INTO Players (player_name, team_id, position, number, age) VALUES
('Carlos Silva', 1, 'Levantador', 7, 28),
('Maria Santos', 2, 'Ponta', 12, 26),
('João Lima', 3, 'Central', 5, 30),
('Ana Costa', 4, 'Oposta', 8, 24),
('Pedro Souza', 5, 'Líbero', 11, 27),
('Renato Mota', 6, 'Levantador', 10, 29),
('Beatriz Gomes', 7, 'Ponta', 3, 22),
('Eduardo Nunes', 8, 'Central', 6, 31),
('Fernanda Prado', 9, 'Oposta', 9, 25),
('Lucas Alves', 10, 'Líbero', 4, 23);


INSERT INTO Coaches (coach_name, team_id, experience_years) VALUES
('Ricardo Oliveira', 1, 15),
('Sandra Pereira', 2, 12),
('Fabiano Costa', 3, 10),
('Marta Souza', 4, 8),
('Bruno Fernandes', 5, 14),
('Claudia Matos', 6, 9),
('Julio Mendes', 7, 11),
('Camila Rocha', 8, 13),
('Rafael Dias', 9, 7),
('Patrícia Silva', 10, 10);


INSERT INTO Tournaments (tournament_name, location, edition, start_date, end_date) VALUES
('Copa do Vôlei Recife', 'Recife', 5, '2025-06-01', '2025-06-05'),
('Torneio Fortaleza de Vôlei', 'Fortaleza', 3, '2025-07-10', '2025-07-15'),
('Campeonato Olinda Vôlei', 'Olinda', 4, '2025-08-05', '2025-08-10'),
('Liga Maceió de Vôlei', 'Maceió', 2, '2025-09-15', '2025-09-20'),
('Circuito Natal Vôlei', 'Natal', 6, '2025-10-01', '2025-10-06'),
('Desafio João Pessoa Vôlei', 'João Pessoa', 1, '2025-11-10', '2025-11-15'),
('Festival Campina Vôlei', 'Campina Grande', 4, '2025-12-01', '2025-12-05'),
('Confronto São Paulo de Vôlei', 'São Paulo', 7, '2026-01-20', '2026-01-25'),
('Torneio Rio Vôlei', 'Rio de Janeiro', 2, '2026-02-10', '2026-02-15'),
('Desafio Belo Horizonte Vôlei', 'Belo Horizonte', 3, '2026-03-05', '2026-03-10');


INSERT INTO TeamTournament (team_id, tournament_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);


INSERT INTO Matches (match_date, team_a_id, team_b_id, score_a, score_b, tournament_id) VALUES
('2025-06-02', 1, 2, 3, 2, 1),
('2025-07-12', 3, 4, 0, 3, 2),
('2025-08-06', 5, 6, 3, 1, 3),
('2025-09-16', 7, 8, 2, 3, 4),
('2025-10-02', 9, 10, 3, 0, 5),
('2025-11-12', 1, 3, 1, 3, 6),
('2025-12-03', 2, 4, 3, 2, 7),
('2026-01-22', 5, 7, 2, 3, 8),
('2026-02-12', 6, 8, 3, 1, 9),
('2026-03-06', 9, 10, 0, 3, 10);


SELECT p.player_name, t.team_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
WHERE p.player_name LIKE '%a%';



SELECT c.coach_name, t.team_name
FROM Coaches c
INNER JOIN Teams t ON c.team_id = t.team_id
WHERE c.coach_name LIKE '%Silva%';



SELECT t.team_name, tr.tournament_name
FROM Teams t
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE t.city LIKE '%a%';



SELECT m.match_date, t1.team_name AS Team_A, t2.team_name AS Team_B, m.score_a, m.score_b
FROM Matches m
INNER JOIN Teams t1 ON m.team_a_id = t1.team_id
INNER JOIN Teams t2 ON m.team_b_id = t2.team_id
WHERE t1.team_name LIKE 'Sesc%';



SELECT p.player_name, t.team_name, tr.tournament_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE p.player_name LIKE '%a' AND tr.tournament_name LIKE '%Copa%';CRIAÇÃO DO BANCO DE DADOS

CREATE DATABASE IF NOT EXISTS voleiDB;
USE voleiDB;

CREATE TABLE Teams (
    team_id INT AUTO_INCREMENT PRIMARY KEY,
    team_name VARCHAR(150) NOT NULL,
    city VARCHAR(100) NOT NULL
);


CREATE TABLE Players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    player_name VARCHAR(100) NOT NULL,
    team_id INT,
    position VARCHAR(50),
    number INT,
    age INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Coaches (
    coach_id INT AUTO_INCREMENT PRIMARY KEY,
    coach_name VARCHAR(100) NOT NULL,
    team_id INT,
    experience_years INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Tournaments (
    tournament_id INT AUTO_INCREMENT PRIMARY KEY,
    tournament_name VARCHAR(150) NOT NULL,
    location VARCHAR(100),
    edition INT,
    start_date DATE,
    end_date DATE
);


CREATE TABLE TeamTournament (
    team_id INT,
    tournament_id INT,
    PRIMARY KEY (team_id, tournament_id),
    FOREIGN KEY (team_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


CREATE TABLE Matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE,
    team_a_id INT,
    team_b_id INT,
    score_a INT,
    score_b INT,
    tournament_id INT,
    FOREIGN KEY (team_a_id) REFERENCES Teams(team_id),
    FOREIGN KEY (team_b_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


INSERT INTO Teams (team_name, city) VALUES
('Dentil/Praia Clube', 'Uberlândia'),
('Sesc-RJ/Flamengo', 'Rio de Janeiro'),
('Gerdau Minas', 'Belo Horizonte'),
('São Cristovão Saúde – Osasco', 'Osasco'),
('Sesi Vôlei Bauru', 'Bauru'),
('Fluminense', 'Rio de Janeiro'),
('Esporte Clube Pinheiros', 'São Paulo'),
('Barueri', 'Barueri'),
('Mackenzie Cia. do Terno', 'Campinas'),
('Brasília Vôlei', 'Brasília');


INSERT INTO Players (player_name, team_id, position, number, age) VALUES
('Carlos Silva', 1, 'Levantador', 7, 28),
('Maria Santos', 2, 'Ponta', 12, 26),
('João Lima', 3, 'Central', 5, 30),
('Ana Costa', 4, 'Oposta', 8, 24),
('Pedro Souza', 5, 'Líbero', 11, 27),
('Renato Mota', 6, 'Levantador', 10, 29),
('Beatriz Gomes', 7, 'Ponta', 3, 22),
('Eduardo Nunes', 8, 'Central', 6, 31),
('Fernanda Prado', 9, 'Oposta', 9, 25),
('Lucas Alves', 10, 'Líbero', 4, 23);


INSERT INTO Coaches (coach_name, team_id, experience_years) VALUES
('Ricardo Oliveira', 1, 15),
('Sandra Pereira', 2, 12),
('Fabiano Costa', 3, 10),
('Marta Souza', 4, 8),
('Bruno Fernandes', 5, 14),
('Claudia Matos', 6, 9),
('Julio Mendes', 7, 11),
('Camila Rocha', 8, 13),
('Rafael Dias', 9, 7),
('Patrícia Silva', 10, 10);


INSERT INTO Tournaments (tournament_name, location, edition, start_date, end_date) VALUES
('Copa do Vôlei Recife', 'Recife', 5, '2025-06-01', '2025-06-05'),
('Torneio Fortaleza de Vôlei', 'Fortaleza', 3, '2025-07-10', '2025-07-15'),
('Campeonato Olinda Vôlei', 'Olinda', 4, '2025-08-05', '2025-08-10'),
('Liga Maceió de Vôlei', 'Maceió', 2, '2025-09-15', '2025-09-20'),
('Circuito Natal Vôlei', 'Natal', 6, '2025-10-01', '2025-10-06'),
('Desafio João Pessoa Vôlei', 'João Pessoa', 1, '2025-11-10', '2025-11-15'),
('Festival Campina Vôlei', 'Campina Grande', 4, '2025-12-01', '2025-12-05'),
('Confronto São Paulo de Vôlei', 'São Paulo', 7, '2026-01-20', '2026-01-25'),
('Torneio Rio Vôlei', 'Rio de Janeiro', 2, '2026-02-10', '2026-02-15'),
('Desafio Belo Horizonte Vôlei', 'Belo Horizonte', 3, '2026-03-05', '2026-03-10');


INSERT INTO TeamTournament (team_id, tournament_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);


INSERT INTO Matches (match_date, team_a_id, team_b_id, score_a, score_b, tournament_id) VALUES
('2025-06-02', 1, 2, 3, 2, 1),
('2025-07-12', 3, 4, 0, 3, 2),
('2025-08-06', 5, 6, 3, 1, 3),
('2025-09-16', 7, 8, 2, 3, 4),
('2025-10-02', 9, 10, 3, 0, 5),
('2025-11-12', 1, 3, 1, 3, 6),
('2025-12-03', 2, 4, 3, 2, 7),
('2026-01-22', 5, 7, 2, 3, 8),
('2026-02-12', 6, 8, 3, 1, 9),
('2026-03-06', 9, 10, 0, 3, 10);


SELECT p.player_name, t.team_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
WHERE p.player_name LIKE '%a%';



SELECT c.coach_name, t.team_name
FROM Coaches c
INNER JOIN Teams t ON c.team_id = t.team_id
WHERE c.coach_name LIKE '%Silva%';



SELECT t.team_name, tr.tournament_name
FROM Teams t
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE t.city LIKE '%a%';



SELECT m.match_date, t1.team_name AS Team_A, t2.team_name AS Team_B, m.score_a, m.score_b
FROM Matches m
INNER JOIN Teams t1 ON m.team_a_id = t1.team_id
INNER JOIN Teams t2 ON m.team_b_id = t2.team_id
WHERE t1.team_name LIKE 'Sesc%';



SELECT p.player_name, t.team_name, tr.tournament_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE p.player_name LIKE '%a' AND tr.tournament_name LIKE '%Copa%';CRIAÇÃO DO BANCO DE DADOS

CREATE DATABASE IF NOT EXISTS voleiDB;
USE voleiDB;

CREATE TABLE Teams (
    team_id INT AUTO_INCREMENT PRIMARY KEY,
    team_name VARCHAR(150) NOT NULL,
    city VARCHAR(100) NOT NULL
);


CREATE TABLE Players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    player_name VARCHAR(100) NOT NULL,
    team_id INT,
    position VARCHAR(50),
    number INT,
    age INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Coaches (
    coach_id INT AUTO_INCREMENT PRIMARY KEY,
    coach_name VARCHAR(100) NOT NULL,
    team_id INT,
    experience_years INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Tournaments (
    tournament_id INT AUTO_INCREMENT PRIMARY KEY,
    tournament_name VARCHAR(150) NOT NULL,
    location VARCHAR(100),
    edition INT,
    start_date DATE,
    end_date DATE
);


CREATE TABLE TeamTournament (
    team_id INT,
    tournament_id INT,
    PRIMARY KEY (team_id, tournament_id),
    FOREIGN KEY (team_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


CREATE TABLE Matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE,
    team_a_id INT,
    team_b_id INT,
    score_a INT,
    score_b INT,
    tournament_id INT,
    FOREIGN KEY (team_a_id) REFERENCES Teams(team_id),
    FOREIGN KEY (team_b_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


INSERT INTO Teams (team_name, city) VALUES
('Dentil/Praia Clube', 'Uberlândia'),
('Sesc-RJ/Flamengo', 'Rio de Janeiro'),
('Gerdau Minas', 'Belo Horizonte'),
('São Cristovão Saúde – Osasco', 'Osasco'),
('Sesi Vôlei Bauru', 'Bauru'),
('Fluminense', 'Rio de Janeiro'),
('Esporte Clube Pinheiros', 'São Paulo'),
('Barueri', 'Barueri'),
('Mackenzie Cia. do Terno', 'Campinas'),
('Brasília Vôlei', 'Brasília');


INSERT INTO Players (player_name, team_id, position, number, age) VALUES
('Carlos Silva', 1, 'Levantador', 7, 28),
('Maria Santos', 2, 'Ponta', 12, 26),
('João Lima', 3, 'Central', 5, 30),
('Ana Costa', 4, 'Oposta', 8, 24),
('Pedro Souza', 5, 'Líbero', 11, 27),
('Renato Mota', 6, 'Levantador', 10, 29),
('Beatriz Gomes', 7, 'Ponta', 3, 22),
('Eduardo Nunes', 8, 'Central', 6, 31),
('Fernanda Prado', 9, 'Oposta', 9, 25),
('Lucas Alves', 10, 'Líbero', 4, 23);


INSERT INTO Coaches (coach_name, team_id, experience_years) VALUES
('Ricardo Oliveira', 1, 15),
('Sandra Pereira', 2, 12),
('Fabiano Costa', 3, 10),
('Marta Souza', 4, 8),
('Bruno Fernandes', 5, 14),
('Claudia Matos', 6, 9),
('Julio Mendes', 7, 11),
('Camila Rocha', 8, 13),
('Rafael Dias', 9, 7),
('Patrícia Silva', 10, 10);


INSERT INTO Tournaments (tournament_name, location, edition, start_date, end_date) VALUES
('Copa do Vôlei Recife', 'Recife', 5, '2025-06-01', '2025-06-05'),
('Torneio Fortaleza de Vôlei', 'Fortaleza', 3, '2025-07-10', '2025-07-15'),
('Campeonato Olinda Vôlei', 'Olinda', 4, '2025-08-05', '2025-08-10'),
('Liga Maceió de Vôlei', 'Maceió', 2, '2025-09-15', '2025-09-20'),
('Circuito Natal Vôlei', 'Natal', 6, '2025-10-01', '2025-10-06'),
('Desafio João Pessoa Vôlei', 'João Pessoa', 1, '2025-11-10', '2025-11-15'),
('Festival Campina Vôlei', 'Campina Grande', 4, '2025-12-01', '2025-12-05'),
('Confronto São Paulo de Vôlei', 'São Paulo', 7, '2026-01-20', '2026-01-25'),
('Torneio Rio Vôlei', 'Rio de Janeiro', 2, '2026-02-10', '2026-02-15'),
('Desafio Belo Horizonte Vôlei', 'Belo Horizonte', 3, '2026-03-05', '2026-03-10');


INSERT INTO TeamTournament (team_id, tournament_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);


INSERT INTO Matches (match_date, team_a_id, team_b_id, score_a, score_b, tournament_id) VALUES
('2025-06-02', 1, 2, 3, 2, 1),
('2025-07-12', 3, 4, 0, 3, 2),
('2025-08-06', 5, 6, 3, 1, 3),
('2025-09-16', 7, 8, 2, 3, 4),
('2025-10-02', 9, 10, 3, 0, 5),
('2025-11-12', 1, 3, 1, 3, 6),
('2025-12-03', 2, 4, 3, 2, 7),
('2026-01-22', 5, 7, 2, 3, 8),
('2026-02-12', 6, 8, 3, 1, 9),
('2026-03-06', 9, 10, 0, 3, 10);


SELECT p.player_name, t.team_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
WHERE p.player_name LIKE '%a%';



SELECT c.coach_name, t.team_name
FROM Coaches c
INNER JOIN Teams t ON c.team_id = t.team_id
WHERE c.coach_name LIKE '%Silva%';



SELECT t.team_name, tr.tournament_name
FROM Teams t
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE t.city LIKE '%a%';



SELECT m.match_date, t1.team_name AS Team_A, t2.team_name AS Team_B, m.score_a, m.score_b
FROM Matches m
INNER JOIN Teams t1 ON m.team_a_id = t1.team_id
INNER JOIN Teams t2 ON m.team_b_id = t2.team_id
WHERE t1.team_name LIKE 'Sesc%';



SELECT p.player_name, t.team_name, tr.tournament_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE p.player_name LIKE '%a' AND tr.tournament_name LIKE '%Copa%';CRIAÇÃO DO BANCO DE DADOS

CREATE DATABASE IF NOT EXISTS voleiDB;
USE voleiDB;

CREATE TABLE Teams (
    team_id INT AUTO_INCREMENT PRIMARY KEY,
    team_name VARCHAR(150) NOT NULL,
    city VARCHAR(100) NOT NULL
);


CREATE TABLE Players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    player_name VARCHAR(100) NOT NULL,
    team_id INT,
    position VARCHAR(50),
    number INT,
    age INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Coaches (
    coach_id INT AUTO_INCREMENT PRIMARY KEY,
    coach_name VARCHAR(100) NOT NULL,
    team_id INT,
    experience_years INT,
    FOREIGN KEY (team_id) REFERENCES Teams(team_id)
);


CREATE TABLE Tournaments (
    tournament_id INT AUTO_INCREMENT PRIMARY KEY,
    tournament_name VARCHAR(150) NOT NULL,
    location VARCHAR(100),
    edition INT,
    start_date DATE,
    end_date DATE
);


CREATE TABLE TeamTournament (
    team_id INT,
    tournament_id INT,
    PRIMARY KEY (team_id, tournament_id),
    FOREIGN KEY (team_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


CREATE TABLE Matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE,
    team_a_id INT,
    team_b_id INT,
    score_a INT,
    score_b INT,
    tournament_id INT,
    FOREIGN KEY (team_a_id) REFERENCES Teams(team_id),
    FOREIGN KEY (team_b_id) REFERENCES Teams(team_id),
    FOREIGN KEY (tournament_id) REFERENCES Tournaments(tournament_id)
);


INSERT INTO Teams (team_name, city) VALUES
('Dentil/Praia Clube', 'Uberlândia'),
('Sesc-RJ/Flamengo', 'Rio de Janeiro'),
('Gerdau Minas', 'Belo Horizonte'),
('São Cristovão Saúde – Osasco', 'Osasco'),
('Sesi Vôlei Bauru', 'Bauru'),
('Fluminense', 'Rio de Janeiro'),
('Esporte Clube Pinheiros', 'São Paulo'),
('Barueri', 'Barueri'),
('Mackenzie Cia. do Terno', 'Campinas'),
('Brasília Vôlei', 'Brasília');


INSERT INTO Players (player_name, team_id, position, number, age) VALUES
('Carlos Silva', 1, 'Levantador', 7, 28),
('Maria Santos', 2, 'Ponta', 12, 26),
('João Lima', 3, 'Central', 5, 30),
('Ana Costa', 4, 'Oposta', 8, 24),
('Pedro Souza', 5, 'Líbero', 11, 27),
('Renato Mota', 6, 'Levantador', 10, 29),
('Beatriz Gomes', 7, 'Ponta', 3, 22),
('Eduardo Nunes', 8, 'Central', 6, 31),
('Fernanda Prado', 9, 'Oposta', 9, 25),
('Lucas Alves', 10, 'Líbero', 4, 23);


INSERT INTO Coaches (coach_name, team_id, experience_years) VALUES
('Ricardo Oliveira', 1, 15),
('Sandra Pereira', 2, 12),
('Fabiano Costa', 3, 10),
('Marta Souza', 4, 8),
('Bruno Fernandes', 5, 14),
('Claudia Matos', 6, 9),
('Julio Mendes', 7, 11),
('Camila Rocha', 8, 13),
('Rafael Dias', 9, 7),
('Patrícia Silva', 10, 10);


INSERT INTO Tournaments (tournament_name, location, edition, start_date, end_date) VALUES
('Copa do Vôlei Recife', 'Recife', 5, '2025-06-01', '2025-06-05'),
('Torneio Fortaleza de Vôlei', 'Fortaleza', 3, '2025-07-10', '2025-07-15'),
('Campeonato Olinda Vôlei', 'Olinda', 4, '2025-08-05', '2025-08-10'),
('Liga Maceió de Vôlei', 'Maceió', 2, '2025-09-15', '2025-09-20'),
('Circuito Natal Vôlei', 'Natal', 6, '2025-10-01', '2025-10-06'),
('Desafio João Pessoa Vôlei', 'João Pessoa', 1, '2025-11-10', '2025-11-15'),
('Festival Campina Vôlei', 'Campina Grande', 4, '2025-12-01', '2025-12-05'),
('Confronto São Paulo de Vôlei', 'São Paulo', 7, '2026-01-20', '2026-01-25'),
('Torneio Rio Vôlei', 'Rio de Janeiro', 2, '2026-02-10', '2026-02-15'),
('Desafio Belo Horizonte Vôlei', 'Belo Horizonte', 3, '2026-03-05', '2026-03-10');


INSERT INTO TeamTournament (team_id, tournament_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);


INSERT INTO Matches (match_date, team_a_id, team_b_id, score_a, score_b, tournament_id) VALUES
('2025-06-02', 1, 2, 3, 2, 1),
('2025-07-12', 3, 4, 0, 3, 2),
('2025-08-06', 5, 6, 3, 1, 3),
('2025-09-16', 7, 8, 2, 3, 4),
('2025-10-02', 9, 10, 3, 0, 5),
('2025-11-12', 1, 3, 1, 3, 6),
('2025-12-03', 2, 4, 3, 2, 7),
('2026-01-22', 5, 7, 2, 3, 8),
('2026-02-12', 6, 8, 3, 1, 9),
('2026-03-06', 9, 10, 0, 3, 10);


SELECT p.player_name, t.team_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
WHERE p.player_name LIKE '%a%';



SELECT c.coach_name, t.team_name
FROM Coaches c
INNER JOIN Teams t ON c.team_id = t.team_id
WHERE c.coach_name LIKE '%Silva%';



SELECT t.team_name, tr.tournament_name
FROM Teams t
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE t.city LIKE '%a%';



SELECT m.match_date, t1.team_name AS Team_A, t2.team_name AS Team_B, m.score_a, m.score_b
FROM Matches m
INNER JOIN Teams t1 ON m.team_a_id = t1.team_id
INNER JOIN Teams t2 ON m.team_b_id = t2.team_id
WHERE t1.team_name LIKE 'Sesc%';



SELECT p.player_name, t.team_name, tr.tournament_name
FROM Players p
INNER JOIN Teams t ON p.team_id = t.team_id
INNER JOIN TeamTournament tt ON t.team_id = tt.team_id
INNER JOIN Tournaments tr ON tt.tournament_id = tr.tournament_id
WHERE p.player_name LIKE '%a' AND tr.tournament_name LIKE '%Copa%';
