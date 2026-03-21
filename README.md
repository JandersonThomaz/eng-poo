# Simulador de Conta Bancária em C#

Exemplo simples em C# para praticar:

- entrada de dados com `Console.ReadLine()`
- orientação a objetos
- associação entre `Conta` e `Cliente`
- composição entre `Conta` e `Movimentacao`
- operações de depósito e saque

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
        this.tipo = tipo;
        this.data = data;
        this.Conta = conta;
        this.Saldo = saldo;
        this.Valor = valor;
    }
    
    //atributos
    private string tipo;
    private DateTime data;
    
    //propriedades
    
    //apontamento para o todo
    public Conta Conta {get; private set;}
    public decimal Valor { get; private set;}
    public decimal Saldo { get; private set;}
    
    
}
```

## Conceitos aplicados

### 1. Associação
A classe `Conta` possui uma referência para `Cliente`, indicando que uma conta está associada a um cliente.

### 2. Composição
A classe `Conta` mantém uma lista de `Movimentacao`, e cada `Movimentacao` aponta para a `Conta` à qual pertence. Isso representa uma relação de todo-parte.

### 3. Encapsulamento
As propriedades usam `private set`, permitindo que alterações sejam feitas apenas pelos métodos da própria classe.

### 4. Regras de negócio
Os métodos `Depositar` e `Sacar` validam valores inválidos e impedem saque maior que o saldo.

## Exemplo de execução

```text
Bem-vindo ao caixa rápido! Digite o valor do deposito
1000
Digite o valor do saque
300
Saque realizado com sucesso! Saldo restante: 700 | Cliente: Janderson
```

## Observações

- O código registra movimentação apenas no saque
- O depósito altera o saldo, mas não adiciona item em `ListaDeMovimentacoes` ainda... será feito na próxima aula
- Os campos `numero`, `agencia`, `rg`, `dataDeNascimento`, `tipo` e `data` existem, mas não estão sendo exibidos no console
- Esse exemplo é útil para estudar associação, composição, encapsulamento e regras simples de negócio
