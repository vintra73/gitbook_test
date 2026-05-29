# Scambi di dati asincroni con callback

## Che cosa sono e a cosa servono

Gli **scambi di dati asincroni con callback** permettono all'erogatore di pubblicare e-service strutturati per gestire, in modo standardizzato, tutti i casi in cui la predisposizione della risposta è un'**attività che richiede tempo**.

In particolare, l'erogatore può: 
* Permettere al fruitore di **recuperare la risposta a blocchi (paginazione)**.
* Richiedere al fruitore di **confermare l'avvenuto recupero della risposta**.

## Modalità di scambio tra erogatore e fruitore

Il flusso di uno **scambio dati asincrono con callback** si articola nelle seguenti fasi:

1. **Avvio dell'interazione**: il fruitore effettua una richiesta accedendo all'e-service dell'erogatore.
2. **Segnalazione di disponibilità della risposta**: quando l'erogatore ha predisposto i dati, notifica il fruitore dell'avvenuta disponibilità della risposta invocando l'**API di callback** implementata e messa a disposizione da quest'ultimo.
3. **Recupero della risposta**: il fruitore, accedendo all'e-service dell'erogatore, provvede a scaricare la risposta. Se l'opzione è stata abilitata dall'erogatore, il recupero può avvenire a blocchi.
4. **Conferma del recupero**: se richiesto dall'erogatore, il fruitore invoca l'e-service per confermare di aver correttamente scaricato i dati.

> La **Piattaforma Digitale Nazionale Dati (PDND)** garantisce la sicurezza e la corretta orchestrazione del flusso assicurando:
> * La generazione di un **identificativo univoco dell'interazione** al momento della richiesta del voucher da parte del fruitore (nella fase di avvio).
> * L'emissione di **voucher dedicati e vincolati** per ciascuna fase, garantendone la corretta successione temporale (ad esempio, PDND non emetterà il voucher per il *recupero della risposta* se prima l'erogatore nnon ha richiesto il voucher per la *segnalazione di disponibilità*).


## Cosa deve fare l'Erogatore

Per configurare e pubblicare un e-service con **scambio di dati asincrono con callback**, l'erogatore deve impostare i seguenti parametri:

* **Informazioni generali**: selezionare la **modalità di scambio asincrono / massivo (in differita)**.
* **Specifiche tecniche**:
    * Registrare la specifica dell'API di callback che i fruitori dovranno implementare.
    * Indicare il **tempo massimo di risposta** entro cui si impegna a predisporre la risorsa.
    * Definire il **numero massimo di risultati per risposta** che si rende disponibile a produrre.
    * Specificare la **durata di disponibilità del dato** (per quanto tempo la risposta resterà salvata e accessibile).
* **Opzioni accessorie** (attivabili a scelta):
    * Consentire al fruitore di **recuperare la risposta a blocchi**.
    * Richiedere al fruitore l'obbligo di **confermare il recupero della risposta**.


> Le scelte effettuate nelle **specifiche tecniche** diventano immutabili una volta pubblicata la versione dell'e-service. Per modificarle, sarà necessario creare una **nuova versione**. 
> 
> La **modalità di scambio** complessiva dell'e-service non può più essere modificata dopo la pubblicazione della prima versione, così da garantire stabilità e coerenza nel tempo. Per cambiare modalità, sarà necessario creare un nuovo e-service e archiviare quello precedente. 



## Cosa deve fare il Fruitore

Per poter utilizzare un e-service basato su questa modalità, il fruitore deve:

1. **Implementare l'API di callback** secondo le specifiche tecniche indicate dall'erogatore.
2. **Comunicare l'endpoint**: in fase di richiesta del voucher per l'**avvio dell'interazione**, il fruitore deve specificare l'URL presso cui ha reso disponibile la propria API di callback.