# Tasks ile Loop İşlemleri

Bot geliştirirken en yaygın yapılan işlemlerden biri loop yani döngülerdir. Python ile discord botu geliştirirken tasks.loop adlı bir API ile bu döngü işlemlerini yapmamız mümkün. Bu API'ye biraz daha yakından bakalım.

&nbsp;

Öncelikle projemizde tasks kullanmak için import etmemiz lazım:

```py
from discord.ext import tasks
```

tasks.loop ile döngü oluşturmak çok basittir. İlk örnek olarak proje direkt çalıştığında terminale <strong> Development Bot With Python </strong> adlı cümleyi 10 saniyede bir ekrana bastırmasını sağlayalım:

```py
import discord
from discord.ext import commands, tasks

intents=discord.Intents.all()
bot = commands.Bot(command_prefix="/", intents=intents)

@tasks.loop(seconds=10)
async def startloop():
    print("Development Bot With Python")

@bot.event
async def on_ready():
    print("Bot is Ready")
    startloop.start()
```

Yukarıdaki kodlardan bazı noktaları açıklayayım. @task.loop adlı bir event oluştururken parametre olarak bir seconds değeri girmek zorundayız. Buradaki seconds parametresinin amacı, bu döngünün kaç saniyede bir çalışacağını belirler. Dolasıyla yukarıda 10 saniye olduğunu belirtmişim. Bot çalıştığı takdirde her 10 saniyede bir <strong> Development Bot With Python </strong> cümlesini ekrana bastıracak. Döngüleri çalıştırma işlemlerini ise <strong> .start() </strong> kodu ile yapıyoruz. Eğer durdurmak istersek <strong>.stop() </strong> ile mümkün.

Dikkat edilmesi gereken bir nokta ise, döngüyü on_ready() altında çalışması. Döngüyü hangi event veya command içerisine çalıştırmak isterseniz onun altına yazmanız gerekir. 

&nbsp;

Şimdi ise basit bir örnek ile bu konuyu daha da aydınlatayım. Şöyle bir senaryo üzerinden ilerlersek, bot çalışmaya başlamasından itibaren her 1 saatte bir https://github.com/amaym0nn linkini belirlenen channel'e gönderen bir bot yapalım:

```py
import discord
from discord.ext import commands, tasks

intents=discord.Intents.all()
bot = commands.Bot(command_prefix="/", intents=intents)

# Öncelikle link ve kullanıcı adını tutan bir class ve dictionary oluşturup bunlardan yararlanacağız.

class Account:
    github = "https://github.com"


acc = {
    "github": "amaym0nn" 
}

def combining() -> str:
    return f"""
        {Account.github}/{acc.get("github")} 
    """
    # Burada Account içerisindeki link ile github içerisinde Value olarak yerleştirdiğim amaym0nn'u birleştirir. Sonuç: https://github.com/amaym0nn


@tasks.loop(seconds=3600) # 3600 = 1 saat
async def startLoop():
    for chnls in bot.get_all_channels(): # Sunucudaki tüm odaları listeler. 
        if chnls.id == 1048456334162538546: 
           await chnls.send(combining())
            # if döngüsü içerisinde, eğer listenen odalardan belirtilen id var ise     hazırladığım def döngüsünü çalıştırmaya başlayacak.
            
@bot.event 
async def on_ready():
    startLoop.start() # Bot çalışmaya başlamasından itibaren döngü başlayacak. 

bot.run(TOKEN)
```

Sırasıyla ilerleyelim. **Account** adlı class içerisine github linkini, **acc** adlı dictionary içerisine ise kullanıcı adımı yerleştirdim. Buradaki asıl amaç bu link ile kullanıcı adını birleşirmek için. def ile oluşturduğum **combining** adlı fonksiyon ile bu birleştirmeyi yapıyorum. 

combining adlı fonksiyon içerisindeki **{Account.github}/{acc.get("github")}** komutta, öncelikle **Account.github** değişken içerisindeki linki alıp daha sonra **acc** adlı dictionary içerisindeki yerleşmiş github key içerisindeki değeri alıp birleştirme yapıyor. 

Daha sonra seconds parametresine 3600 değerini atayarak bir tasks.loop ile döngü başlatılıyor. Asıl önemli kısımlar burada başlıyor:
```py
    for chnls in bot.get_all_channels(): # Sunucudaki tüm odaları listeler. 
        if chnls.id == 1048456334162538546: 
           await chnls.send(combining())
```
Bu kısımda bir for döngüsü başlatılmış. for döngüsünde sunucudaki tüm odalar listeneliyor. For döngüsü altında eğer listenen odalarda belirtilen id var ise combining adlı fonksiyon çalışmaya başlıyor. Bu fonksiyon seconds parametresinde belirtildiği gibi 1 saatte bir kere çalışacak. Böylece belirtilen odanın id'sine <strong> https://github.com/amaym0nn </strong> adlı linki gönderecek.

Eğer program çıktısı bir fotoğraf ile görülmek istenirse aşağıda verilmiştir:
<img src="https://i.ibb.co/TMjFh0C/image.png" alt="image" border="0">