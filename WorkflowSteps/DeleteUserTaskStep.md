# Leírás
A deleteUserTask step adott modulon belül a kapott entitás azonosítóval és típussal azonosít egy entitáspéldányt és ha 
ahhoz usertask tartozik, azt törli.

# Paraméterek

- ***in***:
  - entityKey : String [1]
  - entityType : String [1]

- ***return***:
  - nincs

# Kivételek
1. UserTask nem található!
```
pre: 
UserTask.allInstances()
->select(
    ut : Usertask |
    ut.entityId = entityKey and
    ut.entityType = entityType
).size() = 1
```

# Megszorítások
```
context::deleteUserTaskStep
inv: asd
```

# Lefutás
1. Lekérdezés futtatása a usertask azonosításához.
```
def: UT : Usertask = UserTask.allInstances()
->select(
    ut : Usertask |
    ut.entityId = entityKey and
    ut.entityType = entityType
)
->first()
```
2. Első pontban azonosított **UT** törlése.
3. VÉGE
