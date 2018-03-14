---
redirect_url: https://docs.microsoft.com/
title: WPF reativo - Parte 2 - Reactive UI 
description: WPF reativo - Parte 2 - Reactive UI 
author: MSCommunityPubService
ms.author: andygon
ms.date: 03/13/2017
ms.topic: article
ms.prod: 
ms.technology: 
ms.service: win-dev
ms.custom: CommunityDocs
---


# WPF reativo - Parte 2 - Reactive UI #
No último artigo, você viu uma introdução à programação reativa e como ela pode simplificar a programação, especialmente quando você quer observar dados que vem até você. Como um exemplo, você pode observar eventos ou notificações, informações periódicas e assim por diante. Você pode ver que pode combinar diversas fontes de informação usando operadores LINQ e obter novas informações a partir delas.

E como isto se relaciona com a interface do usuário (IU) de uma aplicação? Há uma grande relação aqui; por exemplo, a IU manda sinais assíncronos ao back-end (eventos de mouse ou teclado, mudança de dados, ativação de comandos, etc), que podem ser observados de maneira desacoplada, podendo assim separar a IU do processamento dos sinais, obtendo um programa mais testável e (isto é muito importante hoje em dia), obtendo um back-end que não está ligado a nenhuma IU específica, facilitando o desenvolvimento de aplicações multi-plataforma.

## O padrão MVVM ##
Trabalhando com XAML e C#, uma coisa que vem à mente é o padrão de projeto Model-View-ViewModel (MVVM), desenvolvido pelos arquitetos da Microsoft e baseado no padrão Model-View-Presenter (MVP), que usa os recursos da plataforma XAML, introduzida no WPF. Com ele, você usa a ligação de dados (Data Binding) e comandos para facilitar a separação entre a IU e a lógica de negócio, usando três camadas:

- __Model__ - Esta é a camada de dados, de onde vem a informação. Pode ser qualquer tipo de dados, como classes POCO (Plain Old CLR Objects - classes simples sem nenhuma informação adicional), dados de bancos de dados, objetos REST e assim por diante. Estas classes, em geral, não têm nenhum tratamento especial e podem ser compartilhadas por diversas aplicações.
- __View__ - Esta é a camada de apresentação, aonde o usuário interage com a informação: caixas de entrada, botões e listas fazem parte desta camada.
- __ViewModel__ - Esta camada faz a intermediação entre a visualização e os dados. ViewModels mudam os dados de maneira que possam ser apresentados na View e recebe as atualizações da IU e as passam ao modelo. Tudo isto é feito usando a infraestrutura XAML, principalmente Data Binding e comandos. O ViewModel não está diretamente ligado à View, de maneira que é completamente testável e pode ser usado em programas multi-plataforma (um único ViewModel pode ser usado para Views das diferentes plataformas). De outro lado, a View normalmente não é ligada ao ViewModel, de maneira que você pode ter diversas Views para o mesmo ViewModel ou muitos ViewModels para a mesma View. O Model também não é ligado ao ViewModel, de modo que pode ser usado independente deste (inclusive muitos desenvolvedores criam os modelos em um assembly independente, para enfatizar o desacoplamento).

## Reactive UI ##
Neste ponto entra ReactiveUI. Ele traz a programação reativa ao padrão MVVM com um "ViewModel Reativo", onde você tem propriedades observáveis e os comandos trabalham de maneira reativa. Isto parece complicado, mas um exemplo irá clarear as coisas. Vamos criar uma pequena tela de login que permite que o usuário se logue a uma aplicação.

No Visual Studio, crie um novo programa WPF e adicione o pacote NuGet ReactiveUI a ele. Então, podemos criar nosso modelo. Crie uma nova pasta e chame-a de Models. Nele, crie uma nova classe e chame-a de __Login__. Nela, adicione este código:

```C#
 public class Login
{
    public string UserName { get; set; }
    public string Password { get; set; }

    public async Task<bool> DoLogin()
    {
        var validData = new Dictionary<string, string>()
        {
            {"john", "wayne"},
            {"robert", "deniro"},
            {"meryl", "streep"},
            {"julia", "roberts"},
            {"richard", "gere"},
            {"drew", "barrymore"}
        };
        if (string.IsNullOrWhiteSpace(UserName) || string.IsNullOrWhiteSpace(Password))
            return false;
        var userName = UserName.ToLowerInvariant();
        await Task.Delay(5000);
        return validData.ContainsKey(userName) && 
            validData[userName] == Password.ToLowerInvariant();
    }
}
```

Esta classe não tem nada de especial: apenas duas propriedades e um método para fazer o login. Note que estou adicionando um atraso de 5s para dar a impressão que há um processamento demorado em execução. O próximo passo é adicionar os controles à janela principal da View. Em MainWindow.xaml, adicione este código:

```XML
<Window x:Class="_1___Introduction.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Title="MainWindow" Height="200" Width="350">
    <Window.Resources>
        <BooleanToVisibilityConverter x:Key="BooleanToVisibilityConverter" />
    </Window.Resources>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="3*"/>
        </Grid.ColumnDefinitions>
        <TextBlock VerticalAlignment="Center" Margin="5" Grid.Column="0" Grid.Row="0" Text="User Name"/>
        <TextBox VerticalAlignment="Center" Margin="5" Grid.Column="1" Grid.Row="0" 
                 Text="{Binding UserName, UpdateSourceTrigger=PropertyChanged}" Height="30" VerticalContentAlignment="Center"/>
        <TextBlock VerticalAlignment="Center" Margin="5" Grid.Column="0" Grid.Row="1" Text="Password"/>
        <TextBox VerticalAlignment="Center" Margin="5" Grid.Column="1" Grid.Row="1" VerticalContentAlignment="Center"
                 Text="{Binding Password, UpdateSourceTrigger=PropertyChanged}" Height="30"/>
        <Button VerticalAlignment="Bottom" HorizontalAlignment="Right" Margin="5" 
                Content="Sign In" Grid.Row="2" Grid.Column="1" Width="65" Height="30"
                Command="{Binding LoginCommand}" />
        <Border HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="#40000000" Grid.ColumnSpan="2" Grid.RowSpan="3"
                Visibility="{Binding IsBusy, Converter={StaticResource BooleanToVisibilityConverter}}"/>
    </Grid>
</Window>
```

A janela tem duas caixas de texto para entrar com o nome do usuário e a senha (eu deveria usar uma _PasswordBox_ para a senha, mas estou usando uma _TextBox_ para não ter que trabalhar com _SecureStrings_), um botão que irá ser usado para processar o login e uma borda que é sobreposta à janela e será mostrada quando o login estiver em processamento.

O passo seguinte é criar um ViewModel que fará a interface entre o Model e a View. Crie uma nova pasta e chame-a __ViewModels__. Adicione uma nova classe e chame-a de __LoginViewModel__. Adicione o seguinte código à classe:

```C#
public class LoginViewModel : ReactiveObject
{
    private readonly Login _login;
    private bool _isBusy;
    private string _password;

    private string _userName;

    public LoginViewModel(Login login)
    {
        _login = login;

        var canLogin = this.WhenAnyValue(x => x.UserName, x => x.Password, x => x.IsBusy,
            (u, p, b) => !b && !string.IsNullOrEmpty(u) && !string.IsNullOrEmpty(p));
        LoginCommand = ReactiveCommand.CreateFromTask<string, bool>(_ => DoLogin(), canLogin);
        LoginCommand.Subscribe(CheckLogin);
        this.WhenAnyValue(x => x.UserName).Subscribe(n => _login.UserName = n);
        this.WhenAnyValue(x => x.Password).Subscribe(p => _login.Password = p);
    }

    public string UserName
    {
        get { return _userName; }
        set { this.RaiseAndSetIfChanged(ref _userName, value); }
    }


    public string Password
    {
        get { return _password; }
        set { this.RaiseAndSetIfChanged(ref _password, value); }
    }

    public bool IsBusy
    {
        get { return _isBusy; }
        set { this.RaiseAndSetIfChanged(ref _isBusy, value); }
    }

    public ReactiveCommand<string, bool> LoginCommand { get;  }

    private async Task<bool> DoLogin()
    {
        IsBusy = true;
        return await _login.DoLogin();
    }

    private void CheckLogin(bool b)
    {
        IsBusy = false;
    }
}
```  

O ViewModel é derivado de ReactiveObject, que implementa a interface _INotifyPropertyChanged_ para notificar as mudanças. A parte principal do ViewModel é o construtor, onde você configura os comandos e as assinaturas. Neste ViewModel, criamos um Observable _canLogin_, que emite um valor cada vez que o nome do usuário, senha ou a propriedade _IsBusy_ mudarem. Ele emitirá um valor verdadeiro quando _IsBusy_ for falso e o usuário e senha tiverem um valor. Então criamos um _LoginCommand_, que executa o método _DoLogin_ quando for ativado e só pode estar ativo quando _canLogin_ emitir _true_.

A última parte do construtor irá atualizar o modelo toda vez que o nome do usuário ou a senha mudam. Em MainPage.xaml.cs, ligamos a View ao ViewModel:

```C#
public MainWindow()
{
    InitializeComponent();
    DataContext = new LoginViewModel(new Login());
}
```

Agora, quando você executar o programa, você verá que o botão _Signin_ só está disponível quando você preenche algo nas duas caixas. Ao clicar no botão Signin, a borda cobre a tela até que o login esteja terminado. Criaremos uma assinatura para _canLogin_ para podermos ver o que acontece toda vez que algo ocorre:

```C#  
public LoginViewModel(Login login)
{
    _login = login;

    var canLogin = this.WhenAnyValue(x => x.UserName, x => x.Password, x => x.IsBusy,
        (u, p, b) => !b && !string.IsNullOrEmpty(u) && !string.IsNullOrEmpty(p));
    canLogin.Subscribe(r =>
    {

    });
    LoginCommand = ReactiveCommand.CreateFromTask<string, bool>(_ => DoLogin(), canLogin);
    LoginCommand.Subscribe(CheckLogin);
    this.WhenAnyValue(x => x.UserName).Subscribe(n => _login.UserName = n);
    this.WhenAnyValue(x => x.Password).Subscribe(p => _login.Password = p);
}
```

Adicione um breakpoint na chave de fechamento da assinatura e execute o programa. Quando o programa parar, você pode usar a varinha mágica do OzCode (http://oz-code.com) para criar um tracepoint com este valor `Result = {r}`:

![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig1.png)
![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig2.png)

Você pode adicionar tracepoinst para os setters de _UserName_, _Password_ e _IsBusy_ com estes valores:

•	UserName novo valor = {value}
•	Password novo valor = {value}
•	IsBusy novo valor = {value}

Então, execute o programa novamente e preencha os valores até que o botão fique habilitado e clique no botão para que o login seja ativado. Depois disso, você pode terminar o programa e abrir a janela de tracepoints clicando no número de mensagens de trace na parte de baixo da janela do Visual Studio:

![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig3.png)

Você verá algo como o seguinte:

![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig4.png)

Na primeira linha, eu ainda estou entrando com o nome do usuário e _canLogin_ emite __False__. Assim que digito a primeira letra da senha (quarta linha), _canLogin_ começa a emuitir __True__ e o botão fica ativo. Na quarta linha de baixo para cima, eu clico no botão _Signin_ e _IsBusy_ fica verdadeiro, fazendo com que _canLogin_ emita __False__. Quando o login é completado, _IsBusy_ retorna a __False__ e _canLogin_ emite _True_ novamente. Como você pode ver, tudo foi configurado no construtor e os observáveis emitem os valores corretos enquanto as coisas acontecem na IU. Legal, não?
Agora podemos criar o programa que trabalha por nós (ops, nosso personagem fictício) quando o modo procrastinação está ligado: o cliente Twitter auto-atualizável.

## Cliente Twitter auto-atualizável ##
O primeiro passo para criar um cliente Twitter é obter uma chave de aplicação no site de desenvolvimento do Twitter [http://apps.twitter.com](http://apps.twitter.com). Lá você pode registrar sua aplicação e obter duas chaves: a consumer key e o consumer secret:


![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig5.png)

Com isto, podemos começar a criar nossa aplicação Twitter. Crie uma nova aplicação WPF e chame-a de __ReactiveTwitter__. No Solution Explorer, clique com o botão direito no nó _References_ e selecione _Manage NuGet Packages_. Adicione os pacotes ReactiveUI e Tweetinvi. Tweetinvi ([https://github.com/linvi/tweetinvi](https://github.com/linvi/tweetinvi)) é uma biblioteca de código aberto que facilita o acesso à API Twitter.

Com o Consumer Key e o Consumer Secret, você deve fazer a autenticação do usuário, para que ele possa ter acesso à aplicação. Isto pode ser feito de duas maneiras:

- Você chama uma página web para autenticação, aonde o usuário dá acesso à aplicação e o Twitter redireciona para uma página com um token que dá acesso à aplicação. Isto é melhor para aplicações web, aonde a página de redirecionamento está na aplicação
- Para aplicações desktop, a melhor maneira de autenticar é abrir uma página aonde o usuário dá acesso e obtém um código de acesso. Este código é digitado na aplicação e, com este código, a aplicação pode opter os tokens de acesso.

Estes tokens devem ser guardados na aplicação, de maneira que o usuário não precisa autenticar novamente cada vez que entra na aplicação. Todas estas etapas serão feitas em uma nova classe, chamada de __TwitterAuthenticator__. Na aplicação, crie uma nova pasta e chame-a de _Model_. Nela, adicione uma nova classe chamada _TwitterAuthenticator_. A classe é semelhante a isto:

```C#
public class TwitterAuthenticator
{
    private static IAuthenticationContext _authenticationContext;
    const string CreadentialsFileName = "ReactiveTwitter.xml";

    public static bool AuthenticateUser()
    {
        var consumerKey = ConfigurationManager.AppSettings["ConsumerKey"];
        var consumerSecret = ConfigurationManager.AppSettings["ConsumerSecret"];
        var userCredentials = GetCredentials();
        if (userCredentials != null)
        {
            Auth.SetUserCredentials(consumerKey, consumerSecret,
                userCredentials.AccessToken, userCredentials.AccessTokenSecret);
            return true;
        }
        var appCredentials = new TwitterCredentials(consumerKey, consumerSecret);

        _authenticationContext = AuthFlow.InitAuthentication(appCredentials);

        Process.Start(_authenticationContext.AuthorizationURL);
        return false;
    }

    public static void CreateAndSetCredentials(string pinCode)
    {
        var userCredentials = AuthFlow.CreateCredentialsFromVerifierCode(pinCode, _authenticationContext);

        Auth.SetCredentials(userCredentials);
        SaveCredentials(userCredentials);
    }

    private static ITwitterCredentials GetCredentials()
    {
        string settingsDirectory = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
        if (!Directory.Exists(settingsDirectory))
            Directory.CreateDirectory(settingsDirectory);
        string credentialsFile = Path.Combine(settingsDirectory, CreadentialsFileName);
        if (!File.Exists(credentialsFile))
            return null;
        var credentialsDoc = XDocument.Load(credentialsFile);
        if (credentialsDoc.Root == null)
            return null;
        return new TwitterCredentials("", "", credentialsDoc.Root.Element("AccessToken")?.Value, 
            credentialsDoc.Root.Element("AccessSecret")?.Value);
    }

    public static void SaveCredentials(ITwitterCredentials credentials)
    {
        string settingsDirectory = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
        if (!Directory.Exists(settingsDirectory))
            Directory.CreateDirectory(settingsDirectory);
        string credentialsFile = Path.Combine(settingsDirectory, CreadentialsFileName);
        XDocument credentialsDoc = new XDocument(
            new XElement("Credentials",
                new XElement("AccessToken", credentials.AccessToken),
                new XElement("AccessSecret", credentials.AccessTokenSecret)));
        credentialsDoc.Save(credentialsFile);
    }
}
```

O primeiro método, _AuthenticateUser_, fará o primeiro passo da autenticação: com o Consumer Key e Consumer Secret, ela irá obter a URL de autenticação e irá abri-la. Antes disso, o método checa se há credenciais salvas (no meu caso, eu as salvei como texto em um arquivo XML, mas você deve criptografá-las para uso real). Se há credenciais salvas, a aplicação usa elas para fazer o login e o método retorna. Se não há credenciais salvas, o método mostra a página de autorização no browser.

O segundo método, __CreateAndSetCredentials__, vai pegar o código de acesso que foi dado ao usuário e usá-lo para criar novas credenciais de acesso e salvá-las no arquivo XML.

O ViewModel principal fará todo o processo de autenticação e obterá os dados. No projeto, crie uma nova pasta e chame-a de _ViewModels_ e adicione uma nova classe a ala, chamando-a de _MainViewModel_. A classe deve ser semelhante a esta:

```C# 
public class MainViewModel : ReactiveObject
{
    private string _userName;
    private string _userPicture;
    private IEnumerable<ITweet> _tweets;
    private bool _isGettingPin;
    private string _pinValue;

    public MainViewModel()
    {
        var authObs = Observable.Start(TwitterAuthenticator.AuthenticateUser);
        authObs.Subscribe(logged =>
        {
            if (!logged)
                IsGettingPin = true;
            else
                SetAuthenticatedUser(User.GetAuthenticatedUser());
        });
        ConfirmPinCommand = ReactiveCommand.Create(DoConfirmPin);
        CancelPinCommand = ReactiveCommand.Create(DoCancelPin);
    }

    private void DoCancelPin()
    {
        IsGettingPin = false;
    }

    private void DoConfirmPin()
    {
        IsGettingPin = false;
        TwitterAuthenticator.CreateAndSetCredentials(PinValue);
        var user = User.GetAuthenticatedUser();
        SetAuthenticatedUser(user);
    }

    private void SetAuthenticatedUser(IAuthenticatedUser u)
    {
        UserName = u.Name;
        UserPicture = u.ProfileImageUrl400x400;
        Observable.Timer(TimeSpan.Zero, TimeSpan.FromSeconds(15)).Subscribe(_ =>
            Tweets = u.GetHomeTimeline(100));
    }

    public string UserName
    {
        get { return _userName; }
        set { this.RaiseAndSetIfChanged(ref _userName, value); }
    }

    public string UserPicture
    {
        get { return _userPicture; }
        set { this.RaiseAndSetIfChanged(ref _userPicture, value); }
    }

    public string PinValue
    {
        get { return _pinValue; }
        set { this.RaiseAndSetIfChanged(ref _pinValue, value); }
    }
    public bool IsGettingPin
    {
        get { return _isGettingPin; }
        private set { this.RaiseAndSetIfChanged(ref _isGettingPin, value); }
    }

    public IEnumerable<ITweet> Tweets
    {
        get { return _tweets; }
        private set { this.RaiseAndSetIfChanged(ref _tweets, value); }
    }

    public ReactiveCommand ConfirmPinCommand { get; }
    public ReactiveCommand CancelPinCommand { get;  }
}
```

O ViewModel começa criando um Observable que chamará _AuthenticateUser_ e retorna se o usuário já está logado. Se o usuário está logado, ele inicializa os dados do usuário e inicia a obter a timeline do usuário. Se não estiver logado, ele configura a propriedade _IsGettingPin_ para __True__ e inicializa os dois comandos necessários para a entrada do código de acesso.

_SetAuthenticatedUser_ irá configurar os dados do usuário e criar umn novo Observable que dispara a cada 15 segundos e pega a timeline do usuário. _Voilà_! Não há mais necessidade de refrescar os dados!

O XAML da janela principal é o seguinte:

```XML
<Window x:Class="ReactiveTwitter.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Title="MainWindow" Height="800" Width="1000">
    <Window.Resources>
        <BooleanToVisibilityConverter x:Key="BooleanToVisibilityConverter" />
    </Window.Resources>
    <Grid>
      <Grid.RowDefinitions>
          <RowDefinition Height="60"/>
          <RowDefinition Height="*"/>
      </Grid.RowDefinitions> 
        <StackPanel HorizontalAlignment="Right" Margin="10,0" VerticalAlignment="Center" Orientation="Horizontal">
            <TextBlock Text="{Binding UserName}" Margin="10,0" VerticalAlignment="Center" FontWeight="Bold"/>
            <Image Width="50" Height="50" Source="{Binding UserPicture}"/>
        </StackPanel>
        <ListBox Grid.Row="1" ItemsSource="{Binding Tweets}" ScrollViewer.HorizontalScrollBarVisibility="Disabled" />
        <Grid Grid.Row="0" Grid.RowSpan="2" Visibility="{Binding IsGettingPin, Converter={StaticResource BooleanToVisibilityConverter}}">
            <Grid Background="Black" Opacity="0.5"/>
            <Border
            MinWidth="250"
            Background="DarkCyan" 
            BorderBrush="Black" 
            BorderThickness="1" 
            HorizontalAlignment="Center" 
            VerticalAlignment="Center">
                <StackPanel>
                    <TextBlock Margin="5" Text="Pin Value:" FontWeight="Bold"  />
                    <TextBox MinWidth="150" HorizontalAlignment="Center" VerticalAlignment="Center" Text="{Binding PinValue}"/>
                    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center">
                        <Button Margin="5" Content="Ok" Width="65" Command="{Binding ConfirmPinCommand}"/>
                        <Button Margin="5" Content="Cancel" Width="65" Command="{Binding CancelPinCommand}" />
                    </StackPanel>
                </StackPanel>
            </Border>
        </Grid>
    </Grid>
</Window>
```

A janela tem duas partes: a segunda parte é uma grid usada para pegar o código de acesso. Ela tem uma caixa de texto e dois botões, onde você pode confirmar ou cancelar a entrada. Esta grid só é mostrada quando _IsGettingPin_ é verdadeiro. Porisso configuramos _IsGettingPin_ para verdadeiro quando o usuário não está logado e resetamos ele para falso quando o usuário clica em algum dos botões. A primeira parte da tela é onde os dados serão mostrados: o nome e imagem do usuário logado e sua timeline. Os dados do usuário são preenchidos no início e a lista de tweets é preenchida a cada 15 segundos.

Agora só precisamos ligar o ViewModel à View em Mainpage.xaml.cs:

```C#
public MainWindow()
{
    InitializeComponent();
    DataContext = new MainViewModel();
}
```

Agora, quando você executa o programa, obtém algo semelhante a isso:

![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig6.png)

E uma página web se abre para você autorizar a app:

![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig7.png)

Quando você autoriza a app, um código de acesso aparecerá na tela:

![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig8.png)

Você deve digitar o número na tela principal e está pronto para obter os dados. Os tweets devem aparecer na tela principal.

![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig9.png)

Esta não é a melhor visualização para os tweets. Ela mosta o valor do método __ToString__ para a classe _Tweet_. Vamos melhorar isso colocando um template de dados (Data Template) para os itens da lista. Queremos colocar a imagem, o nome e o username de quem criou, a data do tweet e o seu texto. Com esta informação, podemos criar o DataTemplate para os itens:

```XML 
<ListBox.ItemTemplate>
    <DataTemplate>
        <Grid Margin="5" TextElement.FontSize="14">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="60"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            <Image Source="{Binding CreatedBy.ProfileImageUrl400x400}" Margin="5" Height="50" Width="50"/>
            <Grid Grid.Column="1">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                <StackPanel Orientation="Horizontal">
                    <TextBlock Text="{Binding CreatedBy.Name}" Margin="0,0,10,0" FontWeight="Bold"/>
                    <TextBlock Text="@"/>
                    <TextBlock Text="{Binding CreatedBy.ScreenName}" Margin="0,0,10,0"/>
                    <TextBlock Text="{Binding CreatedAt}" Margin="0,0,10,0"/>
                </StackPanel>
                <TextBlock Grid.Row="1" Text="{Binding Text}" TextWrapping="Wrap"/>
            </Grid>
        </Grid>
    </DataTemplate>
</ListBox.ItemTemplate>
```

Quando adicionamos este data template à Listbox, obtemos algo semelhante a isto:

![](http://blogs.msmvps.com/bsonnino/files/2017/03/Fig10.png)

## Conclusões ##
Como você pode ver, usar o ReactiveUI é uma maneira fácil de transformar os seus ViewModels reativos, e com pouco código você pode criar um programa que mostra sua timeline, refrescando os dados a cada 15 segundos, sem necessidade de atualização manual. Problema resolvido! Agora, de volta ao trabalho!

O código fonte completo do projeto está em [https://github.com/bsonnino/ReactiveTwitter](https://github.com/bsonnino/ReactiveTwitter)