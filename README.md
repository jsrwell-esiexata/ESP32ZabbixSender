# ESP32ZabbixSender
Library to realize the connection of the ESP32 on Arduino with the Zabbix server.

# How to Start
Add this on variables call:
```c++
ESP32ZabbixSender zSender;
```

Add this on setup after wifi conection successfull:
```c++
// Add correct server IP, Port (commom used is 10051) and unique hostname of ESP32.
zSender.Init(IPAddress(127, 0, 0, 1), 10051, "MY_HOST_NAME");
```

# How to Send Status
To get status of ESP do this on the loop:
```c++
// Heap memory get.
float freeMemory = esp_get_free_heap_size();
float totalMemory = ESP.getHeapSize();
float usedMemory = totalMemory - freeMemory;
float cpuUsage = 100 - (freeMemory * 100 / totalMemory);
```
Then call this function:
```c++
// The keys received on zabbix will be: ping, total, use, free and usage.
zSender.EspStatus(totalMemory, usedMemory, freeMemory, cpuUsage);
zSender.Send();
```

# How to Send Custom Messages
You can send this type of messages:
```c++
zSender.AddItemInt("mykeyname", 123); // To send integers
zSender.AddItemFloat("mykeyname", 123.45); // To send float
zSender.AddItemString("mykeyname", "My Value"); // To send Strings
```

The sequence of call must be:
```c++
// Fist.
zSender.ClearItem(); // To reduce to 0 number of itens add on message.

// Then.
zSender.AddItemFunction("mykeyname", "my_value_in_correct_format");
// "Function" must be replaced to "Int", "Float" or "String" and the second parameter must be the correct type.

// Finally.
zSender.Send(); // To send the group of data to Zabbix.
```
# Based on:  
https://github.com/leruetkins/ESP32ZabbixSender

## Changes: 
The filenames is changed and new fuctions is created on the class, but is in the documentation here.