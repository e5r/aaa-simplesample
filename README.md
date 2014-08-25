Serviço simplificado de AAA para sistemas
=========================================

## AAA (Autenticação, Autorização, Auditoria)

Se propõe um dialeto de requisições sob HTTP que permite um sistema autenticar
usuários e autorizar recursos sem a necessidade de transitar suas credenciais
por um canal não seguro (HTTP). Para isso se utiliza de um servidor específico
que gerencia suas credencias, porém as requisições nesse são HTTP envelopadas
sob SSL (HTTPS), garantindo assim a segurança das informações.

### Premissas:

* Independente de linguagem de programação
* Totalmente compatível com HTTP/1.1
* Não transmite credenciais em PLAINTEXT


Proposto por [Erlimar Silva Campos][erlimar].

## TODO

* Rever códigos HTTP
* Rever inglês

## TODO - Fluxos básicos

* Usuário não autenticado em `autenticador.com` acessa `sistema.com`;
* Usuário autenticado em `autenticador.com` acessa `sistema.com`;
* Exemplo com Autenticação com Windows Authentication

## TODO - Fluxos de exceção

* `sistema.com` requisita serviço com `$token` não assinado;
* `outro.sistema.com` rouba `$token` de acesso e requisita serviço;
* `sistema.com` requisita serviço com `$token` válido, mas o usuário já fez
  logoff em `autenticador.com`;

### Vantagens

* Usuário
    * Pode revogar acessos individuais;
    * Logoff revoga todos os acessos (invalida `$tokens`);
    * Pode acessar o mesmo sistema com vários perfis diferentes;
* Sistema
    * Não precisa controlar credenciais;
    * Pode dispor várias visões de acesso;
    * Tem grafo infinito de funcionalidades;
    * Tem disponível hierarquia de funcionalidades diretamente acessíveis
      (itens como links de menus).
    * Assert remoto disponível;
    * Fazer/consultar LOG personalidado;
    *


[erlimar]:   http://erlimar.github.io
