
```html
        <h1>Loading</h1>
        <h1>Loading</h1>
        <h1>Loading</h1>
        <h1>Loading</h1>
        <h1>Loading</h1>
        <h1>Loading</h1>
        <div class="counter">0%</div>
        <div class="counter">0%</div>
```

Html'in içi böyle ise ve 

```css
body{
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
```

Css de böyle ise body de bütün öğelerin kapsayıcısı yok bu durumda bütün ögeler yan yan gelir 
ve sığmaya çalışır ama sen buraya en üstte bir div atsan kapsayıcı herhangi bir etiket div atarsan o kapsayıcı flex olur kutu mantığı gibi düşün içindeki öğerler etkilenmez bundan.
