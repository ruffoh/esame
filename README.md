# Prima Parte

## 1 analisi e E/R
 Ogni docente si registra e accede nella piattaforma,stessa cosa vale per gli studenti.
 Una volta registrati i docenti posso creare delle classi virtuali con il proprio collegamento condivisibile con gli studenti.
 Gli studenti al interno della piattaforma visualizzeranno i link ai videogiochi proposti dal docente per quella classe.
 Nel videogioco sono presenti quiz ecc che portano al guadagno di monete 
 Grazie alle monete si può stabile una classifica per videogioco e per classe.
 
 (Foto diagramma E/R)[/img/ER]
 
## 2 Schema Logico 
**Entità** 
```sql
    Docenti(ID,nome,password)
    Classi(ID,nome,materia,IDlink,codiceIscrizione,IDDocente)
    Studenti(ID,nome,password) 
    Immagini(ID,link,IDVideogioco)
    Videogiochi(ID,titolo,descrizioneCorta,descrizioneLunga,moneteMax,IDArgomento)
    Argomenti(ID,tipo)
```
**Associazioni** 
```sql
    Classi_Videogiochi(ID,IDClasse,IDVideogioco,link)
    Studenti_Videogiochi(ID,IDVideogioco,IDClasse,IDStudente,monete)
```
## 3 Linguaggio Sql di un sottoinsieme
ho scelto di rappresentare questa parte di codice SQL perché come richiesto contiene vincoli di integrità referenziale ,Argomento.

```sql 
CREATE TABLE Videogiochi(
INT IdVideogioco PRIMARY KEY NOT NULL,
VARCHAR(30) Titolo NOT NULL,
VARCHAR(200) DescrizioneCorta NOT NULL,
INT Monete NOT NULL,
INT Argomento NOT NULL,
COSTRAIN fkArgomentoVideogioco FOREGEIN KEY (Argomeno) REFERENCE Argomenti(ID),
)
```
Ho scelto di rappresentare questa parte di codice SQL perché, come richiesto, contiene vincoli di integrità referenziale (in questo caso sull’argomento).

Ho voluto includere il vincolo di integrità referenziale sull’argomento per rispettare le regole di normalizzazione e garantire che nella tabella Videogiochi non vengano inseriti valori ridondanti o non validi. Questo permette di collegare ogni videogioco a un argomento esistente nella tabella Argomenti, evitando ripetizioni inutili e migliorando la coerenza dei dati.
## 4 Interrogazioni SQL
### Elenco in ordine alfabetico dei giochi classificati per uno specifico argomento
``` sql
SELECT V.Titolo 
FROM Videogiochi AS V
JOIN Argomenti AS A ON 
V.IDArgomento = A.ID
WHETE A.Tipo = 'il nostro argomento da filtrare' 
ORDER BY V.Titolo ASC;
```
Con questa query noi andiamo a collegare la tabella Videogiochi alla tabella Argomenti in base al argomento del videogioco e successivamente restituiamo i Titoli dei Videogiochi in Ordine Alfabetico di quel argomento , basta sostituire sul nostro argomento da filtrare"
### Classifica degli studenti di una certa classe virtuale, in base alle monete raccolte per un certo gioco
``` sql
SELECT S.nome, SV.monete
FROM Studenti AS S
JOIN Studenti_Videogiochi AS SV ON S.ID = SV.IDStudente
JOIN Classi AS C ON SV.IDClasse = C.ID
JOIN Videogiochi AS V ON SV.IDVideogioco = V.ID
WHERE C.ID = 'id_classe' AND V.ID = 'id_videogioco'
ORDER BY SV.monete DESC;
```
### Numero di classi in cui è utilizzato ciascun videogioco del catalogo
``` sql
SELECT V.Titolo, COUNT(CV.IDClasse) AS NumeroClassi
FROM Videogiochi AS V
JOIN Classi_Videogiochi AS CV ON V.ID = CV.IDVideogioco
GROUP BY V.Titolo
ORDER BY NumeroClassi DESC;
```
