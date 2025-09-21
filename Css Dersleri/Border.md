
Şimdi bu kodda konu anlatımı var çalıştır bu kodu clauda yaptırdım Borderlar kenarlık gibi gözükebilir ama kenarları tam 45 derece eğilimli bunların bu yüzden 🔻 böyle olabiliyor width height değeri olmayınca tam belli oluyor ilgilnç bir konu ama şekil oluşturmada sıkça tercih ediliyor.

### Kod Örneği
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Border Köşe Birleşmeleri</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            background: #f0f0f0;
        }
        
        .container {
            display: flex;
            gap: 40px;
            flex-wrap: wrap;
            justify-content: center;
            margin: 20px 0;
        }
        
        .example {
            text-align: center;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        
        .box1 {
            width: 80px;
            height: 50px;
            border-top: 15px solid red;
            border-right: 15px solid blue;
            border-bottom: 15px solid green;
            border-left: 15px solid orange;
            margin: 20px auto;
        }
        
        .box2 {
            width: 40px;
            height: 25px;
            border-top: 15px solid red;
            border-right: 15px solid blue;
            border-bottom: 15px solid green;
            border-left: 15px solid orange;
            margin: 20px auto;
        }
        
        .box3 {
            width: 0px;
            height: 0px;
            border-top: 15px solid red;
            border-right: 15px solid blue;
            border-bottom: 15px solid green;
            border-left: 15px solid orange;
            margin: 20px auto;
        }
        
        .box4 {
            width: 0px;
            height: 0px;
            border-top: 20px solid red;
            border-right: 20px solid transparent;
            border-bottom: 20px solid transparent;
            border-left: 20px solid transparent;
            margin: 20px auto;
        }
        
        h2 {
            color: #333;
            border-bottom: 2px solid #4CAF50;
            padding-bottom: 5px;
        }
        
        h3 {
            color: #666;
            margin: 10px 0 5px 0;
        }
        
        .code {
            background: #f8f8f8;
            border: 1px solid #ddd;
            border-radius: 4px;
            padding: 8px;
            font-family: monospace;
            font-size: 12px;
            margin: 10px 0;
        }
        
        .highlight {
            background: #fff3cd;
            border: 2px solid #ffc107;
            padding: 15px;
            border-radius: 8px;
            margin: 20px 0;
        }
        
        .arrow {
            font-size: 24px;
            color: #4CAF50;
            margin: 0 10px;
        }
        
        .step {
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 20px 0;
            flex-wrap: wrap;
        }
    </style>
</head>
<body>
    <h1>🔍 Border Köşeleri Nasıl Birleşir?</h1>
    
    <div class="highlight">
        <strong>Ana Nokta:</strong> Border'lar köşelerde 45° açıyla birleşir. Bu normalde fark edilmez ama width/height=0 olunca sadece bu eğik kesimler kalır!
    </div>
    
    <h2>Adım Adım Geçiş:</h2>
    
    <div class="step">
        <div class="example">
            <h3>1. Normal Border</h3>
            <div class="box1"></div>
            <div class="code">
                width: 80px;<br>
                height: 50px;<br>
                border: 15px solid;
            </div>
            <p>Köşelere dikkat et! Renkler eğik birleşiyor</p>
        </div>
        
        <div class="arrow">→</div>
        
        <div class="example">
            <h3>2. Daha Küçük</h3>
            <div class="box2"></div>
            <div class="code">
                width: 40px;<br>
                height: 25px;<br>
                border: 15px solid;
            </div>
            <p>Eğik kesimler daha belirgin!</p>
        </div>
        
        <div class="arrow">→</div>
        
        <div class="example">
            <h3>3. Width/Height = 0</h3>
            <div class="box3"></div>
            <div class="code">
                width: 0px;<br>
                height: 0px;<br>
                border: 15px solid;
            </div>
            <p>Sadece eğik kesimler kaldı = 4 üçgen!</p>
        </div>
        
        <div class="arrow">→</div>
        
        <div class="example">
            <h3>4. Tek Kenar Görünür</h3>
            <div class="box4"></div>
            <div class="code">
                width: 0px;<br>
                height: 0px;<br>
                border-top: 20px solid red;<br>
                diğerleri: transparent;
            </div>
            <p>İşte üçgenin!</p>
        </div>
    </div>
    
    <div class="highlight">
        <h3>🎯 Anahtar Anlayış:</h3>
        <p>Browser, border köşelerini her zaman 45° açıyla keser. Normal durumlarda içerik alanı büyük olduğu için bu kesimler küçük kalır ve fark edilmez. Ama width/height=0 olduğunda, sadece bu 45° kesimler kalır ve her biri bir üçgen oluşturur!</p>
    </div>
    
    <h2>🔬 Köşe Detayı:</h2>
    <div style="text-align: center; margin: 20px 0;">
        <svg width="200" height="150" style="border: 1px solid #ccc; background: white;">
            <!-- Sol üst köşe detayı -->
            <rect x="50" y="50" width="100" height="50" fill="none" stroke="#ddd" stroke-dasharray="2,2"/>
            
            <!-- Üst border -->
            <polygon points="50,50 150,50 135,65 65,65" fill="red" opacity="0.8"/>
            <text x="100" y="45" text-anchor="middle" font-size="12" fill="red">ÜST BORDER</text>
            
            <!-- Sol border -->
            <polygon points="50,50 65,65 65,100 50,100" fill="orange" opacity="0.8"/>
            <text x="35" y="80" text-anchor="middle" font-size="12" fill="orange">SOL</text>
            
            <!-- 45° çizgi -->
            <line x1="50" y1="50" x2="65" y2="65" stroke="black" stroke-width="2"/>
            <text x="45" y="75" font-size="10">45°</text>
            
            <text x="100" y="130" text-anchor="middle" font-size="14" fill="#333">Köşede eğik kesim</text>
        </svg>
    </div>
    
</body>
</html>
```


