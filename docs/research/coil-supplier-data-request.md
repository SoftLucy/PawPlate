# Coil supplier data request template

Date prepared: 2026-10-09  
Status: draft only; no supplier has been contacted.

## Purpose

Ask the seller/manufacturer whether technical documentation exists for the exact spare coil **Midea 17466000000118** (listed as a 180 mm, 2,000 W induction-cooktop coil). This is a research request only. The advertised diameter and wattage do not establish compatibility with PawPlate.

## German email draft

**Betreff:** Technische Daten zur Induktionsspule Midea 17466000000118

Guten Tag,

ich recherchiere für ein nicht-kommerzielles Open-Source-Projekt zur Reparierbarkeit von Induktionskochfeldern und interessiere mich für die Ersatzteilnummer **Midea 17466000000118**.

Können Sie mir bitte mitteilen, ob für diese konkrete Spule technische Unterlagen verfügbar sind, oder meine Anfrage an den Hersteller weiterleiten? Besonders hilfreich wären:

- Datenblatt oder technische Zeichnung mit Abmessungen, Wicklungs- und Ferritaufbau sowie Anschlussbelegung
- Hersteller- bzw. Baugruppen-Teilenummer und Revisionsstand
- Induktivität bzw. Impedanz über den vorgesehenen Frequenzbereich, mit und ohne geeignetes Kochgeschirr, einschließlich Messbedingungen und Toleranzen
- Wicklungswiderstand und, falls vorhanden, AC-Widerstand bzw. Gütefaktor über Frequenz
- Zulässige Strom-, Frequenz- und Temperaturbereiche sowie Angaben zu thermischen Grenzen
- Typ, Kennlinie, Toleranz, Position und Anschlussbelegung eines integrierten Temperatursensors
- Vorgesehener Wechselrichter bzw. Resonanzkreis oder eine Liste der konkreten Kochfeldmodelle, für die diese Baugruppe freigegeben ist

Mir ist bewusst, dass einzelne Daten aus Sicherheits- oder Geschäftsgründen möglicherweise nicht weitergegeben werden können. Auch ein vorhandenes Datenblatt oder ein Hinweis auf den zuständigen technischen Ansprechpartner wäre bereits hilfreich.

Vielen Dank für Ihre Zeit.

Freundliche Grüße

[Name]

## English version for the manufacturer

**Subject:** Technical data request — Midea induction coil 17466000000118

Hello,

I am researching a non-commercial open-source project focused on the repairability of induction cooktops. I am looking for technical documentation for the exact spare part **Midea 17466000000118**, listed by retailers as a 180 mm, 2,000 W induction coil.

Could you share any available datasheet or technical drawing, or forward this request to the manufacturer? In particular, I am looking for:

- Mechanical drawing, winding/ferrite construction and terminal/connector pinout
- Exact assembly part number and revision
- Inductance or impedance versus operating frequency, with and without specified cookware, including measurement conditions and tolerances
- Winding resistance and, if available, AC resistance or Q versus frequency
- Permitted current, frequency and temperature ranges, including thermal limits
- Temperature-sensor part number/type, curve, tolerance, location and pinout
- Intended inverter/resonant-network pairing or the exact cooktop models for which the assembly is approved

I understand that some details may be confidential or unavailable. A datasheet, public document link, or referral to the appropriate technical contact would still be appreciated.

Thank you for your time.

Best regards,

[Name]

## Interpretation rules

- A seller saying only “180 mm / 2,000 W” is not sufficient to qualify the coil.
- A list of compatible appliance models establishes OEM replacement fit, not electrical compatibility with PawPlate.
- If no impedance or construction data is available, keep the coil as a candidate with unknown electrical properties. Do not invent values from the nominal wattage or diameter.
- If the seller cannot provide technical data, the next choice is to pursue a documented matched coil/inverter evaluation kit or defer this candidate until qualified characterization is possible.
- Do not purchase the coil solely to guess its electrical properties, and do not perform live mains/resonant measurements as an informal substitute for supplier documentation.
