

Sebebi çok basit sen controller tarafında api işlemi yapmısındır kesin yani createClient demişsindir bu da httpClient  sınfının tetikleyip http isteği atacak apiye ama senin apin ayakta olmadığından ben atamıyorum diyor.

yani sen gidip api işlemlerini silip direk boş bir view çalıştırıtsan sorun olmaz hatata git bir sürü kod yaz ama api işleminde controller tarafında api işlemi varsa kesinlikle apinin açık olaması gerekir

ya zaten postmande bile api ayakta olmassa yine istek atamıyorusn api ayakta olmalı eğer sen apiye istek atacaksan.

onemli::Neden Bir View dosyası yüklenirken Api yani swaggerin açık olması gerekiyor o olmayınca bağlantı açık değil diyor ya