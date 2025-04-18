# 1. Bevezetés

A *To-Do, or not To-Do* alkalmazás egy biztonságos, nemzetközileg használható feladatkezelő rendszer, amelyet a vizsgaremek projekt keretében fejlesztettünk. Célunk egy olyan alkalmazás létrehozása volt, amely segíti a felhasználókat feladataik rendszerezésében, egyszerű kezelhetőséget és áttekinthető felületet biztosítva.

Az alkalmazás támogatja a felhasználói regisztrációt és bejelentkezést, e-mailes megerősítéssel, többnyelvűséget (magyar és angol), valamint adminisztrátori jogosultságokat. A rendszer reszponzív, modern felületet kínál és biztonságos adatkezelést valósít meg JWT tokenek és szerepkör-alapú hozzáférés alkalmazásával.

---

# 2. Felhasznált technológiák és fejlesztői környezet

## 🖥️ Fejlesztői környezet

- Visual Studio Code – a frontend és backend fejlesztéséhez
- XAMPP – a MySQL adatbázis kezeléséhez (phpMyAdmin)
- Insomnia / Postman – manuális API teszteléshez

## 🧩 Felhasznált technológiák

### Frontend (Angular):
- Angular (TypeScript)
- Angular Forms (Reactive Forms)
- RxJS
- Angular Material (dátumválasztó, gombok, dialógusok)
- ngx-translate – többnyelvűség támogatásához

### Backend (Node.js):
- Node.js + Express
- Sequelize ORM – MySQL adatbázis kezeléséhez
- jsonwebtoken – token alapú autentikációhoz
- bcrypt – jelszó titkosításhoz
- nodemailer – e-mail küldéshez

### Adatbázis:
- MySQL – relációs adatbázis-kezelés
- Sequelize – ORM réteg
- phpMyAdmin – grafikus felület az SQL szerkesztéséhez

---

# 3. Felhasználói dokumentáció

Az alábbi szakasz lépésről lépésre bemutatja, hogyan használhatja az alkalmazást egy alapfelhasználó vagy adminisztrátor. A működés egyszerű és intuitív, de részletes magyarázatot nyújtunk minden fontosabb funkcióról.

## 👤 Alapfelhasználó

### ▶️ 1. Belépő oldal (Entry)

Az alkalmazás elsőként a **belépő oldalt (entry)** jeleníti meg a látogatók számára.

Itt két lehetőség közül választhatunk:

- **Regisztráció** – új fiók létrehozása
- **Bejelentkezés** – meglévő fiók elérése

[KÉP: Entry oldal gombokkal]

### 📝 2. Regisztráció

A regisztrációs oldalon egy űrlapot kell kitölteni, amely a következő mezőket tartalmazza:

- Felhasználónév (minimum 6 karakter)
- E-mail cím (valós e-mail formátumban)
- Jelszó (legalább 8 karakter, egy nagybetű és egy szám kötelező)
- Jelszó megerősítése

A sikeres regisztráció után az alkalmazás egy visszaigazoló e-mailt küld a megadott címre.

[KÉP: Regisztrációs űrlap]

### ✉️ 3. E-mail megerősítés

A felhasználó az e-mailben található linkre kattintva megerősíti a fiókját.

Ezután beléphet az alkalmazásba.

[KÉP: Sikeres/hibás e-mail megerősítés nézet]

### 🔐 4. Bejelentkezés

Bejelentkezéskor az e-mail és jelszó megadása kötelező. A rendszer csak akkor enged be, ha az e-mail hitelesítve lett.

Sikeres belépés után a felhasználót átirányítja a **homepage** oldalra.

[KÉP: Bejelentkezési oldal]

### 🏠 5. Főoldal (Homepage)

A főoldal a felhasználó interaktív munkaterülete. Itt megjelennek a korábban létrehozott listák és azokhoz tartozó feladatok.

Lehetséges műveletek:

- Új feladatlista hozzáadása
- Feladatlista szerkesztése / törlése
- Feladat hozzáadása egy listához
- Feladat szerkesztése / törlése
- Feladat állapotának módosítása (kész / nem kész)
- Dátum, prioritás és státusz szerinti rendezés
- Keresés feladatonként listán belül

[KÉP: Homepage + feladatlista nézet]

### ⚙️ 6. Profil módosítás

A felhasználó a „Profilom” oldalon módosíthatja:

- Felhasználónevét
- E-mail címét
- Jelszavát (új megadásával)
- Saját fiókját törölheti is (harakiri gomb)

Törléskor figyelmeztetések jelennek meg. Adminok nem törölhetik saját magukat.

[KÉP: Profil szerkesztés és harakiri gomb]

## 🛡️ Admin felhasználó

### 🧭 1. Admin jogosultságok

Ha egy felhasználó admin jogosultsággal rendelkezik, a belépés után lehetősége van az **Overseer** oldal elérésére.

[KÉP: Admin gombok a fejlécben]

### 🧑‍💼 2. Overseer oldal

Az overseer nézetben az admin a következőket teheti meg:

- Admin és sima felhasználók listázása
- Felhasználók előléptetése adminná
- Admin jog visszavonása másoktól (kivéve saját magát)
- Felhasználók törlése (kivéve saját magát és root admint)
- Felhasználók részletes megtekintése (Inspect gomb)

[KÉP: Overseer oldal admin és user táblázattal]

Ez a rész részletesen bemutatta, hogyan használja egy felhasználó vagy admin az alkalmazást. A képek segítségével vizuálisan is könnyen követhető lesz a felület.

---

# 4. Fejlesztői dokumentáció

## 📁 Frontend – Angular

### Mappastruktúra és komponensek

- **components/** – Újrafelhasználható elemek (feladatok, listák, dialógusok)
    - `app.component` : Ez a komponens tartalmazza az egész alkalmazás alapvázát és a fejlécét (`<header>`), ahol minden globális gomb és állapotérzékeny logika helyet kap.
    - `task-item/TaskItemComponent`: Egyetlen feladat vizuális megjelenítése és műveletei, mint státuszváltás, szerkesztés, törlés, valamint dátumok megtekintése.
    - `task-list/TaskListComponent`: A felhasználó összes feladatlistájának betöltése és kezelése.
    - `userlist/UserlistComponent`: Az admin oldal felhasználókezelő táblázata.
    - `login/LoginComponent`: Bejelentkezési űrlap Reactive Form alapú validációval, szerver oldali visszajelzésekkel.
    - `registration/RegistrationComponent`: Regisztrációs űrlap validátorokkal, jelszó-ellenőrzéssel és hibakezeléssel.
    - `profile/ProfileComponent`: A felhasználó profiljának szerkesztésére szolgáló űrlap, frissítési lehetőséggel és harakiri törléssel.
- **dialog-components/** – Interaktív felugró ablakok.
    - `dialog-comps/taskdialog/TaskdialogComponent`: Új feladat létrehozása vagy meglévő szerkesztése dialógusban.
    - `dialog-comps/tasklistdialog/TasklistdialogComponent`: Új feladatlista létrehozása vagy szerkesztése dialógusban.
    - `dialog-comps/confirmdeldialog/ConfirmdeldialogComponent`: Törlés megerősítésére szolgáló dialógus.
    - `dialog-comps/inspectuserdialog/InspectUserDialogComponent`: Egy kiválasztott felhasználó összes listájának és feladatának megjelenítése (admin funkció).

- **pages/** – Teljes oldalak, router útvonalakhoz rendelve
    - `entry/EntryComponent`: Belépő oldal, ahol a felhasználó regisztrálhat vagy bejelentkezhet.
    - `register/RegisterComponent`: Statikus oldal, amely kizárólag a `RegistrationComponent`-et jeleníti meg komponensként.
    - `log-in/LogInComponent`: Statikus oldal, amely a `LoginComponent`-et ágyazza be.
    - `my-profile/MyProfileComponent`: Statikus oldal, amely a `ProfileComponent`-et jeleníti meg.
    - `verify-email/VerifyEmailComponent`: A hitelesítő link visszaigazolása után megjelenő oldal.
    - `homepage/HomepageComponent`: A fő felhasználói felület, listák és feladatok megjelenítésével.
    - `overseer/OverseerComponent`: Admin felhasználók által elérhető oldal, ahol más felhasználók kezelhetők.

- **services/**
    - `auth.service.ts`: Autentikáció, tokenkezelés, aktuális felhasználó, admin jogok.
    - `task.service.ts`: Task és tasklista CRUD műveletek, API-kommunikáció.

- **pipes/**
    - `sortbypriority.pipe.ts`: Feladatok rendezése prioritás szerint.
    - `duedate.pipe.ts`: Lejárati dátum alapján rendezés.
    - `completionstatus.pipe.ts`: Befejezett feladatok rendezése.
    - `taskFilter.pipe.ts`: Feladatok szűrése.

- **guards/**
    - `auth.guard.ts`: Megakadályozza a belépést, ha a felhasználó nincs bejelentkezve.
    - `admin.guard.ts`: Megakadályozza az overseer oldal elérését nem-admin felhasználóknak.

- **interceptors/**
    - `auth.interceptor.ts`: Minden HTTP kéréshez automatikusan hozzáadja az Authorization tokent.

- **models/** – Típusdefiníciók
    - `task.model.ts`: Task objektum tulajdonságai.
    - `tasklist.model.ts`: TaskList objektum tulajdonságai.
    - `user.model.ts`: User objektum tulajdonságai.

### Metódusok

- **App.component.ts**
    - `constructor()`: Betölti az alkalmazás nyelvét (`Language` localStorage alapján) és alkalmazza a kiválasztott háttérképet.
    - `switchLanguage(lang)`: Átváltja az alkalmazás nyelvét és elmenti a localStorage-be.
    - `applyWallpaper(wallpaper)`: Beállítja a háttérképet a body style segítségével.
    - `changeWallpaper(file)`: Beállítja az új háttérképet és elmenti a localStorage-be.
    - `handleWallpaperChange(event)`: Eseménykezelő, amely meghívja a háttérképváltó függvényt.
    - `LogInCheck()`: Visszaadja, hogy van-e bejelentkezett felhasználó (UI feltételekhez használva).
    - `AdminCheck()`: Ellenőrzi, hogy a felhasználó admin-e (pl. overseer gomb megjelenítéséhez).
    - `switchPage()`: Átvált a homepage ↔ overseer oldalak között.
    - `logoutFromSite()`: Teljes kijelentkezést végez, majd újratölti az alkalmazást.
    - `goBack()`: Visszanavigál az előző oldalra.

#### Services
- **auth.service.ts**
    - `getRootAdminEmail()`: Visszaadja a környezeti változóban megadott root admin e-mail címet.
    - `getCurrentUser()`: Visszaadja a jelenleg bejelentkezett felhasználót a BehaviorSubject-ből.
    - `refreshCurrentUser()`: Frissíti a felhasználó adatait a szerverről, és frissíti a localStorage-t is.
    - `register(userData)`: POST kérés segítségével regisztrálja az új felhasználót.
    - `login(credentials)`: Bejelentkezteti a felhasználót, elmenti a tokent és a felhasználót.
    - `getToken()`: Visszaadja a localStorage-ben tárolt JWT tokent.
    - `isLoggedIn()`: Ellenőrzi, hogy van-e érvényes token.
    - `logout()`: Eltávolítja a felhasználói adatokat és tokent a localStorage-ből, kijelentkezteti a felhasználót.
    - `getUsers()`: Lekéri az összes felhasználót az admin oldal számára.
    - `deleteUser(user_Id)`: Törli az adott user_Id-hoz tartozó felhasználót.
    - `harakiri()`: Saját fiók törlése (DELETE /users/self).
    - `toggleAdmin(user_Id)`: Felhasználó admin státuszának módosítása.
    - `updateSelf(data)`: A felhasználó saját adatainak frissítése (név, e-mail, jelszó).

- **task.service.ts**
    - `getTasks(taskList_Id)`: Lekéri az adott feladatlistához tartozó összes feladatot.
    - `addTask(task)`: Új feladatot hoz létre az adott listában.
    - `updateTask(updatedTask)`: Frissíti az adott feladat adatait.
    - `deleteTask(task_Id)`: Törli a megadott azonosítójú feladatot.
    - `getTaskLists()`: Lekéri az aktuális felhasználóhoz tartozó összes feladatlistát.
    - `addTaskList(tasklist)`: Új feladatlistát hoz létre.
    - `updateTaskList(updatedTaskList)`: Frissíti az adott listát.
    - `deleteTaskList(list_Id)`: Törli az adott listát és hozzá tartozó feladatokat.
    - `getTasklistsForAdmin(userId)`: Adminként lekéri az adott user összes feladatlistáját és feladatait.

#### Guard-ok
- **auth.guard.ts**
    - `canActivate(route, state)`: Ellenőrzi, hogy a felhasználó be van-e jelentkezve; ha nem, automatikusan visszairányítja az `/entry` oldalra.

- **admin.guard.ts**
    - `canActivate(route, state)`: Ellenőrzi, hogy a bejelentkezett felhasználó rendelkezik-e admin jogosultsággal; ha nem, visszairányítja a felhasználót(`/homepage` ha be van jelentkezve, `/entry` ha nem).

#### Interceptor
- **auth.interceptor.ts**
    - `intercept(req, next)`: Módosítja az összes HTTP kérést úgy, hogy ha van érvényes token, hozzáadja az `Authorization` fejlécet a `Bearer` tokennel, majd továbbküldi.

#### Pipes
- **sortbypriority.pipe.ts**
    - `transform(tasks)`: Rendez egy feladatlistát prioritás alapján (high → medium → low).

- **duedate.pipe.ts**
    - `transform(tasks)`: A mai napra vagy lejárt határidejű feladatokat sorolja előre.
    - `isSameDayorNot(date1, date2)`: Belső segédfüggvény a pontos napon belüli összehasonlításhoz.

- **completionstatus.pipe.ts**
    - `transform(tasks)`: A befejezetlen feladatokat helyezi a lista elejére.

- **taskFilter.pipe.ts**
    - `transform(tasks, searchTerm)`: Visszaadja azokat a feladatokat, melyek címe tartalmazza a keresett kifejezést (kis- és nagybetű érzéketlen).


#### Oldal-komponensek
- **homepage.component.ts**
    - `ngOnInit()`: Betölti a felhasználó nevét és a feladatlisták állapotát a localStorage-ből.
    - `check4TasksDueToday()`: Lekérdezi, hogy van-e mára esedékes vagy lejárt feladat.
    - `toggleTaskListsVisibility()`: Elrejti vagy megjeleníti a tasklistákat, és menti a localStorage-be.
    - `ngOnDestroy()`: Megszakítja az aktív observable-ket memóriazárás elkerülésére.

- **verify-email.component.ts**
    - `ngOnInit()`: Lekérdezi az URL-ből a token értékét, majd hitelesíti a felhasználót a szerveren keresztül.

- **overseer.component.ts**
    - `ngOnInit()`: Inicializálja a komponenst, de nem tartalmaz külön logikát.
    - `logout()`: Kijelentkezteti a felhasználót, majd visszairányítja az entry oldalra.

- **entry.component.ts**
    - Statikus oldal, semmilyen logikai műveletet nem tartalmaz.

- **register.component.ts**
    - Statikus oldal, semmilyen logikai műveletet nem tartalmaz.

- **log-in.component.ts**
    - Statikus oldal, semmilyen logikai műveletet nem tartalmaz.

- **my-profile.component.ts**
    - Statikus oldal, semmilyen logikai műveletet nem tartalmaz.


#### Dialog-Komponensek
- **taskdialog.component.ts**
    - `ngOnInit()`: Inicializálja az új task objektumot létrehozáshoz vagy szerkesztéshez.
    - `ngOnChanges(changes)`: Ha a komponens bemeneti értékei megváltoznak, beállítja a `localTask` értékét szerkesztéshez vagy inicializálja új létrehozáshoz.
    - `onSave()`: Beállítja a dátumokat és kibocsátja a `save` eseményt a `localTask` objektummal.
    - `onCancel()`: Kibocsátja a `cancel` eseményt, bezárva a dialógust.

- **tasklistdialog.component.ts**
    - `ngOnInit()`: Inicializálja a `localTaskList` objektumot új lista létrehozásához vagy módosításához.
    - `ngOnChanges(changes)`: Beállítja a `localTaskList` értékeit szerkesztés esetén, vagy alapértelmezett értékre állítja létrehozáshoz.
    - `onSave()`: Frissíti a `update_Date` mezőt és kibocsátja a `save` eseményt.
    - `onCancel()`: Kibocsátja a `cancel` eseményt.

- **confirmdeldialog.component.ts**
    - `ngOnInit()`: Nincs logika benne, csak lifecycle hook-ként szerepel.
    - `onDeleteConfirm()`: Bezárja a dialógust `true` értékkel, jelezve a törlés jóváhagyását.
    - `onCancel()`: Bezárja a dialógust `false` értékkel, jelezve a megszakítást.

- **inspectuserdialog.component.ts**
    - `ngOnInit()`: Lekéri az adott felhasználóhoz tartozó összes feladatlistát és azokhoz tartozó feladatokat adminként.

#### Komponensek
- **task-item.component.ts**
    - `toggleViewDate()`: Megjeleníti vagy elrejti a dátum mezőket (létrehozás, frissítés).
    - `completionToggle()`: Átváltja a feladat státuszát kész / nem kész állapotra, majd frissíti.
    - `edit()`: Kibocsátja az `editTask` eseményt, hogy megnyissa a szerkesztő dialógust.
    - `delete()`: Kibocsátja a `deletedTask` eseményt, hogy törölje az adott feladatot.
    - `get isExpired`: Megvizsgálja, hogy a feladat határideje korábbi-e a mai napnál.

- **task-list.component.ts**
    - `ngOnInit()`: Betölti a felhasználó összes tasklistáját és azok feladatait inicializáláskor.
    - `ngOnDestroy()`: Lezárja az összes előfizetést a memóriafolyás elkerüléséhez.
    - `loadTaskLists()`: Lekéri az összes feladatlistát a szerverről, majd egyesével betölti a hozzájuk tartozó feladatokat.
    - `loadTasks(list_Id)`: Lekéri és elmenti az adott listához tartozó feladatokat.
    - `onTaskToggle(task, list_Id)`: Frissíti a feladat státuszát, majd újratölti a listát.
    - `openTaskDialog4Edit(task)`: Beállítja az adott feladatot szerkesztésre és megnyitja a szerkesztő dialógust.
    - `openTaskDialog4Add(taskList_Id)`: Új feladat hozzáadását indítja el az adott listához.
    - `onTaskDialogSave(task)`: Új feladatot ad hozzá vagy meglévőt frissít a szerkesztő dialógusból, majd újratölti a listát.
    - `closeTaskDialog()`: Bezárja a feladat dialógust és visszaállítja az átmeneti értékeket.
    - `openTaskListDialog4Edit(tasklist)`: Megnyitja az adott lista szerkesztő dialógusát.
    - `openTaskListDialog4Add()`: Előkészíti az új lista létrehozását dialóguson keresztül.
    - `onTaskListDialogSave(taskList)`: Ment egy új vagy szerkesztett listát, majd újratölti az összeset.
    - `closeTaskListDialog()`: Bezárja a lista dialógust és visszaállítja az állapotot.
    - `onTaskDeletion(list_Id, task_Id)`: Törli az adott feladatot és frissíti a listát.
    - `onTaskListDeletion(list_Id)`: Megerősítő dialógus után törli a teljes listát és újratölti a tasklistákat.

- **userlist.component.ts**
    - `ngOnInit()`: Betölti az összes admin jogosultságú felhasználót az oldal megnyitásakor.
    - `ngOnDestroy()`: Leállítja az összes aktív observable-t memóriazárás elkerülése végett.
    - `isAdmin()`: Visszaadja, hogy az aktuális felhasználó rendelkezik-e admin jogokkal.
    - `isRootAdmin()`: Ellenőrzi, hogy az aktuális felhasználó a root admin-e (`EMAIL_USER` alapján).
    - `loadAdmins()`: Lekéri az összes admin státuszú felhasználót a szervertől.
    - `loadRegularUsers()`: Lekéri az összes nem-admin felhasználót.
    - `refreshAllUsers()`: Teljes újratöltést végez az összes felhasználóra (admin + mezei), ha már egyszer betöltöttük őket.
    - `deleteUser(user_Id)`: Megerősítés után törli az adott user_Id-hoz tartozó felhasználót.
    - `toggleAdminState(user_Id)`: Admin státuszt vált az adott felhasználón (előléptetés vagy lefokozás).
    - `inspectUser(userId)`: Megnyitja a kiválasztott felhasználó tasklistáit és taskjait megjelenítő dialógust.
    - `filteredAdmins()`: A beírt keresőkifejezés alapján szűri az admin felhasználók listáját.
    - `filteredRegularUsers()`: A beírt keresőkifejezés alapján szűri a mezei felhasználók listáját.
    - `matchesSearchTerm(user)`: Megvizsgálja, hogy a felhasználónév vagy email tartalmazza-e a keresett kifejezést (kisbetűre konvertálva).

- **profile.component.ts**
    - `ngOnInit()`: Inicializálja az űrlapot az aktuális felhasználó adataival.
    - `ngOnDestroy()`: Lezárja az összes aktív observable-t memóriazárás elkerülése érdekében.
    - `passwordsMustMatch(group)`: Ellenőrzi, hogy az új jelszó és a megerősítő mező egyezik-e (validátor).
    - `onSubmit()`: Profiladatok frissítését végzi, hibakezeléssel és visszajelzéssel.
    - `onClickDeleteAccount()`: Harakiri funkció, saját fiók törlésének megerősítése és végrehajtása.

- **registration.component.ts**
    - `ngOnInit()`: Inicializálja a regisztrációs űrlapot validátorokkal.
    - `ngOnDestroy()`: Megszünteti az előfizetéseket memóriazárás elkerülésére.
    - `passMustMatch(passwordKey, confirmPasswordKey)`: Egyéni validátor, amely ellenőrzi a jelszó egyezését.
    - `onSubmit()`: Elküldi a regisztrációs adatokat a szerverre, hibakezeléssel és sikerüzenettel.

- **login.component.ts**
    - `ngOnInit()`: Inicializálja a bejelentkezési űrlapot validátorokkal.
    - `ngOnDestroy()`: Törli az aktív observable-t a komponens megszűnésekor.
    - `onSubmit()`: Bejelentkezési kérelmet küld, siker esetén átirányítja a felhasználót, hiba esetén snackbart jelenít meg.


### Osztályszintű változók

#### Services
- **auth.service.ts**
    - `currentUser$`: A bejelentkezett felhasználót tároló `BehaviorSubject`, amely minden komponens számára elérhető.
    - `tokenKey`: A localStorage kulcs, amely alatt a JWT token tárolódik.
    - `userKey`: A localStorage kulcs, amely alatt a felhasználói objektum tárolódik.
    - `baseUrl`: Az API alap URL-je az environment fájlból.

- **task.service.ts**
    - `baseUrl`: Az API végpontok alap URL-je az environment fájlból.

#### Guard-ok
- **auth.guard.ts**
    - Nincsenek osztályszintű változók, csakis `constructor`: Ami az `AuthService`-et és `Router`-t injektálja a guard működéséhez.

- **admin.guard.ts**
    - Nincsenek osztályszintű változók, csakis `constructor`: Ami az `AuthService`-et és `Router`-t injektálja a guard működéséhez.

#### Interceptor
- **auth.interceptor.js**
    - Nincsenek osztályszintű változók, csakis `constructor`: Ami az `AuthService` példányát használja a token eléréséhez.


#### Oldal-komponensek
- **homepage.component.ts**
    - `userName`: Az aktuális felhasználó neve, amelyet a fejlécen jelenítünk meg.
    - `taskListVisible`: Boolean flag, amely vezérli a feladatlisták megjelenítését (toggle).

- **verify-email.component.ts**
    - `message`: Változó, amely a hitelesítés eredményének megfelelő üzenetet tárolja.

- **my-profile.component.ts**
    - Nincsenek osztályszintű változók; az oldal statikus szerepet tölt be.

- **entry.component.ts**
    - Nincsenek osztályszintű változók; az oldal statikus szerepet tölt be.

- **overseer.component.ts**
    - Nincsenek osztályszintű változók; az oldal statikus szerepet tölt be.

- **register.component.ts**
    - Nincsenek osztályszintű változók; az oldal statikus szerepet tölt be.

- **log-in.component.ts**
    - Nincsenek osztályszintű változók; az oldal statikus szerepet tölt be.

#### Dialog-komponensek

- **taskdialog.component.ts**
    - `@Input() taskToEdit`: A szerkesztésre kapott Task objektum.
    - `@Input() isVisible`: Boolean flag a dialógus láthatóságához.
    - `@Output() save`: Esemény új vagy szerkesztett Task mentéséhez.
    - `@Output() cancel`: Esemény a dialógus bezárására.
    - `localTask`: A lokálisan használt Task objektum (másolat).

- **tasklistdialog.component.ts**
    - `@Input() taskListToEdit`: A szerkesztendő lista adatai.
    - `@Input() isVisible`: Boolean flag a dialógus megjelenítéséhez.
    - `@Output() save`: Esemény mentéskor.
    - `@Output() cancel`: Esemény megszakításkor.
    - `localTaskList`: A szerkesztett vagy újonnan létrehozott TaskList lokális példánya.

- **confirmdeldialog.component.ts**
    - `@Input() isVisible`: Boolean flag, a dialógus láthatóságát vezérli.
    - `@Output() close`: Esemény, amely a felhasználó válaszát (`true` vagy `false`) adja vissza.

- **inspectuserdialog.component.ts**
    - `@Input() userId`: A megjelenítendő felhasználó azonosítója.
    - `taskLists`: Az adott felhasználó tasklistái.
    - `taskMap`: Minden listához tartozó feladatok.

#### Komponensek

- **task-item.component.ts**
    - `@Input() task`: A megjelenítendő feladat adatai.
    - `@Output() deletedTask`: Esemény, amelyet a törlés indít el.
    - `@Output() editTask`: Esemény, amelyet a szerkesztés indít el.
    - `@Output() toggleTask`: Esemény, amely a státusz változását jelzi.
    - `showDates`: Boolean flag a metaadatok (létrehozás/frissítés) megjelenítéséhez.

- **task-list.component.ts**
    - `taskLists`: Az aktuális felhasználóhoz tartozó listák tömbje.
    - `taskMap`: Egy Map, amely minden listához hozzárendeli a benne lévő feladatokat.
    - `isTaskDialogOpen`: Boolean flag, jelzi, hogy nyitva van-e a task dialógus.
    - `selectedTask`: A kiválasztott feladat objektuma szerkesztéshez.
    - `selectedListId`: Az aktuális taskList azonosító, amihez új task készül.
    - `isTaskListDialogOpen`: Boolean flag a lista dialógus nyitottságáról.
    - `selectedTaskList`: Az aktuálisan szerkesztendő taskList objektum.
    - `isConfirmDialogOpen`: Boolean flag a törlés megerősítő dialógushoz.
    - `listIdToDelete`: A törlésre jelölt lista azonosítója.

- **userlist.component.ts**
    - `admins`: Az admin jogosultságú felhasználók tömbje.
    - `regularUsers`: A nem-admin felhasználók tömbje.
    - `searchTerm`: A keresési kifejezés szövege.
    - `usersLoaded`: Boolean flag, jelzi, hogy a felhasználók már be lettek töltve.

- **registration.component.ts**
    - `registrationForm`: Reactive Form objektum az új felhasználók regisztrációjához.
    - `destroy$`: Observable tisztításához használt `Subject`.

- **login.component.ts**
    - `loginForm`: Reactive Form objektum bejelentkezéshez szükséges mezőkkel.
    - `destroy$`: Observable lezárására szolgál, hogy elkerüljük a memóriazárást.

- **profile.component.ts**
    - `profileForm`: Reactive Form objektum a felhasználói adatok szerkesztésére.
    - `destroy$`: Observable megszüntetéséhez használt `Subject`, memóriazárás elkerülésére.


## Backend

### Mappastruktúra

- `app.js`: A teljes szerver belépési pontja; konfigurálja a middleware-ket, route-okat és kapcsolódik az adatbázishoz.

- **config/**
    - `database.js`: A Sequelize adatbáziskapcsolat beállításai `.env` alapján.

- **controllers/**
        - `authController.js`: Regisztráció, bejelentkezés és e-mail hitelesítés üzleti logikája.
    - `taskController.js`: Feladatok (task) létrehozásának, frissítésének, törlésének kezelése.
    - `tasklistController.js`: Feladatlisták létrehozása, frissítése, törlése.
    - `userController.js`: Felhasználók listázása, törlése, admin státusz kezelése, saját profil frissítése.

- **middleware/**
    - `auth.js`: JWT token ellenőrzése minden védett végpont előtt.
    - `admin.js`: Ellenőrzi, hogy a bejelentkezett felhasználó rendelkezik-e admin jogokkal.
    - `fetchUstate.js`: Beolvassa a felhasználó adatait a token alapján és hozzáfűzi a `req.user` objektumhoz.

- **models/**
    - `user.js`: A `Users` tábla Sequelize modellje.
        - Meghatározza a `Users` tábla mezőit: user_Id, userName, email, password, isAdmin, isEmailVerified, stb.
        - Nem tartalmaz egyedi metódusokat; csak struktúrát és típusokat definiál.
    - `task.js`: A `Tasks` tábla Sequelize modellje.
        - Meghatározza a `Tasks` tábla mezőit: task_Id, taskList_Id, title, description, status, priority, dueDate, owner_Id.
        - Nem tartalmaz logikai metódust, csak meződefiníciót és relációkat.
    - `tasklist.js`: A `TaskLists` tábla Sequelize modellje.
        - Meghatározza a `TaskLists` tábla mezőit: list_Id, title, description, dates, color, owner_Id.
        - Nem tartalmaz saját metódusokat, csak asszociációt a User és Task modellekkel.



- **routes/**
    - `authRoutes.js`: Bejelentkezéssel és regisztrációval kapcsolatos végpontok.
    - `taskRoutes.js`: Feladatokat kezelő végpontok.
    - `tasklistRoutes.js`: Feladatlistákhoz tartozó végpontok.
    - `userRoutes.js`: Felhasználókezelés (CRUD, admin funkciók, profil).

- `database.sql`: Az SQL dump fájl, amely létrehozza a szükséges adatbázis-táblákat és kapcsolatokat.

### Metódusok

- **app.js**
    - `app.use(...)`: Beállítja a szükséges middleware-ket (CORS, JSON body parser, route-ok).
    - `sequelize.sync(...)`: Szinkronizálja a Sequelize modelleket az adatbázissal (alapértelmezetten nem törli az adatokat).
    - `app.listen(...)`: Elindítja a szervert a megadott porton (`process.env.API_PORT`).

#### Kontrollerek

- **authController.js**
    - `register(req, res)`: Új felhasználót regisztrál az adatbázisba, ellenőrzi az e-mailt és nevet, majd megerősítő e-mailt küld; siker esetén HTTP 201 státuszkóddal válaszol.
    - `login(req, res)`: Ellenőrzi a bejelentkezési adatokat, érvényesíti a jelszót, generál JWT tokent; siker esetén visszaküldi a tokent és felhasználói adatokat (HTTP 200).
    - `verifyEmail(req, res)`: Feldolgozza az e-mailben kapott token alapján történő hitelesítést; visszaigazolásként sikeres vagy hibás státuszt küld.

- **taskController.js**
    - `getTasks(req, res)`: Lekéri az aktuális felhasználóhoz és adott listához tartozó összes feladatot; siker esetén visszaküldi JSON tömbként.
    - `addTask(req, res)`: Új feladatot hoz létre az adatbázisban, az aktuális felhasználóhoz rendelve.
    - `updateTask(req, res)`: Frissíti a megadott feladatot (azonosító alapján), módosítja a mezőket és frissíti a `update_Date` értékét.
    - `deleteTask(req, res)`: Törli az adott `task_Id`-hoz tartozó feladatot az adatbázisból.

- **tasklistController.js**
    - `getTaskLists(req, res)`: Lekéri az aktuális felhasználó összes tasklistáját `owner_Id` alapján.
    - `addTaskList(req, res)`: Létrehoz egy új feladatlistát az aktuális felhasználónak, beállítja az időbélyegeket.
    - `updateTaskList(req, res)`: Frissíti az adott azonosítójú listát, és frissíti az `update_Date` mezőt.
    - `deleteTaskList(req, res)`: Törli az adott listát, valamint automatikusan törli a hozzá tartozó taskokat is (ON DELETE CASCADE).

- **userController.js**
    - `getUsers(req, res)`: Lekéri az összes felhasználót, kizárólag admin számára.
    - `deleteUser(req, res)`: Azonosító alapján törli a megadott felhasználót, kivéve ha önmaga vagy root admin.
    - `toggleAdmin(req, res)`: Módosítja egy adott felhasználó admin státuszát (admin ↔ nem admin).
    - `getTasklistsForAdmin(req, res)`: Admin jogosultsággal megtekinti egy másik felhasználó összes listáját és taskját.
    - `getSelf(req, res)`: Visszaadja az aktuálisan bejelentkezett felhasználó adatait.
    - `deleteSelf(req, res)`: A bejelentkezett felhasználó saját fiókjának törlését hajtja végre (ha nem admin).
    - `updateSelf(req, res)`: Frissíti az aktuális felhasználó adatait (név, email, jelszó), ha megadott értékek különböznek a régiektől.

#### Middleware-ek

- **auth.js**
    - `verifyToken(req, res, next)`: Ellenőrzi, hogy a kérés tartalmaz-e érvényes JWT tokent; ha igen, engedélyezi a továbblépést, különben 401-es hibát küld vissza.

- **admin.js**
    - `isAdmin(req, res, next)`: Megvizsgálja, hogy a felhasználó rendelkezik-e `isAdmin` jogosultsággal; ha nem, HTTP 403-as válasszal megtagadja a hozzáférést.

- **fetchUstate.js**
    - `fetchUserState(req, res, next)`: A JWT token alapján lekéri a felhasználó adatait és hozzáfűzi a `req.user` objektumhoz a további middleware és vezérlők számára.


### Osztályszintű változók

- **app.js**
    - `express`, `cors`: A backend alap működéséhez szükséges külső könyvtárak.
    - `app`: Az Express alkalmazás példánya.
    - `dotenv`: A `.env` változók betöltéséhez szükséges modul.
    - `sequelize`: A `config/database.js` által exportált, szinkronizált adatbázis kapcsolat.

- **database.js**
    - `Sequelize`: A Sequelize fő objektum importálása az ORM kezeléséhez.
    - `sequelize`: Egy konfigurált Sequelize példány, amelyet az egész alkalmazás használ.

#### Kontrollerek

- **authController.js**
    - `User`: A Sequelize `User` modell importja, amelyet a regisztráció és belépés során használunk.
    - `bcrypt`: A jelszó hash-eléshez használt külső modul.
    - `jwt`: A JSON Web Token generálásához használt modul.
    - `nodemailer`: Külső könyvtár, amely az e-mail küldést végzi.
    - `EMAIL_USER`, `EMAIL_PASS`: A környezeti változókból kiolvasott e-mail fiók adatok.

- **taskController.js**
    - `Task`: A Sequelize Task modell, amely a feladatokkal kapcsolatos adatbázisműveleteket végzi.

- **tasklistController.js**
    - `TaskList`: A Sequelize TaskList modell.
    - `Task`: Az összes hozzá tartozó Task rekord törlésének biztosításához is használatos.

- **userController.js**
    - `User`: A Sequelize `User` modell.
    - `TaskList`, `Task`: Az admin felhasználók által más felhasználók feladatainak megtekintéséhez.
    - `ROOT_ADMIN_EMAIL`: A környezeti változó, amely alapján az alkalmazás felismeri a root admint.

#### Middleware-ek

- **auth.js**
    - `jwt`: A JSON Web Token dekódolásához szükséges modul.
    - `JWT_SECRET`: A környezeti változóból olvasott titkos kulcs.

- **admin.js**
    - Nincsen külön változó deklarálva.

- **fetchUstate.js**
    - `User`: A Sequelize modell, amelyből a token alapján betölti a felhasználói adatokat.

## Végponttábla

| Végpont                                  | Metódus | Auth szükséges | Leírás                                                                  |
|------------------------------------------|---------|----------------|-------------------------------------------------------------------------|
| /api/register                            | POST    | Nem            | Új felhasználó regisztrálása és e-mail megerősítés indítása.           |
| /api/login                               | POST    | Nem            | Bejelentkezés e-mail + jelszó párossal, JWT token generálása.          |
| /api/verify-email                        | GET     | Nem            | Felhasználó e-mail címének hitelesítése token alapján.                 |
| /api/tasklists                           | GET     | Igen           | Aktuális felhasználó tasklistáinak lekérése.                           |
| /api/tasklists                           | POST    | Igen           | Új tasklist létrehozása.                                               |
| /api/tasklists/:id                       | PUT     | Igen           | Megadott tasklist adatainak frissítése.                                |
| /api/tasklists/:id                       | DELETE  | Igen           | Megadott tasklist törlése (taskokkal együtt).                          |
| /api/tasks                               | GET     | Igen           | Feladatok lekérése megadott taskList_Id alapján.                       |
| /api/tasks                               | POST    | Igen           | Új feladat létrehozása adott listához.                                 |
| /api/tasks/:id                           | PUT     | Igen           | Megadott feladat adatainak frissítése.                                 |
| /api/tasks/:id                           | DELETE  | Igen           | Megadott feladat törlése.                                              |
| /api/users                               | GET     | Igen (admin)   | Az összes felhasználó lekérése (csak admin számára).                   |
| /api/users/:id                           | DELETE  | Igen (admin)   | Felhasználó törlése admin által (root admin nem törölhető).            |
| /api/users/:id/toggle-admin              | PATCH   | Igen (admin)   | Admin státusz módosítása (előléptetés vagy lefokozás).                 |
| /api/users/self                          | DELETE  | Igen           | Saját fiók törlése (admin nem törölheti magát).                        |
| /api/users/profile                       | PATCH   | Igen           | Saját fiók adatainak módosítása (név, jelszó, e-mail).                 |
| /api/users/me                            | GET     | Igen           | Aktuálisan bejelentkezett felhasználó adatainak lekérése.             |
| /api/users/admin/users/:id/tasklists     | GET     | Igen (admin)   | Egy adott felhasználó tasklistjeinek és feladatainak lekérése adminként. |

# 5. Tesztelés

A backend végpontokat **Insomnia** segítségével manuálisan teszteltük. Lefedésre kerültek a regisztráció, belépés, feladat- és lista műveletek, valamint az admin funkciók — minden fontos útvonal kipróbálásra került. A teljes tesztkészlet külön `.json` fájlban van/lesz csatolva.

