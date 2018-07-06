空间管理消息格式：
topic : campus_space
            tag:
            
             body:
                    {
            "msgType": "消息类型",              
            "transId": "唯一ID，防止重复处理",
            "version": "1.0",
         "protocol"
:"http|mq",
            "actionType": "add|delete|update",
            "source": "来源",
                   ---- space|device|ibos
            "eventTime": "下发时间 long型",
            "data":[
                {
                "campusId":"园区ID",
                "connectorId":"connectorID ",
                "deviceId":"设备ID",
                "code":"设备点位编码",
                "value":"值",
                "dataType":"值类型：num,string,enum等"
                }
               ]
}
           
            
data
msgType
事件中心类别
空间单元——新增、删除、修改
[{  
 Long id;
Long buildingId;
  String buildingName;
String typeName;
String code;
Long typeId;
String uuid;
Long campusId;
String campusCode;
String floorName;
String name;
Long floorId;
String campusName;
String typeCode;
Long companyId;
Long category;
BigDecimal area;
BigDecimal height;
Integer status;
Double lat;
Double lng;
String ftId;
 String geoFloorId;
}]
space_unit
space_unit.add
space_unit.delete
space_unit.update
空间分组——新增、删除、修改
[{ 
String id;
String name;
String code;
String uuid;
String description;
Long typeId;
String typeName;
String typeCode;
Long campusId;
Long buildingId;
Long floorId;
}]
space_group
space_group.add
space_group.delete
space_group.update
空间分组——新增成员、移除成员
[{ 
  Long campusId;
List poiIds;
 Long spaceGroupId;
 String typeCode;
}]
space_group_member
space_group_member.add
space_group_member.delete
空间楼层——新增、修改
[{  
  Long id;
 String name;
 Long buildingId;
 Long buildingName;
    String campusCode;
Long campusId;
 Long buildingId;
}]
space_floor
space_floor.add
space_floor.update
空间楼宇——新增、修改
[{ 
  Long id;
 String name;
 String campusCode;
 Long campusId;
 Long buildingId;
 }]
space_building
space_building.add
space_building.update
空间业务属性值——修改
[{
Long id;
 Stirng name;
String typeCode;
 Long campusId;
 String campusCode;
 String attrKey;
 String attrValue;
 String valueType;
 String spaceType;
}]
sapce_attr
sapce_attr.update
用途小类——新增、修改
[{
 Long id;
 Stirng name;
Stirng code;
Long bigTypeId;
String bigTypeName;
String bigTypeCode;

}]
space_type
space_type.add
space_type.update
 设备管理消息格式：
topic：


data
msgType
事件中心类别
设备(包括物理、逻辑)——新增、删除、修改
[{
  String code;
String ip;
String lastUpdateTime;
String version;
String nickname;
String uuid;
String spaceInfo;
Long spaceId;
String spaceName;
Long floorId;
String floorName;
Long buildingId;
String buildingName;
Long campusId;
String campusName;
Boolean beRun;
String templateCode;
String templateName;
Boolean beLogic;
String longitude;
String latitude;
}]
device
device.add
device.delete
device.update
设备类型——新增、删除、修改
{"id":1,"name":"xx",...
}
device_template
device_template_increment
设备数据下发
[
{
"deviceId": 50001,
"beRun": true,
"templateId": "5000",
"templateCode":"light",
"version": "1.0.0",
"uuid": "d252a99c-5b3d-4          255-b8ae-1a4bc165ad33",
"lastUpdateTime": 150150               3133000,
"appProtocol": {
"protocolId": 990999,
"param": {
"opcGroupCode": "t|2                     -6F-D5-E15"
}
},
"metaPointData": [
{
"code": "bright",
"alarm": false,
"mappingPoint": ""
},
{
"code": "onOff",
"alarm": false
}
]
}
]
device-download
device-download
设备——cmd
[{
"campusId":"园区ID",
"connectorId":"connectorID ",
"deviceId":"设备ID",
"code":"设备点位编码",
"value":"值",
"dataType":"值类型：num,string,enum等"
}]
device_control
cmd
