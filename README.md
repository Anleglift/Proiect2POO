# Proiect1POO – Sistem de Documentare pe Internet

## Descriere
Acest proiect C++ implementează un **sistem simplificat de documentare pe internet** care urmărește activitatea de navigare a unui utilizator și generează recomandări pe baza acestui istoric.

Aplicația citește datele vizitelor dintr-un fișier de intrare (`sites.in`), le validează, elimină duplicatele și calculează un **clasament al site-urilor recomandate** folosind o formulă de scor personalizată.

---

## Structura Proiectului
Proiectul este construit în jurul a trei clase principale:

### 1. `Domeniu`
Reprezintă un domeniu web.

**Atribute:**
- `nume` – numele domeniului (ex: google)
- `extensie` – extensia domeniului (ex: .com)

**Funcționalități:**
- Constructori (implicit, cu parametri, de copiere)
- Operator de atribuire
- Destructor cu contor de instanțe
- Getteri și setteri
- Metodă de modificare a domeniului
- Operator `<<` supraîncărcat pentru afișare

---

### 2. `AdresaWeb`
Reprezintă o adresă web completă.

**Atribute:**
- `Domeniu domeniu`
- `protocol` – http / https
- `sigur` – boolean (indică dacă site-ul este sigur)

**Funcționalități:**
- Compoziție cu clasa `Domeniu`
- Consistență automată între protocol și securitate
- Getteri și setteri
- Operator `<<` supraîncărcat

---

### 3. `Vizita`
Reprezintă istoricul de navigare al utilizatorului.

**Atribute dinamice:**
- `AdresaWeb* adrese`
- `char** date`
- `int* clicuri`
- `dimensiune`

**Caracteristici principale:**
- Gestionare completă a memoriei dinamice
- Implementare de copiere profundă (deep copy)
- Constructor, destructor și operator de atribuire complexe

---

## Funcționalități principale

### 1. Citirea din fișier
Programul citește datele din fișierul `sites.in`.

**Exemplu de fișier de intrare:**
3
google .com https 1 12-05-2023_10:30 5
youtube .com http 0 13-05-2023_11:00 3
google .com https 1 14-05-2023_12:00 7


---

### 2. Validarea datelor (`ValidareVizite()`)
Programul normalizează datele incorecte:

- Dată invalidă → `"Dată neștiută"`
- Clicuri < 2 → devin 2
- `http` → site nesigur
- `https` → site sigur
- Extensie fără `.` → se adaugă automat

---

### 3. Eliminarea duplicatelor (`CuratareVizite()`)
Vizitele sunt deduplicate pe baza:
- numelui domeniului
- extensiei

Pentru duplicate:
- clicurile sunt **adunate**
- datele sunt **concatenate**

---

### 4. Calculul scorului (`RecomandareSite()`)

Fiecare site primește un scor calculat astfel:
scor = 50% * clicuri
+ 25% * securitate (1 sau 0)
+ 25% * număr de vizite (bazat pe lungimea șirului de date)


Scorul calculat înlocuiește valoarea câmpului `clicuri`.

---

### 5. Clasamentul site-urilor (Top)
Programul generează un top N (implicit 10):

- Selectează iterativ scorul maxim
- Evită selectarea aceluiași site de mai multe ori

---