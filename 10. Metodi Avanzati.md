## Sinonimia

Nella maggior parte delle collezioni, uno stesso concetto può essere espresso con parole diverse. Questo problema, noto come sinonimia, ha un impatto sul richiamo della maggior parte dei sistemi di recupero dell'informazione. Ad esempio, si desidererebbe che una ricerca del termine _aircraft_ restituisca anche risultati contenenti la parola _airplane_.

## Espansione della query

L'idea alla base dell'espansione della query è quella di arricchirla con sinonimi delle parole chiave. Per esempio, a partire dalla query dell'utente “car”, si può ottenere un'espansione come: “car cars automobile automobiles auto”. Questo approccio consiste quindi nell'aggiungere alla query termini sinonimi e correlati, al fine di migliorarne l'efficacia attraverso la corrispondenza con termini affini.

Sono state sviluppate numerose tecniche automatiche o semi-automatiche per l'espansione delle query. Le tecniche semi-automatiche richiedono l'interazione dell'utente per selezionare i termini di espansione migliori. Una tecnica correlata è la _suggerimento di query_, che consiste nel proporre all'utente query alternative, non necessariamente più lunghe o contenenti più termini.

L'espansione della query può includere tecniche come la ricerca di sinonimi delle parole, la ricerca di termini semanticamente correlati, l'individuazione di tutte le forme morfologiche delle parole attraverso il _stemming_ dei termini presenti nella query, la correzione automatica degli errori ortografici con la ricerca della forma corretta o la sua segnalazione nei risultati, e infine il ripesamento dei termini presenti nella query originale.

## Termini correlati

Dove si possono trovare termini correlati a una query, al fine di espanderla? Le fonti principali includono vocabolari controllati, il database lessicale WordNet (sviluppato dalla Princeton University), collezioni testuali, termini che co-occorrono, termini presenti in documenti rilevanti o recuperati, e termini che si trovano in una finestra adiacente all’interno di documenti rilevanti o recuperati. 

Risorse: 

- https://conceptnet.io/
- https://wordnet.princeton.edu/

## Espansione della query tramite thesaurus

L'espansione automatica delle query basata su un vocabolario controllato generale (thesaurus) non risulta particolarmente efficace, poiché non tiene conto del contesto. Ad esempio, la query “tropical fish tanks” potrebbe essere espansa automaticamente in “tropical fish tanks aquariums”, il che è semanticamente coerente. Tuttavia, una query come “armor for tanks” potrebbe anch’essa essere espansa in “armor for tanks aquariums”, che invece è del tutto inappropriato nel contesto bellico.

## Espansione della query tramite co-occorrenza

In alternativa all’uso di un thesaurus, è possibile estrarre termini correlati direttamente dalle collezioni testuali. A questo scopo si utilizzano diverse misure di co-occorrenza per individuare parole chiave collegate, come il coefficiente di Dice, l'informazione mutua, l'informazione mutua attesa e il test del Chi-quadro di Pearson ($\chi^2$). Queste misure possono essere calcolate considerando interi documenti oppure porzioni più piccole, come frasi, paragrafi o finestre testuali. Per semplicità, si considera ora il caso in cui il calcolo avvenga sull’intero documento.

## Coefficiente di Dice

Supponiamo di voler trovare parole correlate al termine “fish”. Come si può misurare il grado di correlazione di un secondo termine rispetto a “fish”? Una soluzione è misurare la co-occorrenza: quante volte i due termini compaiono insieme, rispetto a quante volte compaiono singolarmente. L’idea è che, maggiore è questo punteggio, maggiore dovrebbe essere la relazione tra le due parole.

Questa misura di associazione tra termini è stata utilizzata fin dai primi studi sulla similarità tra termini e nella costruzione automatica di thesauri, a partire dagli anni ’60 e ’70. Dati due termini $a$ e $b$, il coefficiente di Dice è formalmente definito come:

$$\frac{2n_{ab}}{n_a + n_b}$$

dove $n_{ab}$ è il numero di documenti che contengono entrambi i termini $a$ e $b$, $n_a$ è il numero di documenti che contengono il termine $a$ e $n_b$ è il numero di documenti che contengono il termine $b$.

## Informazione mutua

L'informazione mutua è stata utilizzata in numerosi studi sulla collocazione delle parole. È simile al coefficiente di Dice, ma si basa su probabilità. Per due parole $a$ e $b$, è definita come:

$$\log \dfrac{P(a,b)}{P(a)P(b)}$$

dove $P(a,b)$ è la probabilità che $a$ e $b$ compaiano nella stessa finestra testuale, $P(a)$ è la probabilità che il termine $a$ compaia in una finestra testuale, e $P(b)$ è la probabilità che il termine $b$ compaia in una finestra testuale.

## Problema dell’informazione mutua

Un problema dell’informazione mutua è che tende a favorire i termini a bassa frequenza. Ad esempio, consideriamo due termini $a$ e $b$ con $n_a = n_b = 10$ che co-occorrono nella metà dei casi, quindi $n_{ab} = 5$. L'informazione mutua per questi due termini risulta essere $5 \cdot 10^{-2}$.

Consideriamo ora due altri termini $c$ e $d$ con $n_c = n_d = 1000$ che co-occorrono anch’essi nella metà dei casi, quindi $n_{cd} = 500$. L'informazione mutua per questi due termini è $5 \cdot 10^{-4}$.

Sebbene entrambe le coppie co-occorrono nella metà delle volte in cui compaiono, esse ottengono valori di informazione mutua molto diversi: $0.05$ contro $0.0005$.

## Informazione mutua attesa

L’informazione mutua attesa affronta il problema dei termini a bassa frequenza, pesando il valore dell’informazione mutua utilizzando la probabilità $P(a,b)$. Si è principalmente interessati al caso in cui entrambi i termini compaiono, il che porta alla seguente formula:

$$P(a,b) \cdot \log \dfrac{P(a,b)}{P(a)P(b)}$$

## Test del Chi-quadro di Pearson ($\chi^2$)

Questa misura confronta il numero di co-occorrenze tra due parole con il numero atteso di co-occorrenze nel caso in cui le due parole fossero indipendenti, normalizzando il confronto rispetto al valore atteso. La formula è la seguente:

$$\frac{(n_{ab} - N \cdot \frac{n_a}{N} \cdot \frac{n_b}{N})^2}{N \cdot \frac{n_a}{N} \cdot \frac{n_b}{N}}$$

dove $N$ è il numero di documenti nella collezione, e $N \cdot \frac{n_a}{N} \cdot \frac{n_b}{N}$ rappresenta il numero atteso di co-occorrenze nel caso in cui i due termini siano statisticamente indipendenti.

**Esempio.** Utilizzando una collezione di notizie del TREC, le quattro misure di co-occorrenza sono state applicate a livello di documento per trovare i termini più correlati. Di seguito si riportano i primi cinque termini associati alla parola “fish”, uno per ciascuna misura.

**Risultati dell'espansione della query**  
**Coefficiente di Dice**: _species_, _wildlife_, _fishery_, _fisherman_, _sea_  
**Informazione mutua**: _zoologico_, _zapanta_, _wrint_, _sportk_, _lingcod_  
**Informazione mutua attesa**: _water_, _species_, _wildlife_, _fishery_, _wpfmc_  
**Chi-quadro di Pearson**: _arslq_, _happyman_, _outerlimit_, _wighout_, _lingcod_

L’informazione mutua tende a favorire parole molto rare (a volte parole scritte erroneamente). Anche il test del Chi-quadro cattura parole insolite. Il coefficiente di Dice e l’informazione mutua attesa risultano invece più adatti per l’espansione di query nei sistemi di recupero dell’informazione.

## Espansione della query con feedback di rilevanza

Il _relevance feedback_ (RF) è una tecnica di espansione e raffinamento della query basata sul feedback dell’utente. L’idea generale è la seguente: l’utente emette una query (tipicamente breve e semplice), il sistema restituisce un primo insieme di risultati, l’utente contrassegna alcuni dei documenti restituiti come rilevanti (o non rilevanti), il sistema ricalcola una rappresentazione più accurata del bisogno informativo sulla base del feedback ricevuto e infine mostra un nuovo insieme di risultati rivisti.

---

**Esempio di relevance feedback (RF)**

**Query:** _New space satellite applications_

**Risultati iniziali con feedback dell’utente:**

1. _NASA Hasn’t Scrapped Imaging Spectrometer_ — **Rilevante**    
2. _NASA Scratches Environment Gear From Satellite Plan_ — **Rilevante**
3. _Science Panel Backs NASA Satellite Plan, But Urges Launches of Smaller Probes_ — **Non rilevante**
4. _A NASA Satellite Project Accomplishes Incredible Feat: Staying Within Budget_ — **Non rilevante**
5. _Scientist Who Exposed Global Warming Proposes Satellites for Climate Research_ — **Non rilevante**
6. _Report Provides Support for the Critics Of Using Big Satellites to Study Climate_ — **Non rilevante**
7. _Arianespace Receives Satellite Launch Pact From Telesat Canada_ — **Non rilevante**
8. _Telecommunications Tale of Two Companies_ — **Rilevante**

**Documenti rilevanti secondo il feedback dell’utente:**  
#1 _NASA Hasn’t Scrapped Imaging Spectrometer_  
#2 _NASA Scratches Environment Gear From Satellite Plan_  
#8 _Telecommunications Tale of Two Companies_

**Parole chiave ricorrenti nei documenti rilevanti:**  
_new_, _space_, _satellite_, _application_, _nasa_, _eos_, _launch_, _aster_, _instrument_, _arianespace_, _bundespost_, _ss_, _rocket_, _scientist_, _broadcast_, _earth_, _oil_, _measure_

**Query espansa:**  
_new space satellite application_ + _nasa eos launch aster instrument arianespace bundespost ss rocket scientist broadcast earth oil measure_

---
## Pseudo relevance feedback

Il _pseudo relevance feedback_, noto anche come _blind relevance feedback_, è una tecnica che fornisce un metodo automatico per ottenere feedback di rilevanza, automatizzando la parte manuale del relevance feedback, in modo da migliorare le prestazioni di recupero senza richiedere un’interazione prolungata da parte dell’utente. Il procedimento consiste nei seguenti passaggi:

1. Eseguire un recupero normale per ottenere un primo insieme di documenti più rilevanti.
2. Assumere che i primi $k$ documenti classificati siano rilevanti.
3. Calcolare il feedback di rilevanza come nel caso manuale, partendo da questa assunzione.

## Machine learning e IR: perché?

Supponiamo di voler considerare (e combinare) simultaneamente caratteristiche quali:

- frequenza del termine nel corpo del documento    
- frequenza del termine nel titolo del documento
- lunghezza del documento
- popolarità del documento (ad esempio PageRank)

Tutti questi elementi possono essere considerati come _feature_ per stimare la rilevanza. Ma come si dovrebbero pesare correttamente queste feature? Un modello di _learning to rank_ è in grado di apprendere i pesi a partire da un set di addestramento costituito da feature e giudizi di rilevanza.

## Machine learning e recupero dell’informazione

L’idea consiste nell’utilizzare tecniche di _machine learning_ (ML) per costruire un classificatore che distingua tra documenti rilevanti e non rilevanti. Nonostante il machine learning esista da tempo, questa idea si è cominciata a esplorare seriamente solo di recente, principalmente a causa della disponibilità limitata di dati di addestramento: era infatti molto difficile raccogliere collezioni di test con query e giudizi di rilevanza rappresentativi dei reali bisogni informativi degli utenti.

Le funzioni di ranking tradizionali nei sistemi di recupero dell’informazione si basavano su un numero molto limitato di feature, come:

- frequenza del termine (_term frequency_)    
- frequenza inversa del documento (_inverse document frequency_)
- lunghezza del documento

L’introduzione del machine learning consente di superare questi limiti, integrando molteplici segnali per migliorare l’efficacia del recupero.

## Learning to rank

Negli ultimi dieci anni le cose sono cambiate radicalmente. I sistemi moderni — in particolare quelli sul Web — utilizzano un grande numero di _feature_. Alcuni esempi includono:

la frequenza logaritmica delle parole della query nel testo degli anchor, la presenza delle parole della query nel colore della pagina, il numero di immagini nella pagina, il numero di link in entrata e in uscita, il valore di PageRank della pagina, la lunghezza dell’URL, la presenza dei termini della query nell’URL, la data dell’ultimo aggiornamento della pagina, la lunghezza della pagina, e molte altre.

Oggi sono disponibili enormi quantità di dati di addestramento, raccolti tramite i log delle query derivanti dalle interazioni degli utenti.

---

**Esempio Importante.** Supponiamo di considerare due _feature_ per ciascun documento:

La **frequenza del termine** (_term frequency_, tf), ovvero quante volte i termini della query appaiono nel documento, e il **PageRank** (_pr_), che rappresenta la popolarità del documento.

Il set di addestramento sarà composto da vettori $(tf, pr)$ associati a un giudizio di rilevanza. Un modello di _learning to rank_ prende in input queste _feature_ documentali $(tf, pr)$ e restituisce come output una stima della rilevanza.

**Query:** _fish tank price_

| Doc ID | Term frequency | PageRank | Giudizio di rilevanza |
| ------ | -------------- | -------- | --------------------- |
| 001    | 37             | 11       | 1 (rilevante)         |
| 002    | 38             | 0        | 0 (non rilevante)     |
| 003    | 238            | 8        | 1 (rilevante)         |
| 004    | 248            | 1        | 0 (non rilevante)     |
| 005    | 1741           | 5        | 1 (rilevante)         |
| 006    | 2094           | 18       | 1 (rilevante)         |

Un classificatore impara una funzione che separa i documenti rilevanti (output = 1) da quelli non rilevanti (output = 0) sulla base delle due variabili di input: frequenza del termine e PageRank.

I pesi appresi dal classificatore corrispondono ai coefficienti di una funzione lineare. Questa funzione rappresenta il modello che separa le due classi sulla base delle _feature_ disponibili.

---

## Relevance feedback e apprendimento automatico

Il _relevance feedback_ rappresenta un esempio semplice di utilizzo del _machine learning supervisionato_ nel recupero dell’informazione: i dati di addestramento, ossia i documenti marcati come rilevanti o non rilevanti, vengono utilizzati per migliorare le prestazioni del sistema.

Nell’esempio precedente, abbiamo utilizzato i giudizi di rilevanza di una collezione di test per addestrare un classificatore: questo approccio è noto come _apprendimento offline_. Utilizzare il _relevance feedback_ per regolare dinamicamente i pesi del classificatore e migliorarne la precisione è invece un esempio di _apprendimento online_.