tgbot

Un bot Python telegramma modulare in esecuzione su python3 con un database sqlalchemy.

Originariamente un semplice bot di gestione di gruppo con più funzionalità di amministrazione, si è evoluto, diventando estremamente modulare e semplice da usare.

Può essere trovato su Telegram come Marie .

Distribuisci

Marie ed io stiamo moderando un gruppo di supporto , dove puoi chiedere aiuto per configurare il tuo bot, scoprire / richiedere nuove funzionalità, segnalare bug e rimanere aggiornato ogni volta che è disponibile un nuovo aggiornamento. Ovviamente aiuterò anche quando lo schema di un database cambia e alcune colonne della tabella devono essere modificate / aggiunte. Nota ai manutentori che tutte le modifiche allo schema verranno trovate nei messaggi di commit, ed è loro responsabilità leggere ogni nuovo commit.

Unisciti al canale di notizie se vuoi solo rimanere aggiornato su nuove funzionalità o annunci.

In alternativa, trovami su Telegram ! (Mantieni tutte le domande di supporto nella chat di supporto, dove più persone possono aiutarti.)

AVVISO IMPORTANTE:

Questo progetto non è più in manutenzione attiva. Potrebbero essere rilasciate correzioni di bug occasionali, ma non è prevista l'aggiunta di nuove funzionalità. Gli utenti di Marie sono incoraggiati a migrare a PyroGramBot , che è la versione migliorata di questo progetto, scritta in PyroGram, con l'intenzione di evitare vari singhiozzi dell'API BOT e per proteggere le chat di gruppo da bot (utenti), inondazioni e persino perdenti senza vita.

Avvio del bot.

Una volta impostato il database e completata la configurazione (vedere di seguito), eseguire semplicemente:

python3 -m tg_bot

Configurazione del bot (leggere questo prima di provare a utilizzarlo!):

Assicurati di utilizzare python3.6, poiché non posso garantire che tutto funzioni come previsto sulle versioni precedenti di python! Questo perché l'analisi del markdown viene eseguita iterando attraverso un dict, che sono ordinati per impostazione predefinita in 3.6.

Configurazione

Esistono due modi possibili per configurare il tuo bot: un file config.py o variabili ENV.

La versione preferita è utilizzare un config.pyfile, poiché rende più facile vedere tutte le tue impostazioni raggruppate insieme. Questo file dovrebbe essere inserito nella tua tg_botcartella, insieme al __main__.pyfile. Da qui verrà caricato il token del bot, l'URI del database (se si utilizza un database) e la maggior parte delle altre impostazioni.

Si consiglia di importare sample_config ed estendere la classe Config, poiché ciò garantirà che la configurazione contenga tutti i valori predefiniti impostati in sample_config, rendendo quindi più facile l'aggiornamento.

Un config.pyfile di esempio potrebbe essere:

from tg_bot.sample_config import Config

class Development(Config):

    OWNER_ID = 254318997  # my telegram ID

    OWNER_USERNAME = "SonOfLars"  # my telegram username

    API_KEY = "your bot api key"  # my api key, as provided by the botfather

    SQLALCHEMY_DATABASE_URI = 'postgresql://username:password@localhost:5432/database'  # sample db credentials

    MESSAGE_DUMP = '-1234567890' # some group chat that your bot is a member of

    USE_MESSAGE_DUMP = True

    SUDO_USERS = [18673980, 83489514]  # List of id's for users which have sudo access to the bot.

    LOAD = []

    NO_LOAD = ['translation']

Se non puoi avere un file config.py (ad es. Su heroku), è anche possibile utilizzare variabili d'ambiente. Sono supportate le seguenti variabili env:

ENV: L'impostazione di QUALSIASI COSA abiliterà le variabili env

TOKEN: Il tuo token bot, come una stringa.

OWNER_ID: Un numero intero composto dal tuo ID proprietario

OWNER_USERNAME: Il tuo nome utente

DATABASE_URL: L'URL del tuo database

MESSAGE_DUMP: opzionale: una chat in cui vengono archiviati i messaggi salvati con risposta, per impedire alle persone di eliminare i vecchi

LOAD: Elenco di moduli separati da spazi che desideri caricare

NO_LOAD: Elenco separato da spazi di moduli che non si desidera caricare

WEBHOOK: L'impostazione di QUALSIASI COSA abiliterà i webhook quando ci sono messaggi in modalità env

URL: L'URL a cui il webhook deve connettersi (necessario solo per la modalità webhook)

SUDO_USERS: Un elenco separato da spazi di user_id che dovrebbero essere considerati utenti sudo

SUPPORT_USERS: Un elenco separato da spazi di user_id che dovrebbero essere considerati utenti di supporto (può gban / ungban, nient'altro)

WHITELIST_USERS: Un elenco separato da spazi di user_id che dovrebbero essere considerati nella whitelist - non possono essere banditi.

DONATION_LINK: Opzionale: link dove vorresti ricevere donazioni.

CERT_PATH: Percorso del certificato webhook

PORT: Porta da utilizzare per i tuoi webhook

DEL_CMDS: Se eliminare i comandi dagli utenti che non dispongono dei diritti per utilizzare quel comando

STRICT_GBAN: Applica gban sia ai nuovi gruppi che ai vecchi gruppi. Quando un utente gbannato parla, verrà bannato.

WORKERS: Numero di thread da utilizzare. 8 è l'importo consigliato (e predefinito), ma la tua esperienza potrebbe variare. Nota che impazzire con più thread non necessariamente velocizzerà il tuo bot, data la grande quantità di accessi ai dati sql e il modo in cui funzionano le chiamate asincrone di Python.

BAN_STICKER: Quale adesivo utilizzare quando si bandiscono le persone.

ALLOW_EXCL: Se consentire l'utilizzo di punti esclamativi! per i comandi e /.

Dipendenze Python

Installa le dipendenze python necessarie spostandoti nella directory del progetto ed eseguendo:

pip3 install -r requirements.txt.

Questo installerà tutti i pacchetti Python necessari.

Banca dati

Se desideri utilizzare un modulo dipendente dal database (ad esempio: serrature, note, info utente, utenti, filtri, benvenuto), dovrai avere un database installato sul tuo sistema. Uso postgres, quindi consiglio di usarlo per una compatibilità ottimale.

Nel caso di postgres, questo è il modo in cui imposteresti un database su un sistema debian / ubuntu. Altre distribuzioni possono variare.

installa postgresql:

sudo apt-get update && sudo apt-get install postgresql

passare all'utente postgres:

sudo su - postgres

crea un nuovo utente del database (cambia il TUO_UTENTE in modo appropriato):

createuser -P -s -e YOUR_USER

Questo sarà seguito dalla necessità di inserire la password.

creare una nuova tabella di database:

createdb -O YOUR_USER YOUR_DB_NAME

Cambia YOUR_USER e YOUR_DB_NAME in modo appropriato.

infine:

psql YOUR_DB_NAME -h YOUR_HOST YOUR_USER

Questo ti permetterà di connetterti al tuo database tramite il tuo terminale. Per impostazione predefinita, YOUR_HOST dovrebbe essere 0.0.0.0:5432.

Ora dovresti essere in grado di creare l'URI del tuo database. Questo sarà:

sqldbtype://username:pw@hostname:port/db_name

Sostituisci sqldbtype con il db che stai utilizzando (ad esempio postgres, mysql, sqllite, ecc.) Ripeti per il tuo nome utente, password, nome host (localhost?), Porta (5432?) E nome db.

Moduli

Impostazione dell'ordine di caricamento.

L'ordine di caricamento del modulo può essere modificato tramite le impostazioni di configurazione LOADe NO_LOAD. Entrambi dovrebbero rappresentare elenchi.

Se LOADè un elenco vuoto, tutti i moduli in modules/verranno selezionati per il caricamento per impostazione predefinita.

Se NO_LOADnon è presente, o è un elenco vuoto, verranno caricati tutti i moduli selezionati per il caricamento.

Se un modulo è in entrambi LOADe NO_LOAD, il modulo non verrà caricato - NO_LOADha la priorità.

Creare i propri moduli.

La creazione di un modulo è stata semplificata il più possibile, ma non esitate a suggerire ulteriori semplificazioni.

Tutto ciò che serve è che il tuo file .py si trovi nella cartella dei moduli.

Per aggiungere comandi, assicurati di importare il dispatcher tramite

from tg_bot import dispatcher.

Puoi quindi aggiungere comandi usando il solito

dispatcher.add_handler().

Assegnare la __help__variabile a una stringa che descrive i comandi disponibili di questo modulo consentirà al bot di caricarlo e aggiungere la documentazione per il modulo al /helpcomando. L'impostazione della __mod_name__variabile ti permetterà anche di usare un nome più carino e facile da usare per un modulo.

La __migrate__()funzione viene utilizzata per la migrazione delle chat: quando una chat viene aggiornata a un supergruppo, l'ID cambia, quindi è necessario migrarlo nel db.

La __stats__()funzione serve per recuperare le statistiche del modulo, ad es. Numero di utenti, numero di chat. Vi si accede tramite il /statscomando, disponibile solo per il proprietario del bot.
