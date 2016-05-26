#Swift dominará o mundo! Por que usar?
Por que usar a linguagem mais bacana de todos os tempos, na visão de uma pessoa imparcial 😁

--

#Bruno Isola Reginato 
**facebook**: brunoireginato

**twitter**: brunoireginato

**github**: brunoreginato

**email**: brunoireginato@gmail.com

-

#Ísola != Isóla
(pode chamar de Isóla)

--

###Há 1 ano atrás quando me perguntavam sobre swift...

-

**"Já é hora de usar swift?"**

-

*“Ahhh tá muito cedo ainda, a linguagem parece promissora mas eu ainda não trocaria."*

-

*“Está muito instavel, acho melhor aguardar"*

-

#E agora?

-

*“Veiii sáporra é perfeita "* 😱😱😱😱

-

Brincadeiras a parte... 

-

Swift está bem mais maduro, virou open source, a comunidade aderiu de forma bem rápida. O "time de desenvolvimento" está ouvindo muito a comunidade para que a linguagem evolua baseado no que realmente importa, o usuário (nois)!

-

Nesta talk, abordo pontos que julgo evoluções importantíssimas em vários quesitos: Segurança, Performance e Facilidade na manutenção. Além de outros pontos que não gostei no Swift

-

#Agenda
- Joinhas para o Swift 👍
- Pontos de atenção ⚠️
- Mancadas do Swift 👎
- O que esperar para o futuro? 🤔

-

#Joinhas para o swift 👍👍👍
-

###Optionals:

Cara, se vc conseguir que a sua app crashe por uma referência a um ponteiro nulo, PARABÉNS, você merece tudo de melhor, trabalhando com Javascript 😃

-

Brincadeiras a parte, o Swift nos dá muitas formas de chegarmos a ZERO erros de persistência, eles só acontecem se você realmente assumir o risco.
     
-
###Exemplo:
```swift
var optional:String?

//risco
optional!.characters 

//seguro
guard let safe = optional else {
     throw Generic.DeuRuim
}
```
-

###Capture lists:
Referências ao objeto **self** dentro de blocks/clousure sempre foi um assunto confuso. Em objective-c normalmente criava-se um objeto que segurava uma referencia fraca ao **self** e referênciava o mesmo dentro do block.

-
**Em objective-c...**

```objective-c
__weak MyObject *weakSelf = self;
[self setMyBlock:^(id obj, NSUInteger idx, BOOL *stop) {
  [weakSelf doSomethingWithObj:obj];     
}];
```
-

Em swift, pensando nisso, foram criadas as Capture lists, elas tem a função de ser uma lista de referências que serão passadas para detro do clousure.

-

###Qual a diferença entre o unowned e weak?
- **weak** = Não acrescenta +1 (usado para objetos opcionais, que podem ser nil)
- **unowned** = Não acrescenta +1 (usado para objetos NÃO opcionais, que não podem ser nil)

-

###Exemplo:
```swift
//Eu, como programador, tenho a certeza de que 'self' existirá quando o clousure for chamado
MyApi.request("users") { [unowned self] (result) in	self.update()
}

//Eu, como programador, NÃO tenho a certeza de que 'self' existirá quando o clousure for chamado
MyApi.request("users") { [weak self] (result) in
	self?.update()
}
```

-

###Selectors (Swift 2.2):

A nova forma de definição dos seletores ficou muuuuuito mais segura do que a anterior, os seletores são criados através de referências aos métodos como objetos:

-

###Exemplo:
```swift 
selector: #selector(AppDelegate.meuMetodo)
```
-

###O que ganhamos com isso? 
Consegue-se checar se o selector é valido, em tempo de compilação!

-

###Porém...
O Swift ainda não valida o número de argumentos esperados para aquele seletor

-

###Sdds .h .m

-

Mentira, saudades nada. No swift, como vcs devem saber, temos decoradores de métodos/funções/propriedades que discriminam qual o nível de acesso dos mesmos, sendo eles:

-

- internal: visivel no namespace do projeto
- private: só na classe
- public: \o/
          
-

###let e var (willSet, didSet)
Além da função de separar variáveis de constantes, o **let** e o **var** nos apresentaram algo muito interessante, os *Property Observers*.

-

###willSet
Executa um clousure quando uma propridade será setada...

-

###didSet
Executa um clousure quando uma propridade foi setada...

-
###Exemplo:
```swift
var totalSteps: Int = 0 {
    willSet(newTotalSteps) {
        print("About to set totalSteps to \(newTotalSteps)")
    }
    didSet {
        if totalSteps > oldValue  {
            print("Added \(totalSteps - oldValue) steps")
        }
    }
}
```
-

###Enums
Enums ficaram extremamente interessantes, dentre seus novos atributos, está a definição de funções dentor de um enum...

-
###Exemplo:
```swift
enum HTTPStatus {
    case Success
    case Error(code:Int)
}
```

-

###Com a function...
```swift
enum HTTPStatus {
    case Success
    case Error(code:Int)
    
    func message() -> String {
        switch self {
        case .Success:
            return "All was good :)"
            
        case .Error(code: let code) where 500 ... 599 ~= code:
            return "Internal server error :("
            
        case .Error(code: let code) where 400 ... 499 ~= code:
            return "Resource not found :("
            
        case .Error(code: let code):
            return "Something goes wrong :( \(code)"
        }
    }
}
```

-

### Error handling 👍
Uma das features mais importantes do swift, nada mais são do especificações de ErrorType. Definidos como enums...

-

###Exemplo, definindo os erros:
```swift
enum GenericError : ErrorType {
    case Parse(String)
    case InvalidURL
    case InvalidEncoding
}
```

-

###Lançando-os:
```swift
	guard let name = dict["name"] as? String else {
		throw GenericError.Parse("Error parsing 'name' property")
	}
```

-

###Capturando-os:
```
do {
	guard let name = dict["name"] as? String else {
		throw GenericError.Parse("Error parsing 'name' property")
	}
	
	print(name)
} 
catch GenericError.Parse(message: let msg) {
    print("Opss \(msg)")
}
```

-

###Importante...
A Apple sugere que o uso dos erros para todo e qualquer tipo de validação...

- Validação de campos
- Checar se um recurso está disponível para aquele OS
- Não encontrar um registro em um banco de dados...

-

#Pontos de atenção ⚠️
Se você está pensando em usar o Swift dando suporte para iOS <= 7 eu diria pra vc amiguinho, cuidado. Você pode estar numa cilada. É possível sim utilizar o swift em iOS 7 mas existem algumas features muito bacanas que perderíamos:
     - Dynamic Libs (Parece besta, mas da uma googlada numa lib em swift que da suporte para iOS 7) 😀
     - Utilização de objetos objective-c, a bridge entre o Swift e o objc é muito custosa. Minha dica é, enquanto puder lute para não por nada de objc no seu app swift.
     - Ignore NS’s, use-os em caso de vida ou morte, a maioria dos tipos estão convertidos para Swift (CollectionType, ErrorType, String, Int)

-

#O que esperar para o futuro? 🤔
Muita coisa bacana tem acontecido desde que o Swift tornou-se open source. Eu daria destaque para algumas coisas:
     - PackageManager (nativo)
     - Server Side Swift
     - Swift 3.0

     
-

#Mancadas do Swift 👎
1) Eu esperava, com todo o meu coração corinthiano que quando o swift fosse lançado nós teríamos um Garbage Colector mas infelizmente não, a Apple insiste em lavar nossas mentes de que o Contador de Referências automático é melhor. A discussão não é essa, alias é um ótimo tema para um próximo encontro. Enfim, fiquei bem desapontado com isso pois vejo que na verdade a linguagem não foi construída do zero (ok, seria muita ingenuidade minha acreditar nisso) mas era o que a Apple vendia.

2) Ainda não gosto da forma que nós temos que fazer classes abstratas em Swift, me pareceu algo obsoleto no meio do tanto que se fala em linguagens funcionais/ reativas