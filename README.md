# DietiDeals24 — Frontend (iOS)
 
Frontend iOS del progetto **DietiDeals24**, realizzato in **Swift** nell'ambito
del corso di **Ingegneria del Software** (a.a. 2023/2024) presso il
Dipartimento DIETI, Università degli Studi di Napoli Federico II.
 
L'app comunica con un backend dedicato ([backend-DietiDeals24](https://github.com/terrecruis/backend-DietiDeals24),
Java) e sfrutta **servizi AWS** per la virtualizzazione/hosting
dell'infrastruttura lato server.
 
---
 
## 📱 Descrizione del progetto
 
DietiDeals24 è una piattaforma per la gestione di offerte e aste online.
Questo repository contiene esclusivamente il **client iOS**, sviluppato in
Swift, che si interfaccia con le API esposte dal backend per:
 
- consultare e pubblicare annunci/offerte;
- gestire il profilo utente;
- interagire con il flusso di aste/trattative (dettagli specifici nella
  documentazione di analisi, vedi sotto).
> Nota: questa sezione va integrata con il set esatto di funzionalità
> implementate — se vuoi, posso espanderla a partire dallo screenshot
> dell'app o dall'elenco delle schermate/ViewController presenti nel
> progetto Xcode.
 
---
 
## 🛠 Stack tecnologico
 
| Ambito | Tecnologia |
|---|---|
| Linguaggio | Swift |
| Piattaforma | iOS |
| Backend (repo separato) | Java |
| Infrastruttura | AWS (virtualizzazione/hosting) |
| Versionamento | Git / GitHub |
 
---
 
## 📂 Struttura del repository
 
```
frontend-DietiDeals24/
├── DietiDeals24/                          # Progetto Xcode (sorgenti Swift)
├── analisi dei requisiti/                 # Documenti di analisi (fase iniziale INGSW)
├── specifica dei requisiti/
│   └── UML-PROGETTO/                      # Diagrammi UML (casi d'uso, classi, sequenza...)
├── burocrazia/                            # Documentazione amministrativa del corso
├── .gitignore
└── README.md
```
 
---
 
## 🚀 Getting Started
 
### Prerequisiti
 
- macOS con **Xcode** installato (versione compatibile con il target iOS del progetto)
- Un simulatore iOS o un dispositivo fisico per il testing
- Accesso al backend (locale o istanza AWS) configurato con l'endpoint corretto
### Installazione
 
1. Clona la repository:
```bash
   git clone https://github.com/terrecruis/frontend-DietiDeals24.git
```
2. Apri il progetto in Xcode:
```bash
   cd frontend-DietiDeals24/DietiDeals24
   open DietiDeals24.xcodeproj
```
   *(o `.xcworkspace` se il progetto usa CocoaPods/SPM con dipendenze esterne)*
3. Configura l'URL/endpoint del backend (se previsto in un file di configurazione o costante dedicata).
4. Seleziona un simulatore o dispositivo e avvia il build (`Cmd + R`).
 
---
 
## 🔗 Repository collegate
 
- **Backend**: [backend-DietiDeals24](https://github.com/terrecruis/backend-DietiDeals24) — API Java del progetto
---
 
## 📄 Documentazione
 
La cartella `analisi dei requisiti/` e `specifica dei requisiti/UML-PROGETTO/`
contengono la documentazione prodotta durante le fasi iniziali del progetto
(analisi dei requisiti, diagrammi UML), come richiesto dal corso di
Ingegneria del Software.
 
---
 
## ℹ️ Nota sulla repository
 
Questa è la **seconda repository** del progetto: la prima, contenente oltre
90 commit, è andata persa a causa di un problema sul server Git e non è
stato possibile recuperarla nonostante il contatto con l'assistenza. Lo
storico dei commit precedente a questa repo non è quindi disponibile.
 
---
 
## 👥 Autori
 
Progetto realizzato per il corso di Ingegneria del Software (DIETI, Federico II) da:
 
- **Francesco Terrecuso** ([@terrecruis](https://github.com/terrecruis))
- Socio di progetto (vedi commit history / repository originale per attribuzione completa)
---
 
## 📌 Licenza
 
Progetto accademico — nessuna licenza open source specificata. Per usi al
di fuori dell'ambito didattico, contattare gli autori.
