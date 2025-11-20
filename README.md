# Relational Database Project: “Libreria Italiana”

## 1. Project Overview

This project implements a relational database for the Italian bookstore franchise **“Libreria Italiana”**.

The database is designed to support the management of:

- the **network of stores** (points of sale) and their locations;
- the **workforce** employed in each store and their contracts;
- the **product catalogue**, including books, multimedia items and accessories;
- the **customer base**, distinguishing between loyalty-card holders and non-members;
- the **sales process**, including purchases, references between staff and customers, and loyalty points.

The database has been implemented in **Microsoft Access** and is delivered as a single `.accdb` file together with:

- a conceptual **E–R schema** (`Schema E-R.pdf`);
- a **written report** describing the design process and the relational model (`1) PROGETTO COMPLETO.docx`);
- a short document describing the main **queries** (`2) Descrizione Query.docx`).

---

## 2. Application Scenario

The franchisor *Libreria Italiana* manages multiple **points of sale** located in different cities.  
For each store the database keeps track of:

- **VAT number** *(Partita IVA)* – main identifier of the store;
- **address data** (city, street address, etc.);
- **contact details** (telephone number, e-mail);
- an internal **store code** that may encode the city (e.g. `ROMA01`, `ROMA02` if there are multiple stores in Rome).

In each store, zero or more **employees** can work. Some stores may be fully or partially automated and therefore operate with no on-site staff.

Each **employee** is characterised by:

- **Tax code** *(Codice Fiscale)* – primary identifier;
- **personal data** (name, surname, date of birth);
- **contact details** (phone number, e-mail);
- an internal **employee code**;
- **work experience** and **role / position**.

An employee can be transferred from one store to another. Employment periods are modelled as **contracts**, with start and end dates for each store where the employee has worked.

The **product range** offered by the franchise is organised into:

- **Books**;
- **Multimedia**;
- **Accessories & Stationery**.

Each product is uniquely identified by a **bar code** and is associated with a **price**, a **description** and a **type**.

The **Multimedia** section is further divided into:

- **CDs**;
- **Vinyl records**;
- **Audiobooks**.

The **Books** section can be classified by **Title** or **Author** and covers a rich set of **literary genres** (e.g. crime, thriller, horror, fantasy, psychology, religion, philosophy, history, economics, science, IT, comics, Italian and foreign fiction, etc.).  
Each **Author** is identified by an internal code, and has a **name** and **surname**.

The franchise serves two main categories of **customers**:

- **Loyalty-card customers** (members), identified by:
  - **Tax code** *(Codice Fiscale)*,
  - **card code**,
  - **activation date**,
  - **loyalty points**,
  - **name** and **surname**.
- **Non-members**, identified by:
  - **Tax code**,
  - **name** and **surname**.

For both groups, the database records:

- **purchases** (which products have been bought by which customers);
- **interactions/references** between **employees** and **customers** (who assisted which customer).

This scenario is then translated into:

1. a **conceptual E–R model**;
2. a **relational schema**;
3. a **physical implementation** in Microsoft Access, including tables, integrity constraints and queries.

---

## 3. Conceptual Design (E–R Model)

The **Entity–Relationship (E–R) schema** is provided as a separate PDF:

- **File:** `Schema E-R.pdf`

You can open the PDF to inspect the conceptual design, which includes entities such as:

- **Store** (*Punto Vendita*);
- **Employee** (*Dipendente*);
- **City** (*Città*);
- **Product** (*Prodotto*);
- **Book**, **Audiobook**, **other multimedia items**;
- **Author** and **Genre**;
- **Loyalty-card customer** and **Non-member customer**;
- **Contracts**, **Purchases** and **References** between employees and customers.

[View the E–R schema (PDF)](Schema%20E-R.pdf)

The E–R diagram specifies:

- **entities** and their attributes;
- **identifiers / keys** for each entity;
- **relationships** between entities (one-to-one, one-to-many, many-to-many);
- **cardinalities** and **participation constraints**.

---

## 4. Relational Model

Starting from the E–R schema, the conceptual design is translated into a **relational model**.  
The main tables (in Italian, as implemented in Access) are:

- **Punto Vendita**  
  `Punto Vendita (Partita Iva, E-mail, Numero di telefono, Indirizzo, Città)`

- **Città**  
  `Città (Codice Identificativo)`

- **Dipendente**  
  `Dipendente (Codice Fiscale, E-mail, Nome, Cognome, Recapito Telefonico, Data di nascita, Codice Dipendente, Esperienze lavorative, Punto Vendita, ...)`

- **Assunzione** (employment periods / contracts)  
  `Assunzione (Codice Fiscale Dipendente, Partita IVA, Data Inizio, Data Fine, Mansione)`

- **Prodotto**  
  `Prodotto (Codice a barre, Prezzo, Descrizione, Tipo)`

- **Libri**  
  `Libri (Codice a barre, Titolo)`

- **Audiolibri**  
  `Audiolibri (Codice a barre, Titolo, Libro corrispondente)`

- **Autore**  
  `Autore (Codice Identificativo, Nome, Cognome)`

- **Realizzazione** (association between products and authors)  
  `Realizzazione (Codice a barre, Codice Identificativo Autore)`

- **Scrittura / Genere**  
  `Scrittura (Codice Identificativo Autore, Tipo)`  
  `Genere (Tipo)`

- **Cliente Tesserato** (loyalty-card customers)  
  `Cliente Tesserato (Codice Fiscale, Codice Tessera, Data di attivazione, Punti fedeltà, Nome, Cognome)`

- **Cliente Non Tesserato**  
  `Cliente Non Tesserato (Codice Fiscale, Nome, Cognome)`

- **Acquisto T** (purchases by loyalty customers)  
  `Acquisto T (Codice a barre, Codice Fiscale Cliente Tesserato)`

- **Acquisto N** (purchases by non-members)  
  `Acquisto N (Codice a barre, Codice Fiscale Cliente Non Tesserato)`

- **Riferimento T / Riferimento N** (references / interactions between staff and customers)  
  `Riferimento T (Codice Fiscale Dipendente, Codice Fiscale Cliente Tesserato)`  
  `Riferimento N (Codice Fiscale Dipendente, Codice Fiscale Cliente Non Tesserato)`

Additionally, the model includes intermediate tables such as **Appartiene** (to model product–store membership) and other relationships implied by the E–R schema.

---

## 5. Integrity Constraints and Keys

The relational schema enforces **primary keys** and **foreign keys** reflecting the E–R design.  
Examples of referential integrity constraints include (in conceptual form):

- `PUNTO VENDITA[Città]` → references `CITTA'[Codice Identificativo]`;
- `REALIZZAZIONE[Codice Identificativo Autore]` → references `AUTORE[Codice Identificativo Autore]`;
- `ACQUISTO T[Codice a barre]` → references `PRODOTTO[Codice a barre]`;
- `ACQUISTO T[Codice Fiscale T]` → references `CLIENTE TESSERATO[Codice Fiscale T]`;
- `ACQUISTO N[Codice a barre]` → references `PRODOTTO[Codice a barre]`;
- `ACQUISTO N[Codice Fiscale N]` → references `CLIENTE NON TESSERATO[Codice Fiscale N]`;
- `RIFERIMENTO T[Codice Fiscale D]` → references `DIPENDENTE[Codice Fiscale D]`;
- `RIFERIMENTO T[Codice Fiscale T]` → references `CLIENTE TESSERATO[Codice Fiscale T]`;
- `RIFERIMENTO N[Codice Fiscale D]` → references `DIPENDENTE[Codice Fiscale D]`;
- `RIFERIMENTO N[Codice Fiscale N]` → references `CLIENTE NON TESSERATO[Codice Fiscale N]`.

In Microsoft Access these constraints are implemented through:

- **Primary keys** on the main identifiers (e.g. `Codice a barre`, `Codice Fiscale`, `Partita Iva`, `Codice Identificativo`, etc.);
- **Foreign keys** with **referential integrity** enabled and suitable **update/delete rules**.

---

## 6. Physical Implementation in Microsoft Access

The database is delivered as:

- **File:** `PROGETTO ACCESS.accdb`

The Access database contains:

- All **tables** corresponding to the relational model;
- **Relationships** with referential integrity;
- A set of **saved queries** that implement typical information needs for the bookstore network;
- Optionally, **forms** and **reports** (depending on the version of the project).

The design follows standard normalisation principles to reduce redundancy and avoid anomalies in insert, update and delete operations.

---

## 7. Query Layer

The file:

- `2) Descrizione Query.docx`

provides a short description of the **queries** defined in the Access database.

The queries are intended to support common analyses and operations, such as (examples):

- retrieving purchases by a given **customer** (member or non-member);
- analysing **store performance** (sales by store, by city, etc.);
- listing **employees** working in a specific store or with certain roles;
- analysing **loyalty-card** usage and **loyalty points**;
- extracting statistics on **products**, **categories** and **authors**.

All these information needs are expressed in **SQL** (translated internally by Access) and made available as named queries inside the `.accdb` file.

---

## 8. How to Open and Use the Database

1. **Requirements**
   - Microsoft Access (version 2016 or later is recommended).
   - Read permissions on `PROGETTO ACCESS.accdb`.

2. **Opening the database**
   - Download or clone this repository.
   - Open `PROGETTO ACCESS.accdb` with Microsoft Access.
   - If necessary, enable **content / macros** to allow the execution of queries and relationships.

3. **Exploring the design**
   - Inspect the **tables** and **relationships** (Database Tools → Relationships).
   - Compare the physical model with the conceptual design in `Schema E-R.pdf`.
   - Review the **queries** defined in the database (Objects → Queries) and, where useful, open them in SQL view to inspect the underlying code.

4. **Running queries**
   - Open any saved query to run it and view the results.
   - Filter, sort or export the result sets as needed (e.g. to Excel).

---

## 9. Repository Structure

A recommended structure for the repository is:

```text
.
├─ README.md
├─ PROGETTO ACCESS.accdb
├─ Schema E-R.pdf
├─ 1) PROGETTO COMPLETO.docx
└─ 2) Descrizione Query.docx
