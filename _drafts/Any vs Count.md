---
layout: post
title: Any vs Count
comments: true
---


# Any vs Count

2 observações:

1. Não entendi muito bem o que significava esse método (`CheckCallOtherBackup`) apenas lendo o seu nome. Sugiro alterá-lo para `bool ThereAreBackupCallsFor(tb_ExclusaoAudio audioToDelete)`, de acordo com a descrição no comentário do método, me parece fazer mais sentido. (Lembra-se, o nome do método e os comentário não devem apenas existir, tem que ter um significado claro. Pense sempre que uma pessoa que não sabe nada de nada está lendo o seu código._)

2. No método `CheckCallOtherBackup`, a verificação da existência de registros no banco de dados através do `.Count ()`, neste caso, é ineficiente. Por que é ineficiente?

Quando você executa o `Count` no SQL Server o que basicamente vai acontecer é que o banco de dados vai filtrar todas as linhas da tabela em busca da sua clausula de filtro (o seu `where`), depois disso, ele vai projetar o resultado da busca e contar o total de linhas. 

Uma forma mais eficiente de verificar se existe um registro no banco de dados é utilizando o método `Any`. Quanto você utiliza o método `Any` o SQL gerado não utilizará o comando `Count` e sim o comando `Exists`. O comando `Exists` diferentemente do `Count` não precisa necessariamente percorrer a tabela inteira para lhe dar uma respostas, assim que ele encontrar o primeiro registro, ele para e lhe informa que: sim, existe um registro com as características que você está buscando na tabela procurada.

Então, dito tudo isso...

Mude o seu método de:

	private bool CheckCallOtherBackup(tb_ExclusaoAudio audiodelete)
	{
		// verifica se existe registros na tb_ChamadaBackup com o mesmo ID
		int totalCalls = _context.tb_ChamadaBackup.Where(c => c.inIdChamada == audiodelete.inIdChamada).Count();

		///se existir retorna true se não existir retorna false
		return (totalCalls == 0) ? false : true;
	}

Para:
			
	private bool CheckCallOtherBackup(tb_ExclusaoAudio audiodelete)
	{
		return _context
					.tb_ChamadaBackup
					.Any(c => c.inIdChamada == audiodelete.inIdChamada);
	}	
