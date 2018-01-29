
# JdSmart设备端智能家居控制接入
   * [目录]
      * [修订记录](#修订记录)
      * [概述](#概述)
      * [接入操作指南](#接入操作指南)
      * [功能](#功能)
      * [重点说明](#重点说明)
      * [主要类介绍](#主要类介绍)
      * [参考示例说明](#参考示例说明)
      * [设备命令与设备状态说明](#设备命令与设备状态说明)



## 修订记录
--------

| 版本   | 日期     | 修改记录         | 修改人 |
|--------|----------|------------------|--------|
| V1.0.00 | 20161208 | 初始版本   | Luke |
| V1.0.07 | 20170328 | 增加新的设备类型，温湿一休sensor,烟感，CO2 sensor, 无进度窗帘   | Luke |
| V1.1.08 | 20170504 | 支持Bugly升级 | Luke |
| V1.1.09 | 20170515 | 更新混淆文件proguard-rules.pro<br>支持raw语音，直接将用户语音输出到接口函数 | Luke |
| V1.2.13 | 20170810 | 支持播放TTS文本内容, playTTS(String tts)<br>新增一些设备类型 <br>修改buggly升级 <br>setFilterVoiceMode 和setVoiceTex被禁用 | Luke |
| V1.2.20 | 20171109 | 重新导入时调用refeshDevice()接口<br>增加 ~~notifyDevicesChange()通知上层调用getAlldevice接口~~<br>增加风扇类型 <br>空调增加左右扫风，上下扫风，除湿模式 | Luke |
| V1.2.21 | 20171118 | <br>增加摄像头,投影仪,猫眼,晾衣架,扫地机器人 <br> 更多信息请看sdk中changelog | Luke |
|        |          |                  |        |

## 概述


### JdSmart

JdSmart是声控智能家居系统，可用于控制智能家居主机及设备<br>
* 可控设备：普通灯，调光灯（可调亮度），开关，插座，窗帘(分为精确获取移动位置和不能获得移动位置两种)，空调(红外类型和非红外类型)，空气净化器，开窗器，温控器，红外转发器，电视（红外类型），机顶盒（红外类型），多功能控制盒，猫眼，摄像头，投影仪，抽风机，风扇，机械手，新风，地暖，晾衣架，扫地机<br>
* 传感器：温度传感器，湿度传感器，温度湿度一体传感器，紧急按钮，红外入侵检测传感器，门窗磁传感器，水浸传感器，可燃气体传感器，CO监测器，CO2监测器，PM2.5，烟雾传感器

### JdSmart Open Sdk

是智能家居控制系统开放接入接口，接口简单，接入方便，并提供相应的虚拟设备DEMO供参考

## 接入操作指南


1. 固件默认没有安装有JdSmart SDK DEMO，编译、安装并运行开放SDK DEMO后，必须重启一次背景音乐机器，因为我们不会实时检测应用是否已经安装背景音乐上。

2. 智能家居设备管理
* 进入 桌面  >设置>通用设置>智能家居设置>智能主机管理，会提示输入密码, 密码为admin, 然后输入智能家居账号和密码。
* 进入房间管理页面，点击右上角 + 图标，添加楼层
* 点击任意房间，进入设备管理页面，点击右上角 + 图标，添加智能设备
* 在“添加设备”页面，点击智能设备，会要求选择设备名字,选择后即可添加设备成功，可以在此页面多次添加设备。
* 房间及设备管理完成后，切换到桌面，右滑到左屏，会显示已添加的智能设备，在屏幕左边向右滑动或者点击左上角图标，可以滑出房间选择菜单

3. 你可以语音控制智能设备，支持的语音参考命令为：
* 开灯：      显示所有的灯设备，供用户选择默认设备
* 打开一楼主卧灯：如果一楼主卧只有一个灯，不管什么名字，直接打开。
* 打开主卧灯：如果默认楼层的主卧里面只有一个灯，不管什么名字，直接打开。
* 打开顶灯：  打开默认房间的顶灯

## 功能


-   支持各种智能设备开关，普通灯，调光灯，窗帘，插座，空调，空气净化器，开窗器，温控器，各种传感器等
-   获取主机信息，例如主机名称定义
-   支持登入登出智能网关主机
-   控制智能设备
-   创建，编辑，删除场景
-   创建，编辑，删除设备与场景绑定
-   控制场景
-   raw语音，直接将用户语音输出到接口函数

## 重点说明


1. 通过IJdSmartHost.java接口，JdSmartHostInfo getHostInfo() 可配置主机的能力是否支持登陆，登陆提示信息，是否支持raw语音，是否支持全部场景功能（详情见 getHostInfo函数） 
``` java 
mJdSmartHostInfo.setEnableSceneEdit(true);//默认为true <br>
//如果设为false, 则只支持获取场景名，和执行场景，仅需实现getScenes和controlScene两个接口 <br>
//如果设为true, 则支持场景所有功能，包括场景编辑，执行，场景绑定编辑,需要实现所有场景相关接口 <br>
``` 

2. 实现IJdSmartHost.java不同的接口函数，需要将JdSmartScene，JdSmartCtrlCmd，JdSmartDevice这些类转化为你的智能家居对应的类类型。例如，
  ``` java
getAllDevices(JdbaseCallback callback)接口中需要将智能家居设备转化为JdSmartDevices　
controlDevice(JdSmartCtrlCmd cmd, JdbaseCallback callback) 接口中需要将JdSmartCtrlCmd转化为智能家居设备的执行指令　
``` 

3. 为简化第三方JdSmartOpen App后续升级及设备管理，第三方App需要通过getAppId()接口设置appId，为简化appid管理，我司不分配具体appid，为保证全球唯一，建议设置为贵司的域名。例如：美的集团设置appid值为 midea.com<br>
<font color= "red" >注意每个设备仅能绑定一次，并存入我司后台服务器。</font> 　

4. 可支持bugly升级demo。 要配置升级，不支持热更新
- 请详细查看 https://bugly.qq.com/v2/ 使用帮助，配置自己的版本升级
- 修改MyApplication.java 中APP_ID为自己的APP_ID
- 修改CustomSmartService.java 中， 默认是30分检测一次升级
``` java
timer.schedule(new UpgradeTask(), 1000, 1000 * 60 * 30); //30min

```
- 修改CustomSmartService.java 中， 如下部分
``` java
    class UpgradeTask extends java.util.TimerTask{
        @Override
        public void run() {
            Log.d(TAG, "UpgradeTask is runing");
            //Beta.checkUpgrade();    将此注释取消，修改为 Beta.checkUpgrade(false, true);

```

5. 为了支持设备一键导入功能， 需要填写zoonID, roomId值， 对应函数为setZoneId()，setRoomId()
``` java
public class JdSmartDevice {
    String deviceType;
    String deviceSubType;
    String deviceId;
    String deviceId2;
    String uid;
    String deviceName;
    String zoneId; //楼层名， 强调说明，这个字段不是id，是楼层名，楼层名（只能是一楼，二楼类似这样的名字）
    String roomId; //房间名， 强调说明，这个字段不是id，是房间名
    ...
```
6. 一键导入功能的管理界面有个“重新导入”，它会首先调用CustomHost.java 中的refeshDevice()接口， 再调用getAlldevices()接口
7. ~~notifyDevicesChange~~ 回调无效


## 主要类介绍


### JdSmartAccount类

提供用户登陆智能主机使用。 setName()表示当前登陆的用户名,如果登陆成功，则赋值，如果退出登陆，则设为空字串""

### JdSmartHostInfo类

``` java
public void setSupportLogin(boolean supportLogin)  设定是否支持用户登陆
public void setLoginPrompt(String loginPrompt)     设定登陆时显示的品牌名
public void setEnableSceneEdit(boolean enable)     设定是否使能场景编辑，如果false,则仅支持获取和执行场景。如果为true,则是支持全部场景功能
```
~~public void setFilterVoiceMode(String filterVoiceMode)　设定支持raw语音的模式，如果客户想自己处理语音结果，可以设置合适值~~

### JdSmartDevice类

抽像表示一个智能设备

``` java
setDeviceId()   //唯一表示此设备
setDeviceType() //设备类型
//设备子类型　如果设备是红外设备，需要另外设置为子类型JdSmartDeviceType.DEVICE_SUB_TYPE_IR
//如果是多功能控制盒设备，需要另外设置为子类型JdSmartDeviceType.DEVICE_SUB_TYPE_CONTROL_BOX
setDeviceSubType()
//如果是红外设备，设置此设备已学过的红外键列表,键值分隔符为","
//例如，电视机已经学习了开关，静音键
//则这样调用setJdIRkeyList(JdSmartIRConstant.IR_KEY_TV_ON+"," + JdSmartIRConstant.IR_KEY_TV_OFF+"," + JdSmartIRConstant.IR_KEY_TV_MUTE +",");
setJdIRkeyList() 
setDeviceName() //设备名称
getJdSmartCtrlCmd() //会返回一个JdSmartCtrlCmd变量，表示设备当前的状态，详情见JdSmartCtrlCmd类
```

### JdSmartCtrlCmd类

通过deviceId与具体JdSmartDevice设备关联，JdSmartDevice类中同时包含JdSmartCtrlCmd类<br>
此类包含order,value1,value2,value3,value4,groupData等变量<br>
有两个含义，

1.  智能设备的命令,一般指接收到设备控制命令或场景执行命令时
2.  设备当前状态，当向UI端更新设备的最新状态时，表示当前状态。

更详细信息，见demo 代码CustomSmartHost.java 代码中函数

``` java
void controlDevice(JdSmartCtrlCmd cmd, JdbaseCallback callback)
void changeDeviceStatus(JdSmartCtrlCmd cmd, JdSmartDevice dev)
```

1.如果接收到设备控制命令或场景执行命令时，表示如下<br>
order表示命令，详情见JdSmartDeviceOrder类<br>
value1， value2， value3， value4 表示此命令所带的参数<br>
groupData 是jason字符串，如果设备的命令较复杂，用groupData一次传递传多个参数值<br>
例如：<br>
收到开灯命令：

``` java
JdSmartCtrlCmd.getOrder()将等于JdSmartDeviceOrder.ON 
JdSmartCtrlCmd.getValue1()，因为开灯命令简单不用带参数,不必关注 
```

收到设置空调到下一个模式命令：

``` java
JdSmartCtrlCmd.getOrder()将等于JdSmartDeviceOrder.NEXT
JdSmartCtrlCmd.getValue1()将等于AIRCONDITION_MODE_TYPE  
```

2.更新设备最新状态给UI，可以不必处理JdSmartCtrlCmd.order成员变量，表示如下：<br>
value1- value4 表示此设备的当前状态是什么<br>
groupData 是jason字符串，如果设备的命令较复杂，用groupData一次传递传多个值<br>
例如： 更新灯的当前状态为打开：

``` java
JdSmartCtrlCmd.setValue1("0")
```

更新空调的状态为制冷模式，风速高，左右扫风前,温度28：

``` java
JSONObject jobj = new JSONObject();
jobj.put(JdSmartDeviceOrder.AIRCONDITION_MODE_TYPE, JdSmartDeviceOrder.AIRCONDITION_MODE_COOL);
jobj.put(JdSmartDeviceOrder.AIRCONDITION_WIND_RATE_TYPE, JdSmartDeviceOrder.AIRCONDITION_WIND_RATE_LOW);
jobj.put(JdSmartDeviceOrder.AIRCONDITION_WIND_DIRECTION_TYPE, JdSmartDeviceOrder.AIRCONDITION_WIND_DIRECTION_LEFT_RIGHT);
jobj.put(JdSmartDeviceOrder.TEMPERATURE, "28"); 
airConditionCmd.setGroupData(jobj.toJSONString());
```

### JdSmartDeviceType类

包括开关，普通灯，调光灯，窗帘，插座，空调，电视，空气净化器，开窗器，温控器，多功能控制盒，各种传感器<br>
**注意：**<br>
DEVICE_TYPE_CURTAINS 表示可以精确获取移动位置的窗帘<br>
DEVICE_TYPE_CURTAINS_NO_POSITION 表示不能精确获取移动位置的窗帘

### JdSmartDeviceOrder类

-   ON,OFF,灯，开关，插座等简单设备的开与关
-   OPEN,CLOSE,MOVE\_TO\_LEVEL窗帘，空调，空气净化器，开窗器，温控器的开与关
-   MOVE\_TO\_LEVEL， PAUSE，STOP 表示移动位置，总比是100，例如并开窗帘，则是50，对于空调来说，MOVE\_TO\_LEVEL调整温度
-   NEXT 切换到设备的下一个模式。空调设备是直接切换到下一个工作模式，风速等级或风向
-   SET 设定设备的某个属性值
-   MUTE 静音正在报警的设备

### JdSmartScene类

表示场景类，有一个唯一sceneNo,标识场景
* getSceneNo()和setSceneNo() //获得和设置场景号
* getSceneId()和setSceneId() //已过时，不建议使用

### JdSmartSceneBind类

表示场景绑定，它的成员变量sceneNo与JdSmartScene类的sceneNo关联<br>
场景绑定的含义是，场景中包含哪些设备命令。<br>
例如，假设有一个回家场景，里面包含动作，当主人回家后，开客厅灯，开电视，开主卧房间空间<br>
那么将包含4条指令，List<JdSmartCtrlCmd> getCmdList()将返回4条数据命令

## 参考示例说明
### CustomSmartService.java

CustomSmartService.java是与其他Jd软件服务通信的服务类, 不需要做任何修改。<br>
首先在AndroidManifest.xml 中配置启动CustomSmartService.java服务的action名<br>

``` java
<action android|name="com.judian.service.CustomSmartService" />　//请不要更改这个，因为SDK端依赖这个
```

``` java
 <service
     android|name=".service.CustomSmartService"
     android|exported="true">
     <intent-filter>
        <action android|name="com.judian.service.CustomSmartService" />
     </intent-filter>
 </service>
```

### CustomSmartHost.java

-   CustomSmartHost.java是最终实现的类，实现IJdSmartHost.java接口，客户根据自己的SDK实现相应的接口方法，重点在这个文件
-   控制设备接口函数是controlDevice,根据传入的cmd, 智能主机应该控制真正设备。 控制完设备后，设备状态改变，需要更新设备的状态

``` java
     
    public void controlDevice(JdSmartCtrlCmd cmd, JdbaseCallback callback) {
        Log.d(TAG, "debug cmd=" + cmd);
        if (callback != null) {
            callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
        }
        changeDeviceStatus(cmd, dev); //更新设备的状态
    }
```

IJdSmartHost.java接口

主机相关接口

``` java
 　　JdSmartHostInfo getHostInfo()      获取主机信息
 　　JdSmartAccount getAccount()　　　　　获取登陆的帐户信息
 　　void login(String name, String pwd, JdbaseCallback callback)       登陆
 　　void logout(JdbaseCallback callback)                               退出登陆
 　　void searchAndBindHost(boolean isSearch, JdbaseCallback callback)  搜索并绑定主机
 　　void unbindHost(JdbaseCallback callback)                           解除绑定　
   
```

场景相关

``` java
    void controlScene(JdSmartScene jdsmartScene, JdbaseCallback callback)　　执行场景
    void getScenes(JdbaseCallback callback)　　               　　 获得所有场景
    void createScene(JdSmartScene jdsmartScene, JdbaseCallback callback)　　 创建场景
    void deleteScene(JdSmartScene jdsmartScene, JdbaseCallback callback)　　 删除场景
    void updateScene(JdSmartScene jdsmartScene, JdbaseCallback callback)　　 修改场景
   
```

场景绑定相关

``` java
    void getSceneBind(JdSmartScene jdsmartScene, JdbaseCallback callback)     获得某个场景的场景绑定
    void createSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback)  创建场景绑定
    void deleteSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback)  删除场景绑定
    void updateSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback)  修改场景绑定
   
```

设备相关

``` java
    void refeshDevice()                            自动导入接口中，重新导入时，会首先调用这个接口 
    void controlDevice(JdSmartCtrlCmd cmd, JdbaseCallback callback)            执行设备指令
    void getAllDevices(JdbaseCallback callback)                                获得所有设备信息
    void getDeviceDetail(final String deviceID, final JdbaseCallback callback) 获得指定设备信息
    JdSmartDevices getDevicesByType(int deviceType)                获得同类型所有设备
    void registerDeviceChange(JdbaseCallback callback)                 更新设备状态
    void getSensorRecord(String deviceid, int pageIndex, int pageSize, JdbaseCallback callback)　获得sensor类型设备的存储记录
　　
```

``` java
public interface IJdSmartHost {
    void init(Context context);

    JdSmartHostInfo getHostInfo();

    void login(String name, String pwd, JdbaseCallback callback);

    JdSmartAccount getAccount();

    void logout(JdbaseCallback callback);

    void searchAndBindHost(boolean isSearch, JdbaseCallback callback);

    void unbindHost(JdbaseCallback callback);

    void getScenes(JdbaseCallback callback);

    void createScene(JdSmartScene jdSmartScene, JdbaseCallback callback);

    void deleteScene(JdSmartScene jdSmartScene, JdbaseCallback callback);

    void updateScene(JdSmartScene jdSmartScene, JdbaseCallback callback);

    void createSceneBind(List<JdSmartCtrlCmd> cmdList, JdbaseCallback callback);

    void deleteSceneBind(List<JdSmartCtrlCmd> cmdList, JdbaseCallback callback);

    void updateSceneBind(List<JdSmartCtrlCmd> cmdList, JdbaseCallback callback);

    void getSceneBind(JdSmartScene jdSmartScene, JdbaseCallback callback);

    void getAllDevices(JdbaseCallback callback);

    void getAllDeviceType(JdbaseCallback callback);

    void getDeviceDetail(String deviceID, JdbaseCallback callback);

    JdSmartDevices getDevicesByType(int type);

    void controlDevice(JdSmartCtrlCmd jdSmartCtrlCmd, JdbaseCallback callback);

    void controlScene(JdSmartScene jdSmartScene, JdbaseCallback callback);

    void getSensorRecord(String deviceid, int pageIndex, int pageSize, JdbaseCallback callback);

    void registerDeviceChange(JdbaseCallback callback);
}
```

### void init(Context context)

当程序运行后，将调用init,可将初始化代码搜索主机,第二次登陆主机(第一次成功登陆,并保存用户名,再次开机后登陆)的代码写此处
当成功登陆主机后,为了减少网络延时,建议立即获取设备列表

``` java
  /**
     * 初始化部分，当程序运行后，将调用
     *
     * @param context android上下文
     */
    void init(Context context);
```

### JdSmartHostInfo getHostInfo()

返回一个JdSmartHostInfo，表示主机支持的功能
mJdSmartHostInfo = new JdSmartHostInfo();
mJdSmartHostInfo.setSupportLogin(false); //是否支持用户登陆认证，如果支持则写true, 否则false
mJdSmartHostInfo.setLoginPrompt(“Login”);//登陆提示信息，如果支持登陆，则生效。建议写品牌名
mJdSmartHostInfo.setEnableSceneEdit(true);//默认为true
//如果设为false, 则只支持获取场景名，和执行场景，仅需实现getScenes和controlScene两个接口
//如果设为true, 则支持场景所有功能，包括场景编辑，执行，场景绑定编辑,需要实现所有场景相关接口<br>
~~mJdSmartHostInfo.setFilterVoiceMode(JdSmartConstant.FILTER_VOICE_MODE_NONE);//默认为true~~
~~//如果设为FILTER_VOICE_MODE_NONE, 则用户不能自己处理语音，setVoiceText()接口将接收不到任何信息~~
~~//如果设为FILTER_VOICE_MODE_CONTROL, 则用户仅能处理控制设备的语音，setVoiceText()接口将接收到控制设备的文本~~
~~//如果设为FILTER_VOICE_MODE_ALL, 则用户能处理所有语音，setVoiceText()接口将接收所有信息~~

``` java
    /**
     * 获取主机信息，需要配置JdSmartHostInfo类
     *
     * @return JdSmartHostInfo 返回主机信息
     */
    JdSmartHostInfo getHostInfo();
```

### void login(String name, String pwd, JdbaseCallback callback)

支持登陆，则需要用户实现，否则直接返回成功，callback.onResult(JdbaseContant.RESULT\_SUCCESS, “success”, "");

``` java
    /**
     * 用户认证登陆
     * <pre>
     * 例如|
     *    成功 callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     *    失败 callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     *</pre>
     * @param name     用户名
     * @param pwd      密码
     * @param callback 回调登陆结果
     */
    void login(String name, String pwd, JdbaseCallback callback);
```

### JdSmartAccount getAccount()

返回JdSmartAccount信息
如果不支持登陆，则JdSmartAccount.name 应该设为““
如果未登陆，则JdSmartAccount.name 应该设为””,否则设为当前登陆的用户名，代表已登陆

``` java
    /**
     * 获得已登陆的账户信息
     *
     * @return 如果未登陆，则JdSmartAccount.name 应该设为"",否则设为当前登陆的用户名，代表已登陆
     */
    JdSmartAccount getAccount();
```

### void logout(JdbaseCallback callback)

支持登陆，则需要用户实现，否则直接返回成功，callback.onResult(JdbaseContant.RESULT\_SUCCESS, “success”, "");

``` java

    /**
     * 退出登陆
     *<pre>
     *例如|
     *    成功 callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     *    失败 callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     *</pre>
     * @param callback 回调退出信息
     *
     */
    void logout(JdbaseCallback callback);
```

### void searchAndBindHost(boolean isSearch, JdbaseCallback callback)

一般不用实现，请不要改动
支持搜索主机时，实现此接口，如果不支持，直接返回成功，callback.onResult(JdbaseContant.RESULT\_SUCCESS, “success”, "");

``` java
    /**
     * 搜索，并绑定主机网关
     * <pre>
     *例如|
     *    callback.onResult(JdbaseContant.RESULT_SUCCESS, "网关信息", "");
     * </pre>
     * @param isSearch 是否动态搜索
     * @param callback 回调搜索结果
     */
    void searchAndBindHost(boolean isSearch, JdbaseCallback callback);
```

### void unbindHost(JdbaseCallback callback)

从主机网关解除所有设备与场景，这是危险操作，建议不实现，callback.onResult(JdbaseContant.RESULT\_SUCCESS, “success”, "");

``` java
    /**
     * 从主机网关解除所有设备与场景，危险操作
     * <pre>
     * 例如|
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     * </pre>
     * @param callback 回调结果
     */
    void unbindHost(JdbaseCallback callback);
```

### void getScenes(JdbaseCallback callback)

获取所有场景信息，场景信息通过json字符串返回。

``` java
    /**
     * 获取所有场景
     * <pre>
     * 例如|
     * JdSmartScenes jss = new JdSmartScenes();
     * for(Scenexxxx s| list){
     *      JdSmartScene js = new JdSmartScene();
     *      js.setSceneNo(s.getNo());
     *      js.setSceneId(s.getId()+"");
     *      js.setSceneName(s.getName());
     *      js.setVendor(xxxxx);
     *      jss.addScene(js);
     * }
     * callback.onResult(JdbaseContant.RESULT_SUCCESS, JSON.toJSONString(jss), "");
     * </pre>
     * @param callback 回调结果
     *
     */
    void getScenes(JdbaseCallback callback);
```

### void createScene(JdSmartScene jdsmartScene, JdbaseCallback callback)

创建一个场景，传入的仅是一个jdsmartScene，里面包含新场景名。
实现时，注意检查是否存在相同的场景名。　

``` java
    /**
     * 创建场景
     * <pre>
     * 例如|
     *     JdSmartScene js = new JdSmartScene();
     *     js.setSceneName("xxx"); //与传入的场景名一致
     *     js.setSceneNO(xxxxId() + "");
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS, JSON.toJSONString(js), "");
     * </pre>
     * @param jdsmartScene 创建的场景, 根据场景的名字screneName创建
     * @param callback     回调创建结果
     */
    void createScene(JdSmartScene jdsmartScene, JdbaseCallback callback);
```

### void deleteScene(JdSmartScene jdsmartScene, JdbaseCallback callback)

根据场景号或场景名删除一个场景

``` java
    /**
     * 删除场景
     * <pre>
     * 例如|
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     *     callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     * </pre>
     * @param jdsmartScene 删除的场景， 需要根据jdsmartScene.getSceneNo()删除场景
     * @param callback     回调删除结果
     */
    void deleteScene(JdSmartScene jdsmartScene, JdbaseCallback callback);
```

### void updateScene(JdSmartScene jdsmartScene, JdbaseCallback callback)

更改场景名字

``` java
    /**
     * 修改场景
     * <pre>
     * 例如|
     *    callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     *    callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     * </pre>
     * @param jdsmartScene 修改的场景， 需要根据jdsmartScene.getSceneNo()删除场景
     * @param callback     回调修改的结果
     */
    void updateScene(JdSmartScene jdsmartScene, JdbaseCallback callback);
```

### void createSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback)

创建一个场景绑定。　“场景绑定”是一个名词，表示一些设备与某个场景结合在一起。
JdSmartCtrlCmd　表示一个设备的指令操作，就是将这些设备的指令操作与场景结合在一起。

``` java
    /**
     * 创建场景绑定
     * <pre>
     * 例如|
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     *     callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     * </pre>
     * @param cmds     绑定到场景的命令列表
     * @param callback 回调绑定结果
     */
    void createSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback);
```

### void deleteSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback)

删除场景绑定

``` java
    /**
     * 删除场景绑定
     * <pre>
     * 例如|
     *    callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     *    callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     * </pre>
     * @param cmds     删除的场景的命令列表
     * @param callback 回调删除结果
     */
    void deleteSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback);
```

### void updateSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback)

修改场景绑定

``` java
    /**
     * 修改场景绑定
     * <pre>
     * 例如：
     *     callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     * </pre>
     * @param cmds     修改的场景的命令列表
     * @param callback 回调删除结果
     */
    void updateSceneBind(List<JdSmartCtrlCmd> cmds, JdbaseCallback callback);
```

### void getSceneBind(JdSmartScene jdsmartScene, JdbaseCallback callback)

获得场景的所有场景绑定信息

``` java
    /**
     * 获取场景绑定
     * <pre>
     * 例如|
     *     JdSmartDevices jds = new JdSmartDevices();
     *     JdSmartDevice device1 = new JdSmartDevice();
     *     JdSmartDevice device2 = new JdSmartDevice();
     *     jds.addDevice(device1);
     *     jds.addDevice(device2);
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS, JSON.toJSONString(jds), "");
     * </pre>
     * @param jdsmartScene  场景
     * @param callback  回调结果
     */
    void getSceneBind(JdSmartScene jdsmartScene, JdbaseCallback callback);
```

### void getAllDevices(JdbaseCallback callback)

获取所有设备

``` java

    /**
     * 获取所有设备列表
     * <pre>
     * 例如|
     *    JdSmartDevices jds = new JdSmartDevices();
     *    JdSmartDevice device1 = new JdSmartDevice();
     *    JdSmartDevice device2 = new JdSmartDevice();
     *    jds.addDevice(device1);
     *    jds.addDevice(device2);
     *    callback.onResult(JdbaseContant.RESULT_SUCCESS, JSON.toJSONString(jds), "");
     * </pre>
     * @param callback 回调结果
     */
    void getAllDevices(JdbaseCallback callback);
```

### void getAllDeviceType(JdbaseCallback callback)

获得所有的设备类型值，注意是所有在线的设备类型值

``` java
    /**
     * 得到所有的设备类型值
     * <pre>
     * 例如|
     *    List<Integer> result = new ArrayList<Integer>();
     *    JdSmartDevice device1 = new JdSmartDevice();
     *    JdSmartDevice device2 = new JdSmartDevice();
     *    result.add(device1.getDeviceType());
     *    result.add(device2.getDeviceType());
     *    callback.onResult(JdbaseContant.RESULT_SUCCESS, JSON.toJSONString(result), "");
     * </pre>
     * @param callback 回调类型列表
     */
    void getAllDeviceType(JdbaseCallback callback);
```

### JdSmartDevices getDevicesByType(int deviceType)

获取同类型的所有设备

``` java
    /**
     * 获取同类型的所有设备
     * @param deviceType 设备类型值, 来自于JdSmartDevice.getDeviceType()
     * @return JdSmartDevices 直接返回相同类型所有设备
     */
    JdSmartDevices getDevicesByType(int deviceType);
```

### void getDeviceDetail(String deviceID, JdbaseCallback callback)

获取某个指定devId的设备

``` java
    /**
     * 获取某个指定devId的设备
     * <pre>
     * 例如|
     *     callback.onResult(JdSmartConstant.RESULT_SUCCESS, JSON.toJSONString(jdSmartDevice), "")
     * </pre>
     * @param deviceID 设备的devId, 或者 JdSmartDevice的json字符串
     * @param callback 回调结果
     */
    void getDeviceDetail(String deviceID, JdbaseCallback callback);
```

### void controlDevice(JdSmartCtrlCmd cmd, JdbaseCallback callback)

控制设备，cmd里order表示要执行的操作指令,  value1, value2, value3, value4表示命令参数

``` java
    /**
     * 控制设备
     * <pre>
     * 例如|
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     *     callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     * </pre>
     * @param cmd      设备命令
     * @param callback 回调控制结果
     */
    void controlDevice(JdSmartCtrlCmd cmd, JdbaseCallback callback);
```

### void controlScene(JdSmartScene jdsmartScene, JdbaseCallback callback)

控制场景

``` java
    /**
     * 控制场景
     * <pre>
     * 例如：
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS, "success", "");
     *     callback.onResult(JdbaseContant.RESULT_FAIL, "fail", "");
     * </pre>
     * @param jdsmartScene  场景
     * @param callback     负责回调新的设备信息给上层
     */
    void controlScene(JdSmartScene jdsmartScene, JdbaseCallback callback);
```

### void getSensorRecord(String deviceid, int pageIndex, int pageSize, JdbaseCallback callback)

获取感应器记录的内容，因为内容可能很多，所以根据页索引，页大小返回数据

``` java
    /**
     * 获取感应器记录的内容
     * <pre>
     * 例如：
     *     List<JdSmartSensorRecordGroup> result = new ArrayList<JdSmartSensorRecordGroup>();
     *     JdSmartSensorRecord record1 = new JdSmartSensorRecord();
     *     JdSmartSensorRecord record2 = new JdSmartSensorRecord();
     *     result.add(record1);
     *     result.add(record2);
     *     callback.onResult(JdbaseContant.RESULT_SUCCESS,  JSON.toJSONString(result), "");
     * </pre>
     * @param deviceid  设备ID
     * @param pageIndex 页码,从1开始
     * @param pageSize  每页的数量
     * @param callback  回调信息给上层
     */
    void getSensorRecord(String deviceid, int pageIndex, int pageSize, JdbaseCallback callback);
```

### void registerDeviceChange(JdbaseCallback callback)

当设备信息有变化时，通过callback上传设备信息

``` java
    /**
     * 当设备信息有变化时，应该调用回调
     * <pre>
     * 例如|
     *     callback.onResult(JdSmartConstant.ACTION_REPORT_DEVICE_STATUS, JSON.toJSONString(jdSmartDevice), "")
     * </pre>
     * @param callback 负责回调新的设备信息给上层
     */
    void registerDeviceChange(JdbaseCallback callback);
```

## 设备命令与设备状态说明

### 设备命令
开关类型设备(灯，开关，插座,电视，机顶盒，机械手)

| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | ON     |        |        |        |        |        |
| 关闭   | OFF    |        |        |        |        |        |

调光灯

| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | ON     |        |        |        |        |        |
| 关闭   | OFF    |        |        |        |        |        |
| 移动   | MOVE_TO_LEVEL    |   |亮度值,最高值255  || |        |

新风/热水器/风扇/投影仪

| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | OPEN     |        |        |        |        |        |
| 关闭   | CLOSE    |        |        |        |        |        |


多功能控制盒/无进度窗帘/开窗器

| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | OPEN     |        |        |        |        |        |
| 关闭   | CLOSE    |        |        |        |        |        |
| 暂停   | STOP    |        |        |        |        |        |

有开关和一个进度属性的设备(有进度窗帘,温控器等)

| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | OPEN     |        |        |        |        |        |
| 关闭   | CLOSE    |        |        |        |        |        |
| 移动   | MOVE_TO_LEVEL| 移动比例,例如10, 表示移动10%      |        |        |        |        |
| 停止移动| STOP    |        |        |        |        |        |

警报设备

| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 关闭警报  | MUTE     |        |        |        |        |        |

猫眼和摄像头<br>
当打开猫眼设备时，会发送intent, action为"com.judian.action.goto.cat.camera" <br>
当打开摄像头设备时，会发送intent, action为"com.judian.action.goto.camera" <br>
并且都带有扩展参数"camera_id" 包含设备的id信息<br>

| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | ON     |        |        |        |        |        |
| 关闭   | OFF    |        |        |        |        |        |

扫地机器人 


| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | OPEN   |        |        |   | |   |
| 关闭   | CLOSE   |        |        |  |  |   |
| 清扫模式|SET|SWEEPER_MACHINE_SWEEPER_MODE |   |  |   |  |
| 自动模式|SET|SWEEPER_MACHINE_AUTO_MODE|||||
| 强力模式|SET|SWEEPER_MACHINE_FORCE_MODE|| | |  |
| 静音模式|SET|SWEEPER_MACHINE_MUTE_MODE |    |  | |  |
| 标准模式|SET|SWEEPER_MACHINE_NORMAL_MODE |   |  |  |  |
| 回充模式|SET|SWEEPER_MACHINE_CHARGE_MODE |   |   | ||     

空气净化器 


| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | OPEN   |        |        |   | |   |
| 关闭   | CLOSE   |        |        |  |  |   |
| 自动模式|SET|AIRPURIFIER_ORDER_MODE_AUTO|||||
| 手动模式|SET|AIRPURIFIER_ORDER_MODE_MANUAL|| | |  |
| 休眠模式|SET|AIRPURIFIER_ORDER_MODE_SLEEP |    |  | |  |
| 设为高风|SET|AIRPURIFIER_ORDER_WIND_HIGHT |   |  |  |  |
| 设为中风|SET|AIRPURIFIER_ORDER_WIND_MID |   |   | ||     
| 设为低风|SET|AIRPURIFIER_ORDER_WIND_LOW |   |  |   |  |

地暖

| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | OPEN     |        |        |        |        |        |
| 关闭   | CLOSE    |        |        |        |        |        |
| 移动   | MOVE_TO_LEVEL| 移动比例,例如10, 表示移动10%      |  | |  | |
| 自动模式|SET|FLOOR_HEATING_ORDER_MODE_AUTO|||||
| 手动模式|SET|FLOOR_HEATING_ORDER_MODE_MANUAL|| | |  |
| 休眠模式|SET|FLOOR_HEATING_ORDER_MODE_SLEEP |    |  | |  |

         
智能晾衣架


| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | OPEN | | | | | |
| 关闭   | CLOSE|| | | | |
| 上升|SET|CLOTHES_HANGER_ORDER_MOVE_UP |||||
| 停止|SET|CLOTHES_HANGER_ORDER_MOVE_STOP|||||
| 下降|SET|CLOTHES_HANGER_ORDER_MOVE_DOWN |||||
| 开灯|SET|CLOTHES_HANGER_ORDER_LIGHT_ON || | ||
| 关灯|SET| CLOTHES_HANGER_ORDER_LIGHT_OFF ||| | |
| 烘干开|SET| CLOTHES_HANGER_ORDER_HEAT_ON |||| |
| 烘干关|SET| CLOTHES_HANGER_ORDER_HEAT_OFF| || ||      
| 风干开|SET| CLOTHES_HANGER_ORDER_WIND_ON| || ||   
| 风干关|SET| CLOTHES_HANGER_ORDER_WIND_OFF| || ||   
| 消毒开|SET| CLOTHES_HANGER_ORDER_DESINFECTION_ON ||| ||   
| 消毒关|SET| CLOTHES_HANGER_ORDER_DESINFECTION_OFF||| ||   

空调设备 


| 操作   | order  |  value1| value2 | value3 | value4 | groupData |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 打开   | OPEN   |        |        |        |        |        |
| 关闭   | CLOSE   |        |        |        |        |        |
| 调整温度|MOVE_TO_LEVEL| 温度值，例如26，  <br>表示调置为26度|        |        |        |        |
| 切换到指定工作模式|SET|AIRCONDITION_<br>MODE_TYPE|工作模式值,例如，  <br>AIRCONDITION_<br>MODE_COOL||||
| 切换到指定风速模式|SET|AIRCONDITION_<br>WIND_RATE_TYPE |风速模式，例如，  <br>AIRCONDITION_<br>WIND_RATE<br>_MIDDLE| | |  |
| 切换到指定扫风模式|SET|AIRCONDITION_<br>WIND_DIRECTION<br>_TYPE |扫风模式，例如，  <br>AIRCONDITION_WIND<br>_DIRECTION_<br>LEFT_RIGHT| | |  |
| 直接切换到下一个工作模式| NEXT   | AIRCONDITION_<br>WIND_RATE_TYPE |        |        |        |        |
| 直接切换到下一个风速模式| NEXT   | AIRCONDITION_<br>MODE_TYPE |        |        |        |        |
| 直接切换到下一个扫风模式| NEXT   | AIRCONDITION_<br>WIND_DIRECTION<br>_TYPE |      |   | ||          
         

### 设备状态
开关类型设备(灯，开关，插座,电视，机顶盒，新风，热水器，风扇，投影仪)

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
|0: 表示打开 <br>-1:表示关闭|||||


调光灯

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
|0: 表示打开 <br>-1:表示关闭|填写亮度值,最高值255||||

多功能控制盒/无进度窗帘

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
|当前移动位置(只有0和100两个进度值)         <br>0：表示关闭，100：表示打开，<br>50或其它值表示中途暂停  |||||


有开关和一个进度属性的设备(有进度窗帘,有进度开窗器等)

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
|当前移动位置(0~100)<br>例如填写10, 表示移动10%, 0表示关闭,100表示打开|||||

        
门磁、窗磁、水浸

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
| 填写0表示关闭，不需要报警<br>填写1表示打开，需要报警||填写0表示低电量<br>填写1表示正常电量 | 填写电量值|   

烟感

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
| 填写0表示不报警<br>填写1表示报警 |  |   填写0表示低电量<br>填写1表示正常电量 |    填写电量值   |

人体红外

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
|填写0表示没有报警<br>填写1表示检测到入侵，需要报警  |填写1表示入侵的人一直存在<br>填写0表示没有检测到入侵持续存在|填写0表示低电量<br>填写1表示正常电量| 填写电量值   |

温度或湿度传感器

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
|填写温度,乘以100值|  填写湿度值,乘以100   | |填写电量值（-1表示此设备不是电池供电的设备）  |

照度

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:----: |
|填写传感器上报的测量值MeasuredValue |||     填写电量值   |


扫地机器人

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:---- |
| 填写0表示打开 <br>填写-1表示关闭 |||| json 字符串，键值分别为工作状态值<br>JSONObject jobj = new JSONObject() <br>//工作于清扫模式 <br>jobj.put(JdSmartDeviceOrder.SWEEPER_MACHINE_STATUS, JdSmartDeviceOrder.SWEEPER_MACHINE_SWEEPER_MODE) <br>|

空气净化器

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:---- |
| 填写0表示打开 <br>填写-1表示关闭 |||| json 字符串，键值分别为工作模式，风速等级，PM25，温度，湿度，空气质量等级<br>JSONObject jobj = new JSONObject() <br>//工作模式 <br>jobj.put(JdSmartDeviceOrder.AIRPURIFIER_STATUS_MODE, JdSmartDeviceOrder.AIRPURIFIER_ORDER_MODE_AUTO) <br>//自动工作模式 <br>jobj.put(JdSmartDeviceOrder.AIRPURIFIER_STATUS_WIND, <br>JdSmartDeviceOrder.AIRPURIFIER_ORDER_WIND_MID)<br>//中风 <br>jobj.put(JdSmartDeviceOrder.AIRPURIFIER_STATUS_PM25,"50") <br>//PM2.5 50 <br>jobj.put(JdSmartDeviceOrder.AIRPURIFIER_STATUS_TEMP, "26")<br>//温度26度 <br>jobj.put(JdSmartDeviceOrder.AIRPURIFIER_STATUS_HUMIDITY,"60")//湿度60<br>jobj.put(JdSmartDeviceOrder.AIRPURIFIER_STATUS_AQI,"一级（优）")//空气质量等级 |

地暖

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:---- |
| 填写0表示打开 <br>填写-1表示关闭 |||| json 字符串，键值分别为工作模式，当前温度<br>JSONObject jobj = new JSONObject() <br>jobj.put(JdSmartDeviceOrder.FLOOR_HEATING_STATUS_MODE, JdSmartDeviceOrder.FLOOR_HEATING_ORDER_MODE_AUTO) //工作于自动模式 <br>jobj.put(JdSmartDeviceOrder.FLOOR_HEATING_STATUS_TEMP, "26")//当前温度26 <br>|

智能晾衣架

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:---- |
| 填写0表示打开 <br>填写-1表示关闭 |||| json 字符串，键值分别为移动状态，消毒状态，烘干状态，灯状态，风状态<br>JSONObject jobj = new JSONObject() <br>//移动状态 <br>jobj.put(JdSmartDeviceOrder.CLOTHES_HANGER_STATUS_MOVE, JdSmartDeviceOrder.CLOTHES_HANGER_ORDER_MOVE_STOP) <br>//消毒状态 <br>jobj.put(JdSmartDeviceOrder.CLOTHES_HANGER_STATUS_DESINFECTION, <br>JdSmartDeviceOrder.CLOTHES_HANGER_ORDER_DESINFECTION_OFF)<br>//烘干状态 <br>jobj.put(JdSmartDeviceOrder.CLOTHES_HANGER_STATUS_HEAT, <br>JdSmartDeviceOrder.CLOTHES_HANGER_ORDER_HEAT_OFF) <br>//灯状态 <br>jobj.put(JdSmartDeviceOrder.CLOTHES_HANGER_STATUS_LIGHT, <br>JdSmartDeviceOrder.CLOTHES_HANGER_ORDER_LIGHT_OFF)<br>//风状态 <br>jobj.put(JdSmartDeviceOrder.CLOTHES_HANGER_STATUS_WIND, <br>JdSmartDeviceOrder.CLOTHES_HANGER_ORDER_WIND_OFF)|

空调设备

| value1| value2| value3| value4| groupData|
|:----:| :----:|:----:|:----: |:---- |
| 填写0表示打开 <br>填写-1表示关闭 |||| json 字符串，键值分别为温度，空调模式，风速，风向<br>JSONObject jobj = new JSONObject() <br>//空调温度 <br>jobj.put(JdSmartDeviceOrder.TEMPERATURE, "26") <br>//空调模式 <br>jobj.put(JdSmartDeviceOrder.AIRCONDITION_MODE_TYPE, <br>JdSmartDeviceOrder.AIRCONDITION_MODE_COOL)<br>//空调风速 <br>jobj.put(JdSmartDeviceOrder.AIRCONDITION_WIND_RATE_TYPE, <br>JdSmartDeviceOrder.AIRCONDITION_WIND_RATE_AUTO) <br>//空调风向 <br>jobj.put(JdSmartDeviceOrder.AIRCONDITION_WIND_DIRECTION_TYPE, <br>JdSmartDeviceOrder.AIRCONDITION_WIND_DIRECTION__UP_DOWN)|


### 普通灯，开关，插座

命令

``` java
cmd.setOrder(JdSmartDeviceOrder.ON),  value1不用设置
```

状态：

``` java
cmd.value1("0") //表示状态是开
```

### 调光灯

命令

``` java
cmd.setOrder(JdSmartDeviceOrder.ON) //ON开,OFF关
```

``` java
cmd.setOrder(JdSmartDeviceOrder.MOVE_TO_LEVEL) //亮度比例
cmd.setValue2("30") //最高亮度255
```

状态

``` java
cmd.value1("0")  // value1表示开关状态 "0"表示状态打开,否则为关闭状态
cmd.value2("30") // value2表示亮度,最高亮度255
```

### 窗帘/开窗器/温控器

命令

``` java
cmd.setOrder(JdSmartDeviceOrder.OPEN) //OPEN开,CLOSE关, STOP暂停移动
```

``` java
cmd.setOrder(JdSmartDeviceOrder.MOVE_TO_LEVEL) //移动比例
cmd.setValue1("30") //百分比为30%
```

状态

``` java
cmd.value1("10") //表示状态是10%,如果是100，则设备是全开状态
```

### 空调

命令

-   打开空调

``` java
cmd.setOrder(JdSmartDeviceOrder.OPEN)
```

-   切换空调到下一个工作模式

``` java
cmd.setOrder(JdSmartDeviceOrder.NEXT)
cmd.setValue1(JdSmartDeviceOrder.AIRCONDITION_MODE_TYPE)
```

-   设置制冷模式

``` java
cmd.setOrder(JdSmartDeviceOrder.SET)
cmd.setValue1(JdSmartDeviceOrder.AIRCONDITION_MODE_TYPE)
cmd.setVvalue2(JdSmartDeviceOrder.AIRCONDITION_MODE_COOL)
```

-   设置中风

``` java
cmd.setOrder(JdSmartDeviceOrder.SET)
cmd.setValue1(JdSmartDeviceOrder.AIRCONDITION_WIND_RATE_TYPE)
cmd.setValue2(JdSmartDeviceOrder.AIRCONDITION_WIND_RATE_MIDDLE)
```

-   设置温度26度

``` java
cmd.setOrder(JdSmartDeviceOrder.MOVE_TO_LEVEL)
cmd.setValue1("26")
```

状态

``` java
JSONObject jobj = new JSONObject();
jobj.put(JdSmartDeviceOrder.AIRCONDITION_MODE_TYPE, JdSmartDeviceOrder.AIRCONDITION_MODE_COOL);
jobj.put(JdSmartDeviceOrder.AIRCONDITION_WIND_RATE_TYPE, JdSmartDeviceOrder.AIRCONDITION_WIND_RATE_LOW);
jobj.put(JdSmartDeviceOrder.AIRCONDITION_WIND_DIRECTION_TYPE, JdSmartDeviceOrder.AIRCONDITION_WIND_DIRECTION_LEFT_RIGHT);
jobj.put(JdSmartDeviceOrder.TEMPERATURE, "28"); 
cmd.setGroupData(jobj.toJSONString());
```

### Sensor传感器

命令

-   关闭正在发声的警报

``` java
cmd.setOrder(JdSmartDeviceOrder.MUTE)
```

状态

 
| 类别       | 参数   | 描述 |                                              
|-----------|-----------|-----------|
|门磁、窗磁、水浸 | value1 | 填写0表示关闭，不需要报警，填写1表示打开，需要报警
| |value3 | 填写0表示低电量，填写1表示正常电量
| |value4| 填写电量值
|烟感/可燃气体  |value1| 填写0表示不报警，填写1表示报警 
| |value3| 填写0表示低电量，填写1表示正常电量
| |value4| 填写电量值
|人体红外/SOS  |value1| 填写0表示没有报警，填写1表示检测到入侵，需要报警 
| |value2| 填写1表示入侵的人一直存在，填写0表示没有检测到入侵持续存在
| |value3| 填写0表示低电量，填写1表示正常电量；
| |value4 |填写电量值
|温度传感器 |value1| 填写温度值 ,例如正常温度值是26.331, 需要写成2633.1(乘以100)
| |value4| 填写电量值（-1表示此设备不是电池供电的设备）
|湿度传感器 |value1| 填写湿度值 ,例如正常湿度值是30.212, 需要写成3021.2(乘以100)
| |value4| 填写电量值（-1表示此设备不是电池供电的设备）
|温湿度传感器 |value1| 填写温度值  ,例如正常温度值是26.331, 需要写成2633.1(乘以100)
| |value2| 填写湿度值 ,例如正常湿度值是30.212, 需要写成3021.2(乘以100)
| |value4| 填写电量值（-1表示此设备不是电池供电的设备）
|照度 |value1| 填写传感器上报的测量值Measured value
| |value4| 填写电量值
|PM2.5 / 二氧化碳 |value1| 当前测量值 
|一氧化碳 |value1| 填写0表示关闭，不需要报警，填写1表示打开，需要报警





