# Timbrature Aziendali

Strumento personale per registrare e controllare timbrature, assenze, recuperi e trasferte nel browser. I controlli sono stati impostati in modo prudenziale sulle norme aziendali fornite, in vigore dal 1° agosto 2026.

Il sito non sostituisce SAP, le autorizzazioni preventive, la certificazione delle assenze o la verifica dell’ufficio HR.

## Avvio rapido

1. Apri `index.html` con un browser moderno, preferibilmente tramite un piccolo server locale.
2. In **Azioni rapide → Impostazioni Smart Working**, scegli i giorni settimanali già concordati e caricati in SAP.
3. Seleziona un giorno e registra le timbrature effettive, le causali e le eventuali conferme richieste.
4. Controlla gli avvisi gialli e usa **Esporta riepilogo mensile** per creare una copia CSV.

In **Azioni rapide → Destinazione minuti extra** puoi scegliere, per ogni mese, se conteggiare lo straordinario autorizzato come **Minuti extra per permessi di uscita** oppure come **Straordinario**. La scelta viene salvata insieme al mese e non riclassifica i mesi già chiusi.

Il contatore per i permessi di uscita è uno strumento personale di pianificazione: non costituisce una banca ore ufficiale e non sostituisce autorizzazioni, causali, registrazioni SAP o indicazioni HR.

## Aggiornamento della versione online

La pagina mostra il numero di versione accanto all'avviso **Dati locali**. Dopo aver sostituito i file sul server, verifica che il numero coincida con quello della copia appena pubblicata. Se compare ancora la vecchia schermata, esegui un ricaricamento completo con `Ctrl+F5` e svuota l'eventuale cache dell'hosting o del proxy aziendale. Per evitare che ricapiti, configura l'hosting affinché `index.html` venga servito con `Cache-Control: no-cache, must-revalidate`.

## Regole applicate

- Giornata ordinaria in ufficio: 08:00–16:45, 8 ore nette e 45 minuti di mensa.
- Mensa: fasce 12:00–12:45, 12:30–13:15, 13:00–13:45 e 13:30–14:15. Lo spostamento richiede una causale ammessa e mantiene la durata di 45 minuti.
- Flessibilità: 60 minuti giornalieri complessivi tra ingresso posticipato e rientro mensa, più un paniere di 20 minuti nel mese.
- Minuti extra accumulati: nella scheda del singolo giorno puoi scegliere se coprire il ritardo di ingresso mattutino usando la maggior presenza non classificata disponibile nello stesso mese, anche se matura nei giorni successivi. La scelta resta attiva anche quando il saldo è ancora zero e, se il totale mensile non basta, viene usata solo la quota disponibile lasciando il residuo da gestire. Non vengono usati i minuti accantonati per uscite o gli straordinari autorizzati; la copertura non si applica alla flessibilità oltre soglia né a ritardi diversi da quello mattutino.
- Recuperi: in ordine cronologico, senza accredito preventivo, in periodi di almeno 15 minuti e non nei giorni non lavorativi.
- Ferie a ore: minimo 30 minuti, poi multipli di 15.
- PAR: minimo 60 minuti, poi multipli di 60.
- Permessi a recupero: 3 da 2 ore e 1 da 3 ore al mese; per il profilo Quadro l’ultimo permesso è di 4 ore. Un permesso precedente deve essere recuperato prima di inserirne un altro o classificare straordinario.
- Visita medica: massimo 3 ore; fisioterapia, medicazione o estrazione: massimo 2 ore. È richiesta la certificazione e gli orari vengono arrotondati al quarto d’ora precedente/successivo.
- Straordinario: autorizzazione preventiva e durata minima di 30 minuti. Dopo il riconoscimento, il sito può conteggiarlo come straordinario oppure nel contatore personale dei minuti per permessi di uscita. La scelta non cambia le regole contrattuali applicate al profilo.
- Per B2/B3 il lavoro del sabato viene esposto separatamente come prestazione con indennità; domenica e festivi seguono le regole di straordinario previste dal profilo.
- Smart working: i giorni settimanali selezionati nelle impostazioni sono considerati già concordati e inseriti in SAP. La giornata prevede 8 ore nella fascia 08:00–19:00 e normalmente non genera straordinario; una giornata Smart aggiunta manualmente fuori dalla pianificazione richiede invece la conferma.

Il sito distingue il debito ancora recuperabile dai minuti già da giustificare. Alla chiusura del mese il residuo non recuperato deve essere gestito nel sistema ufficiale secondo la sequenza prevista, prima ferie e poi PAR.

## Casi da confermare con HR

Il documento non definisce in modo univoco ogni combinazione possibile. Il sito segnala invece di inventare regole nei casi relativi a Quadri, trasferte, coincidenze tra sabato e festività, modalità esatta di liquidazione e altre eccezioni contrattuali.

## Dove vengono salvati i dati

- Timbrature e impostazioni restano nel `localStorage` del browser usato.
- Non esistono sincronizzazione, account, backup centrale o registro delle modifiche.
- Cambiando browser o cancellando i dati del sito, le registrazioni possono andare perse.
- CSV e immagini possono contenere dati personali: conservali e condividili solo tramite strumenti aziendali autorizzati.

Per un uso multiutente o ufficiale servono autenticazione aziendale, database, permessi, backup, audit log e approvazione privacy/HR.

## Condivisione del sito

La cartella può essere pubblicata su un hosting statico HTTPS autorizzato dall’azienda. Prima della pubblicazione:

- prova il sito con dati fittizi;
- abilita autenticazione aziendale se contiene dati reali;
- definisci conservazione, backup e responsabilità del trattamento;
- sostituisci Tailwind via CDN con una build locale se la rete aziendale richiede funzionamento offline o una Content Security Policy rigorosa;
- evita di distribuire configurazioni o chiavi segrete insieme ai file del sito.

## Importazione AI da immagine

L’importazione è una bozza da verificare: non può confermare autorizzazioni SAP, certificazioni, causali mensa o straordinario. Ogni giornata importata viene quindi marcata per il controllo manuale.

L’immagine viene inviata a Google Gemini solo dopo il consenso dell’utente. Non usare schermate contenenti dati personali se la policy aziendale non lo consente.

La chiave Gemini non viene salvata nel sito. Il file `gemini-proxy-worker.js` è un esempio di Cloudflare Worker che conserva la chiave lato server.

### Configurazione del Worker

1. Crea un Cloudflare Worker e usa il contenuto di `gemini-proxy-worker.js`.
2. Salva `GEMINI_API_KEY` come secret del Worker.
3. Imposta `ALLOWED_ORIGINS` con l’indirizzo esatto del sito; separa più origini con una virgola.
4. Facoltativamente imposta `GEMINI_MODEL` su `gemini-2.5-flash` o `gemini-2.5-flash-lite`.
5. Proteggi sito e Worker con SSO/Cloudflare Access; dopo averlo attivato imposta `REQUIRE_CF_ACCESS=true`.
6. Inserisci nel sito l’URL HTTPS del Worker dalla sezione di configurazione AI.
7. Configura limiti di frequenza e budget.

La sola lista `ALLOWED_ORIGINS` limita le chiamate dal browser, ma non sostituisce l’autenticazione.

## Miglioramenti inclusi

- controlli normativi per ferie/PAR, visite, fisioterapia, smart working, recuperi e straordinario;
- registri separati per recupero, maggior presenza, straordinario, minuti accantonati per uscite e indennità del sabato;
- validazione degli orari e conferme prima di perdere modifiche o cancellare dati;
- calendario adattivo, tema chiaro/scuro, uso da tastiera e finestre modali accessibili;
- riepilogo ed esportazione CSV più completi;
- importazione immagini limitata a PNG, JPEG e WebP fino a 8 MB;
- Worker AI con origini autorizzate, modello controllato, timeout e risposte sicure.
