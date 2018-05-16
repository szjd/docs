修订记录
--------

| 版本   | 日期     | 修改记录         | 修改人 |
|--------|----------|------------------|--------|
| V1.0.0 | 20160819 | 初始版本         | yeslee |
| V1.1.0 | 20161117 | 开放音乐资源请求 | yeslee |
|        |          |                  |        |

概述
----

JdPlay局域网媒体控制协议，可用于局域网内媒体发现和媒体控制。第三方可以采用两种方式接入

1.  基于开放Sample接入：**推荐方式**, 接入简单，10分钟可以接入完成
2.  基于开放SDK接口接入：采用MVP模式开发

公共接入配置
------------

### 添加包引用

1.  jdplay开放sdk jdplay-x.x.xx.aar包依赖fastjson库，需要将jdplay和fastjson库添加到你的工程，如果你的工程也引用了fastjson库，可以使用其中一个即可。
2.  在Android Studio添加本地jdplay-x.x.xx.aar包引用。参考链接 <http://jingyan.baidu.com/article/2a13832890d08f074a134ff0.html>
3.  添加fastjson-1.2.20.jar 到 libs 目录
4.  添加aar到 libs 目录，在build.gradle 中参考加入:

``` xml
repositories{
    flatDir{
        dirs'libs'
    }
}
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile(name: 'jdplay-x.x.xx', ext: 'aar')
    compile files('libs/fast-json-judian.jar') 可能需要添加
}
```

### 添加权限

在AndroidManifest.xml中添加如下权限

``` xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

### 混淆配置

``` xml
#jdplay && judian-fastjson
-dontwarn com.judian.support.jdplay.**
-keep class com.judian.support.jdplay.**{*;}
-dontwarn com.judian.fastjson.**
-keep class com.judian.fastjson.** { *; }
```

基于开放Sample接入
------------------

### 类说明

| 类                        | 说明                     |
|---------------------------|--------------------------|
| JdDeviceListActivity      | 设备列表选择界面         |
| JdPlayControlActivity     | 播放控制界面             |
| JdPlayPlaylistPopupWindow | 播放列表界面             |
| JdPlayImageUtils          | 用于显示专辑图片的工具类 |
| CircleImageView           | 圆形图片类               |

### 接入步骤

1.  合并res资源到APP工程
2.  合并AndroidManifest.xml Activity定义到APP工程
3.  添加依赖库jdplay-x.x.xx.aar和fast-json-judian.jar（为避免fastjson库版本冲突，我司修改了fastjson原生包名）, 参考 **添加包引用**
4.  修改资源文件以适配你的APP主题风格
5.  在Application中初始化，参考App.java
6.  如果是运行在Android 6.0系统中，需要请求”存储“权限 

``` xml
JdPlayManager.getInstance().initialize(this);
```

基于开放SDK接口接入
-------------------

### JdPlayManager

#### getInstance

public static JdPlayManager getInstance()

获取JdPlayManager实例

**参数**

-   无

**返回值**

-   返回JdPlayManager单例。

#### initialize

public void initialize(Context context)

JdPlayManager初始化,需要在Application里面调用,参考Demo中的App.java

**参数**

-   **context** Android的ApplicationContext

**返回值**

-   初始化成功返回0，非0: 其他错误.

#### setDebugLevel

void setDebugLevel(int level)

设置调试级别

**参数**

-   **level** 调试级别 0: 必要信息打印（默认） 1:详细打印

**返回值**

-   无.

### JdDeviceListContract接口

JdDeviceListContract定义了JdPlay设备信息View层与Presenter层交互接口

``` java
public interface JdDeviceListContract {
    interface View {
        void onJdDeviceInfoChange(List<JdDeviceInfo> infos);
    }

    interface Presenter {
        void selectDevice(String uuid);
        String getSelectedDeviceUuid();
    }
}
```

#### View层接口

##### onJdDeviceInfoChange

void onJdDeviceInfoChange(List<JdDeviceInfo> infos)

当JdPlay设备状态发生改变时回调，设备状态包括设备发现、上线、离线等

**参数**

-   **infos** JdPlay设备信息列表

**返回值**

-   无。

#### Presenter层接口

##### selectDevice

void selectDevice(String uuid)

选中JdPlay设备为当前操作的设备，因为局域网内可能存在多个JdPlay设备

**参数**

-   **uuid** JdPlay设备ID

**返回值**

-   无。

##### getSelectedDeviceUuid

String getSelectedDeviceUuid()

获取当前选中JdPlay设备的id

**参数**

-   无。

**返回值**

-   JdPlay设备ID

### JdPlayControlContract接口

JdPlayControlContract定义了JdPlay播放控制View层与Presenter层交互接口

``` java
public interface JdPlayControlContract {

    interface View  {
        void setVolume(int percent);
        void setPosition(int position);
        void setSeekProgress(int percent);
        void setDuration(int duration);
        void setPlayOrPause(boolean isPlay);
        void setPlayMode(String mode);
        void setSongName(String name);
        void setSingerName(String name);
        void setAlbumPic(String url);
        void setPlaylist(List<JdSong> songs);
        void setPlaylistPosition(int position);
    }

    interface Presenter {
        void togglePlay();
        void next();
        void prev();
        void changePlayMode();
        void setVolume(int percent);
        void seekTo(int percent);
        void getPlayList();
        void playPlaylistWithPos(int pos);
    }
}
```

#### View层接口

##### setVolume

void setVolume(int percent)

设置音量百分比

**参数**

-   **percent** 0-100 音量百分比

**返回值**

-   无。

##### setPosition

void setPosition(int position)

设置当前播放时间

**参数**

-   **position** 时间，单位秒

**返回值**

-   无。

##### setDuration

void setDuration(int duration)

设置总时间

**参数**

-   **duration** 时间，单位秒

**返回值**

-   无。

##### setPlayOrPause

void setPlayOrPause(boolean isPlay)

设置播放或暂停状态

**参数**

-   **isPlay** 0:暂停 1:播放

**返回值**

-   无。

##### setPlayMode

void setPlayMode(String mode)

设置播放模式

**参数**

-   **mode** 播放模式

| mode        | 说明     |
|-------------|----------|
| REPEAT\_ONE | 单曲循环 |
| SHUFFLE     | 随机播放 |
| REPEAT\_ALL | 全部循环 |

**返回值**

-   无。

##### setSongName

void setSongName(String name)

设置歌曲名称

**参数**

-   **name** 歌曲名称

**返回值**

-   无。

##### setSingerName

void setSingerName(String name)

设置歌手名称

**参数**

-   **name** 歌手名称

**返回值**

-   无。

##### setAlbumPic

void setAlbumPic(String url)

设置专辑图片链接

**参数**

-   **url** 专辑图片链接

**返回值**

-   无。

##### setPlaylist

void setPlaylist(List<JdSong> songs)

设置播放列表

**参数**

-   **songs** 播放列表

**返回值**

-   无。

##### setPlaylistPosition

void setPlaylistPosition(int position)

设置播放列表的当前播放歌曲的位置

**参数**

-   **position** 位置

**返回值**

-   无。

#### Presenter层接口

##### togglePlay

void togglePlay()

播放暂停切换

**参数**

-   无。

**返回值**

-   无。

##### next

void next()

下一首

**参数**

-   无。

**返回值**

-   无。

##### prev

void prev()

上一首

**参数**

-   无。

**返回值**

-   无。

##### changePlayMode

void changePlayMode()

改变播放模式

**参数**

-   无。

**返回值**

-   无。

##### setVolume

setVolume(int percent)

设置音量

**参数**

-   **percent** 0-100 音量百分比

**返回值**

-   无。

##### seekTo

void seekTo(int percent)

跳转

**参数**

-   **percent** 跳转百分比

**返回值**

-   无。

##### getPlayList

void getPlayList()

获取播放列表

**参数**

-   无。

**返回值**

-   无。

##### playPlaylistWithPos

void playPlaylistWithPos(int pos)

播放播放列表中点击的歌曲

**参数**

-   **pos** 选中的歌曲位置

**返回值**

-   无。
