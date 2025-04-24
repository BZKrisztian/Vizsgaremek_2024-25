# Statikus Tesztelés – Felhasználói műveletek dokumentációja

Ez a dokumentáció felsorolja az alkalmazás minden interaktív elemét (gomb, űrlap, dialógus), és részletezi azok működését – milyen API hívást indítanak vagy milyen logikát hajtanak végre a frontendben. Ez helyettesíti az automatikus teszteket statikus tesztelés esetén.

---

## 🧷 Entry oldal (`EntryComponent`)
- **Regisztráció gomb** → `/register` oldalra navigál.
- **Bejelentkezés gomb** → `/log-in` oldalra navigál.

---

## 📝 Regisztráció (`RegistrationComponent`)
- **Űrlap elküldése** (submit) → `POST /register` → regisztrál új felhasználót.
  - Email-cím és felhasználónév egyediség ellenőrzése backend oldalon.
  - Siker esetén snackbar + átirányítás `entry` oldalra.

---

## 🔐 Bejelentkezés (`LoginComponent`)
- **Űrlap elküldése** (submit) → `POST /login`
  - Token mentése localStorage-be
  - Sikeres bejelentkezés után:
    - **admin** → `overseer` oldalra irányít
    - **sima felhasználó** → `homepage` oldalra

---

## ✉️ Email megerősítés (`VerifyEmailComponent`)
- **Automatikus verifikáció URL token alapján** → `GET /verify-email?token=...`
- Sikeres/sikertelen visszajelzés és navigációs gombok.

---

## 🏠 Főoldal (`HomepageComponent`)
- **Felhasználói név megjelenítése**
- **Tasklist toggle gomb** → `showHomepageContent` változó váltása + localStorage update
- **Figyelmeztetés mára esedékes vagy lejárt feladatokra**
- **`<app-task-list>` megjelenítése** → lista CRUD és task CRUD lehetőségek

---

## 📋 TaskList komponens (`TaskListComponent`)
- **"Add TaskList" gomb** → megnyitja `TaskListDialog`-ot
- **"Edit TaskList" gomb** → megnyitja szerkesztő dialógust (adatátadás `@Input()` útján)
- **"Delete TaskList" gomb** → `ConfirmDelDialog` → ha elfogadva → `DELETE /tasklists/:id`
- **"Add Task" gomb** → `TaskDialog` megnyitása az adott listához
- **Keresősáv** → `taskFilter` pipe szűri az adott lista elemeit

---

## ✅ TaskItem komponens (`TaskItemComponent`)
- **Check button** → `task_Status` toggle → `PUT /tasks/:id`
- **Edit button** → megnyitja `TaskDialog`-ot szerkesztésre
- **Delete button** → `DELETE /tasks/:id`
- **"Show/Hide Dates" gomb** → `showDates` váltása (létrehozási/frissítési metaadatok)

---

## 🧍 Profil oldal (`ProfileComponent`)
- **Profiladatok frissítése** → `PATCH /users/profile`
- **"Delete Account" gomb** (Harakiri) → 2× `confirm()` JS prompt → `DELETE /users/self` (ha nem admin)

---

## 🧑‍💼 Overseer oldal (`OverseerComponent`)
- **Admin és mezei felhasználók listázása** → `GET /users`
- **"Inspect" gomb** → `GET /users/admin/users/:id/tasklists` → megnyit `InspectUserDialog`
- **"Promote/Demote" gomb** → `PATCH /users/:id/toggle-admin`
- **"Delete" gomb** → `DELETE /users/:id` (ha nem root vagy önmaga)

---

## 🧾 Dialógus komponensek
- `TaskDialog`:
  - **Mentés** → `POST` vagy `PUT /tasks`
  - **Mégse** → emit `cancel()`
- `TaskListDialog`:
  - **Mentés** → `POST` vagy `PUT /tasklists`
  - **Mégse** → emit `cancel()`
- `ConfirmDelDialog`:
  - **Igen/Nem gomb** → `mat-dialog-close(true/false)`
- `InspectUserDialog`:
  - Csak olvasható nézet — nem indít API-t frontendről

---

## 🔚 Fejléc (`AppComponent`)
- **Vissza gomb** → `location.back()`
- **Kijelentkezés gomb** → `logout()`: localStorage clear + redirect to `/entry`
- **Nyelvváltás EN/HU** → `switchLanguage()` + localStorage update
- **Wallpaper választó** → `changeWallpaper()` + háttér csere
- **Oldalváltás `Homepage` ↔ `Overseer`** → `switchPage()`

