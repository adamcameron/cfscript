
Indice  


=================

  * [CFScript documentação](#Documentação-CFScript)
      * [Comentários](#Comentários)
      * [Afirmações](#Afirmações)
      * [Variáveis](#Variáveis)
      * [Operadores](#Operadores)
        * [Decisão](#Decisão)
        * [Aritmética](#Aritmética)
          * [Incremento / decremento](#Incremento-decremento)
          * [Atribuição inline](#Atribuição-inline)
        * [Boleano](#boolean)
        * [Decisão](#Decisão-)
          * [Operador Ternário](#Operador-ternário)
          * [Variação de coalescência nula](#Variação-de-coalescência-nula)
      * [Condições](#Condições)
        * [if/elseif/else](#ifelseifelse)
        * [switch](#switch)
        * [try/catch/finally, throw/rethrow](#trycatchfinally-throwrethrow)
      * [Iteração](#Iteração)
        * [Uso geral para loop](#Uso-geral-para-loop)
        * [Pré-condição loop](#Pré-condição-loop)
        * [Pós-condição loop](#Pós-condição-loop)
        * [Array loop](#array-loop)
          * [Para declaração](#for-statement)
          * [arrayEach()](#arrayeach)
          * [Array.each()](#arrayeach-1)
        * [Estrutura loop](#struct-loop)
          * [Para declaração](#for-statement-1)
          * [structEach()](#structeach)
          * [Struct.each()](#structeach-1)
        * [Query loop](#query-loop)
        * [Lista loop](#list-loop)
        * [Arquivo loop](#file-loop)
        * [Date/time range loop](#datetime-range-loop)
      * [Outras declarações de controle de fluxo](#other-flow-control-statements)
        * [Abortar processamento](#abort-processing)
        * [Sair do arquivo atual](#exit-from-current-file)
      * [Reutilização de codigo](#code-reuse)
        * [Incluir ](#include)
        * [Módulo](#module)
      * [Componentes / interfaces](#components--interfaces)
        * [Atributos](#attributes)
        * [Interface](#interface)
        * [Propriedades](#properties)
        * [Funções](#functions)
        * [Argumentos](#arguments)
        * [Anotações de função / Argumentos](#functionargument-annotations)
        * [Expressões de função](#function-expressions)
        * [Chamando funções dinamicamente](#calling-functions-dynamically)
        * [Importar](#import)
        * [Criação de objeto](#object-creation)
      * [Operações do sistema de arquivos](#file-system-operations)
        * [Diretórios](#directories)
        * [Arquivos](#files)
      * [Base de Dados](#database)
        * [Query](#query)
        * [Procedimento de armazenamento](#stored-procedure)
        * [Insert](#insert)
        * [Update](#update)
        * [DB Info](#db-info)
        * [Transações](#transactions)
      * [Debugging](#debugging)
        * [Dump](#dump)
        * [Log](#log)
        * [Trace](#trace)
        * [Timer](#timer)
      * [Geral / Diversos](#general--miscellaneous)
        * [Output](#output)
        * [Codificação de Arquivo](#file-encoding)
        * [Salvar Conteúdo](#save-content)
        * [Threading](#threading)
        * [Locking](#locking)
        * [Imagem / Manipulação XLS ](#image--xls-manipulation)
        * [Manipulação de PDF](#pdf-manipulation)
      * [Elementos de CFML baseada em tag sem implementação <em>específica</em>  de CFScript](#elements-of-tag-based-cfml-with-no-specific-cfscript-implementation)
        * [Soluções baseadas em CFC](#cfc-based-solutions)
        * [http.cfc](#httpcfc)
        * [mail.cfc](#mailcfc)
        * [The rest](#the-rest)

# Documentação CFScript

Isso tenta documentar todo o CFScript, como um recurso para pessoas que estão migrando do código baseado em tag da velha escola para o código baseado em script. Estou fazendo isso porque nem o ColdFusion nem o Railo / Lucee fornecem muito (ou no caso de Railo / Lucee: _any_) útil [documentação de CFScript](https://wikidocs.adobe.com/wiki/display/coldfusionen/The+CFScript+language).

Este não é um documento para converter tags em script. Não foi escrito do ponto de vista de "se você usar``<cfsometag>`` então você precisa usar [alguma construção de script] ". Ele simplesmente documenta o CFScript.

Existem algumas soluções de sintaxe neutra para algumas construções CFML que estão listadas na parte inferior do documento. Estes são os CFCs encontrados em[customtag]/com/adobe/coldfusion. Estas não são construções de CFScript por si só, mas relacionadas o suficiente para serem relevantes aqui.

Também não é um exercício de ensino de CFML (ou pelo menos a parte do script). Ele pressupõe que você sabe o que está fazendo e é puramente uma referência.

Presumo que seja Railo 4.2 / Lucee 4.5 ou ColdFusion 11, exceto onde indicado.


### Comentários
```cfc
// comentário de linha única
```
```cfc
a=1; // comentário de linha única no final da linha
```
```cfc
/*
    cometário
    de múltiplas
    linhas
*/
```
```cfc
/*
    linha múltipla
     /* comentários */
    não pode ser aninhado
*/
```

Neste caso, o bit comentado é `/ * linha múltipla / * comentários * /`, tornando o próximo bit um erro de sintaxe.

### Afirmações

As declarações terminam em ponto e vírgula:
```cfc
a = 1;
```
Os pontos-e-vírgulas são geralmente _ opcionais no Railo / Lucee:
```cfc
a = 1
```

Onde "geralmente" significa "se o final da declaração for inequívoco sem um ponto e vírgula". É melhor usar sempre ponto e vírgula.

As declarações de bloco (com chaves) não têm ponto e vírgula:
```cfc
while (condition) {
    // statements
}
```

### Variáveis

Atribuindo uma variável:
```cfc
varName = "foo";
```

Atribuindo uma variável local de função:
```cfc
var varName = "foo"; // analogous to local.varName =  "foo";
```

Observe que a palavra-chave var pode aparecer embutida na maioria das instruções onde uma variável é inicializada pela primeira vez, por exemplo:
```cfc
for (var i=1; i <= 10; i++);
```

Atribuição de uma variável nomeada dinamicamente:
```cfc
varName = "foo";
"#varName#" = "bar";
writeOutput(foo); // bar
```
<a href="http://trycf.com/editor/gist/aafe518f91bce312349e/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
É o mesmo que a tag `<cfset>`, mas confunde algumas pessoas por ter uma aparência um pouco estranha. Obviamente, também se pode usar a sintaxe de matriz associativa também (eg: `variables[varName] = "bar";`.Isso é preferível, pois é mais claro o que está acontecendo).

Padronizando uma variável:
```cfc
param numérico variableName = defaultValue; // onde "numérico" pode ser qualquer tipo
```
Para situações mais complexas:
```cfc
param name = "variableName" type = "regex" pattern = "."; // qualquer atributo cfparam é compatível
```

### Operadores

Todos os operadores disponíveis para código baseado em tag ainda funcionam em CFScript. Além disso, CFScript tem estes:

#### Decisão
```cfc
a == 1; // igualdade
a < 1;  // Menor que
a <= 1; // Menor ou igual
a >= 1; // maior que ou igual
a > 1;  // Maior que
a != 1; // desigualdade
a <> 1; // desigualdade (apenas Railo / Lucee)
```
<a href="http://trycf.com/editor/gist/5b6ecbe3d5ebbdc3b8d8/lucee?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### Aritmética

##### Incremento-decremento
```cfc
// incremento
a = 1;
b = a++; // b=1, a=2    // operadores pós-fixados retornam o valor e, em seguida, executam a ação (neste caso: incrementos a)
c = ++a; // c=3, a=3    // operador de prefixo executa a ação e retorna o resultado
```
```cfc
// decremento
a--; // a=2
--a; // a=1
```
<a href="http://trycf.com/editor/gist/eefca5ca3fe8c3ef800a/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
##### Atribuição-inline
```cfc
a += 2; // equivalente a a=a+2
a -= 3; // equivalente a a=a-3
a *= 4; // equivalente a a=a*4
a /= 5; // equivalente a a=a/5
a %= 6; // equivalente a a=a%6
s &= "a"; // equivalente a s = s & "a"
```
<a href="http://trycf.com/editor/gist/717ea2e12e318d6ae18e/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### Boolean
```cfc
!a;     // NOT a
a && b; // a AND b
a || b; // a OR b
```
<a href="http://trycf.com/editor/gist/85479549681ea703256c/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### Decisão-

##### Operador-ternário
```cfc
result = condition ? trueExpression : falseExpression;

//eg:
coinTossResult = randRange(0,1) ? "heads" : "tails";
```
<a href="http://trycf.com/editor/gist/007072ccaf71629f3a48/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
```cfc
// NB: apenas um de trueExpression ou falseExpression é avaliado:
a = 1;
b = 1;
c = false ? ++a : ++b; // a=1, b=2, c=2
```
<a href="http://trycf.com/editor/gist/da4ec1cfa62aa134ca4e/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
##### Variação-de-coalescência-nula
```cfc
result = left ?: right; //a expressão da mão esquerda é usada a menos que seja nula, caso em que a direita é usada

//eg:
a = d ?: "default"; // a = default
```
<a href="http://trycf.com/editor/gist/e1e440bab2b856474e88/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
```cfc
d = 1;
a = d ?: "default"; // a = 1
```
<a href="http://trycf.com/editor/gist/7824257754c541adbacb/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>

Observe que, embora pareça ser o mesmo que o operador "Elvis" de outro idioma, não é o mesmo. O operador "Elvis" verifica se há `false` (e em alguns idiomas` null` é falsey); este operador verifica especificamente por `null`. Isso provavelmente se deve a um mal-entendido por parte da Adobe (e perpetuado pelos desenvolvedores Lucee ao copiá-lo).

### Condições

#### if/elseif/else
```cfc
if (booleanExpression)
    // única instrução executada se booleanExpression for verdade
else if (anotherBooleanExpression)
    // única instrução executada se anotherBooleanExpression for verdadeira
else
   // única instrução executada se as condições forem falsas
```
```cfc
if (booleanExpression) {
  // várias instruções executadas se booleanExpression for true
} else if (anotherBooleanExpression) {
    // várias instruções executadas se outraBooleanExpressionis true
} else {
   // várias instruções executadas se as condições forem falsas
}
```
<a href="http://trycf.com/editor/gist/79d1a9adeea8c2b3d0b6/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### switch
```cfc
switch (expression) {
    case "some constant value": // o valor pode ser dinâmico em Railo / Lucee
         // instruções executadas se expressão = "algum valor constante"
    break;// sair da instrução switch
    case "um valor constante diferente":
       // instruções executadas se expression = "um valor constante diferente"
      // se não houver interrupção, o processamento continua a executar as instruções até que uma interrupção seja encontrada ...
       // ... mas as avaliações de caso subsequentes não são feitas. Um switch é basicamente um mecanismo GOTO, que faz um ...
       // único GOTO o primeiro caso correspondente. NÃO é uma série de instruções if / elseif / else
    caso "terceiro valor constante":
        // instruções executadas se expressão = "um valor constante diferente" ou "terceiro valor constante"
    break;
    case "4th value":
    case "5th value":
            // instruções executadas se a expressão for um de "4º valor" ou "5º valor"
    break;
    default:
      // instruções executadas se nenhum caso foi atendido (ou se o último caso atendido não teve uma pausa)
    break;
}
```
 <a href="http://trycf.com/editor/gist/9d104178a7630d198a16/acf11?theme=solarized_dark" target="_blank">![trycf-logo] Run this example on trycf.com</a>
#### try/catch/finally, throw/rethrow
```cfc
try {
  // afirmações

    throw "message"; // lança uma exceção de aplicativo, com a mensagem fornecida

    // or
    throw (type="ExceptionType", message="message", detail="detail", errorCode="errorCode", extendedInfo="extendedInfo"); // apesar das aparências, esta NÃO é uma função

    // or
    throw (object=JavaExceptionObject);
}
catch (SomeExceptionType variableContainingExceptionObject) {
   // instruções executadas se o código em erros de bloco try com uma exceção SomeExceptionType

    rethrow; // rethrows the caught exception
}
catch (SomeOtherExceptionType variableCOntainingExceptionObject) {
    // instruções executadas se o código em erros de bloco try com uma exceção SomeOtherExceptionType
}
catch (any variableCOntainingExceptionObject) {
   // instruções executadas se o código em erros de bloco try com qualquer tipo de exceção ainda não detectada
}
finally {
    // instruções executadas em qualquer caso, INCLUINDO exceções não tratadas. Este código SEMPRE roda
}
```

### Iteração

#### Uso-geral-para-loop
```cfc
for (initialisation; condition; repetition) statement;
```

or:
```cfc
for (initialisation; condition; repetition) {
   
// afirmações
}
```

EG:
```cfc
for (i=1; i <=5; i++) writeOutput(i); 
// apenas a seguinte instrução única é repetida
```

or:
```cfc
for (i=1; i <=5; i++) {
   // todas as instruções dentro do bloco são executadas em loop
    result = i * 2;
    writeOutput(result);
}
```

A percepção geral é que esta é a única forma de um loop for () de propósito geral: inicializar uma variável de contador, testá-la e ajustá-la (incremento, decremento). Este não é o caso. Cada uma das instruções pode ser _qualquer_coisa_ (a condição precisa ser avaliada como um booleano) e, de fato, são opcionais. Este é um loop infinito, equivalente a while (true):
```cfc
for (;;)
```

Um exemplo muito artificial para demonstrar a liberdade que se tem com os parâmetros do <span id="contrivedFor">for()</span>:
```cfc
i=0;
for (; true; writeOutput(i)) {
    if (++i > 5) break;
}
```

Em geral, todas as construções de loop têm a sintaxe de instrução única ou bloco de instruções. Oferecerei apenas a sintaxe de bloco mais comum (e recomendada, para clareza de código) de agora em diante.

#### Pré-condição-loop

Esta forma de loop avalia uma única condição no início de cada iteração e continua em loop enquanto a condição for verdadeira:
```cfc
while (condition) {
    // statements
}
```

Esta forma de loop será executada zero ou mais vezes.

#### Pós-condição-loop


Esta forma de loop avalia uma única condição no início de cada iteração e continua em loop enquanto a condição para verdadeira:
```cfc
do {
    // statements
} while (condition);
```

Esta forma de loop será executada _uma_ ou mais vezes. É importante considerar que o corpo do loop sempre será executado na primeira vez, porque nenhuma condição é avaliada até o _ final_ do loop.

#### Array loop

##### For statement
```cfc
for (element in [1,2,3,4,5]) {
    writeOutput(element); // 12345
}
```

##### arrayEach()
```cfc
arrayEach(["a","b","c"], function(element,index,array) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
});
```

##### Array.each()
```cfc
a = ["a","b","c"];
a.each(function(element,index,array) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
});
```


Observe que Railo / Lucee pode chamar métodos diretamente em um literal, então isso funciona:
```cfc
["a","b","c"].each(function(element,index,array) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
});
```

#### Struct loop

##### For statement
```cfc
struct = {a=1,b=2,c=3};
for (key in struct) {
    writeOutput("#key#:#struct[key]#;"); // a:1;b:2;c:3; (order of keys not guaranteed, obviously)
}
```

##### structEach()
```cfc
structEach(struct, function(key,value,struct) {
    writeOutput("#key#:#value#;"); // a:1;b:2;c:3;
});
```

##### Struct.each()
```cfc
struct.each(function(key,value,struct) {
    writeOutput("#key#:#value#;"); // a:1;b:2;c:3;
});
```

#### Query loop
```cfc
q = queryNew("id,data", "integer,varchar",[
    [11, "aa"],
    [22, "bb"],
    [33, "cc"]
]);
for (row in q){
    writeOutput("#q.currentRow#:#row.id#:#row.data#;"); // 1:11:aa;2:22:bb;3:33:cc;
}
```

Using grouping:
```cfc
q = queryNew("pk,fk,data", "integer,integer,varchar",[
    [1, 10, "aa"],
    [2, 20, "bb"],
    [3, 20, "cc"],
    [4, 30, "dd"],
    [5, 30, "ee"],
    [6, 30, "ff"]
]);
cfloop(query=q, group="fk") {
    writeOutput("<strong>#fk#</strong>");
    cfloop() {
        writeOutput("#pk#:#data#<br>");
    }
    writeOutput("<hr>");
}
```

Railo/Lucee only:
```cfc
loop query=q group="fk" {
    writeOutput("<strong>#fk#</strong>");
    loop {
        writeOutput("#pk#:#data#<br>");
    }
    writeOutput("<hr>");
}
```

#### List loop
```cfc
list = "a;b;c";
listEach(list, function(element,index,list) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
}, ";");

// or

list.each(function(element,index,list) {
    writeOutput("#index#:#element#;"); // 1:a;2:b;3:c;
}, ";");

// or (ColdFusion only, see [RAILO-3207](https://issues.jboss.org/browse/RAILO-3207))

for (element in "a,b,c,d,e") {
    writeOutput(element); // abcde
}
```

Não tenho certeza de como alguém especificaria um delimitador para o último exemplo: não parece compatível.

Railo/Lucee only:
```cfc
cfloop(list="a;b;c", index="element", delimiters=";") {
    writeOutput(element); // abc
}

// or
loop list="a;b;c" index="element" delimiters=";" {
    writeOutput(element); // abc
}
```

#### File loop
```cfc
filePath = getCurrentTemplatePath();
cfloop(file=filePath, index="chars", characters=16, charset="UTF-8"){
    writeOutput(chars); // outputs the contents of this file
}
```

Railo/Lucee only:
```cfc
loop file=filePath index="chars" characters=16 charset="UTF-8" {
    writeOutput(chars);
}
```

#### Date/time range loop

ColdFusion não tem nenhum construto específico específico de CFScript para isso a partir do ColdFusion 11

Work around:
```cfc
from = now();
to   = dateAdd("d", 7, from);

for (date=from; dateCompare(date, to, "d") <= 0; date = dateAdd("d", 1, date)) {
    writeOutput(dateTimeFormat(date, "yyyy-mm-dd HH:nn:sstt") & "<br>");
}
```

Railo/Lucee only:
```cfc
cfloop(from=from, to=to, index="date", step=createTimespan(1,0,0,0)) {
    writeOutput(dateTimeFormat(date, "yyyy-mm-dd HH:nn:sstt") & "<br>");
}

// or

loop from=from to=to index="date" step=createTimespan(1,0,0,0) {
    writeOutput(dateTimeFormat(date, "yyyy-mm-dd HH:nn:sstt") & "<br>");
}
```

#### Iteration flow control

cfcontinue:
```cfc
for (i=1; i <= 5; i++) {
    writeOutput("#i# is ");
    if (i mod 2) {
        writeOutput("ODD<br>");
        continue;
    }
    writeOutput("EVEN<br>");
}
```

### Other flow control statements

#### Request Redirect

```cfc
/* cflocation */
// ColdFusion
location(url="http://example.com", statuscode="301 OR 302", addtoken=false);

// Railo/Lucee
location url="http://example.com", statuscode="301 OR 302", addtoken=false;
```

#### Abort processing
```cfc
abort;

// or

abort "error message";
```

#### Exit from current file

```cfc
exit;

//or

exit "method";
```

### Code reuse

#### Include

```cfc
include "pathToFile";

// or

include "pathToFile" runonce=true;
```

#### Module

```cfc
// Railo/Lucee
module template="inc.cfm" attr1="val1" attr2="val2";

// ColdFusion
cfmodule(template="inc.cfm", attr1="val1", attr2="val2");
```

### Components / interfaces

```cfc
component {

}
```

#### Attributes
```cfc
component extends="Parent" {

}
```

Or:

```cfc
/**
 * @extends Parent
 */
component {

}
```


Observe que o comentário para anotações é `/ **` não simplesmente `/ *`.


Observe também que a última sintaxe não funciona atualmente no Railo (consulte [RAILO-3169] (https://issues.jboss.org/browse/RAILO-3169)).

#### Interface

```cfc
interface {
    public void function f(required numeric x); // observe que não há colchetes e termina com ponto e vírgula
}
```

#### Properties

Básico:

```cfc
property string myProperty;
```

Com parâmetros adicionais:

```cfc
property type="string" name="myProperty" default="default value"; // and all the same attributes as `<cfproprty>`
```

#### Functions

Básico:

```cfc
function f() { // assumes public function, returntype any

}
```

Com modificadores de tipo de acesso e retorno:

```cfc
private void function f() {
    // statements
}
```

#### Arguments

Basic:
```cfc
function f(x) { // optional argument of type "any"
    //afirmações
}
```

Type:
```cfc
function f(numeric x) { // optional argument of type "numeric"
    //afirmações
}
```
Required:
```cfc
function f(required numeric x) { // required argument of type "numeric"
    // afirmações
}
```
Default value:
```cfc
function f(numeric x = 0) { // optional argument of type "numeric" with default value of 0
    // afirmações
}
```

#### Function/argument annotations
```cfc
/**
 * @x.hint hint for argument x
 * @x.type numeric
 * @x.required true
 */
function f(x) {
    // afirmações
}
```

Note these annotations do not current correctly work on Railo (see [RAILO-3170](https://issues.jboss.org/browse/RAILO-3170))

Also note that this does not currently work on ColdFusion (see [3808960](https://bugbase.adobe.com/index.cfm?event=bug&id=3808960))
```cfc
/**
 * @x.type numeric
 * @x.default 0 // this causes a compile error
 */
function f(x) {
    // afirmações
}
```

#### Function expressions
```cfc
f = function(x) {
    // afirmações
};
```

Funções definidas por expressões de função usam fechamento, funções definidas por uma declaração de função não

Annotations for function expressions are not supported on ColdFusion ([3808978](https://bugbase.adobe.com/index.cfm?event=bug&id=3808978)); are supported on Railo, but have same shortcomings as noted above.

#### Calling functions dynamically
```cfc
test = new Test();
methodToCall = "f";
argumentsToPass = {x=1};
result = invoke(test, methodToCall, argumentsToPass);
```

Railo/Lucee-only:
```cfc
result = test[methodToCall](argumentCollection=argumentsToPass);
```

#### Import
```cfc
import com.domain.app.package.*;
```

#### Object creation
```cfc
myObj = createObject(type, "path.to.class"); // along with other type/situation-specific arguments

// or

myObj = new path.to.some.cfc.file(); // NB: will call the CFC's init() (by default), or method identified by the initmethod attribute of the component (bug in Railo: [RAILO-2294](https://issues.jboss.org/browse/RAILO-2294))
```

### File system operations

#### Directories
```cfc

// criação de diretório simples
directoryCreate("path/to/directory");
```
```cfc
// usando outros atributos opcionais
cfdirectory(action="create", directory="path/to/directory", mode="777");
```
```cfc
// Railo/Lucee only
directory action="create" directory="path/to/directory" mode="777";
```
```cfc
// excluir

directoryDelete("path/to/directory");
```
```cfc
// Lista
listing = directoryList("path/to/directory", true, "query", "*.cfm", "size desc"); // CF11 added an additional "type" attribute. Not currently supported on Railo/Lucee
```
```cfc
// renomear
directoryRename("path/to/directory", "path/to/new/directory");
```

#### Arquivo
```cfc
// read
// text
if (fileExists("path/to/file")) {
   result = fileRead("path/to/file");
}

// ou
fileHandle = fileOpen("path/to/file", "read");
result = fileRead(fileHandle, bytesToRead);
fileClose(fileHandle);

// ou
fileHandle = fileOpen("path/to/file", "read");
while (!fileIsEOF(fileHandle)) {
    result = fileReadLine(fileHandle);
}
fileClose(fileHandle);

```
```cfc
// binário

result = fileReadBinary("path/to/file");

//ou
fileHandle = fileOpen("path/to/file", "readbinary");
result = fileRead(fileHandle, bytesToRead);
```
```cfc
// acrescentar
fileHandle = fileOpen("path/to/file", "append");
fileWrite(fileHandle, textToAppend);
fileClose(fileHandle);
```
```cfc
// cópia 
fileCopy("path/to/file", "path/to/copyOfFile");
```
```cfc
// deletar
fileDelete("path/to/file");
```
```cfc
// mover / renomear
fileMove("path/to/file", "new/path/to/file");
```
```cfc
// upload
fileUpload("path/to/upload/file/to");
fileUpload(destination [, fileField [, accept [, nameConflict]]]);

fileUploadAll("path/to/upload/files/to");
fileUploadAll(destination [, fileField [, accept [, nameConflict]]]);
```
```cfc
// Escrever
fileWrite("path/to/file", data);

// ou

fileWrite(fileHandle, data);
```

### Database

#### Query
```cfc
// Forma geral
recordset = queryExecute(sqlString, params, options);
```
```cfc
// com array params
numbers = queryExecute("
    SELECT    columns
    FROM    table
    WHERE    id BETWEEN ? AND ?
",
[1,4],
{
    datasource    ="myDsn",
    result        = "result"    // this is analogous to the result attribute of `<cfquery>`
});
```
```cfc
// com estrutura params
numbers = queryExecute("
    SELECT    columns
    FROM    table
    WHERE    id BETWEEN :low AND :high
",{low=2,high=3});
```

Para qualificar parâmetros com tipos SQL, você pode especificar o equivalente das opções cfqueryparam com os parâmetros da seguinte maneira:

```cfc
// com tipos sql na estrutura params
numbers = queryExecute("
    SELECT    columns
    FROM    table
    WHERE    id BETWEEN :low AND :high
",
{
    low = { value = 2,
            cfsqltype = "cf_sql_integer"
        },
    high = { value = 3,
             cfsqltype = "cf_sql_integer"
        }
});
```

Para versões anteriores ao ColdFusion 11 (em que queryExecute () foi implementado), existe uma solução baseada em CFC: [Query.cfc](https://wikidocs.adobe.com/wiki/display/coldfusionen/query). An example is as follows:
```cfc
numbers = new Query(
    sql            = "
        SELECT    columns
        FROM    table
        WHERE    id BETWEEN :low AND :high
    ",
    parameters    =[
        {name="low", value=2},
        {name="high", value=3}
    ]
).execute().getResult();
```

#### Stored Procedure
```cfc
cfstoredproc(procedure="procName") {
    cfprocparam(type="in", cfsqltype="cf_sql_varchar", value="someValue");
    cfprocresult(name="result",resultSet=1);
    cfprocresult(name="result2",resultSet=2);
}
```

Railo/Lucee only
```cfc
storedproc procedure="procName" 
    dataSource = "myDataSource" 
    result="response" 
    returncode="true" {
    procparam type="in" cfsqltype="cf_sql_varchar" value="someValue";
    procparam type="out" cfsqltype="cf_sql_integer" variable="myVariable";
    procresult resultSet=1 name="result";
    procresult resultSet=2 name="result2";
}
```

Há uma solicitação de mudança na qual você deve votar para implementar esta sintaxe:
```cfc
options = {
    datasource = "scratch_mssql",
    fetchclientinfo = true,
    returncode = true
};
params = [
    {value=URL.low, type="INTEGER"},
    {value=URL.high, type="INTEGER"},
    {type="out", variable="inclusiveCount", type="INTEGER"},
    {type="out", variable="exclusiveCount", type="INTEGER"}
];

result = executeProcedure("uspGetColours", params, options);
```

See ColdFusion ticket: [3791737](https://bugbase.adobe.com/index.cfm?event=bug&id=3791737); Railo ticket: [RAILO-3184](https://issues.jboss.org/browse/RAILO-3184), and earlier blog article: '[ColdFusion 11: calling a stored procedure from script. And can we please stop wearing out our "c" and "f" keys?](http://blog.adamcameron.me/2014/03/coldfusion-11-calling-stored-procedure.html#executeProcedure)'.

#### Insert

Railo/Lucee only:
```cfc
insert datasource="myDataSource" tableName="myTable" formFields="list,of,form,fields"; // arguments the same as `<cfinsert>`. datasource is optional
```

Note: there is a bug with this: [RAILO-3180](https://issues.jboss.org/browse/RAILO-3180).

ColdFusion only:
```cfc
cfinsert(datasource="myDataSource", tableName="myTable", formFields="list,of,form,fields"); // arguments the same as `<cfinsert>`
```

Observe que a fonte de dados é necessária no momento, o que é um bug:[3814079](https://bugbase.adobe.com/index.cfm?event=bug&id=3814079).

#### Update

Railo/Lucee only:
```cfc
update datasource="myDataSource" table="myTable" formFields="list,of,form,fields"; // arguments the same as `<cfupdate>`. datasource is optional
```

Observe que o mesmo bug se aplica aqui como acontece com o insert.

ColdFusion only:
```cfc
cfupdate(datasource="myDataSource", table="myTable", formFields="list,of,form,fields"); // arguments the same as `<cfupdate>`
```

#### DB Info
```cfc
cfdbinfo(type="tables", name="info"); // arguments the same as `<cfdbinfo>`
```

Railo/Lucee only:
```cfc
dbinfo type="tables" name="info"; // arguments the same as `<cfdbinfo>`
```

#### Transactions
```cfc
transaction {
    try {
        // stuff to do
        transaction action="commit";
    }
    catch (any e) {
        transaction action="rollback";
    }
}
```
Observe que todos os atributos de `<cftransaction>` são suportados como pares nome / valor separados por espaço.

### Debugging

#### Dump
```cfc
writeDump(myVar); // can use either ordered or named arguments.
```

Com argumentos nomeados:
```cfc
writeDump(var=myVar, output=ExpandPath('/debug/log.txt'));
```

Railo/Lucee only:
```cfc
dump(myVar)
```

#### Log
```cfc
writeLog("text to log"); // pode usar argumentos ordenados ou nomeados.
```

#### Trace
```cfc
// Railo/Lucee only
trace category="test" text="trace text" { // mais todos os mesmos parâmetros que `<cftrace>`
    // coisas para rastrear
}
```
```cfc
// COLDFUSION only
trace(category="test", text="trace text") { // mais todos os mesmos parâmetros que `<cftrace>`
    // coisas para rastrear
}
// note that CF11 incorrectly records timing information (see [3811003](https://bugbase.adobe.com/index.cfm?event=bug&id=3811003))
```

#### Timer
```cfc
cftimer(label="timer label" type="outline") { // mais todos os mesmos parâmetros que `<cftimer>`
    // coisas ao tempo
}
```
```cfc
// Railo/Lucee only
timer label="timer label" type="outline" { // mais todos os mesmos parâmetros que `<cftimer>`
    // coisas ao tempo
}
```

### General / Miscellaneous

#### Output
```cfc
writeOutput(expression); // expressão deve ser resolvida em uma string
```

Railo/Lucee only:
```cfc
echo(expression); // expressão deve ser resolvida em uma string
```

#### File Encoding
```cfc
pageencoding "UTF-8";
```

Observe que isso só funciona em arquivos CFC no ColdFusion ([3712167](https://bugbase.adobe.com/index.cfm?event=bug&id=3712167)). Funciona corretamente no Railo / Lucee.

#### Save content
```cfc
savecontent variable="saved" {
    writeOutput("stuff to save");
}
```

#### Threading
```cfc
thread action="run" name="threadName" {
    // código a ser executado em um tópico separado aqui
}
thread action="join" name="threadName,anotherThreadName";
```

#### Locking
```cfc
lock name="lockName" timeout=1 throwontimeout=true {
    // código para bloquear
}
```

#### Image / XLS manipulation

Os equivalentes de função de [`<cfimage>`](https://wikidocs.adobe.com/wiki/display/coldfusionen/Image+functions) and [`<cfspreadsheet>`](https://wikidocs.adobe.com/wiki/display/coldfusionen/Spreadsheet+functions) estão todos bem documentados e não são especificamente construções de CFScript.

#### PDF Manipulation

Devo admitir que nunca fiz nenhum trabalho com PDFs, portanto, não posso fazer comentários informados sobre os equivalentes do CFScript. No entanto, em vez de construções específicas específicas do CFScript que conheço, a sintaxe genérica deve funcionar, por exemplo:

ColdFusion:
```cfc
cfdocument(format="PDF") {
    // marcação aqui
}
```
Railo/Lucee:
```cfc
document format="PDF" {
    // marcação aqui
}
```

O mesmo deve funcionar em outras tags orientadas a PDF. Para versões do ColdFusion anteriores ao CF11, existe um PDF.cfc (semelhante ao Query.cfc, e também em cfusion / CustomTags / com / adobe / coldfusion). Nunca usei, não sei como funciona e não tenho interesse em descobrir. Se alguém quiser doar algum código de exemplo, irei integrá-lo aqui.


### Elements of tag-based CFML with no _specific_ CFScript implementation

#### CFC-based solutions

Pelo que eu posso dizer, não há implementações específicas de CFScript para as seguintes peças de funcionalidade:
*   `<cfftp>`
*   `<cfpop>`
*   `<cfimap>`
*   `<cffeed>`
*   `<cfldap>`
*   `<cfcollection>`
*   `<cfindex>`
*   `<cfsearch>`

Existem wrappers CFC para estes em cfusion / CustomTags / com / adobe / coldfusion. Essas não são soluções específicas do CFScript em si - pode-se usá-las com a mesma facilidade em código baseado em tag - mas podem ser incluídas aqui para fins de integridade. Eu pessoalmente não uso essas construções e, como não fazem parte do CFScript, não tentei incluí-las aqui. Sinta-se à vontade para documentar e incluí-los, se desejar.

#### http.cfc

ColdFusion (CF9+):

__The pattern seems to be `set{ATTRIBUTENAME}( value );` for setting attributes that are normally set via the `<cfhttp>` tag__

```cfc
//há um CFC embutido que lida com isso no CF9 +
httpService = new http();
httpService.setMethod( "post" )
httpService.setCharset( "utf-8" );

//o provedor de API pode exigir que você exporte este certificado
// então você precisará salvá-lo em um lugar seguro ...
// e referenciá-lo aqui
httpService.setClientCert( "#ExpandPath(‘.’)#/my_cert.p12" );
httpService.setClientCertPassword( "mypassword!" );
httpService.setUrl( "https://api.sample.com/" );

//esses parâmetros são campos de formulário para POST
httpService.addParam(type="formfield", name="field1", value="1111");
httpService.addParam(type="formfield", name="field2", value="some text here");

//esta é a maneira cfscript de obter a resposta
httpResponse = httpService.send().getPrefix();

writeDump(httpResponse);
```

#### mail.cfc

ColdFusion (CF9+):

__The pattern seems to be `set{ATTRIBUTENAME}( value );` for setting attributes that are normally set via the `<cfmail>` tag__

```cfc
mailerService = new mail();
/* definir atributos de e-mail usando configuradores implícitos fornecidos * /
mailerService.setType("html");
mailerService.setCharset( "utf-8" );
mailerService.setTo( "adam@sample.com" );
mailerService.setFrom( "test@sample.com;joe@sample.com" );
mailerService.setSubject( "Super interesting subject" );

/* add mailparams */
mailerService.addParam( file=expandpath(form.attachment), type="text/plain", remove=false );
// create the body
savecontent variable="mailBody"{
    WriteDump( CGI );
    WriteDump( SESSION );
}
// envie o email
mailerService.send( body=mailBody );
```


#### The rest


Para usar qualquer outra funcionalidade não listada aqui no CFScript, é necessário usar a sintaxe generalizada.

Em Railo / Lucee, é uma questão de remover o "` <cf` "e o" `>` ", e usar a sintaxe de bloco normal (chaves) onde a versão da tag é uma tag orientada a blocos.

No ColdFusion (CF11 +), substitua "` <cftagname` "por" `cftagname (` ", e" `>` "por" `)` ", e separe os atributos por vírgulas. Observe que isso fará com que a construção _pareça_ com uma função, mas na verdade não é e não pode ser usada como uma função, por exemplo, esta é uma sintaxe inválida:

```cfc
result = cfhttp(method="post", url="http://example.com");
```

Algumas tags possuem um bloco de código / texto dentro delas. Os blocos são representados com chaves, semelhantes a uma instrução de controle de fluxo.

```cfc
cfmail(attributeCollection = attributesforCfmail) {
	cfmailpart(type="text/plain") {
		writeOutput("Se estiver vendo isso, seu cliente de e-mail não oferece suporte a mensagens em HTML. ");
);
	}
	cfmailpart(type="text/html") {
		writeOutput(htmlVersionOfMessage);
	}
}

```

Observe como o texto precisa ser "produzido" usando `writeOutput`. Observe também como as sub-tags (`cfmailpart` aqui) também são expressas usando as mesmas regras das tags-pai.

* * *

[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/)
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

[trycf-logo]: http://trycf.com/img/trycf-logo-tiny.png "TryCF.com"
