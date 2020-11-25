# GW_IOTSDK

GW_IOTSDK提供设备接入国网IoT物联网平台的Java版本的SDK，提供设备和平台之间通讯能力，并且针对各种场景提供了丰富的demo代码。IoT设备开发者使用SDK可以大大简化开发复杂度，快速的接入平台。


* [支持特性](#支持特性)
* [如何使用](#如何使用)
* [初始化参数](#初始化参数)
* [获取设备信息](#获取设备信息)
* [设备管理](#设备管理)
* [下发命令](#下发命令)
* [上行数据](#上行数据)

## 支持特性
- 支持设备管理（增删改查）
- 消息上报
- 命令下发


## 如何使用

依赖的版本：
* JDK ：1.8 +

因为GW_IOTSDK还没有发布到公共仓库，如果要使用，需要先下载代码在本地构建。
    
    mvn clean install 

项目中可以使用dependencyManagement引入依赖。

    <dependency>
                <groupId>com.gw</groupId>
                <artifactId>GW_IOTSDK</artifactId>
                <version>1.0.1</version>
    </dependency>
    
### 初始化参数

初始化参数
```java

	String url;//物联网平台的API网址 http://{ip}:{port}/CloudApi

    String appId;//物联网平台中对应应用的AppID

    String secretKey;//物联网平台中对应应用的SecretKey

    String account;//物联网平台中登录用户账号

    String password;//物联网平台中登录用户密码，进行 md5 加密后值
	
	IOTDeviceAction action = new IOTDeviceAction(String url,String appId,String secretKey,String account,String password);
		
	

```

### 获取设备信息

单个设备信息：
```java

	String productId 产品ID
    String deviceCode 设备唯一编码
    IOTDeviceOutData getDevices(String productId,String deviceCode);
	   
	 
```

批量获取设备信息：
```java

	pageIndex 页索引，从1开始，如果小于等于0表示不启用分页
    pageSize  范围：1-100
    orderBy   空表示不启用
    order
	IOTDeviceOutData getDevices(int pageIndex, int pageSize, String orderBy, String order);
		
	 
```					
完整代码参见MessageSample.java					

### 设备管理


添加设备：
```java

	deviceData 设备基本信息
	
    IOTDeviceOutData addDevice(IOTDevice deviceData)
	
```
更新设备：
```java

	deviceData 设备基本信息
	
    IOTDeviceOutData addDevice(IOTDevice deviceData)
	
```
```java
IOTDevice实体：
	/**
	 * @description: IOT设备实体
	 * @author: zfg
	 * @create: 2020-11-24
	 **/
	@Data
	public class IOTDevice implements Serializable {

		public String _id;
		
		/// 产品ID
		public String productid;
		
		/// 设备ID
		public String deviceid;
		
		/// 设备名称
		public String devicename;
		
		/// 设备状态 0:未激活 1:在线 2:离线
		public String status;
		
		/// 设备密钥
		public String devicesecret;
		
		/// 设备网络地址
		public String remoteip;

		/// 创建时间
		public Date createtime;
		
		/// 修改时间
		public Date modifytime;
		
		/// 产品名称
		public String remark;
		
		/// 固件信息
		public String firmware;
		
		/// 定位点
		public String development;
		
		/// 位置描述
		public String desc;
		
		/// Token周期
		public String reserve;
		
		/// 创建人
		public String createby;
		
		/// 修改人
		public String modifyby;
		
		/// 省
		public String province;
		
		/// 市
		public String city;
		
		/// 区县
		public String district;
	}
```
删除设备：
```java
	productId  产品ID
    
	deviceCode 设备唯一编码

	IOTDeviceOutData deleteDevice(String productId, String deviceCode)


	
```

### 下发命令

```java
	String protocol   协议类型，参考EnumIOTProtocolType
    String productId  产品ID
    String deviceCode 设备唯一编码
    String payload    下发指令参数json格式
    String ocpara     协议是电信 OC 平台时有效，参数json格式：[{"serviceId":,"commandId":,"paraName":,"paraValue":,"isNum":}]
	
    CommandToDevice(EnumIOTProtocolType protocol, String productId, String deviceCode, String payload, String ocpara)
   
```


### 上行数据

```java
	
	protocol 设备所属产品使用的节点类型，参考EnumIOTProtocolType
    object 不同协议下的数据实体，GB26875：GB26875Data；GWProtocol: GB26875Data；MQTT 协议：MqttData；LWM2M 协议：LWM2MData
	
    IOTDeviceOutData AddData(EnumIOTProtocolType protocol, Object object)

```
### 部分实体

接口返回IOTDeviceOutData：

```java
/**
 * @description: 操作IOT设备返回类
 * @author: zfg
 * @create: 2020-11-17
 **/
@Data
public class IOTDeviceOutData implements Serializable {

    private Integer code;//状态码：200正常

    private String msg;//数据内容

    private String extra;//数据条数

    private String data;//其他数据
}

```

协议枚举EnumIOTProtocolType：
```java

/**
 * 协议类型枚举
 */
public enum EnumIOTProtocolType {

    MQTT协议("MQTT"),
    LWM2M协议("LWM2M"),
    GWProtocol("GWProtocol"),
    GB26875("GB26875"),
    电信OC平台("ChinacomOC"),
    电信AEP平台("ChinacomOC");


    //以上是枚举的成员，必须先定义，而且使用分号结束
    private final String methods;

    EnumIOTProtocolType(String methods)
    {
        this.methods=methods;
    }

    public String getMethods()
    {
        return methods;
    }

}
```

完整代码参见IOTDeviceAction.java

作者：Zero
