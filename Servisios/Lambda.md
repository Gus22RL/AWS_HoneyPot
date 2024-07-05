Create a new fuction,select "Author from scratch" and use python3.8.
![[Pasted image 20240705115716.png]]Add a new trigger.
![[Pasted image 20240705115816.png]]

We ar going to us this code.
```
import json
import boto3
import time

glue_client = boto3.client('glue')

def lambda_handler(event, context):
    # Intentar iniciar el crawler
    response = glue_client.start_crawler(Name='Honey-crawler')
    print(f"Crawler initiation response: {response}")
    
    # Verificar el estado del crawler hasta que esté listo
    while True:
        response = glue_client.get_crawler(Name='Honey-crawler')
        crawler_state = response['Crawler']['State']
        
        if crawler_state == "READY":
            break
        
        print(f'The crawler is {crawler_state}')
        time.sleep(5)
    
    print("The crawler is READY")
    return {
        'statusCode': 200,
        'body': json.dumps('Proceso completado con éxito')
    }
```

Set permissions and roles to activet crawler.
![[Pasted image 20240705120015.png]]
