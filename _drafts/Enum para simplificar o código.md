---
layout: post
title: Enum para simplificar o código
comments: true
---


# Enum para simplificar o código

... percebi que em alguns lugares você está utilizando essa conversão. Talvez, uma forma mais simples seja criar uma coluna chamada `inStatus`, mas com o retorno como um `Enum` e não um `int`, e utilizá-la sempre, ao invés de fazer a conversão na mão toda vez.

Para fazer o sugerido:

Na classe tb_VolumeBackup crie uma propriedade chamada `inStatus` que retorna `BackupStatus` e modifique o nome da propriedade `inStatus` atual para `inStatusAsInt`. Veja um exemplo:

	[Column("inStatus")]
	public int inStatusAsInt { get; set; }

	[NotMapped]
	public BackupStatus inStatus
	{
	  get { return (BackupStatus) this.inStatusAsInt; }
	  set { this.inStatusAsInt = (int)value; }
	}

Pronto. Depois disso, você sempre poderá utilizar um `Enum` direto, ao invés de converter toda vez o `Enum` para um `int`.


> Obs.: a partir da versão 5 do Entity Framework já temos suporte para Enum, porém, estamos utilizando a versão 4.1, certo?
