# Custom Directives

### Reusable Components \(HTML\)

* In HTML there is no way to make our own tags. There is an idea on the way called web components that would soon make this easier.
* Say we wanted to create our own HTML structure for search result like this:

```
<h1>Search Results</h1>
<div>
    <h2>Title</h2>
    <p>
        Detail
    </p>
    <a href='#'>...more detail</a>
</div>
```

* For this it would be convenient to package this structure into a tag like this:

```
<searchResult more-detail="#"></searchResult>
```

OR

```
<div searchResult></div>
```

OR

```
<div class="searchResult"></div>
```

* AngularJS brings this concept to us right now. They are called **custom directives**.



