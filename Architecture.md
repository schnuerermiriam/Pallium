# Pallium — Architektur (Stand November 2026, Konzeptphase)

Dieses Dokument beschreibt die Zielarchitektur des Prototyps. Es ist eine Spezifikation, kein Implementierungsstand.

## Grundsätze

1. **Zwischen den Versorgern.** Pallium steuert weder Patient noch Behandlung. Es hält und verbindet.
2. **Kontrolle beim Patienten.** Der Schlüssel zum Gesprächsspeicher liegt ausschließlich beim Patienten. Weder Betreiber noch Einrichtung noch Behandelnde können Gesprächsinhalt lesen.
3. **Inhaltsfreier Alarm.** Nach außen gelangen keine Gesprächsinhalte, nur Zustandscodes. Menschen werden alarmiert, nicht informiert.
4. **Harte Schnittstellen.** Vier getrennte Komponenten mit getrennten Systembenutzern; Kommunikation nur über definierte Schnittstellen.
5. **Textbasiert.** Der Prototyp arbeitet mit Text. Sprachein- und -ausgabe sind als spätere Erweiterung an der Schnittstelle vorgesehen.

## Komponenten

### 1. Gesprächsdienst

- Lokales Sprachmodell mit offenen Gewichten über llama.cpp oder vLLM; Modell wird zu Beginn gewählt und für die gesamte Begleitung eines Patienten gepinnt (keine stillen Modellwechsel)
- **Arbeitsgedächtnis:** vollständiger Gesprächsverlauf im Original; Suchindex für den Abruf; keine verlustbehaftete Zusammenfassung als einzige Quelle
- Gesprächsführung nach Ansätzen aus Palliative Care, Seelsorge und Psychoonkologie; kein Kippen in Ratschläge, keine medizinischen Empfehlungen
- Einziger Prozess, der Klartext hält

### 2. Verschlüsselter Speicher

- Append-only; keine Löschung oder Änderung bestehender Einträge durch das System
- libsodium (XChaCha20-Poly1305); Schlüssel ausschließlich beim Patienten
- Betreiber hält nur Chiffrat; Backups verschlüsselt; Export für den Patienten jederzeit
- **Vertrauensgrenze im Bedrohungsmodell:** Klartext existiert zur Laufzeit im Prozess des Gesprächsdienstes. Confidential Computing ist Ausbauperspektive, nicht Teil des Prototyps
- **Wiederherstellungsnachweis (Meilenstein 2):** nach simuliertem Ausfall sind letzter bestätigter Beitrag, eine frühe Aussage und ihre spätere Korrektur im Original abrufbar; Weiterarbeit mit derselben Modellversion

### 3. Sicherheitsschicht

- Eigener Prozess, eigener Systembenutzer; Kommunikation mit dem Gesprächsdienst nur über Unix-Sockets; seccomp-Profil
- Empfängt ausschließlich Zustandscodes (Enum, kein Text)
- Löst einstellbare Notrufketten aus: Matrix-Nachricht, SMS oder Anruf via SIP, in Einrichtungen das Rufsystem (ESPA 4.4.4)
- Keine automatische Alarmierung von 112; die Kette endet immer bei einem Menschen, den der Patient oder die Einrichtung bestimmt hat
- Schützt den Gesprächsraum: keine Fremdzugriffe, keine Mitlese-Funktionen

### 4. Außenschicht

- Schnittstellenspezifikation für Sensorik, Drittanwendungen und Krankenhaus-/Arztinformationssysteme (KIS/AIS) über HL7 FHIR R4 und HL7 v2
- Im Förderzeitraum: nur Spezifikation und ein Referenzadapter, keine fertige Anbindung
- Herkunftsregel: Was von außen kommt, wird als von außen kommend gekennzeichnet; nichts aus dem Gesprächsraum geht ungefragt hinaus

## Betrieb

- **Entwicklung, KI-Labor, erste Tests mit gesunden Freiwilligen:** GreenTEC Campus Enge-Sande (Innovationspark, CO₂-neutrales Rechenzentrum), Debian, GPU-Server
- **Betrieb mit Patienten (nach dem Förderzeitraum):** verantwortlicher Betreiber; Primärstandort Schleswig-Holstein in einem zertifizierten Rechenzentrum (ISO 27001, TÜViT TSI, C5); georedundante verschlüsselte Sicherung in Dänemark; Anbieter offen
- **Ausbauoption:** lokaler Server in Hospizen, Pflegeheimen und Krankenhäusern
- Kein Cloud-Dienst

## Teststufen

| Stufe | Zeitraum | Gruppe | Substrat |
|---|---|---|---|
| 0 | Jan–Mai 2027 | gesunde Freiwillige, 8–12 Personen | Fremd-API, synthetische Fälle |
| 1 | Jun–Nov 2027 (Förderzeitraum) | dieselbe Gruppe | eigenes Substrat, Enge-Sande |
| 2 | danach, mit Ethikvotum | körperlich Kranke ohne Todesdiagnose | zertifiziertes RZ |
| 3 | später | Geriatrie, Onkologie, Palliativ | RZ, optional lokaler Server |

Ausschlusskriterien früher Stufen sind Stufung, keine Zielgruppenentscheidung: Wer in Stufe 0–2 ausgeschlossen ist, weil Alarmkette und Fürsprache noch nicht validiert sind, ist in Stufe 3 die eigentliche Zielgruppe.

## Was im Prototyp nicht enthalten ist

- Sprachein-/-ausgabe
- Confidential Computing
- Fertige KIS/AIS-Anbindung
- Betrieb mit Patienten
- Medizinprodukt-Zulassung (Pallium ist kein Medizinprodukt)

## Repository-Struktur (geplant)

```
/spec        Schnittstellenspezifikationen (FHIR, HL7 v2, Zustandscodes, Notrufketten)
/threat      Bedrohungsmodell, Vertrauensgrenzen
/service     Gesprächsdienst
/store       Verschlüsselter Speicher
/guard       Sicherheitsschicht
/edge        Außenschicht, Referenzadapter
/tests       Testsuite (pytest), Wiederherstellungstest
/docs        Protokolle, Testprotokolle, Ethik-Unterlagen (öffentlicher Teil)
```
