<!-- 

Struttura:
     
     - descrizione del workflow dell'azienda      x
     - descrizione dell'officina pozzo            x
     - descrizione dell'ospedale di giaveno
     - messe in servizio di impianti              x
     - altro
     - conoscenze acquisite
   
-->


- [Introduzione](#introduzione)
  - [Modalità di svolgimento del tirocinio](#modalità-di-svolgimento-del-tirocinio)
  - [Panoramica sull'azienda](#panoramica-sullazienda)
  - [Descrizione del workflow dell'azienda V.R.A.](#descrizione-del-workflow-dellazienda-vra)
    - [Descrizione dell'hardware e software utilizzato](#descrizione-dellhardware-e-software-utilizzato)
    - [Fase di determinazione dell'hardware necessario](#fase-di-determinazione-dellhardware-necessario)
    - [Fase di disegno dei collegamenti hardware](#fase-di-disegno-dei-collegamenti-hardware)
    - [Fase di sviluppo della logica del PLC](#fase-di-sviluppo-della-logica-del-plc)
    - [Fase di sviluppo dell'interfaccia grafica da visualizzare sul Display](#fase-di-sviluppo-dellinterfaccia-grafica-da-visualizzare-sul-display)
    - [Fase di messa in servizio dell'impianto](#fase-di-messa-in-servizio-dellimpianto)
    - [Fase di stesura del manuale di utilizzo](#fase-di-stesura-del-manuale-di-utilizzo)
    - [Fase di manutenzione](#fase-di-manutenzione)
- [Primo impianto](#primo-impianto)
  - [Logica di programmazione](#logica-di-programmazione)
- [Secondo impianto](#secondo-impianto)
  - [Logica di programmazione](#logica-di-programmazione-1)
    - [Ventilatori](#ventilatori)
    - [Umidificazione](#umidificazione)
    - [Termoregolazione](#termoregolazione)
    - [pompe](#pompe)
- [Altre esperienze](#altre-esperienze)
  - [Messe in servizio](#messe-in-servizio)
    - [Condominio caselle, via gibellini](#condominio-caselle-via-gibellini)
    - [Scuola primaria Luigi Einaudi, Aosta](#scuola-primaria-luigi-einaudi-aosta)
  - [Sopraluogo per un impianto di Illuminazione alla Fondazione Teatro Ragazzi e Giovani Onlus](#sopraluogo-per-un-impianto-di-illuminazione-alla-fondazione-teatro-ragazzi-e-giovani-onlus)
- [Conoscenze acquisite](#conoscenze-acquisite)

<br><br>

# Introduzione
<br>

## Modalità di svolgimento del tirocinio  

L'attività è cominciata il 20 Settembre 2024, finita il <font color="red">Aggiungi la data di fine</font> e si è svolta con giorni e orari flessibili.


## Panoramica sull'azienda    

L'azienda [V.R.A. (Vendita Regolazioni Automatiche)](https://www.linkedin.com/company/vra-srl/?originalSubdomain=it) , in cui ho svolto le 300 ore di tirocinio curricolare, lavora nel settore dell'automazioni di impianti di BMS (Building Managment System), si occupa cioè di realizzare impianti automatizzati di aerazione, pressurizzazione, riscaldamento e illuminazione all'interno di edifici. Nello specifico si occupa della parte di automazione, cioè di stabilire l'attrezzatura necessaria a livello software e hardware per rendere il sistema autonomo, di disegnare gli schemi elettrici inerenti le connessioni tra gli apparati hardware presenti nel sistema, di sviluppare la logica dietro l'impianto e della messa in servizio, in loco, dell'impianto, oltre all'eventuale manutenzione in caso di malfunzionamenti. 
<br>




## Descrizione del workflow dell'azienda V.R.A.  

Le commesse si dividono in due tipi:

1) Commesse prese in carico attraverso l'azienda partner [iSMA-Controlli](https://www.ismacontrolli.com/it/)
2) Commesse prese in carico direttamente dal cliente

La parte in cui differisono le due tipologie è principalmente una, cioè la determinazione dei requisiti hardware dell'impianto.
Il primo task da effettuare, infatti, è stabilire quali device hardware sia meglio utilizzare per ottimizzare il rapporto prestazioni/costo soddisfacendo i requisiti dell'impianto (parte della catena di lavorazione di una commessa a cui ho avuto poco accesso).  
Le commesse del primo tipo non necessitano di questa parte, siccome è già stata effettuata dall'azienda che la dà in carico.


### Descrizione dell'hardware e software utilizzato

Solitamente l'hardware utilizzato consiste in un PLC, un eventuale estensione di porte fisiche se nesessario, collegato tramite protocollo Modbus, e un display touch sul quale l'utente possa visualizzare lo stato dell'impianto ed eventualmente modificarne i parametri, collegato tramite protocollo BACnet.

I controllori utilizzati sono principalmente quelli forniti dall'azienda ISMA-Controlli, quello su cui ho avuto la possibilità di programmare è stato il modello [AAC20-LCD](https://www.ismacontrolli.com/it/isma-b-aac20-lcd-d.html). 
Il PLC si basa su un processore [ARM-M4](https://www.st.com/content/st_com/en/arm-32-bit-microcontrollers/arm-cortex-m4.html) e viene programmato tramite una piattaforma software chiamata [Sedona Framework](https://linsong.github.io/sedona/site/index.html). Piattaforma che permette di programmare tramite un'interfaccia grafica a blocchi logici, ciò rende più semplice e veloce lo sviluppo della logica di controllo dell'impianto di HVAC e la comunicazione con i controllori esterni di supporto, come estensori di porte fisiche e display, e con i sensori, rispetto alla programmazione baremetal in C del microcontrollore in esso contenuto.

Gli estensori di porte più usati sono il [MIX38-IP](https://www.ismacontrolli.com/it/isma-b-mix38-ip.html) e il [MIX18-IP](https://www.ismacontrolli.com/it/isma-b-mix18-ip.html).  
Vengono collegati al PLC principale tramite interfaccia Modbus e permettono di estendere virtualmente, all'interno del Sedona Framework, le porte fisiche del controllore.

I display più utilizzati sono il [GTSmart-07](https://www.ismacontrolli.com/en/gtsmart-07.html) e il [GTSmart-010](https://www.ismacontrolli.com/pl/gtsmart-010.html).  
Questi dispositivi sono unità di elaborazione indipendenti dotate di touchscreen con hardware non specificato dal produttore, che montano Linux RT. Per programmarli ci si serve di un software chiamato [JMobile](https://www.exorint.com/it/prodotti/software/jmobile) che permette di importare le variabili create sul PLC e di modificarle a piacimento attraverso un'interfaccia grafica.  
Quest'ultima viene sviluppata anch'essa su questo software che, tramite un editor grafico, permette di sviluppare in modo semplice un'interpretazione grafica dell'impianto e del suo funzionamento in modo da mettere nelle mani del cliente le sue principali funzioni.  
  

Un altro hardware che può essere installato è un computer fisso, collegato alla rete locale dell'impianto che, tramite collegamento VPN, permette di controllare tutte le funzionalità dello stesso anche da remoto, per scopo manutentivo o di controllo.


### Fase di determinazione dell'hardware necessario

La prima fase è quella in cui viene accettata la commessa e bisogna identificare, in base al progetto dell'impianto, i componenti hardware atti ad automatizzarlo. Quindi le eventuali pompe, ventilatori, sensori, etc... e il controllore in base al numero degli stessi e alla complessità dell'impianto, siccome, in presenza di un numero elevato di periferiche, i controllori meno potenti non sono in grado di garantire performance adeguate a livello di RAM, velocità computazionale o numero e tipo di periferici.  


### Fase di disegno dei collegamenti hardware

In questa fase della commessa, dopo aver decretato di quali apparati hardware necessita l'impianto, si definisce come questi verranno collegati, cioè, fisicamente, a quale ingresso viene collegato ogni sensore, attuatore, pompa, ventola e sistema controllato dal PLC.  
Gli schemi in questione vengono disegnati tramite software come [DraftSight](https://www.draftsight.com/it) e, dopo la revisione di un ingegnere, inviati agli installatori che si occuperanno di installare fisicamente l'impianto in loco.  

### Fase di sviluppo della logica del PLC

Parte principale del tirocinio svolto in cui si determina come il sistema si comporta tramite la realizzazione della logica tramite il Sedona Framework. 


### Fase di sviluppo dell'interfaccia grafica da visualizzare sul Display

Ultima fase dello sviluppo, presente solo in caso di necessità, nella quale si disegna, tramite software JMobile, l'interfaccia che l'utente e l'installatore vedranno e da cui potranno controllare l'impianto.  
Il programma permette non solo di modellare l'interfaccia tramite librerie grafiche, ma anche di simularla direttamente dal pc da cui si sviluppa, così da individuare eventuali problemi di funzionamento. 
L'interfaccia può essere poi scaricata sul display direttamente dal programma tramite interfaccia BACnet.  

### Fase di messa in servizio dell'impianto

Quando ogni parte dello sviluppo è terminata, l'impianto è stato installato e il cablaggio posato si può passare alla messa in servizio dell'impianto, cioè la fase in cui ci si reca nell'impianto.  
In loco si controlla che l'impianto sia stato messo in opera in modo corretto, si carica il codice sul controllore, sugli eventuali display e si verifica che la logica funzioni come previsto, oltre a collegare il sistema locale ad un server esterno VPN se il cliente lo richiede, che dà la possibilità di accedervi da remoto.

### Fase di stesura del manuale di utilizzo

Ogni volta che l'impianto viene messo in servizio è necessario fornire al cliente un manuale di istruzioni per istruire l'utente sull'utilizzo dell'impianto.
Uno relativo al menu creato sul display LCD del controllore stesso, utilizzabile come dispositivo di backup nel caso in cui si riscontrassero problemi di funzionamento con il display touchscreen, un altro relativo al dispositivo con touchscreen integrato, inerente ai menù di interfaccia creati.

### Fase di manutenzione

Se si è stilato col cliente un contratto di manutenzione, in caso di necessità, si elargisce un servizio di risoluzione di eventuali problemi o guasti che possono insorgere durante l'utilizzo dell'impianto.

<br><br>

# Primo impianto

Il primo impianto a cui ho avuto la possibilità di lavorare è inerente ad un sistema di ricircolo dell'aria e riscaldamento ([schema elettrico](https://github.com/davidedidomenico/relazione_tirocinio/blob/main/immagini/primo%20impianto/25226%20-%20Q.E.%20di%20Regolazione_RevB.pdf)).  
È composto da:   

1) Due recuperatori d'aria, cioè dispositivi che permettono di effettuare uno scambio di aria nell'ambiente recuperando energia accoppiando temicamente il flusso d'aria uscente con quello entrante.  
2) Un sistema VRF (Variable Refrigerant Flow), cioè un sistema di climatizzazione centralizzato che permette di aumentare l'efficienza energetica tramite la modulazione del flusso di refrigerante in ciascuna zona.   

La maggior parte del tempo impiegato a sviluppare questo progetto è stata spesa nella comprensione del funzionamento di un impianto di climatizzazione, dei programmi utilizzati, quali JMobile, ISMATool e YABE, dei protocolli BACnet e Modbus e del Sedona Framework. 

## Logica di programmazione

Il controllore designato per questo incarico è l'ISMA-Controlli AAC20 in collegamento con un display touchscreen GTSmart 07. 
Il VRF fornisce al PLC le temperature di ogni stanza, lo stato di accensione dei fan coil (apparato addetto all'effusione dell'aria climatizzata negli ambienti) e permette di modificare il setpoint di temperatura in ogni stanza.  
La logica di funzionamento prevede:   
una lettura delle temperature dal VRF, la creazione di una schedule oraria per l'attivazione dell'impianto e la regolazione dei setpoint di velocità delle ventole dei recuperatori.     
      
Alla fine della programmazione il sistema è in grado di modificare i setpoint di velocità delle ventole dei recuperatori e di temperatura di 8 ambienti differenti. Questo sia tramite il display integrato nel PLC che quello del touchscreen.      
Purtroppo non ho avuto la possibilità di presediere durante la messa in servizio del sistema.  
L'impianto ha tre possibili stati di funzionamento: On, Off e Auto (in quest'ultima segue un programma orario impostato dall'utente). 

![enable](./immagini/primo%20impianto/Screenshot%202025-02-21%20171153.png)    

Se il sistema è abilitato, in base ai setpoint impostati dall'utente, viene inviato il comando inerente alla velocità dei ventilatori. Il segnale viene inviato dal PLC al controllore del recuperatore come segnale analogico con ampiezza che varia linearmente da zero Volt, potenza dissipata dai ventilatori nulla, fino a dieci Volt, potenza dissipata massima.  

![setpoint ventilatori](./immagini/primo%20impianto/Screenshot%202025-02-21%20171140.png)   

I setpoint di temperatura inerenti ai vari ambienti vengono semplicemente acquisiti dal touchscreen, o dal display LCD integrato nel PLC come sistema di backup, e inviati, tramite protocollo Modbus, al VRF.    


# Secondo impianto

Il secondo impianto, di cui ho disegnato lo [schema](https://github.com/davidedidomenico/relazione_tirocinio/blob/main/immagini/secondo%20impianto/Schemi%20Regolazione%20Ospedale%20Giaveno.pdf) e programmato il PLC, riguarda un ospedale situato a Giaveno. Una [UTA](https://github.com/davidedidomenico/relazione_tirocinio/blob/main/immagini/secondo%20impianto/2215_def-ese_me10_Schema_R1.pdf) (unità trattamento aria), cioè un sistema in grado di creare un ricircolo d'aria scaldata o raffreddata in base alle necessità.  
È dotata di 

1) Batteria di recupero. Dispositivo che permette di immagazzinare energia termica derivante dal flusso di aria di recupero, cioè quella in uscita dall'ambiente, utilizzata per scaldare o raffreddare quella di mandata, cioè quella da immettere nell'ambiente.  
Viene effettuato tramite un circuito dell'acqua ai cui estremi sono presenti due serpentine, cioè un ristringimento della sezione che permette di trasferire più efficacemente il calore, mostrando al flusso d'aria una superfice esposta maggiore.

 

2) Batteria di riscaldamento. Parte atta al riscaldamento del flusso d'aria di mandata.

3) Batteria di raffreddamento. Parte dedita al raffreddamento dell'aria di mandata.

4) Umidificatore. Dispositivo in grado di nebulizzare dell'acqua nel flusso di aria di mandata.

5) Pompe. Una per ogni batteria. Nello specifico la logica permette di controllare quelle della batteria di recupero, di riscaldo e di raffreddamento.  

I dispositivi elettronici utlizzati in questo impianto sono stati: controllore AAC20-LCD, espansore di porte MIX38 e un touchscreen GTSmart07.  

## Logica di programmazione

Il sistema viene attivato da una variabile, modificabile tramite il display touch o dalla programmazione oraria se impostato in modalità automatica. 

### Ventilatori

Se la variabile di attivazione dell'impianto è attiva e l'allarme antigelo è disattivo (allarme che si attiva nel caso in cui la temperatura dell'acqua nel sistema scenda sotto una soglia determinata, per evitarne il congelamento), viene comandata l'apertura delle serrande che permettono il passaggio dei flussi di mandata e ripresa. 

![serrande](./immagini/secondo%20impianto/WhatsApp%20Image%202025-02-24%20at%2017.13.25.jpeg)

Se lo stato delle serrande non corrisponde al comando inviato, dopo un determinato delay, viene inviato un allarme, altrimenti viene inviato un comando ai ventilatori di mandata e ripresa. La regolazione del comando in questione è determinata da un algoritmo di controllo di tipo PID (proportional-integral-derivative), i cui parametri vengono decisi al momento della messa in servizio dell'impianto, siccome non si conosce a priori l'inerzia dei ventilatori. Il controllo si basa su dei setpoint di pressione, rilevata come differenza tra quella acquisita a monte e a valle del rispettivo ventilatore.

![pid ventilatori](./immagini/secondo%20impianto/Screenshot%202025-02-24%20154226.png)

### Umidificazione

Il sistema è in grado di umidificare o deumidificare l'aria all'interno dell'ambiente, rispettivamente, tramite un nebulizzatore e la batteria di raffreddamento.   
Per deumidificare, infatti, si sfrutta la dipendenza del punto di rugiada dalla temperatura. Il fenomeno per il quale, abbassando la temperatura, a parità di pressione, un determinato volume d'aria è in grado di contenere una quantità minore di vapore acqueo. Quindi, raffreddando il volume di aria di mandata, è possibile far condensare una quantità d'acqua proporzionale al decremento di temperatura.  
Il controllo del segnale inerente all'intensità dell'azione umidificante, o deumidificante viene effettuato tramite due algoritmi di controllo PID.  
Quello inerente all'umidificatore controlla l'umidità presente nel flusso d'aria di ripresa e, in base al setpoint impostato, regola la risposta del nebulizzatore.   
Quello inerente all'azione deumidificante controlla l'umidità del medesimo flusso e, analogamente, in base al setpoint impostato, regola la risposta della batteria di raffreddamento.  
Un altro parametro da tenere in considerazione è l'umidità dell'aria di mandata, siccome un valore troppo alto potrebbe far uscire acqua dall'umidificatore. Quindi viene messo in parallelo un ulteriore controllo PID, ma con setpoint l'umidità di mandata massima e, con variabile controllata l'umidità di mandata. Di questi due controlli viene poi preso in output il valore minimo.  

![umidificazione](./immagini/secondo%20impianto/Screenshot%202025-02-24%20154436.png)

Inoltre l'azione umidificante viene comandata solo d'inverno e quella deumidificante solo d'estate, a causa dei naturali livelli di umidità presenti a Torino.  

### Termoregolazione

Il sistema di termoregolazione utilizza la variabile di attivazione come enable e si basa su una variabile che indica la stagione per discernere il funzionamento in modalità riscaldamento o raffreddamento.   

In base alla stagione selezionata si interviene sulla temperatura dell'ambiente con un PID diverso, uno per l'inverno e uno per l'estate.   
Il primo agisce controllando la temperatura di ripresa in uscita dall'ambiente e ne modifica il valore, agendo sulla batteria di riscaldamento, in base al setpoint.  
Il secondo agisce controllando la stessa temperatura di ripresa e modificandone il valore, agendo, però, sulla batteria di raffreddamento, in base al setpoint impostato.   
Per entrambi i sottosistemi viene anche eseguito un controllo della temperatura di mandata dell'aria nell'ambiente, in modo da evitare che venga immessa aria a temperatura troppo bassa o troppo alta, così da non intaccare il comfort degli ospiti.    

### pompe

Le pompe vengono comandate con due logiche diverse, una per la pompa del circuito di recupero, una per quelle dei circuiti di raffreddamento e riscaldamento.   

La pompa di recupero, se l'impianto è abilitato, viene sempre accesa e va in allarme nel caso in cui non ci sia plausibilità tra lo stato della stessa e il comando impartitole, oppure se il controllore che ne gestisce la comunicazione con il PLC dell'impianto rileva un allarme.

Le altre due pompe vengono attivate nel caso in cui il carico richiesto al sistema di riscaldamento o raffreddamento sia diverso da zero. Siccome si tratta di pompe gemellari, cioè ogni pompa è composta da due pompe, progettate per non lavorare mai simultaneamente, il comando 




 
 
<br> <br>

# Altre esperienze

## Messe in servizio

Durante il tirocinio ho avuto la possibilità di assistere un impiegato dell'azienda nella messa in servizio di alcuni impianti non sviluppati da me.

### Condominio caselle, via gibellini

Il sistema in questione era una centrale termica e di acqua calda sanitaria di un condominio.  
Il PLC integrato nel sistema dall'azienda permette al gestore del condominio di controllare i setpoint orari delle temperature degli ambienti dei residenti e dell'acqua calda sanitaria.  
La messa in servizio è constata di:  

- attivazione dei contacalorie in ogni appartamento, sensori che permettono di valutare il consumo, in termini energetici, del condominio in termini di acqua calda misurando la differenza di temperatura in ingresso e in uscita all'appartamento.
- caricamento del codice sviluppato sul PLC installato nel condominio e test delle funzionalità
- risoluzione dei problemi di installazione e configurazine degli apparati dell'impianto

Quest'ultima parte si è resa nesessaria siccome all'attivazione dell'impianto una pompa e il calorifero non hanno funzionato come previsto. Dopo un analisi della situazione e una lettura dei datasheet dei dispositivi elettronici che permettono a questi sistemi di comunicare col PLC, si è riusciti a modificare i collegamenti elettrici, cablati in maniera errata, e cambiarne le impostazioni in modo da rendere funzionante il sistema.   

### Scuola primaria Luigi Einaudi, Aosta

Il sistema acquistato dalla scuola in questione era composto da una UTA (unità trattamento aria), cioè un apparato composto da ventilatori, batterie di scambio termico e recuperatori di calore che permette di rinnovare e climatizzare l'aria in diversi ambienti.
Questo impianto consta di un controllore AAC20 e di due display, un GTSmart 07 e un GTSmart 010.  
I touchscreen sono disposti in due ambienti diversi per controllarne la temperatura e il ricambio d'aria, connessi a sensori di rilevazione di CO2 per garantire un livello di comfort adeguato alle diverse situazioni di occupazione degli ambienti, siccome sono rappresentati da un teatro e da un aula di informatica.  
All'inizio dell'operazione ci si è resi conto che la rete LAN non era cablata in modo corretto. infatti il PLC, situato nel ambiente teatro, non comunicava nè con la UTA, nè con il display situato nella aula di informatica.  Si è proceduto al caricamento sul controllore e sui display in loco del programma sviluppato, oltre alla modifica di alcuni parametri di sviluppo, voluti dal cliente.  
Data la condizione del cablaggio della rete locale del sistema, è stato necessario un'ulteriore intervento, dopo la messa a punto della stessa da parte degli installatori. Nel secondo intervento si è verificato che la rete fosse effettivamente funzionasse effettuando un operazione di ping da un pc collegato alla rete, ai vari nodi.  
Dopo la verifica si è proceduto a controllare il funzionamento della UTA. Il suo funzionamento può essere controllato attraverso la comunicazione via rete locale IP dal controllore. Il programma, in base all'orario, alla temperatura e alla quantità di CO2 nell'aria, regola il flusso d'aria negli ambienti e la sua temperatura.   
La velocità delle ventole, e quindi il flusso di aria, viene controllato tramite un algoritmo di controllo PID.    

## Sopraluogo per un impianto di Illuminazione alla [Fondazione Teatro Ragazzi e Giovani Onlus](https://casateatroragazzi.it/)    

Il mio supervisore mi ha dato la possibilità di presiedere ad una prima fase di valutazione di un nuovo progetto di cui l'azienda stava prendendosi carico.     
La struttura in questione presentava già un vetusto e problematico impianto di automazione e gestione delle luci all'interno dei vari ambienti, quindi si è affidata alla V.R.A. per concordare la messa in servizio di un nuovo impianto in grado di integrare e ampliare il sistema di illuminazione di ambienti dediti a uffici e palcoscenici.  
Il sopraluogo è consistito di un colloquio con un responsabile dello stabile affinchè spiegasse lo stato attuale dell'impianto e le modifiche che si intendevano effettuare, dopodichè si è proceduto a verificare quali interventi fossero necessari e la stima del loro costo complessivo. 


# Conoscenze acquisite

Durante questo tirocinio ho avuto modo di apprendere il funzionamento 




