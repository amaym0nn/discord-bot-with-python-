# Komut Hatalarını Yönetme 

Bu yazımda bot komutlarını yanlış kullanma sonucunda oluşacak hataları yönetmeyi anlatacağım. 

&nbsp;

Tabi ki her hatayı yönetemeyiz. Tüm hataları yönetmeyi çalışmak fazlasıyla vakit kaybına neden olabilir. Kabaca en çok yönetilen hatalar kullanıcının komutu yanlış veya eksik yazması veya yetersiz yetki gibi işlemlerde sıklıkla hataları yönetmek için kullanabiliyoruz. Bunlar sadece bir kaç tanesi. 

Tabi ki oluşacak her sorunu siz yönetmeden direkt olarak mesaj göndermesini sağlayabilirsiniz:


```py
@bot.event 
async def on_command_error(ctx, error):
    await ctx.send(error)
```

**on_command_error** API ile hataları yönetiyoruz. Bu API sayesinde her hata sonucunda kullancııya mesaj gönderecektir. Bu API'yi koşulsuz ve sizin yönetmemeniz durumunda hata mesajlarını direkt olarak ingilizce olarak kullanıcıya sunacak. Dilerseniz bir test edelim:

<div align="center">
    <img src="https://i.ibb.co/851CP6m/Screenshot-from-2022-12-10-10-54-11.png" alt="Screenshot-from-2022-12-10-10-54-11" border="0">
</div>
<br/>
Göründüğü gibi bir hata sonucunda ingilizce bir sonuç gösteriyor. İşte bu yüzden hataların daha açıklayıcı olması için gerekli hataları yönetiyoruz.

&nbsp;

Şimdi ise hataları yönetmekten bahsedeyim. Komutları yönetmek oldukça basittir. Daha da aydınlatıcı olması için basit bir örnek yapalım:

```py
@bot.event
async def on_command_error(ctx, error):
    if isinstance(error, commands.MessageNotFound):
        await ctx.send("Böyle bir komut bulunmadı!")

```

Buradaki amaç, eğer error nesnesi commands.MessageNotFound sınıfından türetiliyorsa ekrana hata mesajını gönderiyor. Bunu da isinstance() fonksiyonu ile yapıyoruz. Bir de sonuca bakalım:
<div align="center">
    <img src="https://i.ibb.co/LQLW6bd/Screenshot-from-2022-12-10-11-15-10.png" alt="Screenshot-from-2022-12-10-11-15-10" border="0">
</div>
<br/>
Göründüğü gibi kullanımı çok basit. Eğer oluşturduğunuz komut için özel bir API oluşturarak hata mesajını bastırmak istersek bunu **@.error** API sayesinde yapabiliriz:

```py
@bot.command()
async def clear(ctx, amount:int):
    await ctx.channel.purge(limit=amount)

@clear.error
async def clear_error(ctx, error):
    if isinstance(error, commands.MissingRequiredArgument):
        await ctx.send("Lütfen komutun çalışması için argüman giriniz!")
```

Yukarıdaki kodlarda göründüğü gibi eğer kullanıcı temizlemek istediği mesaj sayısını belirtmediği takdirde **Lütfen komutun çalışması için argüman giriniz!** adlı bir hata mesajı gönderiyor:



<div align="center">
    <img src="https://i.ibb.co/whMVtct/Screenshot-from-2022-12-10-11-44-42.png" alt="Screenshot-from-2022-12-10-11-44-42" border="0">
</div>
</br>

Komutta kullandığım MissingRequiredArgument sınıfı, komut için argüman belirtilmediği takdirde hata mesajı döndüren bir sınıftır. Bunun gibi birçok sınıf mevcuttur. 