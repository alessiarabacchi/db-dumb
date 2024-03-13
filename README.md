## Selezionare tutti i libri in ordine alfabetico per titolo

SELECT titolo FROM libri ORDER BY titolo ASC;

## Selezionare tutti gli autori che hanno scritto almeno un libro

SELECT DISTINCT autori.nome FROM autori
JOIN libri ON autori.id = libri.autore_id;

## Selezionare tutti i libri scritti da un determinato autore

SELECT libri.titolo FROM libri
JOIN autori ON libri.autore_id = autori.id
WHERE autori.nome = 'Nome Autore';

## Selezionare tutti i prestiti effettuati da un determinato utente

SELECT \* FROM prestiti
WHERE utente_id = 'ID Utente';

## contare le copie disponibili per ogni genere

SELECT generi.nome, SUM(libri.numero_copie) AS copie_disponibili FROM libri
JOIN generi ON libri.genere_id = generi.id
GROUP BY generi.nome;

## Selezionare tutti i prestiti restituiti entro una data specifica

SELECT \* FROM prestiti
WHERE data_restituzione <= '2023-01-01';

## Selezionare tutti i libri di un determinato genere

SELECT titolo FROM libri
JOIN generi ON libri.genere_id = generi.id
WHERE generi.nome = 'Nome Genere';

## Selezionare i libri più popolari (quelli prestati più volte)

SELECT libri.titolo, COUNT(prestiti.id) AS volte_prestato FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
GROUP BY libri.titolo
ORDER BY volte_prestato DESC;

## Seleziona il titolo, il nome, il genere e autore di un libro specifico "The Great Gatsby"

SELECT libri.titolo, autori.nome AS autore_nome, generi.nome AS genere FROM libri
JOIN autori ON libri.autore_id = autori.id
JOIN generi ON libri.genere_id = generi.id
WHERE libri.titolo = 'The Great Gatsby';

## seleziona i titoli dei libri, il genere dell'autore di nome "Stephen King"

SELECT libri.titolo, generi.nome AS genere FROM libri
JOIN autori ON libri.autore_id = autori.id
JOIN generi ON libri.genere_id = generi.id
WHERE autori.nome = 'Stephen King';

## Selezionare tutti gli utenti che hanno preso in prestito almeno un libro di un determinato genere

SELECT DISTINCT utenti.nome
FROM utenti
JOIN prestiti ON utenti.id = prestiti.utente_id
JOIN libri ON prestiti.libro_id = libri.id
JOIN generi ON libri.genere_id = generi.id
WHERE generi.nome = 'Nome Genere';

## Seleziona il titolo, il nome, il genere, autore e gli utenti che lo hanno preso in prestito un libro nel 2022

SELECT libri.titolo, autori.nome AS autore_nome, generi.nome AS genere, utenti.nome AS utente_nome FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
JOIN autori ON libri.autore_id = autori.id
JOIN generi ON libri.genere_id = generi.id
JOIN utenti ON prestiti.utente_id = utenti.id
WHERE YEAR(data_prestito) = 2022;

## Contare quanti libri gli utenti hanno preso in prestito un libro nel 2021 raggruppati per genere

SELECT generi.nome AS genere, COUNT(prestiti.id) AS numero_prestiti FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
JOIN generi ON libri.genere_id = generi.id
WHERE YEAR(data_prestito) = 2021
GROUP BY generi.nome;

## Seleziona il titolo, gli utenti e la durata del prestito

SELECT libri.titolo, utenti.nome, DATEDIFF(data_restituzione, data_prestito) AS durata_prestito FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
JOIN utenti ON prestiti.utente_id = utenti.id;

## Seleziona il titolo, gli utenti con durata del prestito maggiore di 20 giorni

SELECT libri.titolo, utenti.nome FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
JOIN utenti ON prestiti.utente_id = utenti.id
WHERE DATEDIFF(data_restituzione, data_prestito) > 20;

## Valutare la durata media del prestito di ogni libro, con il genere ordinati per durata decrescente

SELECT libri.titolo, generi.nome AS genere, AVG(DATEDIFF(data_restituzione, data_prestito)) AS durata_media FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
JOIN generi ON libri.genere_id = generi.id
GROUP BY libri.titolo, generi.nome
ORDER BY durata_media DESC;
