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

## Selezionare i libri più popolari (quelli prestati più volte)

SELECT libri.titolo, COUNT(prestiti.id) AS volte_prestato FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
GROUP BY libri.titolo
ORDER BY volte_prestato DESC;

## seleziona il titolo, il nome, il genere, autore e gli utenti che lo hanno preso in prestito un libro nel 2022

SELECT libri.titolo, autori.nome AS autore, generi.nome AS genere, utenti.nome AS utente
FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
JOIN autori ON libri.autore_id = autori.id
JOIN generi ON libri.genere_id = generi.id
JOIN utenti ON prestiti.utente_id = utenti.id
WHERE YEAR(data_prestito) = 2022;

## seleziona il titolo del libro e nome dell utente con durata del prestito maggiore di 20 giorni

SELECT libri.titolo, utenti.nome
FROM prestiti
JOIN libri ON prestiti.libro_id = libri.id
JOIN utenti ON prestiti.utente_id = utenti.id
WHERE DATEDIFF(data_restituzione, data_prestito) > 20;

## Selezionare tutti gli utenti che hanno preso in prestito almeno un libro di un determinato genere

SELECT DISTINCT utenti.nome
FROM utenti
JOIN prestiti ON utenti.id = prestiti.utente_id
JOIN libri ON prestiti.libro_id = libri.id
JOIN generi ON libri.genere_id = generi.id
WHERE generi.nome = 'Nome Genere';
