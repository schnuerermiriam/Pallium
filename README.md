# Pallium

Offene Infrastruktur für vertrauliche KI-Begleitung kranker bis schwerkranker Patienten zur Überbrückung von Versorgungslücken.

Open infrastructure for confidential AI companionship of seriously ill patients, bridging the gaps between points of care.

Was Pallium ist

Pallium ist eine Open-Source-Infrastruktur für KI-Begleitung kranker und schwerkranker Menschen in den Lücken der Versorgung — nachts, zwischen Terminen, zwischen den Gesprächen mit Angehörigen und Behandelnden.

Das System liegt zwischen den Versorgern. Es steuert weder den Patienten noch die Behandlung. Es ist kein allgemeiner Chat-Assistent, sondern auf die emotionale und psychische Lage bei Angst, schwerer Krankheit und Sterben ausgerichtet; die Gesprächsführung folgt Ansätzen aus Palliative Care, Seelsorge und Psychoonkologie.

Die Kontrolle liegt vollständig beim Patienten. Der Gesprächsinhalt ist ausschließlich für den Patienten lesbar. Nach außen gelangt kein Inhalt, nur ein inhaltsfreier Alarm, wenn die Sicherheitsschicht einen Zustand erkennt, der Hilfe von Menschen erfordert.

Vier Schichten
Schicht	Aufgabe
Gesprächsdienst	Lokal betriebenes Sprachmodell auf offenen Gewichten; hält den vollständigen Gesprächsverlauf im Original (Arbeitsgedächtnis)
Verschlüsselter Speicher	Append-only, Schlüssel ausschließlich beim Patienten; Klartext nur zur Laufzeit im Prozess des Gesprächsdienstes
Sicherheitsschicht	Eigener Prozess ohne Zugriff auf Gesprächsinhalt; empfängt nur Zustandscodes und löst einstellbare Notrufketten aus (Matrix, SMS/SIP, Rufsysteme nach ESPA 4.4.4)
Außenschicht	Schnittstellenspezifikation für Sensorik, Drittanwendungen und KIS/AIS (HL7 FHIR R4, HL7 v2)

Details: Architecture.md

Status

Konzeptphase. Implementierung noch nicht begonnen.

Konzeptionelle Vorarbeit veröffentlicht auf Zenodo (DOI https://doi.org/10.5281/zenodo.24089907)

Geplant: erste Teststufe mit gesunden Freiwilligen auf Fremd-API (Januar–Mai 2027); Implementierung der vier Schichten im Förderzeitraum (ab Juni 2027); Testlauf auf eigenem Substrat am GreenTEC Campus Enge-Sande
Was Pallium nicht ist
kein Medizinprodukt und keine Therapie
kein Ersatz für Ärzte, Pflege, Therapeuten oder Seelsorge — es verbindet die Versorgungspunkte, es ersetzt keinen
kein System, das Diagnosen stellt, Medikation bespricht oder Behandlungsentscheidungen beeinflusst
kein Cloud-Dienst; Betrieb auf eigener Infrastruktur oder in zertifizierten Rechenzentren unter Kontrolle des Betreibers
Mitwirken

Siehe CONTRIBUTING.md. In der Konzeptphase sind vor allem Rückmeldungen aus Palliative Care, Psychoonkologie, Pflege, Seelsorge, IT-Sicherheit und von Betroffenen und Angehörigen willkommen.

Lizenz

GNU Affero General Public License v3.0 (AGPL-3.0). Nachnutzung in Krankenhäusern, Pflegeheimen, Hospizen und zu Hause ist ausdrücklich erwünscht.

Kontakt

Miriam Schnürer · schnuerer.miriam@gmail.com
