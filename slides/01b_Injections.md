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

`uname = "some@one.de"` and<br/>`passwd = "s3CR3t!"`

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

user enters `uname = "' /*"` 
and `passwd = "p"`

```sql
SELECT * FROM `users` 
WHERE 
    `username` = '' /*' 
    AND 
    `password` = 'p';
```

---

## XSS - how does this work

```html
<input name="search" id="searchfield" />
<p id="searchfor"></p>
<section id="result"></section>

<script>
    const searchfield = document.querySelector('#searchfield');
    const searchfor = document.querySelector('#searchfor');

    function onSearch(){
        const searchTerm = searchfield.value;
        searchfor.innerHTML = `Sie suchten nach "${searchTerm}"!`
    }
</script>
```