+++
title = 'Expressões regulares: Uma introdução'
date = 2025-02-08T06:39:17-03:00
draft = false
+++

![regex-feeling-of-power](/regex-feelings-of-power.png)

No mundo incrivelmente rico em dados em que vivemos, saber lidar com grandes cadeias de texto, seja para extrair informações relevantes, substituir algum trecho de dados ou até mesmo simplesmente detectar a presença de algum padrão de forma rápida, é absolutamente crucial. Por isso, compreender expressões regulares é uma ferramenta poderosa para se ter no seu arsenal como desenvolvedor, pois pode trazer um ganho de produtividade na sua rotina diária.

## Aplicações

Sua importância reside no simples fato de que expressões regulares servem para uma ampla variedade de propósitos. Na prática, para listar alguns exemplos, imagine que você está preso em alguma das seguintes situações:

<ol>
    <li>Web scraping - Há uma página HTML em que você deseja extrair todas as URL's contidas no texto;</li>
    <li>Validação de formulário - Você está lidando com um formulário que possui centenas de campos e precisa csontruir validações customizadas para cada um deles: um email deve terminar com @domain.com e não pode possui caracteres especiais. Um número de telefone deve conter apenas números com uma determinada quantidade de dígitos, etc;</li>
    <li>Análise de Log - Em uma análise de log de uma aplicação, onde você gostaria de extrair apenas as informações relevantes de uma classe ou de um arquivo específico a partir de um enorme stack trace.</li>
</ol>

Em todos esses casos, as expressões regulares podem ajudar e te salvar de horas de trabalho.

## Mas, o que é isso, afinal ?

Embora as expressões regulares possam parecer algo que apenas os programadores mais hardcore usariam, elas nada mais são do que uma sequência de símbolos que especifica um padrão de texto. Esse padrão pode ser entendido como uma regra que, por sua vez, será testada contra um determinado texto de entrada, retornando um resultado positivo se a regra corresponder a alguma substring do texto de entrada. Caso contrário, será retornado um resultado negativo.

## Estudando o vocabulário dos lobisomens.

Apenas para esclarecer esse conceito, imagine que nós estamos tentando entender a língua dos lobisomens.

![howling-wolf](/howling-wolf.png)

Their vocabulary is very limited. They only use the following word pattern to make their sentences: starts with letter ‘a’, followed by letter ‘w’, followed by 2 or more instances of letter ‘o’. Finally, there’s an exclamation mark to enforce respect around the gang. Thus, the following are some word examples that a werewolf can express.

Seu vocabulário é extremamente limitado. Eles apenas usam o seguinte padrão de construção de palavra para formar suas frases: uma palavra inicia com a letra 'a', seguida pela letra 'w', seguida por 2 ou mais instâncias da letra 'o'. Por fim, há sempre um ponto de exclamação para impor respeito aos demais membros da alcateia. Portanto, abaixo constam alguns exemplos de palavras que um lobisomem pode expressar.

<ul>
    <li>awoo!</li>
    <li>awooooo!</li>
    <li>awoooooooo!</li>
</ul>


Então, o que precisamos fazer agora é descobrir um padrão que consiga representar todas essas diferentes palavras de  lobisomens. Esse é o ponto onde uma expressão regular vem a calhar. Felizmente (e convenientemente, neste caso), todas as palavras do vocabulário de lobisomens podem ser expressas por uma regex simples. Dê uma olhada na sua estrutura.

![howling-wolf](/regex-werewolves.png)

Antes de mais nada, algo importante de lembrar é que todas as expressões regulares residem dentro de delimitadores. Estas são as fronteiras de sua expressão regular. O caractere mais comum que é utilizado como delimitador é uma batta("/"), mas isso pode variar dependento do padrão de sintaxe utilizado(PCRE, POSIX, etc).

Assim, analisando essa regex, temos:

<ul>
    <li>
        awo: Este é o começo do padrão. Logo, até este ponto, significa que para existirem chances de um "match" ocorrer, o texto de entrada deve conter uma substring começando com awo, obrigatoriamente.
    </li>
    <li>
        o+: O caractere 'o' casa literalmente o próprio caractere 'o'. O '+' é chamado de quantificador e significa "um ou mais". Ele irá casar uma ou mais ocorrências do caractere imediatamente anterior,nesse caso, 'o'.
    </li>
    <li>
        !: É o fim do padrão. Casa exatamente o caractere '!'.
    </li>
</ul>

Portanto, nossa expressão regular nos dará uma resposta positiva para todos os seguintes textos de entrada: awooo!, awoo!, awoooo! e assim por diante.

Um ponto de atenção nessa regex é que, dependendo de qual for o nosso conjunto universo de strings de entrada, ela pode não funcionar como esperado. Essa regex funciona bem para detectar se uma palavra específica pertence à “língua dos lobisomens” no nosso input, mas, se o nosso objetivo for saber quantas palavras do vocabulário dos lobisomens estão presentes em um texto grande, por exemplo, ela vai falhar, uma vez que nossa regex só corresponde à primeira ocorrência desse padrão. Esse problema pode ser facilmente resolvido utilizando um recurso chamado "global flag", que detectará todas as ocorrências do padrão, mas não quero entrar nesses detalhes agora, então vamos assumir que nosso texto de entrada é apenas uma única palavra, que pode ou não fazer parte da linguagem dos lobisomens.

## Outros exemplos

O tópico anterior foi um exemplo bem simples, apenas para te familiarizar com o maravilhoso mundo das regex. Mas, acredite, à medida que as expressões vão ficando mais poderosas, elas também vão ficando mais complexas. Só para te dar uma visão rápida de quão poderosas as expressões regulares podem ser, dê uma olhada nesses outros exemplos.

<ul>
    <li>
        <b>/\/[^\r\n]*[\r\n]/g</b>  → Detecta se um código contém comentários do tipo // ;
    </li>
    <li>
       <b>/\d{5}-\d{4}|\d{5}/g</b>  → Busca por CEP's em um texto;
    </li>
    <li>
        <b>/^([a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6})*$/gm</b>  → Uma validação de email
    </li>
    <li>
        <b>/(?=(.*[0-9]))(?=.*[\!@#$%^&*()\\[\]{}\-_+=~`|:;"'<>,./?])(?=.*[a-z])(?=(.*[A-Z]))(?=(.*)).{8,}/</b>  →  Checa se uma senha é forte, i.e., se contém, no mínimo, 1 letra minúscula, 1 letra maiúscula, 1 número, 1 caractere especial e tenha pelo menos 8 caracteres de tamanho.
    </li>
</ul>

Não se assuste se não for uma das coisas mais compreensíveis que você já viu. Nosso objetivo é entender, devagar e passo a passo, os blocos básicos de construção desse enigmático quebra-cabeça.

## Regex engines

Por fim, nossas expressões regulares só farão sentido se houver alguma entidade capaz de interpretá-las, testá-las contra um texto de entrada e nos entregar um resultado desse processamento. Essa entidade é chamada de regex engine.

Uma regex engine recebe dois inputs: uma expressão regular e um texto de entrada.
O resultado final pode variar. Pode ser apenas um valor booleano indicando a presença ou ausência do padrão descrito pela regex, ou pode ser uma lista de substrings do texto de entrada que correspondem ao padrão. Tudo depende da sua necessidade.

![regex-engine](/regex-engine.png)

## Conclusão

Então, nessa breve introdução, te apresentamos ao mundo das expressões regulares e suas aplicabilidades. Nos próximos artigos, vamos nos aprofundar mais para conhecer os principais caracteres, seus significados e como aplicá-los.

