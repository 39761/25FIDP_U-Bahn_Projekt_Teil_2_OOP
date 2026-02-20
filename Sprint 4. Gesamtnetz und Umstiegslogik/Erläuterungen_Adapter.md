### Erläuterungen zur Umsetzung (Technische Dokumentation)

Um die Kompatibilität mit dem zentralen Testskript zu gewährleisten, muss jede Gruppe eine Datei namens `adapter.py` im Hauptverzeichnis ihres Projekts bereitstellen. Diese Datei fungiert als Brücke zwischen eurem individuellen Code und der automatisierten Prüfung.

#### 1. Struktur der Adapter-Datei
Erstellt im Modul `adapter.py` eine Klasse namens `adapter_klasse`. Diese Klasse muss eine zentrale Methode besitzen, die vom Testskript aufgerufen wird. Das Grundgerüst sieht wie folgt aus:

```python
from datetime import time, timedelta
from typing import List, Dict, Union

# Hier eure eigenen Module importieren, z.B.:
# from mein_projekt import u_bahn_logik

class adapter_klasse:
    def __init__(self):
        """Initialisierung eurer Logik-Klasse"""
        # self.logik = u_bahn_logik()
        pass

    def ausfuehren_testfall(self, 
                           eingabe_start: str, 
                           eingabe_ziel: str, 
                           eingabe_startzeit: str, 
                           eingabe_einzelfahrkarte: bool, 
                           eingabe_sozialrabatt: bool, 
                           eingabe_barzahlung: bool) -> dict:
        """
        Übersetzt die Testskript-Eingaben für euren Code und 
        liefert die Ergebnisse im Zielformat zurück.
        """
        
        # INTERNE VERARBEITUNG:
        # Hier ruft ihr eure Funktionen auf und wandelt die Eingaben um.
        
        # RÜCKGABE:
        # Das Dictionary muss exakt die Keys aus der Spezifikation enthalten.
        return {
            "fehler": False,
            "ausgabe_startzeit_fahrgast": time(0, 0),
            "ausgabe_zielzeit_fahrgast": time(0, 0),
            "ausgabe_startzeit_algo": time(0, 0, 0),
            "ausgabe_zielzeit_algo": time(0, 0, 0),
            "bahnlinien_gesamtfahrt": [],
            "route": {},
            "umstieg_haltestellen": [],
            "umstiege_exakt": {},
            "umstiege_fahrgast": {},
            "umstieg_bahnlinien": [],
            "dauer_gesamtfahrt": timedelta(0),
            "preis_endbetrag": 0.0
        }
```

#### 2. Wichtige Implementierungshinweise

* **Schnittstellentreue:** Die Namen der Variablen (Keys) im zurückgegebenen Dictionary müssen exakt so geschrieben werden wie in der Spezifikation oben. Ein Tippfehler führt zum Scheitern des automatisierten Tests.
* **Zeitformate:**
    * Verwendet für Uhrzeiten den Datentyp `datetime.time`.
    * Verwendet für die `dauer_gesamtfahrt` den Datentyp `datetime.timedelta`.
    * Achtet bei der `ausgabe_zielzeit_fahrgast` auf die Rundungsregeln (aufgerundet auf die volle Minute).
* **Daten-Mapping:** Eure interne Verarbeitung muss in der Lage sein, die String-Eingabe der Haltestellen (z. B. "Langwasser Süd") auf eure interne Datenbank-Struktur abzubilden.
* **Fehlerbehandlung:** Wenn eine Route nicht gefunden werden kann oder die Eingaben ungültig sind, darf der Adapter nicht abstürzen. Setzt in diesem Fall `fehler = True` und füllt die restlichen Felder mit Standardwerten (0 oder leere Listen).



#### 3. Bewertungsrelevanz

* **Automatisierte Prüfung:** Der Adapter wird automatisiert gegen die bereitgestellten Test-Tabellen (Preisliste und Fahrplan-Szenarien) geprüft.
* **Abnahmekriterium:** Ein korrekt funktionierender Adapter, der alle Testfälle valide durchläuft, ist Voraussetzung für die Abnahme der Sprints durch den Kunden.
