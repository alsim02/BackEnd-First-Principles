# Appunti: Cos'è il Routing

## 1. Cos'è il routing
- Processo che mappa le richieste HTTP a specifiche funzioni o handler sul server.
- Esprime il **dove** della richiesta, cioè quale risorsa o funzionalità deve essere attivata in risposta.

## 2. La relazione tra metodo e route
- Le richieste sono composte da un **metodo HTTP** (GET, POST, PUT, DELETE, etc.) e una **route** (URL/path).
- Questi due elementi formano una coppia unica che determina quale handler viene triggerato.
- Esempio: 
  - GET /api/books
  - POST /api/books
- Combinazione di metodo e route sono le chiavi di routing, rendendo ogni richiesta univoca.

## 3. Tipi di route
### Statiche
- Route fisse e costanti, esempio: `/api/books`.
- Non hanno variabili o parametri. Sempre uguali, sempre restituiscono gli stessi dati.

### Dinamiche (Variabili)
- Route con parametri variabili, esempio: `/api/users/:id`.
- Si usa per risposte specifiche, come dettaglio di un utente con ID variabile.
- L'handler può estrarre il parametro dalla route e usarlo perquery o operazioni.

### Routing annidato (Nested)
- Percorsi profondi che concatenano risorse, esempio: `/api/users/123/posts/456`.
- Rispecchiano la gerarchia e la relazione tra risorse.
- Utili per API REST complesse e semantiche.

### Versioning
- Aggiunta di una versione nella route, esempio: `/api/v1/products`, `/api/v2/products`.
- Permette di evolvere l'API senza rompere la compatibilità con clienti o app.

### Catch-all
- Route che cattura tutte le richieste non gestite preventivamente.
- Usata per rispondere con errori amichevoli o fallback: `/api/*`.
- Essenziale per gestione degli errori di route non trovata.

## 4. Come funziona il routing nel server
- Quando arriva una richiesta, il server:
  - Legge metodo e route.
  - Cerca una corrispondenza tra le route indicate e le route definite.
  - Se trovala, esegue il relativo handler.
  - Se no, può applicare un catch-all o risposta di errore "route non trovata".

## 5. Parametri di route e query
- **Path/Route parameters:** variabili incorporate nella URL, esempio: `/users/:id`.
- **Query parameters:** key-value pairs dopo il `?`, esempio: `/search?query=abc&page=2`.
- Usate per passare specifiche informazioni o filtri nell'API.
