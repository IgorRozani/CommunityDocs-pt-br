#Windows Forms Application não morreu

Olá pessoal, hoje eu vou falar um pouco sobre o Windows Forms. Vou mostrar também um software sendo construído passo a passo com menu e sistema básico de login usando App.config. Nesse primeiro passo, vou verificar se o usuário e senha estão iguais ao arquivo de configuração.

***Utilizado:***

1. Ferramenta de desenvolvimento: Visual Studio
2. Linguagem de Programação: C#
3. Plataforma: Windows Forms


Essa plataforma é muito boa ainda, principalmente porque você tem acesso à máquina, sendo PC, notebook ou Surface. Pra quem não sabe, você consegue fazer software local utilizando esta tecnologia. Em muitas empresas ainda existe um legado ou querem criar novos softwares locais, isto é, em caso de não precisar de Internet, o banco de dados pode ser local e precisa de acesso a certas pastas do computador por exemplo.


Ficou muito fácil criar Windows Forms utilizando a ferramenta Visual Studio da Microsoft. Muitas vezes você precisa ler ou escrever um arquivo, gerar uma imagem local ou capturar imagem, ou apenas gravar dados em um banco de dados local.


A instalação de software criado na plataforma Windows Forms é muito simples, a própria ferramenta Visual Studio possui mecanismos para criar um instalador contendo todos os arquivos e banco de dados se necessário. A máquina do cliente precisa ter apenas o Framework .NET instalado para tudo funcionar e hoje, mesmo em máquinas antigas, o Framework pode ser instalado.


A minha dica pra você programador é que, aprenda a trabalhar com essa plataforma pois tem ainda muito mercado. Falo isso porque muitos desenvolvedores sabem apenas Web e na verdade muitos softwares que utilizamos no dia a dia são locais e Windows Forms. Você pode fazer um software local web, mas nem todos os recursos Web estão disponíveis como no Windows Forms.

***Vamos praticar um exemplo simples***

Para iniciar um projeto Windows Forms Application é necessário escolher a opção Windows Desktop e no primeiro item existe o template. Veja a figura 1.

![Image](images/tela-1.png)

Escolhendo template


Digite um nome no campo Name, escolha um local clicando no botão Browse… e clique no botão Ok.


![Image](images/tela-2.png)

Figura 1 - Solution Explorer

O projeto cria formulários com extensão .CS que significa CSharp. Vou precisar criar mais dois formulários, um para o login e outro para fazer um simples cadastro. Você concorda comigo que o formulário de Login (frmLogin.cs) precisa ser o primeiro a aparecer para o usuário e para mudar isso, acesse o arquivo Program.cs e mude o formulário para frmLogin().

Para gerar um novo formulário, basta clicar com o botão direito em cima do WindowsFormsApplication2, escolha a opção Add e New Item… Dê um nome para o formulário e depois clique Ok.

O Visual Studio entrega pra você vários objetos para serem utilizados dentro do seu programa ou software. 

![Image](images/tela-3i.png)

Figura 2 - Ferramenta e objetos

Note que existem objetos como Button, CheckBox, Label, TextBox e muitos outros. O primeiro passo depois de ver os objetos é colocar o formulário principal como mdiContainer = true. Isso faz com que exista um formulário pai e outros filhos. Veja a figura 3.


![Image](images/tela-3.png)

Figura 3 - Colocar o formulário 1 como pai.


Pra que serve que o formulário 1 virar pai? A resposta é simples: para não ficar abrindo várias telas ao mesmo tempo, evita consumir memória da sua máquina e mais. O software acaba gerenciando todo o esquema de tela para você quando utiliza tela pai e tela filhos. 

![Image](images/tela-4.png)

Figura 4 - Formulário pai


Note que o formulário pai é cinza e possui um menu principal. As outras telas não precisam de menu. Para colocar um menu no formulário pai, é necessário encontrar o objeto na tela de ferramentas, clicar, arrastar e jogar.
Acesse o item Menus & Toolbars, dentro do item existe o MenuStrip. O objeto é responsável pela criação do menu simples e prático. Veja a figura 5.

![Image](images/tela-5.png)

Figura 5 - Objeto de menu para a tela principal.

Para gerar o menu é muito simples, basta clicar uma vez e digitar a descrição. A figura 6 eu mostro os três itens criados. Menu Principal, Cadastrar e Sair.

![Image](images/tela-4.png)

Figura 6 - Adicionando menus

Onde está escrito “Type Here” basta clicar e digitar. E como fazer para chamar a tela específica? Basta clicar duas vezes no item que deseja, por exemplo: clique duas vezes em cima do nome Cadastrar. A ferramenta gera um método do menu para chamar. O código 1 mostra o método criado pelo menu.

![Image](images/codigo-1.png)

Código 1 - Método do menu

A primeira linha do código um mostra a criação de instância do formulário. Depois basta indicar a variável como MdiParent e para finalizar chame o método Show() que chama o segundo formulário.

Acessando o menu novamente, cliquei duas vezes no menu Sair. Vou colocar um comando que faz com que o programa feche. Código 2.

![Image](images/codigo-2.png)

Código 2 - Comando de sair
	
O comando de sair é simples basta usar o método Exit() do Application, mostrado no código 2.

O próximo passo é gerar um novo formulário responsável pelo login. Vou usar basicamente dois labels, dois textBoxs e dois botões. Veja a figura 7 mostrando o form.

![Image](images/tela-7.png)

Figura 7 - Tela de login

Seguem os nomes do texts utilizados:
Nome: txtNome
Senha: txtSenha

Seguem os nomes dos botões utilizados:
Entrar: btnEntrar
Sair: btnSair

A regra principal é verificar se o usuário e senha estão iguais aos dados que estão no arquivo de configuração. O arquivo de configuração chamado App.config possui dois atributos abertos. Não é bom ficar aberto, mas neste primeiro momento vamos utilizar sem criptografia. Veja a figura 8.

![Image](images/tela-8.png)

Figura 8 - Arquivo de configuração com dados abertos
	
Acesse o arquivo App.config e digite as tags de nome e senha dentro da tag principal chamada appSettings. Voltando para a tela de login, clique duas vezes no botão de entrar. Veja o código 3.

![Image](images/codigo-3.png)

Código 3 - Botão de entrar

Ao clicar no botão entrar, os dados são pegos do arquivo de configuração e armazenados em duas variáveis específicas ***(_nome e _senha)***. O próximo passo foi verificar se o usuário e senha estão iguais para poder chamar a tela principal. O comando this.Hide(); faz com que o formulário de login se esconda para chamar outro formulário classificado como principal ou pai. No caso da senha e usuário digitado errado, uma mensagem aparece para o usuário limpando os campos e colocar o foco no campo Nome.

Bom, eu vou parar por aqui. Espero que tenha gostado e qualquer dúvida pode entrar em contato pelo site [www.mauriciojunior.org](https://www.mauriciojunior.org).