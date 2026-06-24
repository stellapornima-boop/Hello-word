![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)
![Java](https://img.shields.io/badge/Java-21-orange.svg)
![Build](https://img.shields.io/badge/build-Eclipse-2C2255.svg)
<br>
🎬 Cine-Max
Progetto universitario — Laboratorio Interdisciplinare A · Università degli Studi dell'Insubria · a.a. 2025/2026
Sistema di gestione per cinema monosala da 200 posti. Consente la gestione completa del palinsesto cinematografico e delle prenotazioni tramite interfaccia testuale (TUI), scritto interamente in Java 21 con persistenza su file CSV. Nessuna dipendenza esterna.
---
👥 Autori
Nome	Matricola	Sede	Ruolo nel progetto
Serrao Isabella	766930	VA	TUI, service layer, documentazione, GitHub
Wilson Bernal Gaia	766733	VA	Persistenza CSV (ProiezioneRepository, UtenteRepository, FileManager)
Rahmouni Malek	766890	VA	Persistenza CSV (PrenotazioneRepository, FileManager)
Macrì Pornima	765952	VA	Model layer (Film, Proiezione, Prenotazione, Utente e sottoclassi, Ruolo)
---
⚙️ Requisiti
Java JDK 21 o superiore — download Adoptium
Nessuna libreria esterna richiesta
Verifica la tua installazione:
```bash
java -version
javac -version
```
---
🔨 Compilazione
Apri un terminale nella cartella radice del progetto (dove si trova questo README).
Windows:
```bash
mkdir bin
javac -encoding UTF-8 -d bin src\cinemax\model\*.java src\cinemax\persistence\*.java src\cinemax\service\*.java src\cinemax\ui\*.java src\cinemax\CineMax.java
```
Linux / macOS:
```bash
mkdir -p bin
javac -encoding UTF-8 -d bin src/cinemax/model/*.java src/cinemax/persistence/*.java src/cinemax/service/*.java src/cinemax/ui/*.java src/cinemax/CineMax.java
```
---
▶️ Avvio
```bash
java -cp bin cinemax.CineMax
```
> **Importante:** esegui sempre dalla cartella radice del progetto, altrimenti i file CSV in `data/` non vengono trovati.
Creazione del JAR (opzionale)
```bash
jar --create --file bin/CineMax.jar --main-class cinemax.CineMax -C bin .
java -jar bin/CineMax.jar
```
---
📁 Struttura del Progetto
```
Cine-Max/
│
├── src/
│   └── cinemax/
│       ├── CineMax.java                  ← punto di ingresso (main)
│       │
│       ├── model/                        ← classi di dominio
│       │   ├── Utente.java               (astratta)
│       │   ├── Cliente.java
│       │   ├── Bigliettaio.java
│       │   ├── Proiezionista.java
│       │   ├── Ruolo.java
│       │   ├── Film.java
│       │   ├── Proiezione.java
│       │   └── Prenotazione.java
│       │
│       ├── persistence/                  ← lettura/scrittura CSV
│       │   ├── FileManager.java
│       │   ├── UtenteRepository.java
│       │   ├── ProiezioneRepository.java
│       │   └── PrenotazioneRepository.java
│       │
│       ├── service/                      ← logica applicativa
│       │   └── GestoreApp.java
│       │
│       └── ui/                           ← interfaccia terminale (TUI)
│           ├── MenuUI.java
│           ├── LoginUI.java
│           ├── GuestMenu.java
│           ├── ClienteMenu.java
│           ├── BigliettaioMenu.java
│           └── ProiezionistaMenu.java
│
├── data/
│   ├── utenti.csv
│   ├── proiezioni.csv
│   └── prenotazioni.csv
│
├── autori.txt
└── README.md
```
Il progetto segue un'architettura a tre livelli:
model — classi di dominio pure, senza dipendenze esterne
persistence — accesso ai file CSV tramite repository dedicati
service — logica applicativa che coordina model e persistence
ui — menu testuali che interagiscono con il service layer
---
🔐 Credenziali di test
Tutti gli utenti di esempio usano la password: `password123`
Ruolo	Username
Proiezionista	`mario.rossi`
Proiezionista	`anna.bianchi`
Bigliettaio	`luigi.verdi`
Bigliettaio	`giulia.ferrari`
Bigliettaio	`marco.russo`
Bigliettaio	`sofia.esposito`
Bigliettaio	`andrea.romano`
Cliente	`laura.conti`
Gli utenti ospiti accedono senza autenticazione tramite l'opzione "Accedi come ospite" nel menu principale.
---
📌 Note tecniche
Password — memorizzate come hash SHA-256 (UTF-8), mai in chiaro
Formato data/ora — `dd/MM/yyyy HH:mm` per proiezioni e prenotazioni; `yyyy-MM-dd` per le date di nascita
Separatore CSV — virgola (`,`); le righe che iniziano con `#` sono commenti ignorati
Capienza sala — 200 posti fissi (`Proiezione.CAPIENZA_SALA`)
Sovrapposizione proiezioni — il sistema impedisce proiezioni che si sovrappongono tenendo conto della durata del film
Cancellazione prenotazioni — consentita solo se la proiezione è ancora futura
Modifica prenotazioni — consentita solo se sia la vecchia che la nuova proiezione sono future
Modifica/eliminazione proiezioni — consentita solo se la proiezione non ha prenotazioni attive
---
🖥️ Esecuzione da Eclipse
File → Import → General → Existing Projects into Workspace → seleziona la cartella `Cine-Max`
Verifica che `src/` sia nel Build Path e che l'output sia `bin/`
Tasto destro su `CineMax.java` → Run As → Run Configurations
Nella tab Arguments, sezione "Working directory", seleziona Other e inserisci:
```
   ${workspace_loc:Cine-Max}
   ```
Clicca Run
> Questo passaggio è fondamentale: senza la working directory corretta i file CSV non vengono trovati e il login fallisce.
---
📄 Formato dei file CSV
utenti.csv
```
TIPO,nome,cognome,username,passwordHash,dataNascita,luogoDomicilio
```
proiezioni.csv
```
codice,titolo,genere,regista,anno,durataMinuti,etaMinima,dataOra,prezzoBiglietto
```
prenotazioni.csv
```
codice,usernameCliente,codiceProiezione,numeroPosti,dataPrenotazione
```
I codici di proiezione e prenotazione sono UUID generati automaticamente.
