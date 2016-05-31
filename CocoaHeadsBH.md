#Swift dominará o mundo! Por que usar?
Porque usar a linguagem mais bacana de todos os tempos, na visão de uma pessoa imparcial 😁

![Swift]
(https://9to5mac.files.wordpress.com/2015/12/swift-16-9.jpg)

---

#Bruno Isola Reginato 
**facebook**: brunoireginato

**twitter**: brunoireginato

**github**: brunoreginato

**email**: brunoireginato@gmail.com

![Eu]
(https://scontent-lga3-1.xx.fbcdn.net/v/t1.0-9/12249847_939477392795699_5259770171116949881_n.jpg?oh=98fc77973082395260ba2d2999407074&oe=57C54839)

---

#Ísola != Isóla
(pode chamar de Isóla)

---

###Há 1 ano atrás quando me perguntavam sobre Swift...

---

**"Já é hora de usar swift?"**

---

*“Ahhh tá muito cedo ainda, a linguagem parece promissora, mas eu ainda não trocaria."*

---

*“Está muito instável, acho melhor aguardar"*

---

#E agora?

---

*“Veiii sáporra é perfeita "* 😱😱😱😱

---

Brincadeiras a parte... 

---

###Swift está... 
- Bem mais maduro 
- Virou *open source*
- Comunidade aderiu de forma bem rápida. 
- O "time de desenvolvimento" está ouvindo muito a comunidade para que a linguagem evolua baseada no que realmente importa, o usuário (noixxx)!

---

Nesta talk, abordo pontos que julgo evoluções importantíssimas em vários quesitos: Segurança, Performance e Facilidade na manutenção. Além de outros pontos que não gostei no Swift

---

#Agenda
- Joinhas para o Swift 👍
- Pontos de atenção ⚠️
- Mancadas do Swift 👎
- O que esperar para o futuro? 🤔

---

#Joinhas para o Swift 👍👍👍

---

###Optionals:

Cara, se vc conseguir que a sua app crashe por uma referência a um ponteiro nulo, PARABÉNS, você merece tudo de melhor, trabalhando com Javascript 😃

---

Brincadeiras a parte, o Swift nos dá muitas formas de chegarmos a **ZERO** erros de persistência, eles só acontecem se você realmente assumir o risco.
     
---

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
---

###Capture Lists:
Referências ao objeto **self** dentro de *blocks/closure* sempre foi um assunto confuso. Em Objective-c normalmente criava-se um objeto que segurava uma referência fraca ao **self** e referenciava o mesmo dentro do *block*.

---
**Em objective-c...**

```objectivec
__weak MyObject *weakSelf = self;
[self setMyBlock:^(id obj, NSUInteger idx, BOOL *stop) {
  [weakSelf doSomethingWithObj:obj];     
}];
```
---

Em Swift, pensando nisso, foram criadas as *Capture Lists*, elas têm a função de ser uma lista de referências que serão passadas para dentro do *closure*.

---

###Qual a diferença entre o unowned e weak?
- **weak** = Não acrescenta 1 (usado para objetos opcionais, que podem ser nil)
- **unowned** = Não acrescenta 1 (usado para objetos NÃO opcionais, que não podem ser nil)

---

###Exemplo:
```swift
//Eu, como programador, tenho a certeza de que 'self' existirá quando o clousure for chamado
MyApi.request("users") { [unowned self] (result) in	
	self.update()
}

//Eu, como programador, NÃO tenho a certeza de que 'self' existirá quando o clousure for chamado
MyApi.request("users") { [weak self] (result) in
	
	self?.update()
}
```

---

###Selectors (Swift 2.2):

A nova forma de definição dos seletores ficou muuuuuito mais segura do que a anterior, os seletores são criados através de referências aos métodos como objetos:

---

###Exemplo:
```swift 
selector: #selector(AppDelegate.meuMetodo)
```
---

###O que ganhamos com isso? 
Consegue-se checar se o *selector* é válido, em tempo de compilação!

---

###Porém...
O Swift ainda não valida o número de argumentos esperados para aquele seletor

---

###Sdds .h .m

---

Mentira, saudades nada. No Swift, como vcs devem saber, temos decoradores de métodos/funções/propriedades que discriminam qual o nível de acesso aos mesmos, sendo eles:

---

- **internal**: visível no namespace do projeto
- **private**: só na classe
- **public**: \o/
          
---

###let e var (willSet, didSet)
Além da função de separar variáveis de constantes, o **let** e o **var** nos apresentaram algo muito interessante, os *Property Observers*.

---

###willSet
Executa um *closure* quando uma propridade será setada...

---

###didSet
Executa um *closure* quando uma propridade foi setada...

---

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
---

###Enums
Enums ficaram extremamente interessantes. Dentre seus novos atributos, está a definição de funções em seu corpo...

---

###Exemplo:
```swift
enum HTTPStatus {
    case Success
    case Error(code:Int)
}
```

---

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

---

### Error handling 👍
Uma das *features* mais importantes do Swift, nada mais é do que especificações de ErrorType. Definidos como enums...

---

###Exemplo, definindo os erros:
```swift
enum GenericError : ErrorType {
    case Parse(String)
    case InvalidURL
    case InvalidEncoding
}
```

---

###Lançando-os:
```swift
	guard let name = dict["name"] as? String else {
		throw GenericError.Parse("Error parsing 'name' property")
	}
```

---

###Capturando-os:
```swift
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

---

###Funções que lançam exceções
```swift
func contactFromDict(dict: [String:AnyObject]) throws -> Contact {
	guard let name = dict["name"] as? String else {
		throw GenericError.Parse("'name' must be a String")
	}
	
	guard let phone = dict["phone"] as? String else {
		throw GenericError.Parse("'phone' must be a String")
	}

	return Contact(name: name, phone: phone)
}
```

---

###Chamando funções...
```swift
do {
	try contactFromDict()
} catch let error {
	print("Ops, something goes wrong! \(error)"
}
```

---

###Ignorando a possibilidade de erro! 👎
```swift
try! contactFromDict()
```

---

###Importante...
A Apple sugere que o uso dos erros para todo e qualquer tipo de validação...

- Validação de campos
- Checar se um recurso está disponível para aquele OS
- Não encontrar um registro em um banco de dados...

---

#Pontos de atenção ⚠️
- iOS <= 7
- *Dynamic Libs* (Parece besta, mas dá uma *googlada* numa lib em swift que dá suporte para iOS 7) 😀
- Evite o *bridging* para Objective-c, isso é muito custoso!
- Ignore o quanto puder o uso de objetos "NS", use-os em caso de vida ou morte, a maioria dos tipos estão convertidos para Swift (CollectionType, ErrorType, String, Int ...)

---

#O que esperar para o futuro? 🤔
- PackageManager (nativo) 
- Server Side Swift
- Swift 3.0
- WWDC vem ai...

---

#Mancadas do Swift 👎
- ARC x Garbage Colector
- Ainda não gosto da forma que nós temos que fazer classes abstratas em Swift, me pareceu algo obsoleto no meio do tanto que se fala em linguagens funcionais/ reativas

---

#Obrigado :)