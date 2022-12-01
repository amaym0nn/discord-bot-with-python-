<style>
h2 {
    color: red;
    font-size: 25px;
    text-align: center;
}

p {
    font-size: 15px;
}

</style>

<h2> Events ve Intents </h2>

<p> Eventler, sunucudaki kullanıcının sunucudaki hareketlerine göre geri cevap veren bir takım olaylardır. Bu hareketler, kullanıcının sunucuya girdiği anda hoşgeldin mesajı, kullanıcı sunucudan çıktığında görüşürüz mesajı gibi olaylara örnek verilebilir.  </p> 

</br>

<p> Tabi ki bir çok event vardır. Bunların hepsini öğrenmeye çalışmak bir nebze vakit kaybıdır. Bu yüzden geliştireceğiniz projeye göre eventleri öğrenmek daha avantajlı olacaktır. Aşağıda basit bir event için örnek yapalım: </p>

```py
import discord
from discord.ext import commands

intents = discord.Intents.all()
bot = discord.Bot(command_prefix="?beko", intents=intents)

@bot.event
async def on_ready():
    print("I'm Ready!")

    bot.run("token....")
```

Kodlara bakıldığında **on_ready** adlı bir API görebilirsiniz. Bu discord botları için sık sık kullanılan bir API'dir. Bu event altındaki amaç, bot sorunsuz çalıştığını göstermek için <strong> I'm Ready! </strong> adlı bir output gönderiyor. Bu mesajı kullanıcıya değil terminalden size gönderiyor. </br>

Gel gelelim Intents konusuna. Intents, oluşturmak istediğiniz discord botunuza yapacağınız amacına bağlı olarak izinler vermenizi sağlar. Yukarıdaki örnekte <strong> intents = discord.Intents.all() </strong> adlı bir komut kullandım. Bu komut bot için tüm izinleri vermenizi sağlar. Fakat manuel olarak da bunu yapmak mümkün:

```py
intents = discord.Intents(messages = True,
                          members = True,
                          guilds = True
                          reactions = True)
```

Yukarıdaki koda bakıldığında bot için gerekli izinleri manuel olarak verildiği görülebilir. Tabi yukarıdaki Intents'ler bu kadar sınırlı değil.