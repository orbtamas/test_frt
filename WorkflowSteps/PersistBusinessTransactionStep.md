# Leírás
A PersistBusinessTransactionStep a [context](../Glossary/Glossary.md)ben szereplő businessTransaction alapján leperzisztálja az adatbázisba a businessTransaction példányt.

> **Az alábbi leírás nem teljes, ha erre jársz és nem találod, amit keresel, legyél szíves írd hozzá!.**

# Paraméterek

- ***in***:
  - **transactionStatus : TransactionStatus [1]** - Az az állapot, amellyel lementésre kerül a tranzakció.
  - **persistTransactionWithoutCustomValues : boolean [1]** - Ha értéke igaz, akkor az egyedi mezők értékei is mentésre kerülnek.
  - **transaction : Object [1]** - Az a contextben szereplő entitás, amit perzisztálni szeretnénk.
    - *inv: transaction = BusinessTransaction*
  - **disableAuditLoggingForPersistTransaction : boolean [1]** - ???
- ***return***:
  - **businessTransaction : BusinessTransaction [1]** - A lementett entitáspéldány.

# Kivételek
nincs

# Megszorítások
nincs

# Lefutás
1. A contextben szereplő *businessTransaction*-t a beállításoknak megfelelően lementi (inzertálja) az adatbázisba.
2. Az előző pontban lementett *businessTransaction*-t visszaolvassa és visszaadja.
3. VÉGE
