API `A1` - Autenticação
=======================

## Índice

* API A1
    * __RequestAuthentication__ - Solicita autenticação
    * __SignToken__ - Assina (valida) uma autenticação

## RequestAuthentication

Registra a intenção do usuário autenticar-se no sistema. Se tudo der certo, o
sistema receberá um `$token` de autenticação, porém _não assinado_ (chamado
token de intenção).

> __PS__: O sistema não está autenticando o usuário, somente a __intenção__ de
autenticação do mesmo. Essa (intenção) precisará ser confirmada posteriormente com a
assinatura do `$token` recebido.

Este `$token` não assinado, não serve para o usuário requisitar autorizações no
futuro. Se o mesmo for usado, o servidor irá negar o acesso indiscriminadamente.
Podendo inclusive (opcional) bloquear o acesso até segunda ordem (um tempo
determinado, ou até que um responsável desbloquei explicitamente).

```javascript
var Lib = new A1();

// Exported method
Lib.prototype.requestAuthentication = function(systemToken, userInfo) {
    var request = new Request('https://autenticator.com');
    if(Lib.useExtraProtection){
        var intentionInfo = Lib.crypt(privateSystemToken, userInfo);
        request.addHeader('X-AAA-SystemToken', systemToken);
        request.addHeader('X-AAA-IntentionInfo', intentionInfo);
    }else{
        request.addHeader('X-AAA-SystemToken', systemToken);
        request.addHeader('X-AAA-UserIP', userInfo.IP);
        request.addHeader('X-AAA-UserBrowser', userInfo.Browser);
        /* request.addHeader('X-AAA-N', '...') */
    }
    return request.send().token; // SYNC fake
}
```

##SignToken

Assina um `$token` tornando-o apto a ser usado em requisições de autorização.
Este é o `$token` obtido com a chamada inicial a `RequestAuthentication`.

```javascript
var Lib = new A1();

// Exported method
Lib.prototype.signToken = function(systemToken, intentionToken) {
    var request = new Request('https://autenticator.com');
    request.addHeader('X-AAA-SystemToken', systemToken);
    request.addHeader('X-AAA-IntentionToken', intentionToken);
    return request.send().token; // SYNC fake
}
```
