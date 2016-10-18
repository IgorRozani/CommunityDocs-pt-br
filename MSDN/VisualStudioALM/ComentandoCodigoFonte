---
title: Comentando Código Fonte
description: Olá pessoal, hoje eu vou falar de um conceito que está confundindo a mente de alguns desenvolvedores e acabando com as boas práticas no desenvolvimento de *software*. É um assunto polêmico, mas com a minha experiência e visão do que está acontecendo hoje, precisei escrever um pouco sobre.
author: MSCommunityPubService
ms.date: 10/18/2016
ms.topic: how-to-article
ms.service: Windows Forms
ms.custom: CommunityDocs
---

#Comentando Código Fonte

##**Mauricio Junior**
**MVP, MCAD e MCP Microsoft**
Setembro, 2016

[Homepage](https://www.mauriciojunior.org)

Olá pessoal, hoje eu vou falar de um conceito que está confundindo a mente de alguns desenvolvedores e acabando com as boas práticas no desenvolvimento de *software*. É um assunto polêmico, mas com a minha experiência e visão do que está acontecendo hoje, precisei escrever um pouco sobre.

![codigo fonte](img/ComentandoCodigoFonte-pic01.jpg)

**Utilizado:**

1. Visual Studio Code
2. Linguagem C#
3. Plataforma: Web e Desktop

**Comentar código é uma boa prática?**

Eu começo fazendo uma pergunta que a maioria dos desenvolvedores fazem. A minha resposta e de muitos outros programadores, arquitetos, engenheiros de *software* é que sim, é uma boa prática e é necessário. O que estou vendo hoje em dia no mundo dos desenvolvedores é que estão peguiçosos, criam variáveis de qualquer jeito, sem seguir um padrão e depois de um tempo sem trabalhar no código não vai se lembra do que fez ou onde utiliza a mesma variável dentro do código e porque usa daquele jeito.

Depois de conversar com muitos, percebi que é por simplesmente "preguiça" da parte do programador. O grande problema de não comentar variáveis, não comentar as instruções, não comentar métodos ou que está sendo feito dentro do método, é que depois de alguns anos sem mexer no código tudo aqui pode ser esquecido. Vai levar um tempo grande para descobrir o problema ou erro.

Conheço alguns programadores antigos que ainda ganham a vida dando manutenção em sistemas legados e que se não fosse os comentários, estariam perdidos no momento da manutenção ou alteração do código. Pois muitos pensam que irão lembrar ou que se alterar qualquer parte do código não irá afetar outra parte do *software*. Só que hoje em dia os códigos estão integrados e tudo se comunica.

Concordo que não precisa comentar toda linha de código, mas o programador precisa se acostumar e entender que o comentário faz parte. Fazer métodos auto explicativos já reduz alguns tipos de comentários. Isso não retira a necessidade de comentar dentro do método nos casos de regra de negócio ou coisas mais específicas.

**Exemplo usando C#**

A linguagem C# somado com a ferramenta Visual Studio facilita o trabalho de comentar código. Um grande exemplo é depois de criar o método. Digite três barras "///" na linha acima do método e a ferramenta já cria o necessário para colocar os comentários. Summary: um resumo do que o método faz ou trata e no meu caso coloquei o nome do banco de dados e tabela do banco de dados. Param name: porque o método recebe um parâmetro chamado DisciplinasDTO e foi detectado. Em caso de métodos que não recebem parâmetros, não vai aparecer essa tag. Returns, a última tag criada para esse meu exemplo, definida para colocar o tipo de retorno do método.

Para este exemplo, como não existe regra de negócio comentei algumas linhas desnecessárias, mas serve pra você ver. Passando para dentro do método, basta comentar dentro do código, principalmente em vários comandos; basta digitar duas barras "//" e o comentário. Veja o exemplo no código 1.

Código 1 - Comentando método e dentro do método.

		/// <summary>
        /// Método responsável por inserir dados no banco de dados
        /// Tabela: Disciplinas
        /// Banco: BDAluno
        /// </summary>
        /// <param name="dto">DisciplinasDTO</param>
        /// <returns>Boolean</returns>
        internal Boolean inserirDisciplina(DisciplinasDTO dto)
        {
            using (SqlConnection conn = 
                new SqlConnection(System.Configuration.ConfigurationManager.ConnectionStrings["SQLConexaoLocal"].ToString()))
            {
                try
                {
                    //abre o banco de dados e faz cria o sql para inserir
                    conn.Open();
                    var sql = "INSERT INTO Disciplinas (NomeDisc) VALUES (@descricao) ";

                    //gera a variável de comando responsável por passar o sql e a 
                    //variável de conexão com o banco de dados
                    SqlCommand command = new SqlCommand(sql, conn);

                    //indica o tipo de comando que será enviado
                    //neste caso é um texto normal, pode pode virar Store Procedure por exemplo
                    command.CommandType = CommandType.Text;

                    //adicionando parâmetros baseado no sql
                    //o mesmo nome no valor
                    command.Parameters.AddWithValue("@descricao", dto.descricaoDisc);

                    //executando o comando
                    command.ExecuteNonQuery();

                    //se der tudo certo, retorna true.
                    return true;
                }
                catch (Exception ex)
                {
                    throw ex;
                }
                finally
                {
                    //fecha a conexão, mesmo se der erro na execução do comando
                    conn.Close();
                }
            }
        }


Esse é um código simples mas já serve para você ver como fazer e porque fazer. Lembre-se que se você está desenvolvendo um *framework* e os dados serão transformados em *dll*, é necessário comentar tudo porque fica disponível para quem for utilizar o *framework*. Todos os comentários são exportados dentro da dll, principalmente as classes, métodos e funções com C#.

**Dica**

Não deixe de comentar seus códigos, principalmente porque em algum momento você vai ter que voltar pra alterar, para copiar ou reaproveitar algum código. Tenha certeza que depois de um tempo sem mexer, você vai esquecer das regras, vai esquecer das variáveis, dos métodos criados para resolver a solução e ainda vai perder um tempo para entender tudo novamente. Hoje em dia, muitos *softwares* não tem documentação completa, pra isso, comece a documentar pelo menos que você desenvolve.

Se você está construindo uma plataforma base, no meu ponto de vista, é necessário fazer todo tipo de comentário porque outras pessoas vão utilizar. Comentário mostrando como criar o objeto, como fazer a chamada, como utilizar tal método e muito mais diminui o processo de entendimento.


**Outros tipos de comentários**

Outras linguagens de *software* utilizam o comentário de outra maneira, por exemplo: começa com barra e asterisco /** e finaliza com asterisco e barra **/. Swift, Objective-C, JavaScript (linguagem de interpretação), Java e muitas outras. Isso funciona tanto para método quanto para uma linha simples de código. Tudo que estiver dentro do asterisco fica comentado, pode ser uma ou várias linhas sem qualquer problema. O compilador entende como comentário e tudo funciona.


Espero que não me condene neste artigo, apenas compreenda. Você vai ver que não é perder tempo comentar. Lembro que todo este artigo foi feito de opinião pessoal e de pesquisa com outros profissionais.

Espero que tenha gostado e qualquer dúvida pode entrar em contato pelo site [www.mauriciojunior.org](https://www.mauriciojunior.org).
