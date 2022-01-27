# Домашнее задание к занятию "7.6. Написание собственных провайдеров для Terraform."

1. Найдите, где перечислены все доступные `resource` и `data_source`, приложите ссылку на эти строки в коде на 
гитхабе.   

resource: [](https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a
/aws/provider.go#L411)

data_source: [https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws
/provider.go#L169]()


3. Для создания очереди сообщений SQS используется ресурс `aws_sqs_queue` у которого есть параметр `name`. 
    * С каким другим параметром конфликтует `name`? Приложите строчку кода, в которой это указано.
   
   `ConflictsWith: []string{"name_prefix"}`
   [https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws
   /resource_aws_sqs_queue.go#L56]()
    * Какая максимальная длина имени? 
    * Какому регулярному выражению должно подчиняться имя? 
    
