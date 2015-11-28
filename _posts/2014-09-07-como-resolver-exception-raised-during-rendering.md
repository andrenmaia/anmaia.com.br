---
layout: post
title: Como resolver Exception raised during rendering - java.lang.System.arraycopy([CI[CII)V no Android Studio
comments: true
redirect_from:
  - /como-resolver-exception-raised-during-rendering/
---



Hoje estava experimentando o [Android Studio][1] para desenvolver uma nova app. O suporte para desenhar novas tela é realmente muito bom. Porém, quando coloquei o primeiro controle na tela, recebi o erro:

`_Exception raised during rendering: java.lang.System.arraycopy([CI[CII)V in Android`

Após o erro, fiquei sem saber o que fazer. Como o [Android Studio][1] ainda está na versão beta, tudo bem, dá para relevar. Pesquisei um pouco sobre o erro e descobrir que o problema estava na versão da API do android que vem selecionada por padrão para renderizar o layout no momento do desenvolvimento.

![Versão do Android selecionada no Android Studio]({{ site.url }}/assets/versao_do_android_api_render.png)

Esta versão vem com um `W` no final. Isso significa que a API é para a versão `Wearable` do Android (o novo relógio).


**Para resolver o problema é só selecionar outra versão que não tenha o `W` no final.** Pronto!


 [1]: https://developer.android.com/sdk/installing/studio.html
