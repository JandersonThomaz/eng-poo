# Exemplo de Conta Bancária em C#

Este é um exemplo simples em C# simulando operações básicas de uma conta bancária, com:

- depósito
- saque
- associação entre `Conta` e `Cliente`

## Código

```csharp
using System;
					
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
```

## O que o código faz

O programa:

1. recebe um valor de depósito
2. recebe um valor de saque
3. cria um cliente
4. cria uma conta vinculada ao cliente
5. realiza depósito e saque
6. exibe o saldo final

## Observações

- A classe `Conta` possui uma referência para `Cliente`
- O saldo só pode ser alterado pelos métodos `Depositar` e `Sacar`
- O código usa `Exception` para validar regras simples de negócio

## Exemplo de execução

```text
Bem-vindo ao caixa rápido! Digite o valor do deposito
1000
Digite o valor do saque
300
Saque realizado com sucesso! Saldo restante: 700 | Cliente: Janderson
```
