## SQL Injection

how does this work?

<div style="width:45%;display:inline-block">

you have this sql somewhere

```sql
SELECT * FROM `users` 
WHERE 
    `username` = '${uname}' 
    AND 
    `password` = '${passwd}';
```

</div>

<div style="width:45%;display:inline-block">

user enters<br/>
`uname` = `some@one.de`<br/>
`passwd` = `s3CR3t!`

```sql
SELECT * FROM `users` 
WHERE 
    `username` = 'some@one.de' 
    AND 
    `password` = 's3CR3t!`
```

</div>

--

## SQL Injection Exploit

<div style="width:45%;display:inline-block">

the SQL

```sql
SELECT * FROM `users` 
WHERE 
    `username` = '${uname}' 
    AND 
    `password` = '${passwd}';
```

</div>

<div style="width:45%;display:inline-block">

user enters<br/>
`uname` = `' OR 1 = 1 /*`<br/>
`passwd` = `doesnt matter anymore`

```sql
SELECT * FROM `users` 
WHERE 
    `username` = '' OR 1 = 1 /*' 
    AND 
    `password` = 'doesnt matter anymore';
```

</div>

--

## SQL Injection - what to do?

[SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html#defense-option-3-allow-list-input-validation)

---

## XSS - how does this work

```html
<input name="search" id="searchfield" value="" />
<p id="searchfor"></p>
<section id="result"></section>

<script>
    const searchfield = document.querySelector('#searchfield');
    const searchfor = document.querySelector('#searchfor');
    const resultField = document.querySelector('#result')

    async function onSearch(){
        const searchTerm = searchfield.value;
        searchfor.innerHTML = `Sie suchten nach "${searchTerm}"!`;
        const response = await fetch(`${searchUrl}?q=${searchTerm}`);
        resultField.innerHTML = await response.text();
    }
</script>
```

--

## XSS - happy path + exploit

user searches `apple` and submits

```html
<input name="search" id="searchfield" value="apple" />
<p id="searchfor">Sie suchten nach "apple"!</p>
```

<div class="fragment">

user searches `wtf<img src=x onerror=alert('cool') />` ...

```html
<input name="search" id="searchfield" value="wtf<img src=x onerror=alert('cool') />" />
<p id="searchfor">Sie suchten nach "wtf<img src=x onerror=alert('cool') />"!</p>
```

</div>

--

## XSS prevention

[Cross Site Scripting CheatSheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

and for fun look at [XSS Filter Evasion](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)

---

## IDOR = insecure direct object references

dont do this

```
https://some.where/api/auftrag/1234/details
```

```java
/** backend takes auftragid 1234 and loads from DB */
```

```sql
SELECT * FROM auftrag WHERE auftragid = ?;
```

--

## IDOR - no real improvement

dont do this either

```
https://some.where/api/auftrag/1234/details?userid=7890
```

```java
/** backend takes auftragid 1234 and userid = 7890 */
```

```sql
SELECT * FROM auftrag WHERE auftragid = ? AND userid = ?;
```


--

## IDOR - a better way

better like this

```
https://some.where/api/auftrag/1234/details
```

```java
/** backend takes auftragid 1234 and userid from auth token (after validation!) */
```

```sql
SELECT * FROM auftrag WHERE auftragid = ? AND userid = ?;
```

--

## IDOR - another mitigation


```
https://some.where/api/auftrag/a/details
```

```java
/** 
 * backend takes auftragid a and maps it to the real auftragId 1234
 * AND takes the userid from auth token (after validation!)
 * 
 */
```

```sql
SELECT * FROM auftrag WHERE auftragid = ? AND userid = ?;
```

--

## IDOR Prevention

[IDOR CheatSheet](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html)