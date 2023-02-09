# Übung 14: Security I + II

## Ziele
- Vertraulichkeit + Maßnahmen:
   - Daten können nicht von Außenstehenden gelesen werden.
   - Gesicherte Kanäle sollten etabliert werden (auch über unsichere Kommunikationsmedien).
   - Maßnahmen: Kryptographie bzw. Verschlüsselung
- Integrität + Maßnahmen:
   - Datenmanipulation ausschließen
   - Änderungen nachvollziehen können
   - Maßnahmen: Hashfunktionen
- Authentizität + Maßnahmen:
   - Datenursprung bzw. Sender sicherstellen
   - Verbindlichkeit (z.B. Vertragsunterzeichnungen) -> Nicht-Abstreitbarkeit
   - Maßnahmen: digitale Signaturen, Zertifikate, (H)MACs ((Hash-based) Message Authentication Code)
- Verfügbarkeit:
   - Funktionalität eines Systems gewährleisten
   - hier nicht betrachtet

## Kryptographie
- symmetrisch vs. asymmetrisch:
   - symm.: gleicher Schlüssel zum Ver- und Entschlüsseln
   - asymm.: unterschiedliche Schlüssel (public / private key)
- Anforderungen:
   - bei symm. Krypt.:
      - Schlüssel muss bekannt sein
      - Geheimtext darf keine Informationen zu Schlüssel bzw. Klartext preisgeben
      - Geheimtext möglichst "zufällig" verteilt (hohe Entropie: alle Zeichen gleichwahrscheinlich)
   - bei asymm. Krypt.:
      - private key nicht aus public key ableitbar
      - private key vor Zugriff schützen
- Grundkonzept: Kombination von symm. und asymm. Verfahren
   - Übertragen einen zufälligen symm. Schlüssel via asymm. Verfahrens
   - Gründe: Performance, Einfachheit, Nachrichtenlänge
- Beispiele
   - symm.: AES (Advanced Encryption Standard)
      - ursprünglich Rijndael
      - in Hardware implementiert (z.B. spezielle Prozessorinstruktionen)
      - Vorgänger: DES (Data Encryption Standard, teilweise als 3-DES genutzt)
   - asymm.: RSA (Rivest-Shamir-Adleman)
      - `m^e mod n` => Übertragung => `(m^e)^d mod n = m^(e * d) mod n = m` mit `(e, n)` als public key und `d` als private key
      - `e * d mod n = 1` (`e` und `d` sind inverse Elemente)
- Wie hilft Kryptographie bei der Erfüllung der Ziele?
      - Grundkonzept Vertraulichkeit: Verschlüsseln mit dem public key des Gegenübers, Entschlüsseln mit dem eigenen private key
      - Grundkonzept Authentizität: Verschlüsseln mit dem eigenen private Key, Entschlüsseln mit dem public key des Gegenübers => digitale Signatur (in der Praxis über einen verschlüsselten Hash)
      - Schlüsselinfrastruktur (PKI, Public Key Infrastructure) zur Verteilung der öffentlichen Schlüssel

## Hashfunktionen
- Definition
   - n: Digest Length, Hashlänge
   - Funktion, die Bitstrings beliebiger Länge auf Bitstrings fester Länge abbildet: `{0, 1}^m -> {0, 1}^n`, `n` fest, `m` variabel, typischerweise `n << m`
   - Injektivität (gleiche Bilder -> gleiche Urbilder)? Nicht möglich, da nur eine endliche Zahl unterschiedlicher Hashes existiert (`2^n` Stück) -> Kollisionen (gleiche Bilder für unterschiedliche Urbilder)
   - Surjektivität? Nicht essentiell, aber wünschenswert für gute Verteilung
   - Kryptographische Hashfunktionen:
      - Kollisionen zu finden, ist schwer!
      - Einweg- / Falltürfunktion: `h(x) = y` zu bilden, ist einfach, `h^-1(y) = x` zu bilden, ist schwer (Passwörter speichern, Stichwort: Salt)
      - Lawinen- / Chaoseffekt: Kleine Änderungen am Urbild erzeugen völlig unterschiedliches Bild (1 Bit in der Nachricht kippen -> im Optimalfall 50 gekippte Bits im Hashwert)
- Anforderungen
- Beispiele:
   - MD5: `n` = 128 Bit = 16 Byte
- Schwache vs. starke Kollisionsresistenz:
   - schwache Kollisionsresistenz: Es ist schwer, zu einem festen Urbild ein weiteres Urbild mit dem gleichen Hash zu finden.
   - starke Kollisionsresistenz: Es ist schwer, zwei Urbilder mit dem gleichen Hash zu finden.
- Geburtstagsparadoxon
   - schwache Variante: >= 253 Gäste für >= 50%
   - starke Variante: >= 23 Gäste für >= 50%
