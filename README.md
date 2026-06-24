# Ghid — Trupa (Setlist & Calendar)

Aplicația e un singur fișier: **`index.html`**. Ține datele partajate între toți membrii printr-un proiect **Firebase** (gratis, fără card). Mai jos: ce face fiecare parte a aplicației, apoi pașii de configurare și de publicare.

---

## Ce face fiecare secțiune

### Login
La deschidere, fiecare alege din listă **cine este** și pune **parola**. Userii și parolele sunt în tabelul de la final. După login, dispozitivul ține minte cine ești (până apeși „Ieși").

### Piese
- **Adaugă o piesă:** completezi **Titlu**, **Artist** și, opțional, un **Link** către piesă (YouTube, Spotify, Google Drive — orice adresă). Apeși „Adaugă piesa". Piesa apare la toți.
- Pe fiecare piesă, butonul **↗ Deschide link** deschide adresa pusă la piesă.
- **Vot** (dropdown, per persoană): 👍 „O facem" sau 👎 „N-o facem". E părerea ta; fiecare are votul lui. Sus pe piesă vezi totalul: câți 👍 și câți 👎.
- **Studiat** (dropdown, per persoană): Da/Nu — dacă tu ai studiat piesa.
- **Pot cânta** (dropdown, per persoană): Da/Nu — dacă tu o poți cânta.
- **Track-uri** (dropdown, la nivel de **trupă**): Da/Nu — dacă aveți track-urile scoase pentru piesa aia. E o singură valoare comună (de obicei o setează Nicu); oricine o poate schimba.
- **Trupa — …** : dacă apeși pe rândul ăsta, se desface și vezi exact ce a ales fiecare membru (vot, studiat, cântă), cu culoarea fiecăruia.
- **Șterge piesa:** oricine poate șterge o piesă, pentru toți.

### Calendar (tab-ul „Calendar")
- **Zilele mele ocupate:** apeși pe o zi din calendar ca s-o marchezi indisponibilă (apeși din nou ca s-o scoți). Astea le văd toți pe pagina Acasă.
- **Adaugă repetiție / concert:** alegi tipul (Repetiție sau Concert), data și, opțional, ora și locul. Apare în calendar la toți. Tot de aici le poți și șterge.

### Acasă
- **Următoarea repetiție:** bannerul de sus arată automat cea mai apropiată repetiție viitoare.
- **Disponibilitate trupă:** un calendar unde fiecare membru are o culoare; punctele colorate dintr-o zi arată **cine NU e disponibil** în acea zi. Așa vedeți rapid în ce zile să nu puneți repetiții sau concerte.
- **Ce urmează:** lista cu repetițiile și concertele care vin.

Toate datele se sincronizează **live** între toate telefoanele/laptopurile, în câteva secunde.

---

## Configurare Firebase (o singură dată, ~15 min)

### Partea 1 — Creează proiectul
1. Intră pe **https://console.firebase.google.com**, loghează-te cu un cont Google (de preferat unul al trupei).
2. **Add project** -> nume (ex: `trupa-setlist`) -> poți dezactiva Analytics -> **Create**.

### Partea 2 — Adaugă aplicația Web și ia configurarea
3. Pe pagina proiectului, apasă iconița **`</>`** („Web").
4. Nume (ex: `trupa-web`) -> **Register app**. *NU* bifa Hosting.
5. Îți arată un obiect **`firebaseConfig = { ... }`** — copiază-l tot.
6. Deschide **`index.html`** într-un editor de text, caută la începutul scriptului blocul `const firebaseConfig = { apiKey: "PUNE_AICI", ... }` și **înlocuiește valorile** cu cele copiate. Salvează.

   > `apiKey` și restul sunt normale de pus într-un fișier public — nu sunt parole secrete. Datele sunt protejate de regulile de la Partea 5.

### Partea 3 — Baza de date (Firestore)
7. **Build -> Firestore Database -> Create database**.
8. Locația `eur3 (europe-west)` -> **Next**.
9. **Start in production mode** -> **Enable**.

### Partea 4 — Conectarea (Authentication)
10. **Build -> Authentication -> Get started**.
11. La **Sign-in method**, deschide **Anonymous** -> **Enable** -> **Save**.

### Partea 5 — Regulile de securitate (Firestore)
12. **Firestore Database -> tab-ul „Rules"**. Șterge ce e și pune exact:

    rules_version = '2';
    service cloud.firestore {
      match /databases/{database}/documents {
        match /{document=**} {
          allow read, write: if request.auth != null;
        }
      }
    }

   Apasă **Publish**.

**Gata — aplicația funcționează.** Deschide `index.html` în browser, intră cu un user și ar trebui să apară cele 6 piese de pornire.

---

## Publicare — pe GitHub (GitHub Pages)
1. Cont pe **https://github.com** (gratis).
2. **New repository** -> nume (ex: `trupa`) -> poate fi Private -> **Create**.
3. **Add file -> Upload files** -> trage `index.html` -> **Commit changes**.
4. **Settings -> Pages** -> Source: **Deploy from a branch** -> Branch: **main** / **/(root)** -> **Save**.
5. După ~1 min, apare linkul: `https://NUMELE-TAU.github.io/trupa/`. Ăsta e linkul pentru trupă.

> Repo Private -> site-ul tot e public la acel link (greu de ghicit). Pentru o trupă e ok.
> Pe telefon, din browser, „Adaugă pe ecranul principal" îl pune ca o iconiță de aplicație.

## Mai târziu — pe hostingul tău
Urcă `index.html` prin File Manager sau FTP în folderul public (de obicei `public_html`). Nimic de schimbat în cod.

## Modificări ulterioare
Editezi `index.html`, îl urci din nou peste cel vechi în repo — site-ul se actualizează singur.

---

## Utilizatori (user / parolă)

| Nume | User | Parolă |
|---|---|---|
| Kara Molnar | Kara | KR |
| Andrei Ardelean | Andrei | AND |
| Nicu Chirteș | Nicu | NC |
| Raul Prodan | Raul | RCP |
| Bogdan Buzatu | Bogo | BO |

Ca să schimbi useri/parole sau culorile membrilor: în `index.html`, caută blocul `const MEMBERS = {` și editează acolo.

---

## Notă de securitate
Userul și parola din login sunt o **poartă de comoditate** — aleg „cine ești" și țin la distanță vizitatorii întâmplători. Nu sunt securitate bancară: parolele sunt scurte și se află în codul paginii, iar cine are linkul se poate conecta la baza de date. Pentru o unealtă privată de trupă e în regulă. Pentru un strat în plus: în **Google Cloud Console -> APIs & Services -> Credentials** poți restricționa cheia API după domeniu (HTTP referrer).

## Probleme frecvente
- **„Nu mă pot conecta la Firebase"** la login -> n-ai activat **Anonymous** (Partea 4) sau n-ai publicat regulile (Partea 5).
- **Piesele nu se salvează / nu apar** -> verifică regulile Firestore (Partea 5) și că Firestore e creat (Partea 3).
- **Nu văd ce face altcineva** -> toți trebuie să folosească **același** link (același proiect Firebase).
