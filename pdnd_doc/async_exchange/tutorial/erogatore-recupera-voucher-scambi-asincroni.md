# Come richiedere un voucher Bearer per la API callback di un fruitore per scambi di dati asincroni

Il contesto di applicazione del presente tutoriale sono gli [**scambi di dati asincroni con callback**](<INSERT URL:riferimenti-tecnici/e-service>).

## Il flusso in breve
In sostanza, il processo end-to-end richiede cinque passaggi:

1. l'erogatore genera la client assertion;
2. l'erogatore chiede il voucher al server autorizzativo di PDND;
3. il server autorizzativo di PDND effettua le verifiche necessarie. In caso di esito positivo, restituisce un voucher;
4. l'erogatore fa una richiesta verso l'API callback del fruitore; inserisce il voucher rilasciato da PDND Interoperabilità nell'header Authorization;
5. il fruitore effettua le verifiche necessarie. In caso di esito positivo, elabora la richiesta dell'erogatore.

## Prerequisiti
Si assume che il fruitore abbia:

- associato un portachiavi all'e-service ([vedi tutorial](<INSERT URL:tutorial/tutorial-per-lerogatore/come-associare-un-portachiavi-ad-un-e-service>)).


## Step 1: Generazione della client assertion
Il primo passo è costruire una client assertion valida. La client assertion è composta da un header e un payload, contenenti i seguenti campi.

Header:

| Nome campo | Significato |
| --- | --- |
| kid | l'id della chiave che si usa per firmare l'asserzione, reperibile su PDND Interoperabilità |
| alg | l'algoritmo usato per firmare il JWT (per ora, sempre RS256) |
| typ | il tipo di oggetto che si sta inviando (sempre JWT) |

Payload:
| Nome campo | Significato |
| --- | --- |
| iss | l'issuer, in questo caso il clientId |
| sub | il subject, in questo caso sempre il clientId |
| aud | l'audience, reperibile su PDND Interoperabilità |
| jti | il JWT ID, un id unico random assegnato da chi vuole creare il token, si usa per tracciare il token stesso. Deve essere cura del chiamante assicurarsi che l'id di questo token sia unico per quanto riguarda la client assertion |
| iat | l'issued at, il timestamp riportante data e ora in cui viene creato il token, espresso in [UNIX epoch](https://datatracker.ietf.org/doc/html/rfc3339) (valore numerico, non stringa)|
| exp | l'expiration, il timestamp riportante data e ora di scadenza del token, espresso in [UNIX epoch](https://datatracker.ietf.org/doc/html/rfc3339) (valore numerico, non stringa) |
| purposeId | l'id della singola finalità per la quale si vuole ottenere un voucher, disponibile sul front office | 
| scope | la fase dell'interazione in essere, in questo caso sempre *callback_invocation* |
| entityNumber | il nuemro di entità presenti nella risposta |
| interactionID | riporta l'*interactionID* in essere generato dal server autorizzativo di PDND |

Si ricorda che il fruitore recupera e memorizza l'*interactionID* in essere presente nel voucher con *scope* = *start_interaction* che ha autorizzato l'avvio dell'interazione.

A titolo esemplificativo, di seguito un esempio di contenuto di client assertion deserializzata, in modo da evidenziarne il contenuto.

Header:
```
{
  "alg": "RS256",
  "kid": "2MJFa7aSSveFte8ULX9U-MaaygcoL5fBIJDTXBdba64",
  "typ": "jwt"
}
```

Payload:
```
{
  "iss": "8e9f24ca-78f5-4c69-9e4f-0efbeac7bb2b", 
  "sub": "8e9f24ca-78f5-4c69-9e4f-0efbeac7bb2b",
  "aud": "auth.interop.pagopa.it/client-assertion",
  "jti": "23387ac1-c192-4573-8350-207a4213d4be",
  "iat": 1616170068,
  "exp": 1616170668,
  "purposeId": "34f1624b-91cb-4b05-b8c0-cad208a30222",
  "scope": "callback_invocation",
  "entityNumber": 100;
  "interactionID": "123e4567-e89b-12d3-a456-426614174000"
}

```

Dopo aver costruito una client assertion valida, questa deve essere firmata con la propria chiave privata (che deve essere l'omologa della chiave pubblica depositata nel portachiavi associato all'e-service).

È inoltre disponibile una funzione per verificare la validità della propria client assertion ed evidenziare eventuali errori. Lo strumento è disponibile nel front office su **Tool per lo sviluppo > Debug client assertion**.

## Step 2: Richiedere il voucher al server autorizzativo
Il secondo passaggio è chiamare il server autorizzativo di PDND Interoperabilità con la client assertion firmata per ottenerne in cambio un voucher spendibile presso le API di callback del fruitore.

L'URL dell'endpoint alla quale si trova il server autorizzativo cambia in funzione dell'ambiente in cui ci si trova e sarà chiaramente visibile sull'interfaccia all'interno del front office.

L'endpoint andrà chiamato con alcuni parametri nel body:

| Nome campo | Significato |
| --- | --- |
| client_id | di nuovo il clientId usato nell'assertion |
| client_assertion | il contenuto dell'asserzione firmata nel primo passaggio |
| client_assertion_type | il formato della client assertion, come indicato in RFC (sempre urn:ietf:params:oauth:client-assertion-type:jwt-bearer) |
| grant_type | la tipologia di flusso utilizzato, come indicato in RFC (sempre client_credentials) |

## Step 3: Il server autorizzativo verifica, e rilascia il voucher
Se tutto è impostato correttamente, PDND Interoperabilità risponderà con un voucher valido all'interno del body della risposta alla proprietà **access_token**.

Sempre nella risposta, sarà contenuta anche la durata di validità del voucher in secondi (es. **"expires_in": 600** indica un voucher con validità 10 minuti, 10 * 60 secondi = 600). La durata del voucher è scelta dall'erogatore sulla base delle proprie considerazioni di sicurezza, e può variare da un e-service all'altro.

La risposta che il server autorizzativo di PDND Interoperabilità restituisce è la seguente:

```
{
  "access_token": "eyJ0eXAiOiJhdCtqd3QiLC...",
  "expires_in": 600
}
```

Se decodifichiamo il campo dedicato all'access_token, troviamo

Header:
```
{
  "typ": "at+jwt",
  "alg": "RS256",
  "kid": "{KID_CHIAVE_PDND}"
}
```

Payload:
```
{
  "iss": "interop.pagopa.it", 
  "nbf": 1747408537,
  "iat": 1747408537,
  "exp": 1747409537,
  "jti": "12297ac1-c192-4573-8350-207a4213e5ac",
  "aud": "https://eservice.pa.it/api/v1",
  "sub": "9b361d49-33f4-4f1e-a88b-4e12661f2309",
  "client_id": "9b361d49-33f4-4f1e-a88b-4e12661f2309",
  "purposeId": "1b361d49-33f4-4f1e-a88b-4e12661f2300",
  "producerId" : "0e9e2dab-2e93-4f24-ba59-38d9f11198ca",
  "consumerId" : "69e2865e-65ab-4e48-a638-2037a9ee2ee7",
  "eserviceId" : "b8c6d7ad-93fc-4eaf-9018-3cd8bf98163f",
  "descriptorId": "9525a54b-9157-4b46-8976-ec66f20b7d7e",
  "scope": "callback_invocation",
  "entityNumber": 100;
  "interactionID": "123e4567-e89b-12d3-a456-426614174000"
}

```

## Step 4: Invocare la API di callback del fruitore 
Il voucher andrà inserito nell'header della chiamata successiva verso la API di callback del fruitore. Andrà inserito nell'header di **Authorization**, come segue:

```
Authorization: Bearer <voucher>
```

Si ricorda che il fruitore recupera l'*urlCallback* per richiamare la API di callback nel voucher con *scope* = *start_interaction* che ha autorizzato l'avvio dell'interazione.


## Step 5: Attendere le verifiche del fruitore
Il fruitore effettua tutte le verifiche necessarie. Se tutto è in ordine, elabora la richiesta dell'erogatore.

Il fruitore esegue le verifiche indicate nella [sezione dedicata](<INSERT URL:tutorial/tutorial-per-lerogatore/come-verificare-la-validita-di-un-voucher-bearer>), a cui si aggiungono alle **Verifiche sul payload**:
 - **scope**: lo scope che deve essere uguale a **start_interaction**;
 - **interactionID**: deve essere presente;
 - **entityNumber**: deve essere presente e maggiore di 0.

# Come richiedere un voucher DPoP per la API callback di un fruitore per scambi di dati asincroni
Per richiedere un voucher DPoP per la API di un erogatore per scambi di dati asincroni si applica quando indicato per normali e-service, si veda la [sezione dedicata](<INSERT URL:tutorial/tutorial-per-il-fruitore/come-richiedere-un-voucher-dpop-per-le-api-di-un-erogatore-base>), fatta salva la generazione della client assertion nei modi indicati al precedente [**Step 1: Generazione della client assertion**](#step-1-generazione-della-client-assertion).

