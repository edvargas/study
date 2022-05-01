
# Java 8
- Default methods -> à partir do java8 metodos podem ser implementados dentro de interfaces com a sintaxe abaixo, são chamados de default methods. Isso ajuda na  manutenção de interfaces sem prejudicar metodos já implementados.
	default void sort(args[..] a) {
		...
	}

- Interface Funcional -> interface onde só existe um metodo abstrato


-------------------


# Kubernets

## POD -> 
	é uma capsula aonde pode existir um ou mais containers
	POD possui um endereço IP, podendo mapear as portas para o IP atribuido ao POD
	PODs são efêmeros, estão ali para ser substituídos caso necessite

## SVC -> 
	abstração que expõe a aplicação executada em um ou mais pods
	proveem IP's fixos para comunicação, os serviços vão ter IP fixo apontando para um ou mais POD's com diferentes IP's
	SVC também pode é responsável pelo balanceamento de cargas
* ClusterIP -> comunicação entre diferentes POD's em um mesmo cluster
* NodePort -> é um serviço que permite a comuniação com o mundo externo do cluster
* LoadBalancer -> é um ClusterIp que permite a comunicação entre o mundo externo e os PODs usando o Load Balancer do proverdor

## Config Map -> 
	responsável por armazenar configurações definições necessárias em outros recursos, podendo armazenar dentro dele para não acoplar recursos com informação de configuração, como variáveis de ambiente, infos de config. Permite a reutilização e desacoplamento de infos em diferentes SVCs e PODs

## Alguns comandos importantes
* kubectl get nodes -o wide
* kubectl apply -f ./arquivo_para_apply
* kubectl get pods
* kubectl get svc
* kubectl describe pods
* kubectl describe pods nome-pod
* kubectl delete svc -all
* kubectl exec -it kubernates-name -- bash


-------------------


# AWS SQS

## Short Polling -> 
	busca frequente de mensagens, faz várias requisições até o cliente perceber que existe uma mensagem nova, e processa a mensagem

## Long Polling -> 
	cria uma conexão até encontrar uma mensagem nova, ou até atingir o tempo limite da conexão(200 seg no SQS)
	* essa prática é realizada para ter um intervalo de polling maior, assim economizando requisições para o servidor(and money $) - pesquisar um pouco mais sobre esse item em relação à boas práticas

## DLQs (Dead Letter Queues) -> 
	são filas configuradas como destino de uma fila de origem caso esta fila possua uma "poison message", sendo assim, essa mensagem é encaminhada para a sua respectiva DLQ, caso configurada na fila de origem

## Tipos de fila
	* SQS/Standard -> é um tipo de fila que não garante ordenação, as mensagens podem ser entregues pela fila fora de ordem e também podem ser duplicadas, o SQS sempre vai utilizar a melhor ordenação possível pelo algoritmo SQS
	* FIFO -> ordem garantida, quem chega primeiro, é processado primeiro, também é garantido que não haverá duplicidade(existe uma config que garante por um determinado tempo que essa menagem não poderá entrar na fila caso seja duplicada)


-------------------


# Testes unitários/integração
	- Teste unitário -> menor parte do sistema
	* Given / When / Then


-------------------


# Spring cloud

## Service discovery -> 
	os serviços relacionados à esse contexto se registrarão ao service discovery, assim, informando o endereço e referência, nesse caso os endereços e referências dos serviços são centralizados em um lugar só

## Eureka(service discovery) -> 
	porta padrão = 1761

## LoadBalanced ->


-------------------


# React

* npm start
* Todo código HTML escrito, não é HTML, é um código JSX, quando o react roda, transforma todo o JSX em algo que o navegador intenda, nesse caso, HTML.
* Class components
* index.js é um arquivo comum dentro dos componentes, para ajudar nos imports de outros lugares, e também para não expor possíveis outros arquivos que não devem ser exportados
* usar bind para associar o this dentro de eventos, pois o this do JS é dinâmico
* lembrar que em JS por convenção se usa o _ como indicador que o método é privado

## eventos HTML
	* preventDefault -> evita que recarregue a página em um submit
	* stopPropagation -> evita propagação do evento no DOM, pode ser usado em todos eventos que o HTML dispara

## props -> 
	props serve para passar propriedades(seja métodos, valores, variáveis...) para componentes filhos

## state -> 
	state é uma propriedade que é gerenciada pelo react, sempre quando é atualizada, o render do component referente ao state atualizado, é chamado


-------------------


# Golang

- linguagem modularizada
- sintaxe curta
- go run ~classe main~ -> build and execute
- tipo da variável é preenchido depois do nome -> 'var nome string'
- as variáveis são tipadas, mas podem ser inferidas -> 'var nome = "ed"'
- no go, também não é necessário usar a palavra reservada var para declaração de variáveis, podemos usar o ':=' -> 'nome := "Ed"'


--------------------


# SOLID

- Single Responsibility Principle, Open Closed Principle, Liskov Substitution Principle, Interface Segregation Principle, Dependency Inversion Principle

- Coesão -> "União harmônica entre uma coisa e outra" - Harmonia e união entre atributos e métodos de uma classe, sempre representar o que se pertence realmente à classe, que representam um mesmo significado. Classes que fazem uma única coisa. "Classes não coesas tendem a crescer indefinidamente, o que as tornam difíceis de manter".

- Encapsulamento -> Encapsulamento é o ato de blindar uma classe de manipulação externas que não condizem. "Classes não encapsuladas permitem violação de regras de negócio, além de aumentarem o acoplamento".

- Acoplamento -> Sempre maner o acoplamento de uma classe para a outra o mais baixo possivel. Exemplo: sempre usando meodos e getters para ter valores da outra classe ao inves de pegar diretamente. Falta de encapsulamento leva a um acoplamento maior, sendo uma ma pratica.

- Single responsibilty -> uma classe deve executar apenas acao de responsabilidade daquela classe. Exemplo: A classe funcionario, deve realizar apenas operacoes sobre funcionario, alteracoes de salario devem ocorrer no contexto separado apenas para salario.

- Open Close Principles ->  Entidades de software, deveriam sempre abertas para extensao, porem, fechadas para modificacao. Voce deve poder adicionar novos principos sem estar sempre alterando apenas uma classe. ˜Voce nao precisa fazer uma cirurgia de peito aberto, para colocar um casaco˜. Sempre buscar um codigo extensivel, assim nao precisando alterar codigo existente, e sim adicionando novo codigo com novos comportamentos.
Exemplo: uma classe que possui varias validacoes em uma rotina, pode ser extraida para uma interface com um metodo validar, e cada validacao, uma implementacao desta interface.

- Liskov substitution principle -> voce deve tomar cuidado para nao ferir o principio da heranca, cuidado na abstracao. Se q(x) é um objeto demonstravel dos objetos x de tipo T, entao q(y) deve ser verdadeiro para objetos y de tipo S, onde S é subtipo de T.
	- Composicao -> algumas ocasioes, heranca nem sempre é a melhor alternativa na implementacao, nesses casos pode ser usado a tecnica de composicao, aonde dois models, por exemplo, podem partilhar dos mesmos dados, sem necessariamente dar extends em outro model. Exemplo: ao ter uma classe Funcionario com dados e metodos de reajuste de salario e promocao, uma classe de Tercerizado, que teoricamente tambem é um funcionario, nao faz sentido possuir metodos de reajuste e promocao, nesse caso, podemos usar a composicao com os dados basicos de cadastro do funcionario para os dois models, e tambem nao tendo codigo duplicado.

- Dependency inversion principle -> Abstracoes nao devem depender de implementacoes, implementacoes devem depender de abstracoes. Exemplo: tendo classes implementadas de uma interface, as implementacoes dependem da interface(abstracao).

- 


--------------------


### Project patterns

## MVC
- divisão do projeto em camadas muito bem definidas, Model, o Controller e a View
- A utilização do padrão MVC traz como benefício o isolamento das regras de negócios da lógica de apresentação, que é a interface com o usuário. 
- Isto possibilita a existência de várias interfaces com o usuário que podem ser modificadas sem a necessidade de alterar as regras de negócios, proporcionando muito mais flexibilidade e oportunidades de reuso das classes.

## DDD
- Domain-Driven Design ou Projeto Orientado a Domínio é um padrão de modelagem de software orientado a objetos que procura reforçar conceitos e boas práticas relacionadas à OO.
- Em resumo, as classes modeladas e os seus métodos deveriam representar o negócio da empresa, usando inclusive a mesma nomenclatura. A persistência dos dados é colocada em segundo plano, sendo apenas uma camada complementar.
- Quando não usar DDD
	- Às vezes só é necessário um CRUD
	- Compartilhando dados com outros sistemas, rotinas de integração que recebem ou disponibilizam dados para outros sistemas não devem ser "inteligentes".

# tips
- ENUM > um enum pode ter implementacao de metodos abstratos nas suas constantes, otimo para definir novos valores dado determinado valor original.