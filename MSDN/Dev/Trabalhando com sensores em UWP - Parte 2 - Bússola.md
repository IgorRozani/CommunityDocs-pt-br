---
title: Trabalhando com sensores em UWP - Parte 2 - Bússola
description: Trabalhando com sensores em UWP - Parte 2 - Bússola
author: MSCommunityPubService
ms.author: andygon
ms.date: 02/10/2017
ms.topic: article
ms.prod: 
ms.technology: 
ms.service: win-dev
ms.custom: CommunityDocs
---


# Trabalhando com sensores em UWP - Parte 2 - Bússola #
No [último artigo](https://msdn.microsoft.com/pt-br/communitydocs/dev/trabalhando%20com%20sensores%20em%20uwp%20–%20parte%201%20–%20sensor%20de%20luz), eu mostrei como usar o sensor de luz para obter os dados de luminosidade e oferecer uma melhor experiência para o usuário. Neste artigo, irei mostrar outro sensor disponível nos dispositivos 2-em-1 e nos tablets: a bússola. A bússola é um sensor que mostra a direção em relação ao norte magnético.  
Em bússolas físicas esta leitura é dada pela agulha magnética que sempre aponta para o norte magnético. Quando você gira a bússola, ela mostra em que direção você está indo. Em computadores e smartphones, não há agulha magnética, mas o sensor mostra o ângulo do dispositivo em relação ao norte magnético. Para usar o sensor, devemos criar um programa que executa os mesmos três passos do artigo anterior: 
 
- Obter uma instância do sensor com o método GetDefault  
- Configurar suas propriedades  
- Configurar um manipulador para o evento ReadingChanged

Neste artigo, iremos criar um programa que simula uma bússola física e mostra a direção do dispositivo.  

## Criando um simulador de bússola em UWP ##  
O primeiro passo é criar um projeto UWP em branco. Neste projeto, adicione imagens para o fundo da bússola e a agulha (eu baixei meu fundo de [http://www.clipartbest.com/images-of-a-compass]( http://www.clipartbest.com/images-of-a-compass )  e a agulha de 
[http://www.clker.com/clipart-compassnorth.html](http://www.clker.com/clipart-compassnorth.html)) ao diretório Assets do projeto.  
Então, adicione duas imagens à janela principal:  
```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">  
    <Image Source="Assets\\Compass.png" HorizontalAlignment="Stretch"
           VerticalAlignment="Stretch" Stretch="Uniform"/>
    <Image Source="Assets\\Compassnorth-Hi.png" HorizontalAlignment="Stretch"
           VerticalAlignment="Stretch" Stretch="Uniform" x:Name="Needle"/>
</Grid>
```  
No construtor de MainWindow, devemos obter a instância da bússola e configurar o evento ReadingChanged:

```c#
public MainPage()    
{  
    this.InitializeComponent();  
    var compass = Compass.GetDefault();  
    if (compass == null)  
    {  
        return;  
    }  
    compass.ReadingChanged += async (s, e) =>  
    {  
        await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>  
        {  
            RotateNeedle(e.Reading.HeadingMagneticNorth);  
        });  
    };  
}  
```
O manipulador chama apenas o método RotateNeedle, passando o ângulo de leitura como parâmetro. RotateNeedle é muito simples:

```c#
private void RotateNeedle(double angle)
{
    var transform = new RotateTransform() { Angle = angle };
    Needle.RenderTransformOrigin = new Windows.Foundation.Point(0.5, 0.5);
    Needle.RenderTransform = transform;
}
```
Ele cria um RotateTransform, configurando o ângulo para o ângulo de leitura , configura a origem da rotação para o meio da agulha e, então, configura a propriedade RendreTransform para esta transformação.  
Com isto, vcê tem um simulador de bússola que trabalha tanto em desktops quanto em smartphones. Quando você gira o dispositivo, a agulha irá apontar para o norte magnético e a frente irá mostrar a direção.

![](http://blogs.msmvps.com/bsonnino/files/2016/12/image_thumb-4.png)

## Conclusões ##
Como você pode ver, trabalhar com a bússola é muito simples, você pode obter facilmente a leitura do sensor e mudar a visualização, conforme a posição do dispositivo. Esta é mais uma ferramenta que você pode usar para dar ao usuário uma melhor experiência. 

Se você quiser ver um vídeo relacionado a este artigo, pode ir para [meu canal no Channel9](https://channel9.msdn.com/Series/Windows-Development/Trabalhando-com-Sensores-em-UWP-Parte-2-Bssola).

O código fonte para este artigo está em  [https://github.com/bsonnino/Compass](https://github.com/bsonnino/Compass)