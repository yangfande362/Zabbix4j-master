# 注意：由于原zabbix4j团队已经于2017年不再更新该组件，本人也只是临时用到对个别接口适配zabbix6,本项目仅供参考，不可做生产使用。 
另有发现一个demo组件，更加轻量化，理论上适配zabbix各个版本， 各位可以移步参考：https://gitee.com/cicdi/Zabbix-Api?_from=gitee_search

## Zabbix4j

Zabbix4j is a Zabbix API library for the Java language.

Zabbix4j includes software from JSON.org to parse JSON response from the Zabbix API.
You can see the license term at http://www.JSON.org/license.html

Zabbix4j classes and methods structure is based on Zabbix API vsersion 2.2. 
You should see [Zabbix API document](https://www.zabbix.com/documentation/2.2/manual/api)

## Usage

How to create new Host.

```
String user = "admin";
String password = "zabbix";

// login to zabbix
ZabbixApi zabbixApi = new ZabbixApi("http://localhost/zabbix/api_jsonrpc.php");
zabbixApi.login(user, password);
            
final Integer groupId = 25;
final Integer templateId = 10093;

HostCreateRequest request = new HostCreateRequest();
HostCreateRequest.Params params = request.getParams();
params.addTemplateId(templateId);
params.addGroupId(groupId);

// set macro
List<Macro> macros = new ArrayList<Macro>();
Macro macro1 = new Macro();
macro1.setMacro("{$MACRO1}");
macro1.setValue("value1");
macros.add(macro1);
params.setMacros(macros);

// set host interface
HostInterfaceObject hostInterface = new HostInterfaceObject();
hostInterface.setIp("192.168.255.255");
params.addHostInterfaceObject(hostInterface);

params.setHost("test host created1." + new Date().getTime());
params.setName("test host created1 name" + new Date().getTime());

HostCreateResponse response = zabbixApi.host().create(request);

int hostId = response.getResult().getHostids().get(0);
```


## How to release

$ ./gradlew build

## How to make jar

$ ./gradlew jar

## License

This software is distributed under the MIT License.


## 为对接zabbix6,对2022年11月16日的myaaaaa-chan/Zabbix4j项目进行修改（目前仅对接获取最新数据、主机列表两个接口）：
1、ItemGetRequest的delay等数值型字段改成String;
2、ItemGetRequest的output改成List<String>;
