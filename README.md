# Lab 4
[Fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) this repo and clone it to your machine to get started!

## Team Members
- team member 1 Brendan Best
- team member 2

## Lab Question Answers

Answer for Question 1: Apoligies again I will be turning in my files here. I am still having some trouble with my git push and I will be working on get it working before this next lab.

vm_cont_chain

import paho.mqtt.client as mqtt
import socket
import time

def on_connect(client, userdata, flags, rc):
    print("Connected to server (i.e., broker) with result code "+str(rc))
    client.subscribe("btbest/ping")
    client.message_callback_add("btbest/ping", on_message_from_pong)
    
def on_message(client, userdata, msg): 
    print("default callback - topic: " + msg.topi + " msg: " + str(msg.payload, "utf-8"))

def on_message_from_pong(client, userdata, message):
    num = int(message.payload.decode())
    new_num= num+1
    print("custom callback - Integer: "+str(num))
    client.publish("btbest/pong", new_num)
    print("number being published "+ str(new_num))
    time.sleep(4)
   

if __name__ == '__main__':
    client = mqtt.Client()
    client.on_connect = on_connect
    client.on_message = on_message
    client.connect(host="eclipse.usc.edu", port=11000, keepalive=60)    
    client.loop_forever()

vm_start_chain

import paho.mqtt.client as mqtt
import socket
import time

"""This function will be executed when this client receives 
a connection acknowledgement packet response from the server and then it subscribes to the other file. """
def on_connect(client, userdata, flags, rc):
    print("Connected to server "+str(rc))
    client.subscribe("btbest/pong")
    client.message_callback_add("btbest/pong", on_message_from_pong)
    
    
"""This function is the default callback for messages receieved"""    
def on_message(client, userdata, msg): 
    print("default callback - topic: " + msg.topi + " msg: " + str(msg.payload, "utf-8"))

"""These are just custom callback functions"""
def on_message_from_pong(client, userdata, message):
    num = int(message.payload.decode())
    new_num = num+1
    print("custom callback - Integer: "+str(num))
    client.publish("btbest/ping", new_num)
    print("number being published "+ str(new_num))
    
vm_pub

def on_connect(client, userdata, flags, rc):
    print("Connected to server (i.e., broker) with result code "+str(rc))

if __name__ == '__main__':
    ip_address='68.181.32.115'
    client = mqtt.Client()
    
    client.loop_start()
    time.sleep(1)

    while True:
        client.publish("btbest/ipinfo", "68.181.32.115")
        print("Publishing ip address")
        time.sleep(4)

        current_time=datetime.now()
        client.publish("btbest/date", f"{current_time.month,current_time.day,current_time.year}")
        client.publish("btbest/time", f"{current_time.hour, current_time.minute}")
              
vm_sub

import paho.mqtt.client as mqtt
def on_connect(client, userdata, flags, rc):

    print("Connected to server (i.e., broker) with result code "+str(rc))
    client.subscribe("btbest/ipinfo")
    client.subscribe("btbest/date")
    client.subscribe("btbest/time")
    
    client.message_callback_add("btbest/ipinfo", on_message_from_ipinfo)
    client.message_callback_add("btbest/date", on_message_from_date)
    client.message_callback_add("btbest/time", on_message_from_time)


def on_message(client, userdata, msg):
    print("Default callback - topic: " + msg.topic + "   msg: " + str(msg.payload, "utf-8"))

def on_message_from_ipinfo(client, userdata, message):
   print("Custom callback  - IP Message: "+message.payload.decode())
def on_message_from_date(client, userdata, message):
   print("custom callback  - Date: "+message.payload.decode())
def on_message_from_time(client, userdata, message):
   print("custom callback  - Time: "+message.payload.decode())

if __name__ == '__main__':
    
    client = mqtt.Client()
    client.on_message = on_message
    client.on_connect = on_connect

      
    client.connect(host="68.181.32.115", port=11000, keepalive=60)
    client.loop_forever()
...
