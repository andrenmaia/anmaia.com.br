---
layout: post
title: A ordem importa
comments: true
redirect_from:
    - /a-ordem-importa/
---

Quando falamos de `pattern matching` em Elixir a ordem importa!

Rode o `iex` e crie o módulo a seguir:

```elixir
defmodule MyModule do
    def print(text), do: IO.puts "text"
    def print(:my_atom_text), do: IO.puts "my_atom_text"
end
```

Agora execute a função `print` passando o `atom` para garantir o matching na segunda função:

```sh
iex(2) > MyModule.print(:my_atom_text)
text
:ok
```

Isso pode parecer inesperado porque o resultado é `text` e não `my_atom_text`. Isso ocorre porque o matching acontece com a primeira função do módulo que é genérica. O mecanismo de matching procura a função mais próxima e quando ele acha será ela mesma a executada.

Para ter o comportamento esperado, redeclare o módulo com as funções mais específicas primeiro. Veja:

```elixir
defmodule MyModule do
    def print(:my_atom_text), do: IO.puts "my_atom_text"
    def print(text), do: IO.puts "text"    
end
```

Agora execute a função `print` novamente. O resutado será outro.
```sh
iex(2) > MyModule.print(:my_atom_text)
my_atom_text
:ok
```

Aprendi isso com um bug dentro de uma aplicação em um cenário bem complicado de enxergar a ordem das coisas. Então lembre-se: **a ordem importa** quando estamos falando de pattern matching em Elixir.
