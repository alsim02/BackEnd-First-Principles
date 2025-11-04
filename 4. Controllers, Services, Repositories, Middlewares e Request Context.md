# Appunti: Controllers, Services, Repositories, Middlewares e Request Context

## 1. Ciclo di vita della richiesta nel backend
- Quando il client invia una richiesta HTTP, questa arriva al server attraverso una porta.
- Il server riceve la richiesta e la smista tramite il routing a un handler specifico (controller).

## 2. Ruoli di Handlers (Controllers), Services e Repositories
- **Handler/Controller:**
  - Punto di ingresso logico per una richiesta specifica.
  - Riceve oggetti request e response.
  - Estrae dati dalla richiesta (query params, body, path params).
  - Fa il *deserialization* del payload JSON (se necessario).
  - Valida e trasforma i dati prima di passarli al service.
  - Decide il codice di risposta HTTP e invia la risposta al client.
  - Si occupa degli aspetti legati a HTTP (formati, codici, errori).

- **Service Layer:**
  - Contiene la logica di business pura, indipendente dal protocollo HTTP.
  - Riceve dati validati e trasformati dal controller.
  - Coordina chiamate ai repository (database) o altre operazioni come notifiche esterne.
  - Ideale per raggruppare più operazioni o orchestrare chiamate multiple.

- **Repository Layer:**
  - Responsabile delle interazioni con il database o sistemi di persistenza.
  - Costruisce e esegue query basate sui dati forniti dal service.
  - Deve avere responsabilità unica (es. un metodo per ottenere tutti i libri, un altro per uno specifico libro).
  - Restituisce dati grezzi al service.

## 3. Pipeline di elaborazione della richiesta
- Request arriva e viene assegnata a un handler tramite routing.
- Handler deserializza la richiesta, valida e trasforma i dati.
- Handler passa dati al service.
- Service elabora la logica e chiama repository se necessario.
- Repository esegue operazioni DB e restituisce dati.
- Service riceve dati, li elabora e fornisce risposta all’handler.
- Handler imposta codice e corpo della risposta e la invia al client.

## 4. Middleware: cosa sono e ruolo
- Funzioni che si inseriscono “in mezzo” nel ciclo di vita di una richiesta.
- Hanno accesso a request, response e a una funzione `next()` per passare il controllo.
- Possono modificare request/response, terminare la richiesta anticipatamente, o passare avanti.
- Usati per operazioni comuni tra molte API: logging, autenticazione, controllo cors, rate limiting, compressione, global error handling.

## 5. Perché usare i middleware
- Ridurre duplicazioni di codice evitando di ripetere logiche comuni in ogni handler.
- Disaccoppiare responsabilità e migliorare manutenibilità.
- Possibilità di eseguire operazioni di sicurezza, validazioni globali, trasformazioni comuni.

## 6. Ordine dei middleware
- L’ordine di esecuzione è critico: alcune operazioni devono avvenire prima (ad esempio CORS per bloccare dati non autorizzati).
- Solitamente: CORS → Logging → Autenticazione → Rate limiting → Routing → Handler → Error handling globale.
- Error handling globale deve essere l’ultimo middleware per poter intercettare errori ovunque.

## 7. Request Context
- Memorizza dati legati alla singola richiesta (es. user ID, permessi dopo autenticazione).
- Permette di condividere dati tra i vari middleware e handler senza passare esplicitamente parametri.
- Isolato per ogni richiesta, evita problemi di concorrenza e dati condivisi tra richieste diverse.
- Presente come concetto e implementazione in quasi tutti i framework backend moderni.

---
