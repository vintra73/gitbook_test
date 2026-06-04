# Scambi di dati asincroni con callback

## Che cosa sono e a cosa servono

Gli **scambi di dati asincroni con callback** permettono all'erogatore di pubblicare e-service per gestire, in modo standardizzato, tutti i casi in cui la predisposizione della risposta è un'**attività che richiede tempo**.

L'erogatore può: 
* Permettere al fruitore di **recuperare la risposta a blocchi (paginazione)**.
* Richiedere al fruitore di **confermare l'avvenuto recupero della risposta**.

## Modalità di scambio tra erogatore e fruitore

Il flusso di uno **scambio dati asincrono con callback** si articola nelle seguenti fasi:

1. **avvio dell'interazione**: il fruitore effettua una richiesta accedendo all'e-service dell'erogatore.
2. **segnalazione di disponibilità della risposta**: quando l'erogatore ha predisposto i dati, notifica al fruitore la disponibilità della risposta invocando l'**API di callback** messa a disposizione dal fruitore.
3. **recupero della risposta**: il fruitore, accedendo all'e-service dell'erogatore, provvede a scaricare la risposta. Se abilitato dall'erogatore, il recupero può avvenire a blocchi.
4. **conferma del recupero**: se richiesto dall'erogatore, il fruitore invoca l'e-service per confermare di aver correttamente scaricato i dati.

> La **Piattaforma Digitale Nazionale Dati (PDND)** garantisce la sicurezza e la corretta sequenza delle fasi previste, assicurando:
> * La generazione di un **identificativo univoco dell'interazione**, nella fasel **avvio dell'interazione**, al momento della richiesta del voucher da parte del fruitore.
> * L'emissione di **voucher dedicati e vincolati** per ciascuna fase, garantendone la corretta successione temporale (ad esempio, PDND non emetterà il voucher per il **recupero della risposta** se prima l'erogatore non ha richiesto il voucher per la **segnalazione di disponibilità**).


## Cosa deve fare l'Erogatore

Per configurare e pubblicare un e-service per **scambio di dati asincrono con callback**, l'erogatore deve impostare i seguenti parametri:

* **Informazioni generali**:
    * Impostare la **modalità di scambio** a **asincrono / massivo (in differita)**.
* **Specifiche tecniche**:
    * Registrare la specifica dell'**API di callback** che i fruitori devono implementare.
    * Indicare il **tempo massimo di risposta** entro cui si impegna a predisporre la risorsa.
    * Definire il **numero massimo di risultati per risposta** che si rende disponibile a produrre.
    * Specificare la **durata di disponibilità del dato**, per quanto tempo si impegna a rendere disponibile la risposta.
* **Opzioni accessorie** (attivabili a scelta):
    * Consentire al fruitore di **recuperare la risposta a blocchi**.
    * Richiedere al fruitore l'obbligo di **confermare il recupero della risposta**.


> Le scelte effettuate per le **Specifiche tecniche** e **Opzioni accessorie** diventano immutabili una volta pubblicata la versione dell'e-service. Per modificarle, sarà necessario creare una **nuova versione**. 
> 
> La **modalità di scambio** dell'e-service, definita nelle **Informazioni generali**, non può più essere modificata dopo la pubblicazione della prima versione, così da garantire stabilità e coerenza nel tempo. Per cambiare la **modalità di scambio**, sarà necessario creare un nuovo e-service e, se necessario, archiviare quello precedente. 



## Cosa deve fare il Fruitore

Per poter utilizzare un e-service con **scambio di dati asincrono con callback**, il fruitore deve:

* **Implementare l'API di callback**: 
    * Prima di avviare le interazione con l'erogatore, il fruitore deve implementare l'**API di callback** secondo le specifiche tecniche indicate dall'erogatore.
* **Comunicare l'endpoint della API di callback**: 
    * Il fruitore, nella richiesta del voucher per l'**avvio dell'interazione**, deve specificare l'URL presso cui rende disponibile la propria **API di callback**.
