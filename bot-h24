import time
import requests
import datetime
import sys

# Configurazione del Bot H24 e Telegram
CHECK_INTERVAL_SECONDS = 15
TARGET_API_URL = "https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=eur"

# DATI CONFIGURATI CON SUCCESSO:
TELEGRAM_BOT_TOKEN = "9258102430:AAFO_KmxUAxjpWqr4TKxLH7E-CvotmSh4g8"
CHAT_ID = "9258102430"

def log_message(message):
    """Stampa un messaggio formattato con timestamp nei log."""
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    print(f"[{timestamp}] {message}")
    sys.stdout.flush()

def send_telegram_message(message):
    """Invia una notifica direttamente sul tuo smartphone tramite Telegram."""
    if TELEGRAM_BOT_TOKEN == "INSERISCI_IL_TUO_TOKEN_QUI":
        return
    
    url = f"https://api.telegram.org/bot{TELEGRAM_BOT_TOKEN}/sendMessage"
    payload = {
        "chat_id": CHAT_ID,
        "text": f"🤖 [Bot H24]\n{message}"
    }
    try:
        requests.post(url, json=payload, timeout=5)
    except Exception as e:
        log_message(f"Errore invio notifica Telegram: {e}")

def run_automation_cycle(cycle_count):
    log_message(f"Avvio ciclo operativo #{cycle_count}...")
    try:
        response = requests.get(TARGET_API_URL, timeout=5)
        if response.status_code == 200:
            data = response.json()
            btc_price = data.get('bitcoin', {}).get('eur', 0)
            log_message(f"Connessione API riuscita. Prezzo Bitcoin attuale: € {btc_price:,.2f}")
            if cycle_count % 10 == 0:
                send_telegram_message(f"Report Ciclo #{cycle_count}: Prezzo BTC € {btc_price:,.2f} - Sistema Operativo OK.")
        else:
            log_message(f"Attenzione: Risposta API con codice anomalo {response.status_code}")
    except Exception as e:
        log_message(f"Errore di rete: {e}")

def main():
    log_message("=== AVVIAMENTO BOT H24 IN CORSO ===")
    send_telegram_message("🚀 Il tuo Bot H24 è stato avviato con successo ed è in ascolto!")
    cycle_count = 0
    try:
        while True:
            cycle_count += 1
            run_automation_cycle(cycle_count)
            time.sleep(CHECK_INTERVAL_SECONDS)
    except KeyboardInterrupt:
        send_telegram_message("🛑 Il Bot H24 è stato arrestato.")

if __name__ == "__main__":
    main()
