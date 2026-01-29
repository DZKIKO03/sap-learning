RAP Programming Model
Managed vs Unmanaged (Exam + Real Understanding)
RAP Managed Scenario
Nel Managed RAP, il framework RAP gestisce automaticamente gran parte della logica standard del business object.
Il developer:
    • definisce il data model (CDS entities)
    • definisce il behavior (what is allowed)
    • implementa solo la business logic specifica
Il framework gestisce automaticamente:
    • create / update / delete
    • transaction handling
    • locking
    • draft handling (se attivato)
    • ETag
    • save sequence
Caratteristiche chiave:
    • rapido da sviluppare
    • meno codice custom
    • ideale per nuovi business object
    • perfettamente allineato al paradigma cloud
Quando usare Managed:
    • nuovi oggetti custom
    • applicazioni Fiori cloud-native
    • quando non esiste una logica legacy da riutilizzare
    • quando si vuole massima integrazione con framework RAP

RAP Unmanaged Scenario
Nel Unmanaged RAP, il developer mantiene il pieno controllo della logica di business.
Il developer:
    • implementa manualmente
        ◦ create
        ◦ update
        ◦ delete
        ◦ save
    • gestisce esplicitamente la persistenza
    • controlla il flusso transazionale
Il framework:
    • espone il BO come servizio RAP
    • fornisce struttura, non logica
Caratteristiche chiave:
    • più flessibile
    • più codice da scrivere
    • maggiore responsabilità sul developer
Quando usare Unmanaged:
    • wrapping di logica legacy
    • integrazione con codice esistente
    • quando non è possibile delegare la persistenza al framework
    • migrazioni progressive verso RAP

Differenze chiave (da esame)
Aspetto	Managed	Unmanaged
CRUD	Framework	Developer
Persistenza	Automatica	Manuale
Codice	Ridotto	Maggiore
Cloud-ready	Sì	Sì, ma più complesso
Uso tipico	Nuovi BO	Legacy / wrapping
Exam trap tipica:
Managed ≠ “meno potente”
Unmanaged ≠ “non cloud”
Entrambi sono cloud-compliant, cambia chi fa cosa.

CDS & OData
Annotazioni essenziali per servizi OData
Le CDS annotations arricchiscono il modello dati con semantica, UI hints e metadati necessari all’esposizione OData.
SAP all’esame NON chiede liste infinite, ma:
    • quali sono necessarie
    • quali sono comuni
    • quali influenzano UI / servizio

Annotazioni CDS fondamentali
@EndUserText.label
Definisce una descrizione leggibile per l’utente finale.
    • Usata per:
        ◦ entità
        ◦ elementi
    • Influenza:
        ◦ UI
        ◦ metadati OData
È fortemente consigliata, spesso considerata “mandatory” a livello di best practice.

@AccessControl.authorizationCheck
Definisce se e come viene applicato il controllo autorizzativo.
Valori tipici:
    • #CHECK
    • #NOT_REQUIRED
Importante per:
    • sicurezza
    • comportamento runtime del servizio

@ObjectModel
Famiglia di annotazioni usata per:
    • identificare chiavi semantiche
    • definire text associations
    • gestire lifecycle del BO
Esempi comuni:
    • @ObjectModel.text.element
    • @ObjectModel.semanticKey

Annotazioni UI (exam-oriented)
Queste NON sono sempre obbligatorie, ma SAP le cita spesso.
@UI.lineItem
Definisce i campi mostrati in una lista (table).
@UI.identification
Definisce i campi mostrati nella pagina oggetto.
@UI.selectionField
Definisce i campi usati per il filtro.
Nota importante:
Le annotazioni UI non sono obbligatorie per OData,
ma sono fondamentali per Fiori Elements.

Annotazioni di esposizione servizio
In contesto RAP / OData moderno:
    • l’esposizione avviene tramite service definition
    • le CDS annotations arricchiscono, non sostituiscono il servizio
Exam trap tipica:
Le CDS annotations da sole NON creano un servizio OData.
Serve sempre:
    • service definition
    • service binding

Riassunto secco (da memorizzare)
    • RAP Managed → framework gestisce la logica standard
    • RAP Unmanaged → developer gestisce la logica
    • @EndUserText.label → descrizione leggibile
    • UI annotations → migliorano UI, non creano servizi
    • OData → CDS + service definition + binding


Entity Manipulation Language (EML) – Uso ed errori da esame

Cos’è la Entity Manipulation Language (EML)
La Entity Manipulation Language (EML) è il linguaggio ABAP utilizzato nel modello RAP per leggere e modificare entità di business definite tramite CDS.
EML:
    • opera su entità, non su tabelle
    • rispetta automaticamente:
        ◦ behavior definition
        ◦ controlli di autorizzazione
        ◦ validazioni e determinations
    • è il modo standard e raccomandato per accedere ai dati nel RAP
Nota da esame:
In RAP non si usa SELECT diretto sulle tabelle, ma EML sulle entità.

READ ENTITIES
READ ENTITIES viene usato per leggere dati da una o più entità RAP.
Caratteristiche chiave:
    • legge dati tramite il modello RAP
    • applica automaticamente:
        ◦ authorization
        ◦ feature control
    • restituisce:
        ◦ dati letti
        ◦ informazioni di errore
Esempio concettuale:
READ ENTITIES OF zi_order
  ENTITY order
  ALL FIELDS
  WITH VALUE #( ( order_id = lv_id ) )
  RESULT DATA(lt_order).

MODIFY ENTITIES
MODIFY ENTITIES viene usato per:
    • CREATE
    • UPDATE
    • DELETE
sempre tramite il behavior RAP.
Caratteristiche:
    • esegue la logica definita nel behavior
    • attiva validazioni e determinations
    • non scrive direttamente sul database
Esempio concettuale:
MODIFY ENTITIES OF zi_order
  ENTITY order
  UPDATE
  FIELDS ( status )
  WITH VALUE #( ( order_id = lv_id status = 'A' ) ).

FAILED e REPORTED (fondamentale da esame)
Le istruzioni EML possono restituire due strutture fondamentali:
    • FAILED
        ◦ contiene gli oggetti per cui l’operazione è fallita
        ◦ indica errori bloccanti
    • REPORTED
        ◦ contiene messaggi (warning / info)
        ◦ non blocca necessariamente l’operazione
Exam trap tipica:
FAILED ≠ REPORTED
FAILED blocca, REPORTED informa.

EML nel contesto RAP
Nel modello RAP:
    • EML è usata:
        ◦ nelle actions
        ◦ nelle validations
        ◦ nelle determinations
    • Managed e Unmanaged usano entrambi EML
    • cambia chi gestisce la persistenza, non il linguaggio
Nota importante:
Managed ≠ “senza EML”
Unmanaged ≠ “non cloud”

Esercizi (mirati all’esame)
Esercizio 1 – Concettuale
Domanda:
In una action RAP, è corretto usare SELECT sulla tabella?
Risposta:
No. In RAP si usa EML sulle entità per rispettare behavior, authorization e framework.

Esercizio 2 – READ vs MODIFY
Abbina correttamente:
    • leggere dati → READ ENTITIES
    • cambiare stato → MODIFY ENTITIES
    • eseguire validazioni → MODIFY ENTITIES

Esercizio 3 – Vero / Falso (stile esame)
    1. EML accede direttamente alle tabelle del database.
→ FALSO
    2. FAILED indica un errore bloccante.
→ VERO
    3. In RAP Managed non si usa EML.
→ FALSO

Appendice B – Determinations e Validations nel RAP
Determinations nel RAP
Una determination è una logica di business che viene eseguita automaticamente dal framework RAP in risposta a determinati eventi sul business object.
Caratteristiche principali:
    • viene eseguita implicitamente
    • non è invocata dal consumer
    • aggiorna campi derivati o calcolati
    • fa parte del behavior definition
Esempi di utilizzo tipico:
    • calcolo automatico di uno stato
    • valorizzazione di campi derivati
    • inizializzazione di valori default
Nota da esame:
Una determination non valida i dati, ma li calcola o li completa.

Quando viene eseguita una determination
Una determination può essere collegata a eventi specifici, ad esempio:
    • on save
    • on modify
    • on create
Il framework RAP decide quando eseguirla, in base alla definizione del behavior.
Nota da esame:
Il developer non chiama mai direttamente una determination.

Validations nel RAP
Una validation è una logica di controllo che verifica se i dati rispettano determinate regole di business.
Caratteristiche principali:
    • viene eseguita automaticamente
    • può bloccare l’operazione
    • restituisce messaggi di errore
    • fa parte del behavior definition
Esempi di utilizzo tipico:
    • controllare che una quantità sia maggiore di zero
    • verificare la coerenza di date
    • validare stati consentiti
Nota da esame:
Se una validation fallisce, l’operazione non viene salvata.

Differenza chiave tra Determinations e Validations (DOMANDA CLASSICA)
Aspetto	Determination	Validation
Scopo	Calcolare / valorizzare	Controllare / bloccare
Modifica dati	Sì	No
Può bloccare il save	No	Sì
Esegue logica di business	Sì	Sì
Exam trap tipica:
Validation ≠ AUTHORITY-CHECK
La validation controlla i dati, non le autorizzazioni.

Determinations e Validations vs AUTHORITY-CHECK
Confronto concettuale:
    • AUTHORITY-CHECK
        ◦ verifica se l’utente è autorizzato
        ◦ riguarda la sicurezza
    • Validation
        ◦ verifica la correttezza dei dati
        ◦ riguarda la logica di business
Nota da esame:
Non usare validation per controlli di autorizzazione.

Determinations e Validations nel flusso RAP
Nel modello RAP:
    • entrambe sono definite nel behavior
    • vengono eseguite automaticamente dal framework
    • sono valide sia per:
        ◦ RAP Managed
        ◦ RAP Unmanaged
Nota importante:
Managed ≠ assenza di logica custom
Unmanaged ≠ assenza di framework

Esercizi (stile esame)
Esercizio 1 – Scelta corretta
Devi:
    • calcolare automaticamente lo stato di un ordine
    • impedire il salvataggio se la data di fine è precedente a quella di inizio
Soluzione:
    • stato → determination
    • controllo date → validation

Esercizio 2 – Vero / Falso
    1. Una determination può bloccare il salvataggio.
→ FALSO
    2. Una validation può modificare i dati.
→ FALSO
    3. AUTHORITY-CHECK e validation hanno lo stesso scopo.
→ FALSO

Esercizio 3 – Scenario da esame
Scenario:
Un campo “total_amount” deve essere sempre ricalcolato quando cambia la quantità.
Soluzione corretta:
Implementare una determination, non una validation.

Service Definition e Service Binding (OData) – Concetti da esame
Service Definition e Service Binding (OData) – Concetti da esame
Perché servono Service Definition e Service Binding
Nel modello RAP, le CDS entities non sono automaticamente esposte come servizi OData.
Per rendere un business object consumabile (ad esempio da Fiori o via API), sono necessari due passaggi distinti:
    1. Service Definition
    2. Service Binding
Questo approccio separa:
    • cosa viene esposto
    • come viene esposto

Service Definition
La Service Definition definisce quali entità vengono esposte come parte di un servizio.
Caratteristiche principali:
    • seleziona le CDS entities da esporre
    • non definisce il protocollo
    • non rende il servizio immediatamente consumabile
Esempio concettuale:
define service ZUI_ORDER_SRV {
  expose ZI_Order;
}
Nota da esame:
La Service Definition non crea un servizio OData attivo.

Service Binding
La Service Binding collega una Service Definition a un protocollo specifico, tipicamente OData.
Caratteristiche:
    • rende il servizio attivo e consumabile
    • definisce:
        ◦ tipo di protocollo (OData)
        ◦ versione (V2 o V4)
    • è l’oggetto che viene pubblicato
Nota fondamentale da esame:
Senza Service Binding, nessun servizio è accessibile, anche se la Service Definition esiste.

Relazione tra CDS, Service Definition e Service Binding
Flusso corretto nel RAP:
    1. CDS Entity
    2. Service Definition
    3. Service Binding
    4. Servizio OData consumabile
Exam trap tipica:
Le CDS annotations non sostituiscono Service Definition e Service Binding.

OData nel contesto RAP
Nel RAP:
    • l’esposizione avviene sempre tramite:
        ◦ Service Definition
        ◦ Service Binding
    • le CDS annotations:
        ◦ arricchiscono il modello (UI, semantica)
        ◦ non attivano il servizio
Concetto chiave SAP:
“Annotations enrich the service, they do not expose it.”

Esercizi (stile esame)
Esercizio 1 – Concettuale
Domanda:
Hai creato CDS, behavior e service definition. Il servizio non è accessibile. Perché?
Risposta:
Perché manca la Service Binding.

Esercizio 2 – Ordine corretto
Metti in ordine i passaggi:
    • Service Binding
    • CDS Entity
    • Service Definition
Ordine corretto:
    1. CDS Entity
    2. Service Definition
    3. Service Binding

Esercizio 3 – Vero / Falso
    1. Una Service Definition rende il servizio OData immediatamente disponibile.
→ FALSO
    2. La Service Binding definisce il protocollo OData.
→ VERO
    3. Le CDS annotations sono sufficienti per esporre un servizio.
→ FALSO


Service Definition e Service Binding (OData) – Concetti da esame
OData V2 e OData V4 nel contesto SAP
SAP supporta sia OData V2 sia OData V4.
Nel contesto ABAP Cloud e RAP, OData V4 è la versione raccomandata.

OData V2
Caratteristiche principali:
    • versione più vecchia
    • ampiamente utilizzata in applicazioni legacy
    • compatibile con molte app SAP GUI e Fiori più datate
Limitazioni:
    • meno orientata al cloud
    • minore supporto per funzionalità moderne

OData V4
Caratteristiche principali:
    • versione moderna dello standard OData
    • progettata per scenari cloud e REST
    • supporta:
        ◦ semantica più ricca
        ◦ maggiore flessibilità
        ◦ migliore integrazione con RAP
Nota da esame:
Nel RAP, OData V4 è la scelta standard e raccomandata.

Differenze chiave (DOMANDA TIPICA)
Aspetto	OData V2	OData V4
Anzianità	Legacy	Moderna
Cloud-ready	Limitato	Sì
Uso in RAP	Possibile	Raccomandato
Futuro SAP	In mantenimento	Strategico

OData V2 / V4 e Service Binding
La versione OData:
    • non viene decisa nella Service Definition
    • viene scelta nella Service Binding
Nota da esame molto frequente:
La Service Binding definisce protocollo e versione OData.

Mini-quiz (stile esame)
    1. OData V4 è la versione raccomandata per RAP.
→ VERO
    2. La versione OData si definisce nella CDS.
→ FALSO
    3. OData V2 non è supportata in SAP.
→ FALSO
