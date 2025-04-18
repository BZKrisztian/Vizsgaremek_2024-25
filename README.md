# Vizsgaremek_2024-25 — To-Do App ✅

### 📁 Fejlesztési repository linkek:
A fejlesztés során különálló repository-kban dolgoztunk. Ezek az alábbi linkeken érhetők el:

- **Frontend GitHub repository:**  
  https://github.com/BZKrisztian/Vizsgaremek_Frontend.git

- **Backend GitHub repository:**  
  https://github.com/CatNipSniffer01/todo-back

---

## ⚙️ Setup instrukciók

A projekt környezeti változókat (`environment`) használ, melyek **biztonsági okokból nem kerültek feltöltésre**. Az alábbi útmutató segítségével beállíthatók a futtatáshoz szükséges adatok.

### ❗ Fontos:

- Az environment fájlokban megadott **email-cím alapján ismeri fel a rendszer a root admint**. Ügyelj arra, hogy a `.env` fájlban (`EMAIL_USER`) megadott email-cím **megegyezzen** a root admin adatbázisba beszúrt email-címével.

- A frontend environment-ben található `rootAdminEmail` változó alapján a felhasználói felület képes felismerni, hogy az éppen bejelentkezett felhasználó a root admin.  
  ➤ **Ez az érték szintén egyezzen meg** a `.env`-ben szereplő `EMAIL_USER` értékkel, hogy a jogosultságokat megfelelően jelenítse meg a felhasználói felületen.

- A root admin jelszavának biztonságos kezeléséhez használd a backend mappában található `passwordgen.js` fájlt:
  - Írd be a kívánt jelszót a `bcrypt.hash('ide_írd_a_jelszót', 10)...` részbe.
  - Futtasd le terminálból:
     ```bash
     node passwordgen.js
     ```
  - A konzolban megjelenik a **besózott jelszó**, amit beilleszthetsz az SQL parancsba.

- Mivel a rendszer **nodemailer** segítségével küld emailt (regisztráció megerősítéséhez), **Google-fiókhoz tartozó app-jelszóra** lesz szükség. Ez a Google-fiók biztonsági beállításainál igényelhető.

---

### 🟦 Frontend environment (`src/environments/environment.ts`)

```ts
export const environment = {
  production: false,
  apiUrl: 'http://localhost:7096/api',
  rootAdminEmail: 'email@cim.com'
};
```

### 🟥 Backend `.env` fájl + SQL root admin beszúrás

```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=felhasznalonev
DB_PASSWORD=jelszo
DB_NAME=todo_app

JWT_SECRET=tokenNev_szuperTitkos
FRONTEND_URL=http://localhost:4200

EMAIL_USER=email@cim.com
EMAIL_PASS=appjelszo

API_PORT=3000
```

```sql
INSERT INTO Users (userName, password, email, isAdmin, isEmailVerified)
VALUES ('rootadmin', 'besozott_jelszo_ide', 'email@cim.com', true, true);
```

> 🔐 Az `email@cim.com` cím **meg kell egyezzen** az `.env` fájlban megadott `EMAIL_USER` értékével!

---

## Indítás
#### Adatbázis (XAMPP + phpMyAdmin)
1. Indítsd el a **XAMPP**-ot
   - `Start` gomb -> **Apache** és **MySQL**
   - **MySQL** → `Admin` gomb
2. phpMyAdmin-ban:
   - Navigálj a **Felhasználói fiókok** menüpontra
   - Hozz létre egy új felhasználót a `.env`-ben megadott adatok alapján
3. Nyisd meg az **SQL** fület, és illeszd be a projektben található **SQL dump fájl** tartalmát (`database.sql`)
#### Backend:
```
npm i
node app.js
```
#### Frontend:
```
npm i
ng serve -o
```
