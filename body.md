{
  "StartAt": "Base de conhecimento",
  "States": {
    "Base de conhecimento": {
      "Type": "Pass",
      "Next": "Resposta da IA",
      "Result": {
        "objetivo": "Crie uma função em python sobre a raiz quadrada de 12 e explique passo a passo"
      },
      "ResultPath": "$.objetivo"
    },
    "Resposta da IA": {
      "Type": "Task",
      "Resource": "arn:aws:states:::bedrock:invokeModel",
      "Parameters": {
        "ModelId": "arn:aws:bedrock:us-east-1::foundation-model/anthropic.claude-3-haiku-20240307-v1:0",
        "ContentType": "application/json",
        "Accept": "*/*",
        "Body": {
          "anthropic_version": "bedrock-2023-05-31",
          "max_tokens": 200,
          "messages": [
            {
              "role": "user",
              "content.$": "$.objetivo.objetivo"
            }
          ]
        }
      },
      "ResultPath": "$.result_one",
      "ResultSelector": {
        "result_one.$": "$.Body.content[0].text"
      },
      "End": true
    }
  }
}