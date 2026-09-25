# AQ Coaching — Training System

Prima versione reale della piattaforma: autenticazione, area coach, area cliente, tutto collegato al database Supabase già configurato (`aq-coaching`, progetto `ybeuiwttzzpiuussufqf`).

## Cosa fa già

- **Login / registrazione coach / inviti clienti** con password reale, recupero password incluso
- **Dashboard coach**: clienti attivi, check scaduti/da compilare/ricevuti, sedute recenti
- **Gestione clienti**: creazione con invito via link, scheda cliente, note private (mai visibili al cliente)
- **Programmazione**: crei settimane con giorni, esercizi e serie programmate individualmente (reps/RIR/recupero per singola serie)
- **Allenamento lato cliente**: apre il giorno, registra carico/reps/RIR/note serie per serie, vede l'ultima prestazione e il badge "progressione disponibile"
- **Check periodico**: il cliente compila peso e feedback soggettivo, il coach lo revisiona e risponde
- **Storico prestazioni** per esercizio, lato cliente
- Tutta la sicurezza (RLS) e i vincoli (settimana bloccata dopo l'inizio, audit delle correzioni) sono già nel database, non nel codice del sito — quindi valgono comunque anche se in futuro si aggiungono altre interfacce

## Aggiornamento: tutte le parti erano incluse, ora sono complete

- **Libreria esercizi AQ** (`/coach/library`): aggiungi un esercizio una volta, poi richiamalo per nome (autocompletamento) ovunque costruisci una programmazione — viene collegato automaticamente, non duplicato come testo libero
- **Template** (`/coach/templates`): crei una struttura tipo (giorni/esercizi/serie) una volta, poi la duplichi per ogni nuovo cliente dalla pagina "Nuova settimana" senza riscrivere nulla
- **Foto nel check**: il cliente carica frontale/laterale/posteriore, private (storage dedicato, mai pubbliche); tu le vedi direttamente nella pagina di revisione del check
- **Comunicazioni** (`/coach/communications`): scrivi un messaggio a un cliente specifico o a tutti, vedi quanti l'hanno letto — ogni cliente ha il proprio stato di lettura
- **Documenti** (`/coach/documents` e, lato cliente, `/app/documents`): carichi un PDF/file, generale o per un cliente specifico, lui lo apre dalla sua area

Tutto scritto sopra la stessa base dati e sicurezza già configurate — nessuna delle parti precedenti è stata riscritta.

## Cosa resta volutamente fuori da questa v1

- Pagamenti (le tabelle nel database sono già pronte per quando vorrai attivarli)
- CRM/lead (stessa cosa)
- Notifiche push native (per ora tutto è "notifica interna" visibile aprendo l'app)

---

## Come metterlo online (passo passo)

### 1. Il database è già pronto
Non devi fare nulla su Supabase: tabelle, sicurezza e permessi sono già a posto.

### 2. Carica il codice su GitHub
1. Vai su **github.com**, crea un account gratuito se non ce l'hai
2. Crea un nuovo repository (es. `aq-coaching`), **privato**
3. Carica dentro tutti i file di questa cartella (puoi trascinarli dall'interfaccia web di GitHub, oppure usare Git da terminale se preferisci)

### 3. Crea un account Vercel
1. Vai su **vercel.com** → "Sign Up" → scegli **"Continue with GitHub"** (così sono già collegati)
2. Una volta dentro, clicca **"Add New" → "Project"**
3. Seleziona il repository `aq-coaching` che hai appena caricato

### 4. Imposta le variabili d'ambiente
Durante l'importazione (o dopo, in **Project Settings → Environment Variables**), aggiungi queste due, prendendole dal file `.env.local.example` incluso qui dentro:

```
NEXT_PUBLIC_SUPABASE_URL=https://ybeuiwttzzpiuussufqf.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=(il valore lungo che inizia con eyJ..., dentro .env.local.example)
```

Poi clicca **Deploy**. In 1-2 minuti il sito è online con un link tipo `aq-coaching.vercel.app`.

### 5. Un'impostazione importante su Supabase (per far funzionare login/reset password)
Vai su **supabase.com/dashboard/project/ybeuiwttzzpiuussufqf/auth/url-configuration** e imposta:
- **Site URL**: il link del tuo sito Vercel (es. `https://aq-coaching.vercel.app`)
- **Redirect URLs**: aggiungi lo stesso link seguito da `/**` (es. `https://aq-coaching.vercel.app/**`)

Senza questo passaggio, i link di reset password e conferma email puntano al posto sbagliato.

### 6. Crea il tuo account coach
Apri il sito online, vai su **"Crea il tuo account"** nella pagina di login, inserisci i tuoi dati. Diventi automaticamente il coach amministratore.

### 7. Crea il primo cliente
Dalla tua dashboard → Clienti → Nuovo cliente. Ti verrà generato un link da mandare al cliente: aprendolo, il cliente imposta la propria password ed entra direttamente nella sua area.

---

## Sviluppo in locale (facoltativo)

Se in futuro vuoi lavorarci con Claude Code o un editor:

```bash
npm install
cp .env.local.example .env.local
npm run dev
```

Apri `http://localhost:3000`.
