---
redirect_url: https://docs.microsoft.com/
title: Trabalhando com sensores em UWP - Parte 4 - Acelerômetro
description: Trabalhando com sensores em UWP - Parte 4 - Acelerômetro
author: MSCommunityPubService
ms.author: andygon
ms.date: 03/06/2017
ms.topic: article
ms.prod: 
ms.technology: 
ms.service: win-dev
ms.custom: CommunityDocs
---

# Trabalhando com sensores em UWP - Parte 4 - Acelerômetro #

Nos últimos artigos ([Sensor de Luz](https://msdn.microsoft.com/pt-br/communitydocs/dev/trabalhando%20com%20sensores%20em%20uwp%20%E2%80%93%20parte%201%20%E2%80%93%20sensor%20de%20luz), [Bússola](https://msdn.microsoft.com/pt-br/communitydocs/dev/trabalhando%20com%20sensores%20em%20uwp%20-%20parte%202%20-%20b%C3%BAssola) e [Inclinômetro](https://msdn.microsoft.com/pt-br/communitydocs/dev/trabalhando%20com%20sensores%20em%20uwp%20-%20parte%203%20-%20inclin%C3%B4metro)), mostrei como usar alguns sensores em uma aplicação UWP. Neste artigo irei mostrar como usar outro sensor, o acelerômetro. O acelerômetro mede a aceleração de um dispositivo. Esta medida é muito útil para saber quando o dispositivo foi movimentado.

Este sensor mostra a aceleração nos três eixos, X, Y e Z e tem uma particularidade: como ele mede as forças a que o dispositivo está sendo submetido, esta medida normalmente não é 0, pois o dispositivo está sempre submetido à força de gravidade (a menos que você o leve ao espaço). A medida normal para o dispositivo parado é 1, a força da gravidade. Quando você o move, a medida deve ser diferente de 1. 

## Fazendo uma bola pular em UWP ##
Neste artigo irei desenvolver um programa que mostra uma bola na parte de baixo da janela e, quando o usuário chacoalha o dispositivo, a bola pula com uma animação. Quanto mais forte for a sacudida, mais alto a bola pula.\

O sensor nos dá três medidas, mas eu não estou interessado na aceleração em cada eixo, eu só quero a magintude da aceleração, que é dada pela fórmula 

```
magnitude = sqrt(x^2 + y^2 + z^2)
```

A magnitude da aceleração é a raiz quadrada da soma dos quadrados das três medidas.

Com esta informação, podemos iniciar nosso programa. Crie um projeto UWP em branco. Na página principal, mude o elemento principal para um Canvas e adicione uma elipse a ele:
   
```xml
<Canvas Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
   <Ellipse Width="80" Height="80" Fill="Red" x:Name="Ball"/>
</Canvas
```

No construtor de MainPage, adicione o código para posicionar a bola, obter a instância do acelerômetro e adicionar o manipulador do evento __ReadingChanged__:

```C#
public MainPage()
{
    this.InitializeComponent();
    Loaded += (s, e) =>
    {
        Canvas.SetLeft(Ball, ActualWidth / 2 - 40);
        Canvas.SetTop(Ball, ActualHeight - 80);
        var accelerometer = Accelerometer.GetDefault();
        if (accelerometer == null)
        {
            return;
        }
        accelerometer.ReadingChanged += async (s1, e1) =>
        {
            await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
            {
                var magnitude = Math.Sqrt(
                    e1.Reading.AccelerationX * e1.Reading.AccelerationX +
                    e1.Reading.AccelerationY * e1.Reading.AccelerationY +
                    e1.Reading.AccelerationZ * e1.Reading.AccelerationZ);
                if (magnitude &gt; 1.5)
                    JumpBall(magnitude);
 
            });
        };
    };
}
```

Este código é muito semelhante àquele dos artigos anteriores: posicionamos a bola na parte de baixo da janela e configuramos o manipulador do evento __ReadingChanged__. Este manipulador pega a magnitude da leitura e, se a magnitude for maior que 1,5g, ele chama o método __JumpBall__, para fazer a bola pular.. O código de __JumpBall__ é:

```C#
private void JumpBall(double acceleration)
{
    var da = new DoubleAnimation()
    {
        From = ActualHeight-80,
        To = ActualHeight-80-acceleration * 200,
        AutoReverse = true,
        Duration = new Duration(TimeSpan.FromSeconds(1))
    };
    Storyboard.SetTarget(da, Ball);
    Storyboard.SetTargetProperty(da, "(Canvas.Top)");
    var sb = new Storyboard();
    sb.Children.Add(da);
    sb.Begin();
}
```

Criamos uma DoubleAnimation usando a parte de baixo da janela como ponto inicial, e o ponto final é dado pela intensidade da força de aceleração. A animação está relacionada à propriedade anexada __Canvas.Top__ ligada à bola. Com isto definido, executamos a animação.

Desta maneira, quando o usuário sacode o dispositivo, a bola pula. Agora, quando você executa a aplicação e chacoalha o dispositivo, a bola irá pular.

![](http://blogs.msmvps.com/bsonnino/files/2016/12/image_thumb-6.png)

## Conclusões ##
Com o acelerômetro você pode dar a seus usuários mais interatividade, detectando quando eles movimentam ou chacoalham o dispositivo, reagindo a estas ações. O acelerômetro é muito simples de usar e pode melhorar muito a usabilidade de jogos.

O código fonte para este projeto está em [https://github.com/bsonnino/JumpingBall](https://github.com/bsonnino/JumpingBall) 