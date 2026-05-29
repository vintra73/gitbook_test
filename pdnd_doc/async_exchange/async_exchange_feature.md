# Scambi di dati asincroni con callback

## Che cosa sono e a cosa servono

Gli **scambi di dati asincroni con callback** permettono all'erogatore di pubblicare e-service che, in maniera standardizzata, permettono la gestione dei casi in cui la predisposizione della risposta è un **attività che richiede del tempo**.

L'erogatore può: 
- permette al fruitore di **recuperare la risposta a blocchi**;
- richiedere al fruitore di **confermare di aver recuperato la risposta**.

## Modalità di scambio tra erogatore e fruitore

Gli **scambi di dati asincroni con callback** prevedono le seguenti fasi:

- **avvio interazione**: l'erogatore effettua una richiesta accedendo all'e-service del fruitore;
- **segnalazione della disponibilità della risposta**: quando il fruitore predispone la risposta, avvisa il fruitore della disponibilità della risposta accedendo alla **API callback** predisposta da quest'ultimo; 
- **recupero della risposta**: il fruitore accedendo all'e-service del fruitore, provvede a recuperare la risposta e, se abilitato dall'erogatore, recupera la stessa a blocchi;
- **conferma del recupero della risposta**: se richiesto dall'erogatore, il fruitore accedendo all'e-service del fruitore, conferma il recupero della risposta.

## Cosa deve fare l’Erogatore
L'erogatore alla definizione di un e-service:

- nelle **informazioni generali** seleziona la modalità di scambio **asincrono / massivo (in differita)**.

- nelle **specifiche teniche** indica:
    - registra la specifica dell'API di callback che i fruitori devo implementare;
    - il **tempo massimo di risposta** entro cui predisporrà la risorsa;
    - il **numero massimo di risultati per risposta** che si rende a disponibile a produrre;
    - per quanto tempo rende disponibile la risposta indicando la **durata di disponibilità del dato**;

    e abilita o meno le opzioni per:

    - permette al fruitore di **recuperare la risposta a blocchi**;
    - richiedere al fruitore di **confermare di aver recuperato la risposta**.

## Cosa deve fare il Fruitore
Il fruitore per utilizzare un e-service con **scambio di dati asincrono con callback** deve implemantare la API di callback indicata dall'erogatore.
