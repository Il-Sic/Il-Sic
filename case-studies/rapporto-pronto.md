# Rapporto Pronto

Italiano · [English](rapporto-pronto.en.md) · [Profilo](../README.md)

Sto sviluppando Rapporto Pronto per i tecnici che devono documentare gli interventi di manutenzione. A fine intervento il tecnico sa che cosa ha fatto, mentre all’ufficio serve un resoconto scritto. Con l’app può dettare la propria nota e verificare il rapportino che ne viene ricavato.

<img src="img/rapporto-pronto/percorso.gif" alt="Rapporto Pronto sul telefono di un tecnico, in poco più di venti secondi. Il tecnico tiene premuto il microfono e detta il lavoro. L’app compila il rapportino, mostra le parole della nota da cui viene la durata e chiede com’è finito l’intervento. Il tecnico risponde «da completare» e passa il telefono al cliente, che legge il documento dall’inizio e firma con il dito." width="320">

<img src="img/rapporto-pronto/nota-dal-computer.png" alt="La stessa pagina nel browser di un computer. In alto c’è la nota vocale del tecnico con la sua trascrizione. Sotto c’è il rapportino che l’app ne ha ricavato, con l’esito che manca e la domanda: «Com’è finita: risolto, non risolto o da completare?». In fondo c’è il microfono per la nota successiva." width="820">

## Dalla nota al rapportino

Il tecnico può dettare o scrivere una nota. Se parte da una registrazione, durante la revisione può leggere la trascrizione e riascoltare il passaggio a cui si riferisce un dato. Se il nome di un ricambio sembra sbagliato, può tornare alle proprie parole, correggere la bozza e confermare il rapportino.

<img src="img/rapporto-pronto/bozza-in-revisione.png" alt="Bozza di un rapportino di manutenzione in revisione su computer. Per la durata e per il compressore è aperta la trascrizione della nota vocale, con le parole da cui viene il dato evidenziate e il pulsante Ascolta per riascoltarle. In fondo ci sono i pulsanti Conferma e Scarica l’anteprima del PDF." width="820">

Ho sviluppato l’interfaccia di revisione in React e TypeScript con Next.js. Il backend Node.js elabora le note in background e salva i rapportini in PostgreSQL. Ho mantenuto la nota originale a portata di mano durante la revisione perché anche una frase plausibile può descrivere un intervento diverso da quello eseguito.

## Integrazioni AI e modalità simulata

Nel primo prototipo l’interpretazione del testo era simulata. Nella versione attuale ho integrato Deepgram per trascrivere l’audio e OpenAI per interpretare il testo e compilare la bozza. Per lo sviluppo e i test posso anche simulare entrambe le fasi con trascrizioni e risposte di prova, senza chiamare i due servizi.

## Scelte tecniche

- **Dati separati per azienda.** I dati di ogni azienda sono isolati in PostgreSQL con Row Level Security. Accedo al database con Drizzle.
- **Accessi su invito.** Si entra con un invito monouso, gestito con Better Auth.
- **Lavori lunghi in coda.** La trascrizione e la scrittura della bozza passano da una coda pg-boss, con un worker separato dall’applicazione web.
- **PDF.** I rapportini in PDF sono generati con pdfkit.
- **Test e controlli.** Scrivo i test con Vitest e Playwright e li faccio girare su GitHub Actions. La modalità simulata permette di provarli senza chiamare i servizi esterni.
- **Rilascio.** L’ambiente Docker è già preparato, con backup cifrati e monitoraggio degli errori, ma non è ancora attivo.

<img src="img/rapporto-pronto/rapportino-pdf.png" alt="Il rapportino in PDF con il nome dell’azienda: cliente, data, durata del lavoro e del viaggio, attività svolta, esito, materiali, prossima azione e note. In alto una riga avverte che i valori vengono dalla modalità simulata, e sotto si legge chi ha confermato il rapportino, chi l’ha firmato e che non è ancora approvato. In fondo c’è la firma del cliente con il timbro FIRMATO, il nome, la data e l’ora." width="560">

## Stato del progetto

L’acquisizione della nota, la creazione della bozza, la revisione e la firma del cliente sono implementate nella versione di sviluppo. Rapporto Pronto non è ancora in produzione.

Il codice è privato perché è un prodotto in sviluppo. Lo mostro volentieri in call.
