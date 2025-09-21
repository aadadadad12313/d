
```css
<!DOCTYPE html>

<html lang="en">
  <head>
    <meta charset="UTF-8" />

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>Document</title>

    <style>
      .be::after {
        content: "Sonra";
      }

      .be::before {
        content: "Önce";
      }
    </style>
  </head>

  <body>
    <div class="be">
      <!--en üstte Önce var ama html de yazılmadıgıdan gözükmüyor -->

      <div>d</div>

      <div>d</div>

      <div>d</div>

      <div>d</div>

      <div>d</div>

      <div>d</div>

      <div>d</div>

      <div>d</div>

      <!--en altta Sonra var ama html de yazılmadıgıdan gözükmüyor -->
    </div>
  </body>
</html>
```
