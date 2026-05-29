# Scambi di dati asincroni con callback

## Che cosa sono e a cosa servono

Gli **scambi di dati asincroni con callback** permettono all'erogatore di pubblicare e-service che, in maniera standardizzata, permettono la gestione dei casi in cui la predisposizione della risposta è un **attività che richiede del tempo**.

L'erogatore può: 
- permette al fruitore di **recuperare la risposta a blocchi**;
- richiedere al fruitore di **confermare di aver recuperato la risposta**.

## Modalità di scambio tra erogatore e fruitore

Gli **scambi di dati asincroni con callback** prevedono le seguenti fasi:

1. **avvio interazione**: l'erogatore effettua una richiesta accedendo all'e-service del fruitore;
2. **segnalazione della disponibilità della risposta**: quando il fruitore predispone la risposta, avvisa il fruitore della disponibilità della risposta accedendo alla **API callback** predisposta da quest'ultimo; 
3. **recupero della risposta**: il fruitore accedendo all'e-service del fruitore, provvede a recuperare la risposta e, se abilitato dall'erogatore, recupera la stessa a blocchi;
4. **conferma del recupero della risposta**: se richiesto dall'erogatore, il fruitore accedendo all'e-service del fruitore, conferma il recupero della risposta.


La **Piattaforma Digitale Nazionale Dati (PDND)** assicura agli erogatori e fruitori di e-service con **scambio di dati asincrono con callback**:
- la generazione dell'**identificativo univoco della interazione** avviata, al momento della richiesta del voucher da parte del fruitore per la fase **avvio interazione**;
- l'emissione di voucher specializzati per ogni fase prevista e la corretta successione di queste (ad esempio non emette il voucher per la fase **recupero della risposta** se non è gia stato emesso un voucher per la fase **segnalazione della disponibilità della risposta**).

## Cosa deve fare l’Erogatore
L'erogatore per definire un e-service con **scambio di dati asincrono con callback**:

- nelle **informazioni generali** seleziona la **modalità di scambio asincrono / massivo (in differita)**;

- nelle **specifiche teniche** indica:
    - registra la specifica dell'API di callback che i fruitori devo implementare;
    - il **tempo massimo di risposta** entro cui predisporrà la risorsa;
    - il **numero massimo di risultati per risposta** che si rende a disponibile a produrre;
    - per quanto tempo rende disponibile la risposta indicando la **durata di disponibilità del dato**;

    e abilita o meno le opzioni per:

    - permette al fruitore di **recuperare la risposta a blocchi**;
    - richiedere al fruitore di **confermare di aver recuperato la risposta**.


Quanto indicato dall'erogattore nelle **specifiche teniche**, dopo la pubblicazione di una versione, sono immutabili. È comunque possibile creare una **nuova versione** dell'e-ervice per modificare tale scelte effettuate dall'erogatore.

La **modalità di scambio** di un'e-service resta invariata dopo la pubblicazione della prima versione dell’e-service, per garantire stabilità e coerenza del servizio nel tempo. È comunque possibile creare nuovi e-service e archiviare quelli non più utilizzati.

## Cosa deve fare il Fruitore
Il fruitore per utilizzare un e-service con **scambio di dati asincrono con callback** implementa la API di callback indicata dall'erogatore.

Il fruitore indica, nella richiesta di voucher per la fase **avvio interazione**, l'URL dove rendede disponibile la API di callback implementata. 
