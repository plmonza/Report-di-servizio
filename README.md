Copia il contenuto seguente in un nuovo file e salvalo con il nome **`README.md`**, usando la codifica **UTF-8**:

````markdown
# PL Monza Reports

Web app in formato single-page per la compilazione dei **report di servizio della Polizia Locale e Protezione Civile di Monza**.

L'applicazione è realizzata interamente lato client: non richiede un backend, un database o una procedura di build. Il modulo può essere compilato da desktop o da dispositivo mobile, firmato direttamente sullo schermo e convertito in PDF.

## Funzionalità

### Report di servizio

Il modulo è organizzato nelle seguenti sezioni:

1. **Dati generali del turno**
   - Data del servizio
   - Turno predefinito: mattino, pomeriggio, sera o notte
   - Turno personalizzato
2. **Operatori in servizio**
   - Cognome e nome
   - Matricola
   - Veicolo o targa
   - Chilometri di inizio e fine servizio
   - Aggiunta o rimozione dinamica degli operatori
3. **Interventi della centrale operativa**
   - Sinistri stradali, con indicazione della presenza di feriti
   - TSO/ASO
   - Posti di controllo
   - Viabilità
   - Servizi presso scuole
   - Altri interventi
4. **Atti redatti**
   - Relazioni di servizio
   - Annotazioni di servizio
   - Sequestri e fermi
   - Sequestri penali
   - Altre attività
5. **Protocolli**
   - Integra
   - Richiesta accertamenti
   - Gesting
   - Rapporti di servizio
6. **Interventi di iniziativa**
7. **Violazioni riscontrate**
   - Preavvisi e VdC CdS
   - Regolamento di Polizia
   - Regolamento edilizio
   - Regolamento benessere animali
   - Normativa annonaria
   - Altre norme
   - Calcolo automatico del totale
8. **Note per l'UDT**
9. **Firma degli operatori**
   - Firma con mouse su desktop
   - Firma con dito su smartphone o tablet
   - Inserimento della firma nel PDF generato

### Salvataggio e ripristino

- Il pulsante **Salva** memorizza la bozza nel browser.
- La compilazione viene salvata automaticamente durante l'inserimento dei dati.
- È previsto un ulteriore salvataggio automatico periodico.
- I dati vengono ripristinati dal browser alla riapertura dell'applicazione.
- Le firme vengono salvate come immagini PNG codificate in Base64.

La chiave utilizzata nel `localStorage` è:

```text
plmonza_report_draft
```

### Generazione PDF

Il pulsante **Genera PDF** crea e scarica un documento PDF A4 contenente:

- intestazione del Settore Polizia Locale e Protezione Civile;
- dati del turno;
- operatori, veicoli e chilometri;
- interventi della centrale operativa;
- atti e protocolli;
- interventi di iniziativa;
- violazioni con totale;
- note per l'UDT;
- firme degli operatori;
- dichiarazione finale e piè di pagina istituzionale.

Il file viene scaricato con un nome simile a:

```text
Report_Servizio_20260923.pdf
```

Se la data non è compilata, il nome contiene `nodata`.

### Portali web

La seconda sezione dell'app raccoglie collegamenti rapidi verso portali esterni utilizzati dagli operatori, tra cui:

- ingresso e uscita cittadini stranieri;
- REVE;
- Alphatango;
- identificazione veicoli;
- difensore d'ufficio;
- ANCITEL veicoli rubati;
- SOPROV;
- nazionalità della targa;
- visura imprese;
- albo avvocati CNF;
- calcolo del sovraccarico;
- verifica della classe ambientale del veicolo;
- VDS Monza.

I collegamenti vengono aperti in una nuova scheda.

## Avvio

Il file HTML principale è:

```text
attached_assets/index_1790150410135.html
```

### Apertura diretta

È possibile aprire il file direttamente nel browser:

```text
attached_assets/index_1790150410135.html
```

### Avvio tramite server locale

Per evitare eventuali limitazioni del browser legate all'apertura di file locali, avviare un semplice server statico dalla cartella del progetto:

```bash
python3 -m http.server 8000
```

Poi aprire:

```text
http://localhost:8000/attached_assets/index_1790150410135.html
```

In alternativa, se il file viene spostato nella directory principale e rinominato `index.html`, l'app sarà disponibile direttamente su:

```text
http://localhost:8000/
```

## Dipendenze

L'app non utilizza un sistema di pacchetti o un framework JavaScript. Carica da CDN:

- [jsPDF 2.5.1](https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js) per la generazione dei PDF;
- [Inter](https://fonts.google.com/specimen/Inter) e [Playfair Display](https://fonts.google.com/specimen/Playfair+Display) da Google Fonts.

Per la generazione del PDF e il caricamento dei font è quindi necessaria una connessione Internet, salvo futura inclusione locale delle dipendenze.

## Struttura tecnica

Il file HTML contiene:

- markup HTML dell'interfaccia;
- stile CSS inline;
- logica JavaScript inline;
- gestione dello stato in memoria;
- salvataggio e ripristino tramite `localStorage`;
- firme tramite elemento HTML `canvas`;
- generazione PDF tramite jsPDF.

Lo stato principale dell'app comprende:

```javascript
{
  opCount: 0,
  counters: {
    sin: 0,
    tso: 0,
    pos: 0,
    via: 0,
    scu: 0,
    alt: 0,
    ini: 0
  },
  sigPads: {}
}
```

## Compatibilità

L'app è pensata per browser moderni con supporto a:

- HTML5;
- CSS Grid;
- JavaScript moderno;
- `localStorage`;
- elemento `canvas`;
- API touch per la firma su dispositivi mobili.

È consigliabile utilizzare una versione aggiornata di Chrome, Edge, Firefox o Safari.

## Privacy e sicurezza dei dati

I dati inseriti non vengono inviati a un server dell'applicazione: vengono conservati nel `localStorage` del browser e rimangono associati al profilo/browser utilizzato.

Di conseguenza:

- la bozza non è condivisa tra browser o dispositivi;
- la cancellazione dei dati del sito può eliminare la bozza;
- chiunque abbia accesso allo stesso profilo browser può potenzialmente visualizzare i dati salvati;
- il PDF scaricato deve essere gestito secondo le procedure interne applicabili ai dati di servizio.

I portali esterni aperti dall'applicazione sono soggetti alle rispettive condizioni, policy e modalità di trattamento dei dati.

## Limitazioni note dello snapshot

- L'applicazione è completamente locale e non offre autenticazione, ruoli utente o sincronizzazione centralizzata.
- Le sezioni di archivio, report settimanale e report mensile presenti nei commenti del codice non risultano attive nell'interfaccia visualizzata.
- Il comando **Azzera** ricarica la pagina; nello snapshot il salvataggio persistente della bozza non viene esplicitamente cancellato. Se esiste una bozza in `localStorage`, questa può quindi essere ripristinata al caricamento successivo.
- I link ai portali esterni possono cambiare, richiedere autenticazione o non essere più disponibili.
- La generazione del PDF dipende dal caricamento della libreria jsPDF da CDN.
- Non sono presenti test automatici o una pipeline di build.

## Possibili evoluzioni

- Separare HTML, CSS e JavaScript in moduli più facilmente manutenibili.
- Aggiungere un comando per eliminare esplicitamente la bozza salvata.
- Implementare un archivio locale dei report già generati.
- Aggiungere viste settimanali e mensili funzionanti.
- Esportare e importare una bozza come file JSON.
- Integrare autenticazione e salvataggio centralizzato.
- Rendere configurabili i portali web e i dati istituzionali del PDF.
- Servire localmente jsPDF e i font per consentire l'utilizzo offline completo.
- Introdurre test automatici per il calcolo delle violazioni, il salvataggio e la generazione del PDF.

## Licenza

Non è indicata una licenza specifica nel file HTML. Prima di distribuire o modificare pubblicamente l'applicazione, definire la licenza e verificare i diritti relativi a:

- marchi e dati istituzionali;
- contenuti e collegamenti verso portali terzi;
- librerie e font caricati da CDN.
````
