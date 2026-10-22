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

---

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
`uname` = `' /*`<br/>
`passwd` = `doesnt matter anymore`

```sql
SELECT * FROM `users` 
WHERE 
    `username` = '' /*' 
    AND 
    `password` = 'doesnt matter anymore';
```

</div>

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
        resultField.innerHTML = response.text();
    }
</script>
```

---

## XSS - happy path + exploit

user searches `apple` and submits

```html
<input name="search" id="searchfield" value="apple" />
<p id="searchfor">Sie suchten nach "apple"!</p>
```

<div class="fragment">

user searches `wtf<script>alert('cool')</script>` ...

```html
<input name="search" id="searchfield" value="wtf<script>alert('cool')</script>" />
<p id="searchfor">Sie suchten nach "wtf<script>alert('cool')</script>"!</p>
```

</div>