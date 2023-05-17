

**Cartelle**
Struttura della cartella di progetto. Alcune parti non sono state ancora guardate in modo approfondito. Il meccanismo delle certificazioni e delle chiavi non mi è ancora molto chiaro, quindi alcune frasi potrebbero essere imprecise o sbagliate. 

**bin**
Comandi utilizzati durante le varie fasi di creazione della blockchain. I più importanti: 
- *configtxgen*: comandi per la gestione delle transazioni, tra cui anche la creazione dei canali e delle varie entità a partire da determinati file di configurazione
- *cryptogen*: comandi utilizzati per generare e gestire materiale crittografico (autogenerati)
- *orderer/peer*: comandi relativi alle due entità: nodo peer e nodo orderer
- *osnadmin*: comandi per la gestione degli Orderer Service Nodes (OSN), per esempio i nodi orderer

**config**
Detiene i file di configurazione di default relativi agli elementi principali:
	- *configtx.yaml* : configurazione di default delle entità iniziali presenti in una rete blockchain, come ad esempio struttura delle organizzazioni e relazioni, configurazione del/dei canali, profili, ecc..
	- *orderer/peer :* configurazioni di default dei nodi peer e orderer

**CURSA**
Cartella principale del progetto. 

**compose**
Contiene le cartelle *docker* e *podman* . Essi contengono i necessari file di configurazione per far eseguire la rete su uno dei due motori di containerizzazione. In questo caso, viene utilizzato *Docker Compose*

**configtx**
Cartella contenente l'omonimo *file yaml*. Questo è molto importante in quanto in esso sono presenti tutte le configurazioni necessarie per creare "l'architettura" della rete che si vuole creare.

**monitordocker.sh**
Script non ancora analizzato. Funzionalità deducibile dal nome del file.

**network.sh**
File utilizzabile come "interfaccia" iniziale per la realizzazione della rete di Hyperledger.
Da qui si fanno eseguire i container e le configurazioni che definiscono la rete.

**organizations**
Cartella che contiene i file necessari per realizzare i materiali criptografici. Questo è possibile farlo tramite due strumenti differenti: *cryptogen* e *fabric-ca*. 

- *cryptogen* - contiene i file di configurazione per la realizzazione di certificazioni fittizie. Preferibilmente utilizzabile solo in fase di *testing*, implementazione semplificata ma poco sicura. 
- *fabric-ca* - strumento proprietario per l'uso di *ca* riconosciute da fabric. Da utilizzare in fase di produzione

N.B. 
Sono presenti altri file di template per la gestione di materiale criptografico. File non ancora analizzati bene, da capire bene il loro ruolo. Probabilmente utilizzabili nel caso di uso dello strumento di *fabric-ca*

N.B. 2 
Le cartelle *ordererOrganizations* e *peerOrganizations* sono cartelle molto importanti che vengono create durante i processi di creazione dei certificati. Essi contengono anche la struttura gerarchica dei vari certificati

**Scripts**
Cartella contenente gli script che permettono di eseguire le varie funzionalità di una blockchain, tra cui la gestione dei suoi componenti (come la creazione della rete, l'aggiunta di canali, il setting delle relative variabili d'ambiente necessarie per farlo). Questi scripts vengono invocati anche nel file *network.sh*.

**Configurazione Generale**

Le configurazioni vengono effettuate principalmente in 3 file:
 - *crypto-config.yaml*:  file utilizzato dallo strumento cryptogen per la generazione dei certificati
 - *configtx.yaml*:  file molto importante definire la struttura del canale. Maggiori info a: https://hyperledger-fabric.readthedocs.io/en/release-2.5/create_channel/create_channel_config.html
 - *compose-test-net.yam*l: docker-compose file in cui vengono definiti tutti i container con relative variabili

Entrypoint per la creazione della rete blockchain è il file *network.sh* che, il quale esegue operazioni differenti in base alle opzioni usate durante la sua esecuzione. 

Ad esempio il comando:

		./network.sh up

fa si che 1) vengano generati i certificati specificati in *crypto-config.yam*l 2) vengano eseguiti i container specificati in *compose-test-net-yaml* 

Altrimenti, il comando:

		./network.sh createChannel -c <nomecanale>

se non esiste ancora una rete, la crea seguengo i passi visti per il comando *up* . Successivamente, grazie all'invocazione e all'esecuzione dello script *createChannel.sh*, vengono creati i canali desiderati (questo grazie alla definizione di varie proprietà nel file *configtx.yaml*)
