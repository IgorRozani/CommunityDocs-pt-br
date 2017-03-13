---
title: Trabalhando com sensores em UWP - Parte 5 - Localização
description: Trabalhando com sensores em UWP - Parte 5 - Localização
author: MSCommunityPubService
ms.author: andygon
ms.date: 03/06/2017
ms.topic: article
ms.prod: 
ms.technology: 
ms.service: win-dev
ms.custom: CommunityDocs
---



# Trabalhando com sensores em UWP - Parte 5 - Localização #
Esta última parte da série de artigos sobre sensores irá mostrar um tipo diferente de sensor - o sensor de localização. Na realidade, não podemos chamar ele de sensor, pois o serviço de localização irá obter a localização atual a partir do sensor de GPS, se ele existir. Se não, ele usará outros métodos, como a rede ou o WiFi para detectar a localização atual.

Para obter a localização atual, devemos trabalhar de maneira diferente que nos outros sensores:


- Você deve adicionar a capacidade de Localização ao manifesto da aplicação.
- O usuário deve consentir com o uso da localização atual
- Você deve instanciar uma nova instância de um Geolocator com __new__ (e não obter uma com __GetDefault__)
- Não há evento __ReadingChanged__, mas sim __StatusChanged__

Mesmo com estas mudanças, você verá que é muito fácil trabalhar com a localização. Neste artigo iremos desenvolver um programa que obtém a localização atual, consulta um serviço meteorológico para obter a previsão e ver se vai chover.  

## Será que vai chover? ##
No Visual Studio, crie um projeto UWP em branco. Para consultar o tempo, usaremos um projeto Open Source que tem uma API C# para consultar o serviço OpenWeather ([http://openweathermap.org/API](http://openweathermap.org/API)), localizado em [https://github.com/joancaron/OpenWeatherMap-Api-Net](https://github.com/joancaron/OpenWeatherMap-Api-Net). Podemos adicionar esta biblioteca usando o gerenciador de pacotes NuGet (clique no nó de referências no Solution Explorer e selecione _Manage NuGet Packages_. Procure por __OpenWeather__ e instale a biblioteca:

![](http://blogs.msmvps.com/bsonnino/files/2016/12/image_thumb-7.png)

Uma nota aqui: o pacote oficial não suporta o Windows 10, então você deve escolher um fork do projeto, o terceiro projeto da lista, para instalar.

Em seguida, você deve adicionar a capacidade de localização ao manifesto da aplicação. No Solution Explorer, abra o arquivo _package.appxmanifest_ e vá apara a aba __Capabilities__. Ali, selecione a caixa __Location__.

![](http://blogs.msmvps.com/bsonnino/files/2016/12/image_thumb-8.png)

Na janela principal, adicione este XAML:

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
   <TextBlock x:Name="RainText"
       HorizontalAlignment="Center"
       VerticalAlignment="Center"
       FontSize="30"/>
</Grid>
```

No construtor de MainPage, devemos perguntar pelas permissões ao usuário e processar a resposta:

```C#
public MainPage()
{
    this.InitializeComponent();
    InitializeLocator();
}
 
public enum RainStatus
{
    Rain,
    Sun
};
 
private async void InitializeLocator()
{
    var userPermission = await Geolocator.RequestAccessAsync();
    switch (userPermission)
    {
        case GeolocationAccessStatus.Allowed:
            RainText.Text = "Updating rain status...";
            Geolocator geolocator = new Geolocator();
            Geoposition pos = await geolocator.GetGeopositionAsync();
            try
            {
                RainText.Text = await GetRainStatus(pos) == RainStatus.Rain ?
                    "It's possible to rain, you'd better take an umbrella" :
                    "It will be a bright an shiny day, go out and enjoy";
            }
            catch
            {
                RainText.Text = "Got an error while getting weather";
            }
            break;
 
        case GeolocationAccessStatus.Denied:
            RainText.Text = "I cannot check the weather if you don't give me the access to your location...";
            break;
 
        case GeolocationAccessStatus.Unspecified:
            RainText.Text = "I got an error while getting location permission. Please try again...";
            break;
    }
}
```

Se o usuário permitir o acesso aos dados de localização, obtemos a posição atula com __GetGeopositionAsync__ e usamos esta posição para obter o status de chuva, usando a API do OpenWeather map. O método __GetRainStatus__ é:

```C#
private async Task GetRainStatus(Geoposition pos)
{
    var client = new OpenWeatherMapClient("YourPrivateId");
    var weather = await client.CurrentWeather.GetByCoordinates(new Coordinates()
    {
        Latitude = pos.Coordinate.Point.Position.Latitude,
        Longitude = pos.Coordinate.Point.Position.Longitude
    });
    return weather.Precipitation.Mode == "no" ? RainStatus.Sun : RainStatus.Rain;
}
```

Para usar a API OpenWeather, você deve se registrar no site e obter uma chave da API. Para uso baixo, a chave e o uso são gratuitos. Você pode se registrar em [http://openweathermap.org/appid](http://openweathermap.org/appid).

Quando você roda o programa, você já pode saber se precisa levar um guarda-chuva ao sair:

![](http://blogs.msmvps.com/bsonnino/files/2016/12/image_thumb-9.png)

## Conclusões ##
Como você pode ver, trabalhar com sensores é muito fácil e pode oferecer uma melhor experiência aos seus usuários. Você pode integrá-los com suas aplicações em diferentes maneiras. Neste artigo, vimos como usar uma API de meteorologia para saber se irá chover na sual localização, um programa simples, mas muito útil!

Todo o código fonte para este projeto está em [https://github.com/bsonnino/GeoLocation](https://github.com/bsonnino/GeoLocation).

Você pode ver esta série de artigos também em vídeo em meu canal no Channel9: [https://channel9.msdn.com/Niners/bsonnino](https://channel9.msdn.com/Niners/bsonnino)
