CREATE TABLE pessoas(
	id SERIAL PRIMARY KEY,
	nome varchar(120) NOT NULL,
	cpf varchar(11) NOT NULL UNIQUE,
	conjuge_id INTEGER,
	
	CONSTRAINT fk_pessoas_conjuge FOREIGN KEY (conjuge_id)
	REFERENCES pessoas(id) ON DELETE SET NULL
)
SELECT * FROM pessoas
INSERT INTO pessoas(nome, cpf) VALUES('Isaac', '12930293829');
INSERT INTO pessoas(nome, cpf) VALUES('Jaque', '34534545355');

INSERT INTO pessoas(nome, cpf, conjuge_id) 
VALUES('Joao', '32930293829', 4);

UPDATE pessoas SET conjuge_id = 2 WHERE id = 1
UPDATE pessoas SET conjuge_id = 1 WHERE id = 2
