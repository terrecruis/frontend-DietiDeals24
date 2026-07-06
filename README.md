# 📱 DietiDeals24 — Frontend (iOS Client)

<div align="center">

![Swift](https://img.shields.io/badge/Swift%205.8+-FA7343?style=for-the-badge&logo=swift&logoColor=white)
![SwiftUI](https://img.shields.io/badge/SwiftUI-0070C9?style=for-the-badge&logo=swift&logoColor=white)
![MVVM](https://img.shields.io/badge/Architecture-MVVM-4B0082?style=for-the-badge)
![AWS Amplify](https://img.shields.io/badge/AWS%20Amplify-FF9900?style=for-the-badge&logo=awsamplify&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-iOS%2016%2B-000000?style=for-the-badge&logo=apple&logoColor=white)

**Client iOS nativo ad alte prestazioni, basato su architettura reattiva MVVM e servizi AWS.**<br>
*Progetto per il corso di Ingegneria del Software (INGSW) — Università degli Studi di Napoli Federico II*

</div>

---

## 📑 Indice

- [🎯 Focus Architetturale e Design del Codice](#-focus-architetturale-e-design-del-codice)
- [🧩 Analisi dei Componenti e Layer del Codice](#-analisi-dei-componenti-e-layer-del-codice)
- [🔄 Flusso dei Dati (Data Flow & State Management)](#-flusso-dei-dati-data-flow--state-management)
- [🌐 Networking e Integrazione Backend](#-networking-e-integrazione-backend)
- [☁️ Servizi AWS e Virtualizzazione](#-servizi-aws-e-virtualizzazione)
- [📂 Albero del Codice Sorgente (`Source/`)](#-albero-del-codice-sorgente-source)
- [🛠 Build e Configurazione del Progetto Xcode](#-build-e-configurazione-del-progetto-xcode)

---

## 🎯 Focus Architetturale e Design del Codice

Il codice del client iOS è strutturato applicando rigorosamente il design pattern **MVVM (Model-View-ViewModel)** accoppiato con il paradigma dichiarativo e reattivo di **SwiftUI** e **Combine**. 

Questo approccio garantisce una netta separazione tra l'interfaccia grafica e la business logic, facilitando la testabilità e la manutenibilità delle singole componenti:

```
┌────────────────────────────────────────────────────────┐
│                   UI LAYER (SwiftUI)                   │
│   [ Views ] ◄── Data Binding (Reactive) ──► [ Views ]  │
└───────────────────────────▲────────────────────────────┘
                            │ (@StateObject / @ObservedObject)
┌───────────────────────────▼────────────────────────────┐
│                  LOGIC LAYER (Combine)                 │
│                      [ ViewModels ]                    │
└───────▲────────────────────────────────────────▲───────┘
        │ (Call API / Fetch)                     │ (Auth & Media)
┌───────▼──────────────────────┐         ┌───────▼──────────────────────┐
│       NETWORKING LAYER       │         │      MANAGERS & SERVICES     │
│   [ Network / REST Clients ] │         │ SessionManager / PhotoManager│
└───────▲──────────────────────┘         └───────▲──────────────────────┘
        │ (HTTP / JSON / REST)                   │ (AWS Cognito / S3)
┌───────▼────────────────────────────────────────▼───────┐
│              BACKEND & CLOUD INFRASTRUCTURE            │
│       [ Java JAX-RS Backend ]  &  [ AWS Services ]     │
└────────────────────────────────────────────────────────┘
```

---

## 🧩 Analisi dei Componenti e Layer del Codice

L'intero codice sorgente applicativo risiede all'interno del modulo principale `DietiDeals24/DietiDeals24/Source/` ed è suddiviso in responsabilità ben definite:

### 1. Layer di Visualizzazione (`Views/`)
- Sviluppato al 100% in **SwiftUI**, sfruttando interfacce dichiarative e reattive.
- Le viste sono prive di logica di business: si limitano a inviare eventi utente (es. tap, form submit) al proprio ViewModel e ad ascoltarne i cambiamenti di stato per il rendering dinamico dell'interfaccia.

### 2. Logica di Presentazione (`ViewModels/`)
- I ViewModel espongono proprietà pubblicate (`@Published`) che rappresentano lo stato attuale della schermata (es. lista di aste attive, dettagli utente, stati di caricamento o errore).
- Orchestrano le chiamate ai servizi di rete e gestiscono la trasformazione dei dati grezzi ricevuti dai model per renderli pronti all'uso nella UI.

### 3. Layer dei Modelli (`Models/`)
- Contiene le strutture dati conformi ai protocolli `Codable` e `Identifiable`.
- Definisce i contratti dati per il parsing automatico dei payload JSON scambiati con il backend RESTful (es. `Auction`, `Bid`, `User`, `Notification`).

### 4. Core Managers (`SessionManager.swift` & `PhotoManager.swift`)
- **`SessionManager.swift`**: È il Singleton / ObservableObject responsabile del ciclo di vita della sessione utente. Gestisce lo stato di autenticazione, il salvataggio sicuro dei token, il log-in/log-out e la persistenza dei dati utente correnti.
- **`PhotoManager.swift`**: Servizio dedicato alla gestione, compressione e virtualizzazione delle risorse multimediali (immagini del profilo, foto delle aste). Si occupa del caricamento e scaricamento ottimizzato verso l'infrastruttura di storage cloud (AWS S3).

---

## 🔄 Flusso dei Dati (Data Flow & State Management)

Il flusso di aggiornamento dei dati all'interno dell'app segue un ciclo unidirezionale e reattivo:

1. **Azione Utente**: L'utente interagisce con una vista (es. piazza un'offerta in un'asta al ribasso).
2. **Intent verso il ViewModel**: La vista chiama un metodo del proprio `ViewModel` (es. `viewModel.placeBid(amount: 50.0)`).
3. **Chiamata Network / AWS**: Il `ViewModel` si interfaccia con il layer di `Networking` o con il `SessionManager`.
4. **Aggiornamento di Stato**: Alla ricezione della risposta HTTP dal server Java, il `ViewModel` aggiorna le sue variabili di stato marcate con `@Published`.
5. **Re-rendering UI**: Grazie al data-binding nativo di SwiftUI (`@ObservedObject` / `@StateObject`), la UI si ridisegna automaticamente riflettendo i nuovi dati, senza alcun bisogno di manipolazione manuale del DOM/gerarchia delle view.

---

## 🌐 Networking e Integrazione Backend

Il layer di rete (`Networking/`) costituisce il ponte di comunicazione verso il server di backend ([backend-DietiDeals24](https://github.com/terrecruis/backend-DietiDeals24)):

- **Protocollo**: HTTP/HTTPS RESTful basato sullo scambio di payload in formato **JSON**.
- **Gestione Asincrona**: Implementata utilizzando le moderne API della concorrenza di Swift (**`async / await`** e `Task`), eliminando i callback hell e garantendo performance fluide sul thread principale (MainActor).
- **Gestione Errori**: Mapping strutturato degli HTTP status code (es. 401 Unauthorized, 404 Not Found, 400 Bad Request) in enum di errori Swift personalizzati per mostrare feedback chiari all'utente.

---

## ☁️ Servizi AWS e Virtualizzazione

Il codice client si avvale di **AWS Amplify** per delegare compiti infrastrutturali complessi e alleggerire il carico sul backend centrale:

- **Configurazione**: I file `amplifyconfiguration.example.json` e `awsconfiguration.example.json` definiscono i puntatori ai servizi cloud.
- **AWS Cognito / Auth**: Gestione della virtualizzazione delle identità, autenticazione sicura e gestione dei token di sessione.
- **AWS S3 / Storage**: Hosting e virtualizzazione scalabile dei file multimediali (immagini degli annunci e avatar utenti), gestiti via codice tramite `PhotoManager.swift`.

---

## 📂 Albero del Codice Sorgente (`Source/`)

Focus esclusivo sulla struttura ingegneristica dell'applicazione Swift:

```text
DietiDeals24/DietiDeals24/
├── Source/
│   ├── DietiDeals24App.swift       # Entry point del ciclo di vita dell'app SwiftUI (@main)
│   ├── SessionManager.swift        # Gestore di sessione, autenticazione e stato globale utente
│   ├── PhotoManager.swift          # Servizio di gestione e upload immagini verso AWS S3
│   ├── Models/                     # Data entities e strutture di mapping JSON (Codable)
│   ├── Views/                      # Componenti grafici dichiarativi SwiftUI suddivisi per feature
│   ├── ViewModels/                 # Controller logici e reattivi legati alle singole Views
│   └── Networking/                 # Client REST, endpoint routing e gestione richieste async/await
├── amplify/                        # Configurazione del cloud e integrazioni AWS Amplify
├── DietiDeals24Tests/              # Suite di Unit Test per la verifica di Models e ViewModels
├── DietiDeals24UITests/            # Test automatizzati di interfaccia utente
├── GoogleService-Info.plist        # Configurazione servizi aggiuntivi di notifica / analytics
└── Info.plist                      # Metadati applicativi, permessi di sistema e capabilities
```

---

## 🛠 Build e Configurazione del Progetto Xcode

### Prerequisiti di Sistema
- **macOS** 13+ (Ventura o superiore raccomandato).
- **Xcode 14+** con supporto al SDK iOS 16+.
- **Swift 5.8+**.

### Configurazione per lo Sviluppo

1. **Clona la Repository**:
   ```bash
   git clone https://github.com/terrecruis/frontend-DietiDeals24.git
   cd frontend-DietiDeals24/DietiDeals24
   ```

2. **Setup File AWS / Configurazione**:
   - Rinomina i file di esempio aggiungendo le credenziali reali del tuo ambiente AWS/Amplify:
   ```bash
   cp amplifyconfiguration.example.json amplifyconfiguration.json
   cp awsconfiguration.example.json awsconfiguration.json
   ```

3. **Apertura e Build in Xcode**:
   - Apri il file di progetto `client.xcodeproj` facendo doppio clic su di esso o tramite terminale:
   ```bash
   open client.xcodeproj
   ```
   - Assicurati di selezionare un simulatore iOS (es. *iPhone 14 Pro*) o un dispositivo fisico connesso.
   - Premi **`Cmd + B`** per compilare il codice o **`Cmd + R`** per avviare l'applicazione.

4. **Esecuzione Test Unitari**:
   - Per eseguire la suite di test sulla business logic dei ViewModel e dei Model, premi **`Cmd + U`** direttamente da Xcode.

---
<div align="center">
  <small>Documentazione tecnica focalizzata sul codice e sull'architettura software — DietiDeals24 Client iOS</small>
</div>
