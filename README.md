# Mine-GeoJSON
A vendor-neutral GeoJSON standard for minefield reconnaissance data

## Mine-GeoJSON v0.3

---

### 1. Einleitung

**Hinweis:** Mine-GeoJSON ist ein verbindlicher Industriestandard. Um die Einheitlichkeit zu gewährleisten und die Interoperabilität zu sichern, ist es nicht gestattet, zusätzliche oder eigene Attribute außerhalb der definierten Struktur zu ergänzen. Erweiterungen erfolgen ausschließlich durch die verantwortliche Projektleitung und werden im Rahmen offizieller Versionierungen dokumentiert.

Dieses Handbuch beschreibt den Aufbau und die Anwendung des Datenstandards **Mine-GeoJSON**, eines interoperablen, offenen Formats zur standardisierten Bereitstellung von Detektionsdaten aus der abstandsfähigen Kampfmittelerkundung.

Der Standard wurde im Rahmen eines Innovationsvorhabens des Cyber Innovation Hub der Bundeswehr in Zusammenarbeit mit der Luftlandepionierkompanie 270 entwickelt, um die Vielzahl an proprietären Sensorformaten zu vereinheitlichen und die Integration in zentrale Führungssysteme zu ermöglichen. Ziel ist es, mit Mine-GeoJSON ein gemeinsames Lagebild aus heterogenen Detektionsquellen zu erzeugen, Daten vergleichbar zu machen und eine vernetzte Gefahreneinschätzung in SitaWare Frontline zu ermöglichen.

### 1. Introduction

Note: Mine-GeoJSON is a binding industry standard. To ensure consistency and interoperability, it is not allowed to add additional or own attributes outside the defined structure. Extensions are made exclusively by the responsible project management and are documented as part of official versioning.

This manual describes the design and application of the Mine-GeoJSON data standard, an interoperable, open format for the standardized provision of detection data from standoff-capable explosive ordnance reconnaissance.

The standard was developed as part of an innovation project by the Bundeswehr Cyber Innovation Hub in cooperation with the 270 Airborne Engineers to unify the variety of proprietary sensor formats and enable integration into central command and control systems. The aim is to generate a common situational picture from heterogeneous detection sources using Mine-GeoJSON, to make data comparable and to enable a networked hazard assessment in SitaWare / MESBw.


---

### 2. Überblick über Mine-GeoJSON

Mine-GeoJSON basiert auf dem GeoJSON-Standard (RFC 7946) und erweitert ihn um kampfmittelspezifische Eigenschaften (Properties).

Der Standard ist:

- **offen**: frei zugänglich und dokumentiert
- **herstellerunabhängig**: für alle Sensorsysteme nutzbar
- **interoperabel**: durch Standardisierung auf JSON-Basis
- **zukunftssicher**: modular erweiterbar (z. B. neue Sensorarten, KI-basierte Attribute)

**Verwendungsszenarien**:

- UAV-basierte Minendetektion
- UGV-basierte Minendetektion
- kombinierte Sensorplattformen
- Import in GIS-Systeme und militärische Lageinformationssysteme

**Zielgruppe:** Softwareanbieter, Systemintegratoren, Sensorhersteller, öffentliche Stellen

### 2. Overview of Mine-GeoJSON

MINE-GeoJSON is based on the GeoJSON standard (RFC 7946) and extends it to include ordnance-specific properties.
This standard is:
- **open**: freely accessible and documented
- **manufacturer-independent**: usable for all sensor systems
- **interoperable**: through JSON-based standardization
- **future-proof**: expandable with new modules (e.g. new sensor types, AI-based attributes)

**Usage scenarios**:

- UAV-based mine detection
- UGV-based mine detection
- combined sensor platforms
- import into GIS and military situational information systems

**Target group:** software vendors, system integrators, sensor manufacturers, public authorities


---

### 3. Technische Grundlagen

- **Formatbasis:** GeoJSON (RFC 7946-konform)
- **Datenstruktur:** FeatureCollection → Feature → geometry + properties
- **Geometrietyp:** Punkt ("Point") für Einzelfunde, optional Fläche ("Polygon")
- **Koordinatensystem:** **MGRS / UTMREF (WGS84) -** [abweichend von GeoJSON-Vorgabe WGS84]
    - Hinweis: Obwohl GeoJSON offiziell WGS84 (Längengrad/Breitengrad) vorschreibt, verwendet Mine-GeoJSON aus praktischen und taktischen Gründen den NATO Standard MGRS. Systeme, die Mine-GeoJSON verarbeiten, müssen dies berücksichtigen.
- **Zeitformat:** ISO 8601 (z. B. 2025-03-20T10:45:00Z)
    - Alternativ kann zusätzlich die militärische Datum-Zeit-Gruppe (DTG) verwendet werden (z. B. 260900AMAR25 für 26. März 2025, 09:00 Uhr UTC+1).

### 3. Technical specifications

- **Format base**: GeoJSON (RFC 7946-compliant)
- **Data structure**: FeatureCollection  feature  geometry + properties
- **Geometry type**: Point for single finds, area optional (“polygon”)
- **Coordinate system**: MGRS / UTMREF (WGS84) – [Deviating from GeoJSON Specification WGS84]
    - Note: Although GeoJSON officially requires WGS84 (longitude/latitude), Mine-GeoJSON uses the NATO MGRS standard for practical and tactical reasons. Systems processing Mine-GeoJSON must take this into account.
- **Time format**: ISO 8601 (e.g. 2025-03-20T10:45:00Z)
    - Alternatively, the DTG may also be used (e.g. 260900AMAR25 for 26 March 2025, 09:00 UTC+1).


---

### 4. Struktur des Datenformats

Mine-GeoJSON ist als standardisierte GeoJSON-Datei aufgebaut. Der Kern der Datei ist eine `FeatureCollection`, die einzelne Objekte (Funde) als `Feature` enthält. 

Jedes Feature besteht aus:

- einem `geometry`Block mit den Koordinaten (MGRS)
- einem `properties`Block mit den zugehörigen Attributen

Beispielstruktur (vereinfacht):

```json
{
"type": "FeatureCollection",
"features": [
{
"type": "Feature",
"geometry": {
"type": "Point",
"coordinates": [32UNB, 392000.212, 5821000.213]
},
"properties": {
"id": "UXO-20250320-001",
"type": "UXO",
"hazard_status": "suspected",
"last_update": "2025-03-20T10:45:00Z"
}
}
]
}
```

Weitere Details zu den Feldern finden sich im nächsten Kapitel.

### 4. Structure of the data format

MINE-GeoJSON is a standardized GeoJSON file. The core of the file is a `FeatureCollection` that contains individual objects (finds) as  `Feature`  .

Each feature consists of:
- A   block of coordinates (MGRS)
- A   block with associated attributes

Example structure (simplified):

```json
{
"type": "FeatureCollection",
"features": [
{
"type": "Feature",
"geometry": {
"type": "Point",
"coordinates": [32UNB, 392000.212, 5821000.213]
},
"properties": {
"id": "UXO-20250320-001",
"type": "UXO",
"hazard_status": "suspected",
"last_update": "2025-03-20T10:45:00Z"
}
}
]
}
```

---

### 5. Feldreferenz (Properties)

Die folgende Tabelle listet alle aktuell definierten Felder (Properties) auf, die im properties-Block jedes GeoJSON-Features verwendet werden können. 
Diese Definitionen sind verbindlich für alle Hersteller und Systeme, die Mine-GeoJSON implementieren.

The following table lists all currently defined fields (properties) that can be used in the properties block of each GeoJSON feature.
Manual for Industrial Partners
These definitions are binding for all manufacturers and systems implementing Mine-GeoJSON.


|Nummer| Feldname | Herkunft | Beschreibung | Datentyp |
| --- | --- | --- | --- | --- |
| 1 | `id` | Eigene Definition Own Defenition| Eindeutige Fund-ID[z.B. “1,2,3,4,5,6”] <br/> Unique ID of the find [e.g. “1.2,3,4,5,6”]| Number |
| 2 | `sensor` | Eigene Definition Own Defenition| Messgerät [z.B. “Magnetometer”] <br/> Measuring equipment [e.g. “magnetometer”]| String |
| 3 | `unit` | Eigene Definition Own Defenition| Einheit der Messung[z.B. “nT”, “m”, “EA”] <br/> Unit of measurement e.g. “nT”, “m”, “EA”] | String |
| 4 | `detection_confidence` | Eigene Definition Own Defenition| Wahrscheinlichkeit für Echtheit in %[z.B. 0.87, 99, 1] <br/> Probability of authenticity in % [e.g. 0.87, 99, 1] | Number |
| 5 | `status` | Eigene Definition Own Defenition| Status[”Bestätigt” / “Unbestätigt”]  <br/> Status ["confirmed” / “unconfirmed”] | Boolean |
| 6 | `source` | Eigene Definition Own Defenition| Trägerplattform [z.B. “UAV”, “UGV”]  <br/> Carrier platform [e.g. “UAV”, “UGV”] | String |
| 7 | `depth_estimate` | Eigene Definition Own Defenition| Geschätzte Tiefe[z.B. -2.1, -0.3]  <br/> Estimated depth [e.g. -2.1, -0.3] | Number |
| 8 | `last_udpate` | Eigene Definition Own Defenition| Letzte Aktualisierung[z. B. “260900Amar25”]  <br/> Last update [e.g. “260900Amar25”] | String |
| 9 | `image` | Eigene Definition Own Defenition| Optionales Bildmaterial <br/> Optional imagery |  |
| 10 | `geotiff` | Eigene Definition Own Defenition| Georeferenziertes Bildmaterial (GeoTIFF)  <br/> Georeferenced imagery (GeoTIFF) |  |
| 11 | `hazard_id` | IMSMA | Minenfeld- oder Bereichs ID  <br/> Minefield or area ID | String |
| 12 | `hazard_status` | IMSMA | Status des Gebietes[”suspected”, “confirmed”]  <br/> Status of the area [“suspected”, “confirmed”] | String |
| 13 | `land_use` | IMSMA | Aktuelle Landnutzung[z.B. Feld, Straße]  <br/> Current land use [e.g. field, road] | String |
| 14 | `accessibility` | IMSMA | Zugangsbeschränkungen des sondierten Gebietes[Freitext]  <br/> Access restrictions regarding the probed area [free text] | String |
| 15 | `survey_type` | IMSMA | Erkundungsmethode [”technical”, “non-technical”]  <br/> Reconnaissance method [“technical”, “non-technical”] | String |
| 16 | `data_source` | IMSMA | Quelle der Information [”KI”, “NGO”, “Soldat”} <br/> Source of information [“AI”, “NGO”, “Soldier”] | String |
| 17 | `verfication_type` | IMSMA | Art der Überprüfung nach Sondierung[Freitext: “visuell”, “manuell”, “etc.”)  <br/> Type of verification after probing [free text: “visually”, “manually”, “etc.”) | String |
| 18 | `dtg` | NATO-STANAG 2221 | Datum-Zeit-Gruppe des Fundes / der Messung[z. B. “260900Amar25”] <br/> Date-time group of the find/measurement [e.g. “260900Amar25”] | String |
| 19 | `reporting_unit` | NATO-STANAG 2221 | Meldende Einheit [z.B. “LLPiKp 270”]  <br/> Reporting unit [e.g. “270 AB Engr Coy”] | String |
| 20 | `linkup_location` | NATO-STANAG 2221 | Ort für Verbindungsaufnahme mit meldender Einheit (UTM-Format)[z.B. “347669, 5576127”] <br/> Location for establishing communication with to reporting unit (UTM format) [e.g. “347669, 5576127”] | String |
| 21 | `linkup_method` | NATO-STANAG 2221 | Kommunikationsart für Verbindungsaufnahme mit meldender Einheit[z.B. “Funkfrequenz” , “Sichtzeichen”] <br/> Communication type for establishing communication with reporting unit [e.g. “radio frequency”, “visual signal”] | String |
| 22 | `type` | NATO-STANAG 2221 | Art des Kampfmittels[z.B. Geschoss, 155mm / Mine, Panzerabwehr/Schützenabwehr / Granate, Hand] <br/> Type of explosive ordnance [e.g. shell, 155mm / mine, anti-tank / antipersonnel / grenade, hand] | String |
| 23 | `subtype` | NATO-STANAG 2221 | Munitionssorte des Kampfmittels [z.B. HE / HE-FRAG / AP / HEAT / APHE / …] <br/> Ammunition type of ordnance [e.g. HE / HE - FRAG / AP / HEAT / APHE / …] | String |
| 24 | `amount` | NATO-STANAG 2221 | Anzahl der Kampfmittel [z.B. 2 EA ("each"] <br/> number of EOs [e.g. 2 EA (“each”] | String | 
| 25 | `description` | NATO-STANAG 2221 | Freitextbeschreibung des Kampfmittels [z.B. zylindrisches Geschoss, Kaliber ca. 155mm, mit kegelförmiger Spitze] <br/> Free text description of the ordnance [e.g. cylindrical projectile, calibre approx. 155mm, with conical tip] | String |
| 26 | `location` | NATO-STANAG 2221 | Fundort des Kampfmittels - MGRS[z.B.”32UNB, 347669, 5576127”] <br/> Location of the ordnance item – MGRS [ e.g. “32UNB, 347669, 5576127”] | String |
| 27 | `tactical_situation` | NATO-STANAG 2221 | Einfluss des gefundenen Kampfmittels auf die eigene Auftragserfüllung [”None” , “Low”, “Medium”, “High”] <br/> Influence of the found ordnance on own mission performance [ˮNoneˮ, "Lowˮ, "Mediumˮ," Highˮ] | String |
| 28 | `collateral_dmg` | NATO-STANAG 2221 | Bereits eingetretene oder mögliche Begleitschäden [z.B. “Funkturm als kritische Infrastruktur direkt nebenan”] <br/> [e.g. "Radio tower as critical infrastructure directly adjoining”] | String |
| 29 | `protective_actions` | NATO-STANAG 2221 | eingeleitete Schutzmaßnahmen [z.B. “umliegende Häuser evakuiert”] <br/> Protective measures initiated [e.g. “surrounding houses evacuated”] | String |
| 30 | `recommended_priority` | NATO-STANAG 2221 | empfohlene Priorität gem. Bewertung der meldenden Einheit [”immediate” , “urgent”,  “routine”  , “no threat”] <br/> Recommended priority IAW assessment of the reporting unit [“immediate”, “urgent”, “routine”, “no threat”] | String |
| 31 | `rank_name` | Eigene Definition Own Defenition| Messung durchgeführt durch[z.B. “HF Müller”] <br/> Measurement performed by [e.g. “OR-7 Müller”] | String |

---

### 6. Erweiterbarkeit & Zukunftssicherheit

Mine-GeoJSON wurde so konzipiert, dass der Standard mit zukünftigen Anforderungen wachsen kann. Die Struktur des Formats erlaubt es, zusätzliche Felder in den `properties`-Block zu integrieren – jedoch **ausschließlich** durch die verantwortlichen Herausgeber des Standards.

### Prinzipien der Erweiterung:

- **Zentral gepflegt**: Erweiterungen erfolgen ausschließlich durch die zentrale Projektleitung und werden über Versionierungen bekannt gemacht.
- **Einheitlichkeit gewährleisten**: Um die Interoperabilität und Vergleichbarkeit der Daten sicherzustellen, ist die Erweiterung durch Dritte nicht vorgesehen.
- **Dokumentationspflicht**: Alle Änderungen werden in der offiziellen Dokumentation veröffentlicht.

### Warum keine offenen Erweiterungen?

Ein offenes Hinzufügen von Feldern durch die Industrie würde den einheitlichen Standard aufweichen und könnte die automatische Verarbeitung sowie die Integration in Führungssysteme behindern. Mine-GeoJSON dient als **Single Point of Truth** – ein zentral gepflegter, verlässlicher Standard.

### Versionierung:

Jede offizielle Änderung oder Erweiterung des Standards wird mit einer neuen Versionsnummer gekennzeichnet (z. B. v0.1 → v0.2). Die aktuelle Version wird in der technischen Dokumentation und im JSON-Schema eindeutig referenziert.

### 6. Extensibility & sustained future use

MINE-GeoJSON is designed to allow the standard to grow with future requirements. The structure of the format allows to integrate additional fields into the   block, but only by the responsible publishers of the standard.
Principles of extension:
- **Central maintenance**: Extensions are made exclusively by the central project management and are made known via versioning.
- **Ensure uniformity**: In order to ensure the interoperability and comparability of the data, the extension by third parties is not intended.
- **Duty of documentation**: All changes will be published in the official documentation.

### Why no open extensions?

An open addition of fields by the industry would weaken the uniform standard and could impede automatic processing and integration into command and control systems. Mine-GeoJSON serves as a single point of truth – a centrally maintained, reliable standard.

### Versioning:

Any official change or extension of the standard will be marked with a new version number (e.g. v0.1 → v0.2). The current version is unambiguously referenced in the technical documentation and the JSON format.


---

### 7. Validierung

Um eine einheitliche Datenqualität und Kompatibilität sicherzustellen, wird ein validierbares JSON-Schema für Mine-GeoJSON bereitgestellt. Dieses Schema definiert alle Pflichtfelder, Datentypen, erlaubten Wertebereiche sowie optionale Felder.

### Ziel der Validierung

- Sicherstellung der Datenqualität und Formatkonformität
- Minimierung manueller Prüfprozesse
- Automatisierte Integration in Lageinformationssysteme

### Validierungsmöglichkeiten

- Online-Validator (z. B. https://jsonschemavalidator.net)
- Lokales Prüfskript (Python, Node.js etc.) mit bereitgestelltem Schema

### Gültigkeitskriterien

- Die GeoJSON-Struktur muss formal gültig sein

Das Schema wird gemeinsam mit dem Handbuch und der Beispieldatei bereitgestellt.

### 7. Validation

To ensure consistent data quality and compatibility, a JSON form (validation-capable) is provided for Mine-GeoJSON. This schema defines all mandatory fields, data types, permitted value ranges, and optional fields.

Aim of validation
- Ensure data quality and format compliance
- Minimize manual testing processes
- Automated integration into situational information systems

## Validation options
- Online validator (e.g. https://jsonschemavalidator.net)
- Local test script (Python, node.js, etc.) with provided format

### Validity criteria

- The GeoJSON structure must be formally valid.

The format is provided with the manual and sample file.


---

### 8. Bild- & Geodatenintegration

Mine-GeoJSON unterstützt die Einbindung visueller Informationen zur Ergänzung der strukturierten Detektionsdaten. Zwei spezielle Felder stehen dafür zur Verfügung:

### `image` – Verlinktes Objektbild

- **Typ:** URL oder relativer Pfad
- **Zweck:** Darstellung eines Fotos oder Screenshots des detektierten Objekts
- **Formatempfehlung:** JPG, PNG (komprimiert, max. 2 MB)
- **Beispiel:** `"image": "images/UXO-20250320-001.jpg"`

### `geotiff` – Georeferenziertes Flächenbild

- **Typ:** URL oder Dateipfad
- **Zweck:** Bereitstellung großflächiger Daten z. B. aus Heatmaps, Rasterscans, Kartenauswertungen
- **Format:** GeoTIFF (mit UTM-Koordinatenbezug)
- **Beispiel:** `"geotiff": "data/sector3_scan.tif"`

### Anforderungen & Empfehlungen

- GeoTIFFs müssen georeferenziert vorliegen (UTM)
- Transparente Layer (Alpha-Kanal) empfohlen für bessere Überlagerung
- Dateigröße: <50 MB zur Sicherstellung der Performance in Viewern
- Einheitliche Dateibenennung analog zu `hazard_id` oder `id`

### Integration in Viewer

- **Overlay** auf Karte mit Zoomfähigkeit
- Verknüpfung mit Funden über ID oder Lagebezug
- Möglichkeit zur Aktivierung/Deaktivierung einzelner Layer

### Nutzen

- Visuelle Nachvollziehbarkeit der Funde
- Verbesserung der operativen Entscheidungsgrundlagen
- Unterstützung bei Übergabe, Dokumentation und Analyse

### 8. Integration of image & geospatial data

MINE-GeoJSON supports the integration of visual information to complement structured detection data. Two dedicated fields are available for this purpose:

### `image` - Linked object image
- **Type**: URL or relative path
- **Purpose**: Depiction of a photo or screenshot of the detected object
- **Format recommendation**: JPG, PNG (compressed, max. 2 MB)
- **Example**: `"image": "images/UXO-20250320-001.jpg"`  

### `geotiff`-  Geo-referenced area image

- **Type**: URL or file path
- **Purpose**: Provision of large-area data e.g. from heat maps, raster scans, map evaluations
- **Format**: GeoTIFF (with UTM coordinate reference)
- **Example**: `"geotiff": "data/sector3_scan.tif"` 

### Requirements & recommendations
- GeoTIFFs must be in georeferenced form (UTM)
- Transparent layers (alpha channel) recommended for better overlay
- File size: <50 MB to ensure performance in viewers
- Uniform file naming analogous to `hazard_id` or `id`  

### Integration in the viewer
**Overlay** on map with zoom capability
- Link to finds by ID or location reference
- Option to enable/disable individual layers

### Benefit
- Visual traceability of finds
- Improvement of basis for operational decision-making bases
- Support for handover, documentation and analysis


---

### 9. Anwendungsbeispiele

Im Folgenden werden typische Anwendungsfälle dargestellt, in denen Mine-GeoJSON genutzt wird. Die Beispiele zeigen sowohl einfache Punktdaten als auch erweiterte Szenarien mit Zusatzinformationen.

The following are typical use cases where Mine-GeoJSON is used. The examples show both simple point data and advanced scenarios with additional information.

### Beispiel 1: Einzelfund (UXO)
### Example 1: Single find (UXO)

```json
{
"type": "Feature",
"geometry": {
"type": "Point",
"coordinates": [32UNB, 392000, 5821000]
},
"properties": {
"id": "UXO-20250320-001",
"type": "UXO",
"hazard_status": "suspected",
"recorded_at": "2025-03-20T10:45:00Z",
"sensor": "MAG-01",
"detection_confidence": 0.89,
"reporting_unit": "LLPiKp 270"
}
}
```

### **Beispiel 2: Flächenerkundung mit GeoTIFF**
### **Example 2: GeoTIFF area survey**

```json
{
"type": "Feature",
"geometry": {
"type": "Polygon",
"coordinates": [[[32UNB, 392000, 5821000], [32UNB, 392050, 5821000], [32UNB, 392050, 5821050], [32UNB, 392000, 5821050], [32UNB, 392000, 5821000]]]
},
"properties": {
"hazard_id": "AREA-03",
"hazard_status": "confirmed",
"geotiff": "data/area03_heatmap.tif"
}
}
```

### **Beispiel 3: Mehrfachfund – Typ "TM-62M" (Reihenfund mit 2 m Abstand)**
### **Example 3: Multiple Discovery - Type “TM-62M” (row discovery with 2m distance each)**

```json
{
"type": "FeatureCollection",
"features": [
{
"type": "Feature",
"geometry": { "type": "Point", "coordinates": [32UNB, 392000, 5821000] },
"properties": {
"amount": "1 EA",
"image": "images/TM62M-001.jpg",
}
},
{
"type": "Feature",
"geometry": { "type": "Point", "coordinates": [32UNB, 392002, 5821000] },
"properties": {
"amount": "1 EA",
"image": "images/TM62M-002.jpg",
}
},
{
"type": "Feature",
"geometry": { "type": "Point", "coordinates": [32UNB, 392004, 5821000] },
"properties": {
"amount": "1 EA",
"image": "images/TM62M-003.jpg",
}
},
{
"type": "Feature",
"geometry": { "type": "Point", "coordinates": [32UNB, 392006, 5821000] },
"properties": {
"amount": "1 EA",
"image": "images/TM62M-004.jpg",
}
},
{
"type": "Feature",
"geometry": { "type": "Point", "coordinates": [32UNB, 392008, 5821000] },
"properties": {

"amount": "1 EA",
"image": "images/TM62M-005.jpg",
}
}
]
}
```

---

### 10. Integration in bestehende Systeme

Mine-GeoJSON ist so gestaltet, dass es sich nahtlos in bestehende Informationssysteme integrieren lässt. Dies umfasst sowohl militärische Führungssysteme als auch zivile GIS-Anwendungen.

### Kompatible Plattformen

- QGIS, ArcGIS, Kepler.gl, u.v.m.
- Lageinformationssysteme mit JSON/GeoJSON/Mine-GeoJSON-Schnittstelle

- Erprobt wurde Mine-GeoJSON mit dem Plug-In "Minesweeper Connector" für Sitaware Frontline.  

### Empfehlungen zur Umsetzung

- Parser/Importmodul zur automatischen Mine-GeoJSON-Validierung
- Integration über REST-API, Datenbank-Import oder Dateiupload
- Verwendung standardisierter Layer- und Objektbeschreibungen

### 10. Integration in existing systems
M
INE-GeoJSON is designed to integrate seamlessly with existing information systems. This includes both military command and control systems and civilian GIS applications.
Compatible platforms
- QGIS, ArcGIS, Kepler.gl, etc.
- Location information systems with JSON/GeoJSON interface

### Recommendations for implementation
- Parser/import module for automatic GeoJSON validation
- Integration via REST-API, database import or file upload
- Use of standardized layer and object descriptions


---

### 11. Konventionen & Best Practices

Zur Sicherstellung der Datenqualität empfiehlt Mine-GeoJSON folgende Konventionen:

### Dateibenennung

- `minegeojson_<gebiet>_<datum>.minegeojson`
- GeoTIFF: `minegeo_<hazard_id>.tif`

### Zeitangaben

- ISO 8601, UTC-Zeit
    - Beispiel: `2025-03-20T10:45:00Z`
- Datum-Zeit-Gruppe (DTG)
    - Beispiel: `260900AMAR25`

### Strukturhinweise

- Felder mit `null`Werten können entfallen

### 11. Conventions & best practices

To ensure data quality, MINE-GeoJSON recommends the following conventions:

### File naming
- `minegeojson_<gebiet>_<datum>.minegeojson`
- GeoTIFF: `minegeo_<hazard_id>.tif`

### Time details

- ISO 8601, UTC time
    - Example: `2025-03-20T10:45:00Z`
•	Date-time group (DTG)
    - Example: `260900AMAR25`

### Notes on structure

 Fields with  `null` values can be omitted


---

### 12. Lizenzierung & Nutzungsbedingungen

Mine-GeoJSON ist ein offener, nicht-proprietärer Datenstandard. Er darf frei genutzt, erweitert und in Softwaresysteme integriert werden, sofern folgende Bedingungen eingehalten werden:

- Quellenvermerk auf Mine-GeoJSON erforderlich
- Keine kommerzielle Weiterverwertung ohne Zustimmung
- Änderungen am Standard dürfen nicht als offizielle Version weitergegeben werden


MINE-GeoJSON is an open, non-proprietary data standard. It may be freely used, extended and integrated into software systems, provided that the following conditions are met:

- Indicate source as Mine-GeoJSON
- No commercial re-use without consent
- Changes to the standard may not be passed as an official version


---

### 13. Kontakt & Weiterentwicklung

Der Datenstandard wurde initiiert durch den Cyber Innovation Hub der Bundeswehr und maßgeblich entwickelt unter der Projektleitung von Alexander Petri, Ulf Steden, Erik Krämer und der Luftlandepionierkompanie 270 im Rahmen des Innovationsvorhaben #172 - Minesweeper.
Youtube Link: https://youtu.be/I4TvcCWCnZo?si=7fIkJjQ65l3BrP6O

Wir freuen uns über Ihre Kontaktaufnahme, wenn Sie Interesse an der Nutzung von Mine-GeoJSON haben oder uns Feedback geben möchten!

### 13. Contact & further development
The data standard was initiated by the Bundeswehr Cyber Innovation Hub and was significantly developed under the project management of Alexander Petri, Ulf Steden, Captain Heinemann and Captain Krämer as part of the innovation project #172 – Minesweeper.

### Kontaktstelle

---

░█████╗░██╗██╗░░██╗██████╗░░██╗░░░░░░░██╗
██╔══██╗██║██║░░██║██╔══██╗░██║░░██╗░░██║
██║░░╚═╝██║███████║██████╦╝░╚██╗████╗██╔╝
██║░░██╗██║██╔══██║██╔══██╗░░████╔═████║░
╚█████╔╝██║██║░░██║██████╦╝░░╚██╔╝░╚██╔╝░
░╚════╝░╚═╝╚═╝░░╚═╝╚═════╝░░░░╚═╝░░░╚═╝░░

**Cyber Innovation Hub der Bundeswehr**

Franklinstr. 10

10587 Berlin

E-Mail: bwi.fp.cih-innovation@bwi.de

https://www.cyberinnovationhub.de/

---

### Versionsführung

- Aktuelle Version: **v0.3** (Stand: 01.10.2025)
