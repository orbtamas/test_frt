# Leírás
Rövid leírás arról hogy mit csinál, hogyan működik az adott step.

# Paraméterek

- ***in***:
    - **test : String [1]** - a bemenő paraméter neve, típusa, számossága, leírása

- ***return***:
    - **test : String [1]** - a return paraméter neve, típusa, számossága, leírása

# Kivételek
1. A hiba leírása (esetleg megjelenítendő hibaüzenet)
```
//előfeltétel leírása OCL-ben, ami, ha nem teljesül, kiváltódik a kivétel. Például: 
pre: 
UserTask.allInstances()
->select(
    ut : Usertask |
    ut.entityId = entityKey and
    ut.entityType = entityType
).size() = 1
```

# Megszorítások
1. Megszorítás leírása. A megszorításnak minden időben igaznak kell lennie.
```
//a megszorítás leírása OCL-ben
inv: asd
```

# Lefutás
1. Az adott step lefutásának pontokba szedett leírása, lehetőség szerint kiegészítve formális leírással OCL-ben. **[OCL guide](https://wiki.eclipse.org/Acceleo/OCL_Operations_Reference)**
```
//példa
def: UT : Usertask = UserTask.allInstances()
->select(
    ut : Usertask |
    ut.entityId = entityKey and
    ut.entityType = entityType
)
->first()
```
3. VÉGE
