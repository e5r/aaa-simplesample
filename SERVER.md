Servidor `A1` - Autenticação
============================

Esses são os protótipos das funcionalidades do servidor de autenticação.

## ValidateSystem

Quando um sistema requisita autenticação com `API.RequestAuthentication()`,
ele informa seu `systemToken` (que é na verdade uma chave) pública do seu registro
no servidor de autenticação), pois ele (o sistema) deve estar previamente cadastrado
no servidor. Além disso, também informa os dados do usuário que solicita a
autenticação.

```javascript
var App = new PrivateServer();

// Internal methods
App.prototype.internal = new InternalServer();

// Private method
App.prototype.validateSystem = function(systemToken, httpRequest) {
    if(App.useExtraProtection){
        if(!App.internal.systemIpInRegisteredRange(
            systemToken, httpRequest.IP)) {
            return false;
        }
        if(!App.internal.systemInAuthorizedTime(systemToken)){
            return false;
        }
        if(App.internal.systemRequestCountExceded(systemToken)){
            return false;
        }
    }
    var system = App.internal.getSystemByToken(systemToken);
    if(!system){
        return false;
    }
    if(!App.internal.systemIsActive(system)){
        return false;
    }
    return true;
}
```

## GenerateIntentionToken

Gera um `$token` com a intenção do usuário se autenticar em um sistema.

```javascript
var App = new Server();

// Private methods
App.prototype.private = new PrivateServer();

// Internal methods
App.prototype.internal = new InternalServer();

// Exported method
App.prototype.generateIntentionToken = function(systemToken, userInfo){
    if(!App.private.validateSystem(systemToken)){
        return new HttpError(401, 'Unauthorized');
    }

    if(App.useExtraProtection){
        if(!App.internal.userIpInRegisteredRange(
            systemToken, userInfo)) {
            return new HttpError(401, 'Unauthorized');
        }
        if(!App.internal.userInAuthorizedTime(systemToken, userInfo)){
            return new HttpError(401, 'Unauthorized');
        }
        if(App.internal.userRequestCountExceded(systemToken, userInfo)){
            return new HttpError(401, 'Unauthorized');
        }
    }

    
}
```
