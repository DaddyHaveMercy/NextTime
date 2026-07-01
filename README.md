

Pour la regex VARCHAR, remplacer la version stricte par une qui rend `CHAR`/`BYTE` optionnel :

```python
elif re.match(r"^(VARCHAR2?|CHAR|NVARCHAR2?|NCHAR)\s*\(\s*\d+\s*(CHAR|BYTE)?\s*\)\s*$", ftype.upper()):
    return "V"
```

Ça couvre `VARCHAR2(254)`, `VARCHAR2(254 CHAR)`, `VARCHAR(50)`, `CHAR(10)`, etc.

Et le point critique, dans la grande branche `elif (...)` de l'image 3 : retirer les deux lignes

```python
or ftype[0:2] == "V "
or ftype[0:1] == "V"
```

Ce sont elles qui aspirent tout VARCHAR échappé vers `"N"`. Une fois retirées, un type non reconnu tombe dans le `else` final → `logger.warning("Unknown format")` + `return "V"`, ce qui est déjà le défaut sûr pour un VARCHAR.
