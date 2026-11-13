# Payara Full & MariaDB Docker Environment

Ambiente di sviluppo e deployment containerizzato basato su **Docker Compose**, che integra un server **Payara Full** e un database **MariaDB**, configurato per gestire in modo sicuro le credenziali tramite variabili d'ambiente.

---

## 📋 Prerequisiti

* Docker e Docker Compose installati sulla macchina (VPS o locale).

---

## ⚙️ Configurazione delle Variabili d'Ambiente (.env)

Per motivi di sicurezza, le credenziali e i parametri sensibili non sono tracciati da Git (sono esclusi tramite `.gitignore`). 

Prima di avviare i container, devi creare un file denominato **`.env`** nella root del progetto (nella stessa cartella del `docker-compose.yml`).

Crea il file `.env` con il seguente contenuto, personalizzando i valori a seconda delle tue esigenze:

```env
# Credenziali MariaDB
DB_USER=tuo_utente_db
DB_PASSWORD=tua_password_db
DB_NAME=tuo_nome_db
DB_ROOT_PASSWORD=tua_password_root_db

# Credenziali Amministratore Payara
PAYARA_ADMIN_USER=admin
PAYARA_ADMIN_PASSWORD=tua_password_amministratore_payara
```

Queste variabili verranno lette automaticamente dal file `docker-compose.yml` e iniettate all'interno dei container di MariaDB e Payara al momento dell'avvio.

---

## 📦 Deploy delle Applicazioni Jakarta EE (Volume `webapps/`)

Nel file `docker-compose.yml`, la directory locale **`webapps/`** è montata come volume persistente direttamente nella cartella di autodeploy del container **Payara**.

* **Come pubblicare un'applicazione:** Per mettere online una nuova applicazione web Jakarta EE, è sufficiente copiare o caricare il relativo pacchetto **`.war`** all'interno della cartella `webapps/`.
* **Deploy automatico:** Payara monitora costantemente questo volume; non appena rileva un nuovo file `.war` (o un aggiornamento di un file esistente), esegue automaticamente il deploy e rende l'applicazione disponibile online senza dover riavviare i container.
* **Rimozione:** Eliminando il file `.war` dalla cartella `webapps/`, l'applicazione verrà automaticamente rimossa (undeploy) dal server.

---

## 🚀 Istruzioni per l'Avvio

1. **Posizionati nella cartella del progetto:**
   ```bash
   cd payarafull
   ```

2. **Assicurati di aver creato il file `.env`** (come descritto sopra).

3. **Inserisci i tuoi file `.war`** nella directory `webapps/` (se non già presenti).

4. **Avvia i container in background:**
   ```bash
   docker compose up -d
   ```

5. **Verifica lo stato o i log:**
   ```bash
   docker compose ps
   docker compose logs -f
   ```

---

## 🛑 Arresto dei Container

Per fermare i servizi mantenendo i dati del database persistenti:
```bash
docker compose down
```