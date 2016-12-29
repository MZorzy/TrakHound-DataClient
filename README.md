![TrakHound DataClient](dataclient-logo-100px.png)
<br>
<br>
TrakHound DataClient reads MTConnect® streams and sends the data to TrakHound DataServers to be stored. 


# Features
- Automatically finds and configures MTConnect devices on a network
- Data filtering with triggers to collect all data or only what is needed
- Ability to send data to multiple TrakHound DataServers to create data redundancy or to meet data security requirements (local vs cloud)
- Utitlizes streaming connections for both MTConnect and connections to TrakHound DataServers
- Supports SSL(TLS) for sending data to TrakHound DataServers
- Non-volatile buffering to retain collected data between connection interruptions



# Configuration
Configuration is read from the **server.conf** XML file in the following format:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<DataServer>
  <Devices>
    <Device deviceId="1234" deviceName="VMC-3Axis">http://agent.mtconnect.org</Device>
    <Device deviceId="456" deviceName="OKUMA.Lathe">http://74.203.109.245:5001</Device>
  </Devices>
  <DataServers>
    <DataServer url="http://api.trakhound.com" bufferPath="c:\TrakHound\Buffers\">
      <DataTypes>
        <DataType>POSITION</DataType>
        <DataType>STATUS</DataType>
        <DataType>PROGRAM</DataType>
      </DataTypes>
    </DataServer>
  </DataServers>
</DataServer>
```

```xml
<?xml version="1.0" encoding="utf-8" ?>
<DataClient>
  <Devices>
    <Device deviceId="1234" deviceName="VMC-3Axis">http://agent.mtconnect.org</Device>
    <Device deviceId="456" deviceName="OKUMA.Lathe">http://74.203.109.245:5001</Device>
  </Devices>
  <DataServers>
    <DataServer url="http://api.trakhound.com" bufferPath="c:\TrakHound\Buffers\">
      <DataTypes>
        <DataType>POSITION</DataType>
        <DataType>STATUS</DataType>
        <DataType>PROGRAM</DataType>
      </DataTypes>
    </DataServer>
  </DataServers>
</DataClient>
```

## Device 
Represents each MTConnect Agent that the Device Server going to be reading from.

#### Device ID 
###### *(XmlAttribute : deviceId)*
The globally unique identifier for the device (usually a GUID)

#### Device Name
###### *(XmlAttribute : deviceName)*
The DeviceName of the MTConnect Device to read from

#### Address
###### *(XmlText)*
The base Url of the MTConnect Agent. Do not specify the Device Name in the url, instead specify it under the deviceName attribute.

## Data Server
Represents each TrakHound Data Server that data is sent to in order to be strored and processed.

#### Url 
###### *(XmlAttribute : url)*
The base Url of the TrakHound Data Server to send data to

#### Buffer Path
###### *(XmlAttribute : bufferPath)*
The directory where the buffer files should be stored. The Buffer is used to store data that hasn't been successfully sent yet.


### DataGroups
DataGroups allow configuration for what data is captured and sent to the DataServer. Data is filtered by Type or by their parent container type. DataGroups can include a list for allowed types as well as for denied types. A CaptureMode can also be defined to configure when the data in the DataGroup is sent.

#### Name
The identifier for the DataGroup. This is primarily used when the DataGroup is being included in another group.

#### CaptureMode
The mode in when data is captured.
  - ACTIVE : Always capture and send data defined in the DataGroup
  - PASSIVE : Only capture and send when included in another DataGroup. This can be used for constantly changing data such as Axis Position to reduce the amount of data stored in the DataServer's database.
  
#### Allow
A list of Types and Ids to capture. This can also include container paths with the wildcard character (*) to allow any types or ids within the container.

- ID : DataItem/Component/Device Id to allow (case sensitive)
- TYPE : DataItem/Component/Device Type to allow. Can be in the format of "PATH_FEEDRATE" or "PathFeedrate".

#### Deny
A list of Types to not capture. This overrides any allowed types.

#### Include
A list of other DataGroups to include when capturing for the current DataGroup. For example, this can be used to capture position data only when another group changes in order to reduce the amount of data stored in the DataServer's database.










