
View' e gelen datayı iki türlü ekrana bastırabilirsin For ve Foreach ile

```csharp
@model List<UdemyCarbook.Domain.Entities.Car>

<h2>Arabalar</h2>

<table class="table table-bordered">
    <thead>
        <tr>
            <th>Marka</th>
            <th>Model</th>
            <th>Yıl</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var car in Model)
        {
            <tr>
                <td>@car.Brand.Name</td>
                <td>@car.Model</td>
                <td>@car.Year</td>
            </tr>
        }
    </tbody>
</table>
```


```csharp
@model List<UdemyCarbook.Domain.Entities.Car>

<h2>Arabalar</h2>

<table class="table table-bordered">
    <thead>
        <tr>
            <th>#</th>
            <th>Marka</th>
            <th>Model</th>
            <th>Yıl</th>
        </tr>
    </thead>
    <tbody>
        @for (int i = 0; i < Model.Count; i++)
        {
            <tr>
                <td>@(i + 1)</td>
                <td>@Model[i].Brand.Name</td>
                <td>@Model[i].Model</td>
                <td>@Model[i].Year</td>
            </tr>
        }
    </tbody>
</table>
```

