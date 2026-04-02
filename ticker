import requests
import time
from datetime import datetime

API_KEY = "DEIN_TANKERKOENIG_API_KEY"  # hier deinen Key eintragen

# Wendelstein (ca. PLZ 90530)
LAT = 49.352   # grobe Koordinate
LNG = 11.150
RADIUS_KM = 10
FUEL_TYPE = "e10"   # e5, e10, diesel
SORT = "price"      # nach Preis sortieren

def fetch_prices():
    url = "https://creativecommons.tankerkoenig.de/json/list.php"
    params = {
        "lat": LAT,
        "lng": LNG,
        "rad": RADIUS_KM,
        "sort": SORT,
        "type": FUEL_TYPE,
        "apikey": API_KEY,
    }
    r = requests.get(url, params=params, timeout=10)
    data = r.json()

    if not data.get("ok"):
        print("Fehler von der API:", data.get("message"))
        return []

    return data.get("stations", [])

def print_ticker():
    stations = fetch_prices()
    if not stations:
        print("Keine Daten erhalten.")
        return

    print("\n=== E10-Preise rund um 90530 (±10 km) ===")
    print("Stand:", datetime.now().strftime("%d.%m.%Y %H:%M:%S"))
    for s in stations:
        price = s.get("price")
        if price is None:
            continue
        name = s.get("name")
        brand = s.get("brand") or ""
        dist = s.get("dist")
        street = s.get("street")
        place = s.get("place")
        print(f"{price:4.2f} €  | {brand} {name} | {street}, {place} | {dist:.1f} km")

if __name__ == "__main__":
    # alle 5 Minuten aktualisieren
    while True:
        print_ticker()
        print("-" * 60)
        time.sleep(5 * 60)
