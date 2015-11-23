---
layout: post
title: Como testar exceções em .net 
comments: true
---

# Melhorar a semântica nos teste utilizando todos os recursos das asserções 


... nesses casos ao invés de utilizar o `Assert.IsTrue` utilize o `Assert.AreEqual`.

Atual:

    Assert.IsTrue(context.tb_ChamadaBackup.Count() == 10);
    // ou
    Assert.IsTrue(volume.dtChamadaInicial.ToString() == "04/12/2014 20:00:00");


Sugerido:

    Assert.AreEqual(10, context.tb_ChamadaBackup.Count());
    // ou
    Assert.AreEqual("04/12/2014 20:00:00", volume.dtChamadaInicial.ToString());

Vai melhorar a semântica do seu teste.