# Huawei_Sample
Samples on HueaweiPushMesssages


Este es un ejemplo de como mandar llamar las notificaciones Push desde una app Tipo Postman, recuerde que debe de tener aceso a la cuenta "Consola" de AGC de Huawei y tener habilitado el apartdo PushKit para que al final de la página aparezcan. 
1. Las credenciales de generación de Token de sesion que expira. este es un Postman por aparte que genera un token que se va a poner en el Header de "Authentication" del POSTAMN 2, expira asi que debe de generarse y pasarlo al postman que ya va a generar las Push. tienes 5 minutos para usarlo una vez.
2. Ahora se debe contar con los mismo datos anteriores  y con el ID de la app para generar el Message, en el body que comparto el TOKEN del final no es el token de autenticacion sino es el token del Device o del dispositivo que tienen nuestra App ya desplegada, no es uno por dispositivo sino mas bien es uno por app que se encuentra instalada en el dispositivo haciendolo mas particular y específico aún. Esto para saber desde donde viene y ha donde va pasando por el AGC de Huawei. Yo uso el Token de dispositivo porque Mando OTP aleatorio como factor de autenticacion extra para mi App.

POSTMAN 1 [GET]
URL https://oauth-login.cloud.huawei.com/oauth2/v3/token

con datos de la consola AGC de OAuth 2.0 client ID:
Client ID = DATO DE AGC...
Client secret = DATO DE AGC...


grant_type       client_credentials
client_id        DATO DE AGC...
client_secret    DATO DE AGC... 

Salida del tipo
{
    "access_token": "AQUI REGRESA EL TOKEN PARA USAR EN EL POSTMAN2",
    "expires_in": 3600,
    "token_type": "Bearer"
}



POSTMAN 2.  [POST]
URL https://push-api.cloud.huawei.com/v1/[AppID que sacas de AGC en Settings de tu App]/messages:send
anexo Authorization
No AUTH

anexo PARAMS
grant_type       client_credentials
client_id        DATO DE AGC...
client_secret    DATO DE AGC... 

anexo Headers
Dejale todos y pon dos al final
POST       /oauth/v2/token HTTP/1.1
Authorization     AQUI PON EL TOKEN DE SESION ANTERIOR POSTMAN1  
anexo Body:
{
"message" : {
 "data" : "{ 'score' : '7', 'time' : '10:00' }" ,
 "notification" : { 
     "title" : "Huawei Developer RZT Eruditsioon", 
     "body"  : "OTP ejemplo 997684"
 },
 "android" : {
     "data" : "{'androidData' : '7', 'time' : '10:00'}",
     "notification" : {
         "click_action" : {
             "type" : 3
         }
     }
 } ,
 "token": [
    "aqui va tu token generado por tu APP en el dispositivo"
 ]
} 
}

Salida del tipo 
{
    "code": "80000000",
    "msg": "Success",
    "requestId": "68356865460032.."
}
