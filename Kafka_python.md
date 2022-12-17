# Kafka using Python, Json and MySQL

Similar to what we did in the cmd to run zookeepr and kafka server and then send messages from the producer to the consumer, we do this in python. We then connect the
python script along with the MySQL database which is accessed using DBeaver.
  We first create a zookeeper and kafka server on the cli itself,
  then we write the code on python to connect them,
 
 
 ### Producer code:
 ```
 from time import sleep
from json import dumps
from kafka import KafkaProducer
import json

topic_name='sql'
producer=KafkaProducer(bootstrap_servers=['localhost:9092'],
                       value_serializer=lambda x:json.dumps(x).encode('utf-8'),
                       linger_ms=10)


f=open('data.json',)
data=json.load(f)

for i in data['students']:
    print("sending message////")
    producer.send(topic_name, value=i)
    sleep(2)
 ```
 
 ### Consumer Code:
 ```
 from kafka import KafkaConsumer
from json import loads
import json
import pymysql



topic_name='sql'

consumer=KafkaConsumer(topic_name,
                       bootstrap_servers=['localhost:9092'],
                       value_deserializer=lambda x: loads(x.decode('utf-8')),
                       auto_offset_reset='latest',
                       group_id='1'
                       )


    
class KafkaSql:
    def __init__(self) :
        self.con=pymysql.connect(host="localhost",
                                 user="root",
                                 password="Pokemon09*",
                                 db="kafkadb")
        
        
    def update_tables(self,data):
        
        ID=data['ID']
        Name=data['Name']
        print("Executing Query//")
        cur=self.con.cursor()
        cur.execute("""INSERT INTO students(ID, Name) VALUES(%s,%s)""",(ID,Name))
        self.con.commit()
        cur.close()
        print(ID,Name)
        #self.con.close()
    
x = KafkaSql()

for message in consumer:
    data=message.value
    print("Recieving data//")
    x.update_tables(data)
 ```
 
 ### Json File:
 ```
 {
 "students":[
 {
    "ID":"01",
    "Name":"Gaurav"
  },
  {
    "ID":"02",
    "Name":"Rahul"
  },
  {
    "ID":"03",
    "Name":"Raj"
  },
  {
    "ID":"04",
    "Name":"Rohan"
  }
]
}
 ```
 
#### What basicllly happens in this code is, a Json file is imported by the producer code and the dictionary is iterated at every entry. 
#### The producer then sends each entry. The consumer prints every entry it gets and then that particular entry is inserted into a MySQL database.
