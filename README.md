# Simulador de Conta Bancária em C#

Este projeto apresenta um exemplo simples em C# para praticar conceitos de Programação Orientada a Objetos, simulando operações básicas de uma conta bancária.

## Conceitos trabalhados

- entrada de dados com `Console.ReadLine()`
- criação de objetos
- associação entre `Conta` e `Cliente`
- composição entre `Conta` e `Movimentacao`
- encapsulamento com `private set`
- regras básicas de negócio para depósito e saque
- uso de lista para armazenar histórico de movimentações

## Código

```csharp
using System;
using System.Collections.Generic;
					
public class Program
{
	public static void Main()
	{
		Console.WriteLine("Bem-vindo ao caixa rápido! Digite o valor do deposito");
		
		decimal valorDoDeposito = Convert.ToDecimal(Console.ReadLine());
		
		Console.WriteLine("Digite o valor do saque");

		decimal valorDoSaque = Convert.ToDecimal(Console.ReadLine());

		//instancia
		Cliente objCliente = new Cliente("Janderson", "999.999.999-99", "12345", "14/03/1989");
		Conta objConta = new Conta("12345", "345", objCliente);
		
		objConta.Depositar(valorDoDeposito);
		objConta.Sacar(valorDoSaque);
		
		Console.WriteLine($"Saque realizado com sucesso! Saldo restante: {objConta.Saldo} | Cliente: {objConta.Cliente.Nome}");
		
		foreach(var movimentacao in objConta.ListaDeMovimentacoes)
		{
		    	Console.WriteLine($"{objConta.Cliente.Nome} | {movimentacao.Data} | {movimentacao.Valor} | {movimentacao.Tipo}");
		}
	
	}
}

//Todo de Movimentacao (Regra: O todo dever ter uma lista da parte)
public class Conta
{
	public Conta(string numero, string agencia, Cliente cliente)
	{
		this.numero = numero;
		this.agencia = agencia;
		this.Cliente = cliente;
	}
	
	//atributos
	private string numero;
	private string agencia;
	
	
	//propriedades
	public Cliente Cliente {get; private set; }
	public decimal Saldo { get; private set; }
	
	//apontamento para a parte
	public List<Movimentacao> ListaDeMovimentacoes {get; private set;} = new List<Movimentacao>();
	
	public void Sacar(decimal valor)
	{
		if(valor < 0)
		{
			throw new Exception("Valor não pode ser menor que zero!");
		}
		
		if(valor > Saldo)
		{
			throw new Exception("Valor maior que o saldo atual, ele é pobre");
		}
		
		Saldo = Saldo - valor;
		
		Movimentacao objMovimentacao = new Movimentacao("Saque",DateTime.Now, this, valor, Saldo);
		
		this.ListaDeMovimentacoes.Add(objMovimentacao);
	}
	
	public void Depositar(decimal valor)
	{
		if(valor < 0)
		{
			throw new Exception("Valor não pode ser menor que zero!");
		}
		
		Saldo = Saldo + valor;
		
		Movimentacao objMovimentacao = new Movimentacao("Deposito",DateTime.Now, this, valor, Saldo);
		
		this.ListaDeMovimentacoes.Add(objMovimentacao);
	}
	
}

public class Cliente
{
	public Cliente(string nome, string cpf, string rg, string dataDeNascimento)
	{
		this.Nome = nome;
		this.Cpf = cpf;
		
		this.rg = rg;
		this.dataDeNascimento = dataDeNascimento;
	}
	
	//atributos
	private string rg;
	private string dataDeNascimento;
	
	//comportamentos
	
	//propriedades
	public string Nome {get; private set;}
	public string Cpf { get; private set;}
	
	
}

//Parte de Conta (regra: a parte sempre aponta para o todo que é conta)
public class Movimentacao
{
    public Movimentacao(string tipo, DateTime data, Conta conta, decimal valor, decimal saldo)
    {
        this.Tipo = tipo;
        this.Data = data;
        this.Conta = conta;
        this.Saldo = saldo;
        this.Valor = valor;
    }
    
    //atributos
    

    
    //propriedades
    
    //apontamento para o todo
    public Conta Conta {get; private set;}
    
    public string Tipo {get; private set;}
    public DateTime Data {get; private set;}

    public decimal Valor { get; private set;}
    public decimal Saldo { get; private set;}
    
    
}
```

## Estrutura das classes

### Cliente
Representa o titular da conta, contendo informações básicas como nome e CPF.

### Conta
Representa a conta bancária. Ela possui:
- número
- agência
- cliente
- saldo
- lista de movimentações

Além disso, contém os métodos:
- `Depositar`
- `Sacar`

### Movimentacao
Representa cada operação realizada na conta, armazenando:
- tipo da movimentação
- data
- valor
- saldo após a operação
- referência para a conta

## Relacionamentos

### Associação
`Conta` possui um `Cliente`.

### Composição
`Conta` possui uma lista de `Movimentacao`.  
As movimentações pertencem à conta e fazem parte do seu histórico.

## Exemplo de execução

```text
Bem-vindo ao caixa rápido! Digite o valor do deposito
1000
Digite o valor do saque
300
Saque realizado com sucesso! Saldo restante: 700 | Cliente: Janderson
Janderson | 27/03/2026 10:00:00 | 1000 | Deposito
Janderson | 27/03/2026 10:00:10 | 300 | Saque
```

## Observações

- Agora o sistema registra tanto depósito quanto saque no histórico
- O histórico é exibido ao final com `foreach`
- O código é bom para estudar associação/agregação, composição, encapsulamento e lista de objetos
- Os campos `numero`, `agencia`, `rg` e `dataDeNascimento` existem, mas ainda não são exibidos na saída
```
