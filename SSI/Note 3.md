---
date: 16-03-2023
draft: true
---

# Certification Authority
Abbiamo necessità di associare una **identità** ad un chive pubblica.
Io potrei tranquillamente esporre la mia chiave pubblica spacciandomi per chiunque altro.
Per fare ciò abbiamo bisogno di un **certificato** di identità, e di una **entità** che emette i certificati.

Una volta eletta una **certification authority** che emette tali certificati, bisogna andare **fisicamente** (una volta) a prendere la chiave pubblica della CA per poterci comunicare.

Il formato di un certificato (**X.509**), con le seguenti informazioni:
- l'**issuer**, ovvero l'emittente.
- il **subject**, ovvero a chi è riferito il certificato.
- la **pubblic key** del subject.
- la **data d'inizio** (di emissione) del certificato.
- la **data di fine** in cui un certificato ha validità.
- uno **scope**, l'utilizzo che ne faccio del certificato.
- una **firma** dell'issure.

Anche la CA ha un certificato di identità.
Il suo certificato è però (l'<u>unico</u>) **self signed**, ovvero dove il subject e l'issuer **concidono**.

I componenti in gioco in un meccanismo di certificati sono:
- una **root** CA
- una eventuale **sub**-CA
- un server SSL
- un client SSL

Questo modello funziona nel momentto in cui noi possiamo andare **fisicamente** dalla CA, verificandone la sua *fisicità*.

Nel mondo reale nessuno però va mai fisicamente da una CA.
Quello che avviene però è che i moderni browser hanno **cablati** (*hard-coded*) al loro interno una serie di **certificati** di root-CA.

Per controllare la valenza di un certificato, basta ferificarne la firma.
Per esempio, richiedo ad amazon il suo **certificato**.
Innanzitutto devo avere tra i miei self-signed certificati quello rispettivo all'issuer del certificato che ci ha dato amazon.
Dopodiché verifico che la firma sia valida.
Infatti, avendo la **chiave pubblica** dell'issuer potrò decifrare la firma del certificato.
Una volta verificata l'autenticità del certificato, posso verificare gli altri campi (scope, scadenza, ecc...).

