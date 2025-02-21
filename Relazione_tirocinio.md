
<!-- Struttura:
     
     - descrizione del workflow dell'azienda
     - descrizione dell'officina pozzo
     - descrizione dell'ospedale di giaveno
   
-->



# Introduzione
<br>

## Modalità di svolgimento del tirocinio  

L'attività è cominciata il 20 Settembre 2024, finita il <font color="red">Aggiungi la data di fine</font> e si è svolta con giorni e orari flessibili.


## Panoramica sull'azienda    

L'azienda V.R.A. (Vendita Regolazioni Automatiche), in cui ho svolto le 300 ore di tirocinio curricolare, lavora nel settore dell'automazioni di impianti di BMS (Building Managment System), si occupa cioè di realizzare impianti automatizzati di aerazione, pressurizzazione, riscaldamento e illuminazione all'interno di edifici. Nello specifico si occupa della parte di automazione, cioè di stabilire l'attrezzatura necessaria a livello software e hardware per rendere il sistema autonomo, di disegnare gli schemi elettrici inerenti le connessioni tra gli apparati hardware presenti nel sistema, di sviluppare la logica dietro l'impianto e della messa in servizio, in loco, dell'impianto, oltre all'eventuale manutenzione in caso di malfunzionamenti. 
<br>




## Descrizione del workflow dell'azienda V.R.A.  

Le commesse si dividono in due tipi:

1) Commesse prese in carico attraverso l'azienda partner iSMA CONTROLLI
2) Commesse prese in carico direttamente dal cliente

La parte in cui differisono le due tipologie è principalmente una, cioè la determinazione dei requisiti hardware dell'impianto.
Il primo task da effettuare, infatti, è stabilire quali device hardware sia meglio utilizzare per ottimizzare il rapporto prestazioni/costo soddisfacendo i requisiti dell'impianto (parte della catena di lavorazione di una commessa a cui ho avuto poco accesso).  
Le commesse del primo tipo non necessitano di questa parte, siccome è già stata effettuata dall'azienda che la dà in carico.


### Descrizione dell'hardware e software utilizzato

Solitamente l'hardware utilizzato consiste in un PLC, un eventuale estensione di porte fisiche se nesessario, collegato tramite protocollo Modbus, e un display touch sul quale l'utente possa visualizzare lo stato dell'impianto ed eventualmente modificarne i parametri, collegato tramite protocollo BACnet.

I controllori utilizzati sono principalmente quelli forniti dall'azienda ISMA-Controlli, quello su cui ho avuto la possibilità di programmare è stato il modello AAC20-LCD. 
Il PLC si basa su un processore ARM-M4 e viene programmato tramite una piattaforma software chiamata Sedona Framework. Piattaforma che permette di programmare tramite un'interfaccia grafica a blocchi logici, ciò rende più semplice e veloce lo sviluppo della logica di controllo dell'impianto di HVAC e la comunicazione con i controllori esterni di supporto, come estensori di porte fisiche e display, e con i sensori, rispetto alla programmazione baremetal in C del microcontrollore in esso contenuto.

Gli estensori di porte più usati sono il MIX38-IP e il MIX18-IP.  
Vengono collegati al PLC principale tramite interfaccia Modbus e permettono di estendere virtualmente, all'interno del Sedona Framework, le porte fisiche del controllore.

I display più utilizzati sono il GTSmart-07 e il GTSmart-010.  
Questi dispositivi sono unità di elaborazione indipendenti dotate di touchscreen con hardware non specificato dal produttore, che montano Linux RT. Per programmarli ci si serve di un programma chiamato JMobile che permette di importare le variabili create sul PLC e di modificarle a piacimento attraverso un'interfaccia grafica.  
Quest'ultima viene sviluppata anch'essa su questo software che, tramite un editor grafico, permette di sviluppare in modo semplice un'interpretazione grafica dell'impianto e del suo funzionamento in modo da mettere nelle mani del cliente le sue principali funzioni.  
  

Un altro hardware che può essere installato è un computer fisso, collegato alla rete locale dell'impianto che, tramite collegamento VPN, permette di controllare tutte le funzionalità dello stesso anche da remoto, per scopo manutentivo o di controllo.


### Fase di determinazione dell'hardware necessario

La prima fase è quella in cui viene accettata la commessa e bisogna identificare, in base al progetto dell'impianto, i componenti hardware atti ad automatizzarlo. Quindi le eventuali pompe, ventilatori, sensori, etc... e il controllore in base al numero degli stessi e alla complessità dell'impianto, siccome, in presenza di un numero elevato di periferiche, i controllori meno potenti non sono in grado di garantire performance adeguate a livello di RAM, velocità computazionale o numero e tipo di periferici.  


### Fase di disegno dei collegamenti hardware

In questa fase della commessa, dopo aver decretato di quali apparati hardware necessita l'impianto, si definisce come questi verranno collegati, cioè, fisicamente, a quale ingresso viene collegato ogni sensore, attuatore, pompa, ventola e sistema controllato dal PLC.  
Gli schemi in questione vengono disegnati tramite software come DraftSight e, dopo la revisione di un ingegnere, inviati agli installatori che si occuperanno di installare fisicamente l'impianto in loco.  

### Fase di sviluppo della logica del PLC

Parte principale del tirocinio svolto in cui si determina come il sistema si comporta tramite la "scrittura" della logica tramite il Sedona Framework. 


### Fase di sviluppo dell'interfaccia grafica da visualizzare sul Display

Ultima fase dello sviluppo, presente solo in caso di necessità, nella quale si disegna, tramite software JMobile, l'interfaccia che l'utente e l'installatore vedranno e da cui potranno controllare l'impianto.  
Il programma permette non solo di modellare l'interfaccia tramite librerie grafiche, ma anche di simularla direttamente dal pc da cui si sviluppa, così da individuare eventuali problemi di funzionamento. 
L'interfaccia può essere poi scaricata sul display direttamente dal programma tramite interfaccia BACnet.  

### Fase di messa in servizio dell'impianto

Quando ogni parte dello sviluppo è terminata, l'impianto è stato installato e il cablaggio posato si può passare alla messa in servizio dell'impianto, cioè la fase in cui ci si reca nell'impianto.  
In loco si controlla che l'impianto sia stato messo in opera in modo corretto, si carica il codice sul controllore, sugli eventuali display e si verifica che la logica funzioni come previsto, oltre a collegare il sistema locale ad un server esterno VPN se il cliente lo richiede, che dà la possibilità di accedervi da remoto.

### Fase di manutenzione

Se si è stilato col cliente un contratto di manutenzione, in caso di necessità, si elargisce un servizio di risoluzione di eventuali problemi o guasti che possono insorgere durante l'utilizzo dell'impianto.

<br><br>

# Primo impianto

Il primo impianto a cui ho avuto la possibilità di lavorare è inerente ad un sistema di ricircolo dell'aria e riscaldamento.  
È composto da due recuperatori d'aria, cioè dispositivi che permettono di effettuare uno scambio di aria nell'ambiente recuperando energia termica accoppiando temicamente il flusso d'aria uscente con quello entrante 
