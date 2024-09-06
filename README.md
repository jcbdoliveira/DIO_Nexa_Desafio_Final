# DIO Nexa Desafio Final
 Criando um Assistente de Delivery com AWS Step Functions e Bedrock

```
{
  "Comment": "A workflow to create a complete menu suggestion with food, drink, and location",
  "StartAt": "GetFoodSuggestion",
  "States": {
    "GetFoodSuggestion": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "arn:aws:lambda:REGION:ACCOUNT_ID:function:suggest-food",
        "Payload": {}
      },
      "ResultPath": "$.menu.food",
      "Next": "GetDrinkSuggestion"
    },
    "GetDrinkSuggestion": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "arn:aws:lambda:REGION:ACCOUNT_ID:function:suggest-drink",
        "Payload": {}
      },
      "ResultPath": "$.menu.drink",
      "Next": "GetLocationSuggestion"
    },
    "GetLocationSuggestion": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "arn:aws:lambda:REGION:ACCOUNT_ID:function:suggest-location",
        "Payload": {}
      },
      "ResultPath": "$.menu.location",
      "Next": "CompileMenu"
    },
    "CompileMenu": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "arn:aws:lambda:REGION:ACCOUNT_ID:function:compile-menu",
        "Payload": {
          "menu.$": "$.menu"
        }
      },
      "ResultPath": "$.finalMenu",
      "End": true
    }
  }
}
```

Este workflow consiste nos seguintes estados:

1. GetFoodSuggestion: Um estado de tarefa que invoca uma função Lambda para obter uma sugestão de comida. O resultado é armazenado sob o caminho $.menu.food.

2. GetDrinkSuggestion: Um estado de tarefa que invoca uma função Lambda para obter uma sugestão de bebida. O resultado é armazenado sob o caminho $.menu.drink.

3. GetLocationSuggestion: Um estado de tarefa que invoca uma função Lambda para obter uma sugestão de local (país, estado e cidade). O resultado é armazenado sob o caminho $.menu.location.

4. CompileMenu: Um estado de tarefa final que invoca uma função Lambda para compilar todas as sugestões em um menu completo. O resultado final é armazenado sob o caminho $.finalMenu, e este estado marca o fim do workflow.

Para replicar e testar você precisará substituir REGION, ACCOUNT_ID e os nomes das funções Lambda pelos valores apropriados para o seu ambiente. 