---
redirect_url: https://docs.microsoft.com/
title: Trabalhando com sensores em UWP - Parte 3 - Inclinômetro
description: Trabalhando com sensores em UWP - Parte 3 - Inclinômetro
author: MSCommunityPubService
ms.author: andygon
ms.date: 02/10/2017
ms.topic: article
ms.prod: 
ms.technology: 
ms.service: win-dev
ms.custom: CommunityDocs
---


# Trabalhando com sensores em UWP - Parte 3 - Inclinômetro #
Nos artigos anteriores (Sensor de luz e Bússola), eu mostrei como usar os sensores em uma aplicação UWP. Trabalhar com eles é muito fácil e eles podem aumentar a usabilidade de sua aplicação. Neste artigo, irei mostrar como usar outro sensor, o inclinômetro.

Este sensor é muito usado para jogos, pois ele detecta a inclinação do dispositivo. Desta maneira, o usuário pode controlar o jogo inclinando o dispositivo. Legal, não? Este não é um sensor puro, você provavelmente não tem um inclinômetro em seu dispositivo, mas ele irá obter suas medidas de outros dois sensores, o acelerômetro e o giroscópio.

O uso do sensor é semelhante aos outros sensores e ele mostra três medidas, Yaw (a rotação no eixo Z), Pitch (rotação no eixo X) e Roll (rotação no eixo Y:

![medidas](http://blogs.msmvps.com/bsonnino/files/2016/12/inclinometer.png)

(fonte – [https://msdn.microsoft.com/en-us/windows/uwp/devices-sensors/sensors](https://msdn.microsoft.com/en-us/windows/uwp/devices-sensors/sensors))

## Brincando com uma bola em UWP ##
Para mostrar o uso do inclinômetro, criaremos um projeto que pode ser um ponto de partida para um jogo. Desenharemos uma bola na janela e, dependendo de como o usuário inclina o dispositivo, a bola se move mais devagar ou mais rápida, para a direita ou para a esquerda.

Iniciaremos criando uma nova aplicação UWP em branco. Mude o elemento principal para um Canvas e adicione uma elipse a ele:

```xml
<Canvas Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Ellipse Width="80" Height="80" Fill="Red" x:Name="Ball"/>
</Canvas>
```

No construtor de MainPage, posicionamos a bola e adicionamos o código para inicializar o inclinômetro e configurar o seu evento __ReadingChanged__:

```C#
public MainPage()
{
    this.InitializeComponent();
    Loaded += (s, e) =>
    {
        Canvas.SetLeft(Ball, ActualWidth / 2 - 40);
        Canvas.SetTop(Ball, ActualHeight - 80);
        var inclinometer = Inclinometer.GetDefault();
        if (inclinometer == null)
        {
            return;
        }
        inclinometer.ReadingChanged += async (s1, e1) =>
        {
            await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
            {
                var position = Canvas.GetLeft(Ball);
                var newPosition = position + e1.Reading.RollDegrees;
                if (newPosition  ActualWidth - 80)
                    newPosition = ActualWidth - 80;
                Canvas.SetLeft(Ball, newPosition);
            });
        };
    };
}
```

Como você pode ver, estamos fazendo a inicialização no manipulador do evento __Loaded__ da página. Fazemos isto porque as propriedades __ActualWidth__ e __ActualHeight__ da página não estão configuradas antes que o evento __Loaded__ é disparado, e se isto não for feito assim  a bola será posicionada no local errado. Como estamos usando um Canvas, posicionamos a bola com as propriedades anexadas __Left__ e __Top__. No código, isto é feito com os métodos estáticos __Canvas.SetLeft__ e __Canvas.SetTop__.

O passo seguinte é obter a instância do inclinômetro com __GetDefault__ e configurar o manipulador do evento __ReadingChanged__. Ele irá obter a posição atual da bola e irá configurar a nova posição dependendo do valor do Roll (número de graus da inclinação na direção Y). Desta maneira, se o usuário inclinar mais, a bola irá se mover mais rápido. Se inclinar menos, ela se moverá mais devagar.

Com o código no lugar, temos nosso projeto completo e podemos executá-lo. Se seu dispositivo tiver um inclinômetro, você verá a bola se movimentando. Se não tiver, você pode usar um Windows Phone e ver o mesmo aplicativo rodando lá.

![imagem do programa](http://blogs.msmvps.com/bsonnino/files/2016/12/image_thumb-5.png)

## Conclusões ##
Usar o inclinômetro é muito simples e você pode dar aos seus usuários uma nova maneira de entrada com ele, você só precisa obter sua leitura e mover os elementos em sua aplicação de acordo com a inclinação do dispositivo.

O código fonte para esta aplicação está em [https://github.com/bsonnino/MovingBall](https://github.com/bsonnino/MovingBall)
