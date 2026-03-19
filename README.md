# eng-poo

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
