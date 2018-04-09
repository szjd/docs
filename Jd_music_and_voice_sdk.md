修订记录
--------

| 版本   | 日期     | 修改记录         | 修改人 |
|--------|----------|------------------|--------|
| V1.0.0 | 20160819 | 初始版本         | yeslee |
| V1.1.0 | 20161117 | 开放音乐资源请求 | yeslee |
|        |          |                  |        |

概述
----
SDK包含以下功能模块
1. 获取主机在线音乐内容
2. 获取主机推荐歌单
3. 获取主机本地音乐和扫描本地场景文件夹
4. 语音合成和拦截语义自行处理
5. 获取主机接受的485数据与主机发送485数据
6. 主机模式切换

集成可以根据自己的需求选择功能

公共接入配置
------------
### 下载demo 
百度网盘下载链接:https://pan.baidu.com/s/1mhQr86W 密码: nxkj     <br>
路径：JdRelease/jd_music_and_voice_open_sdk <br>
demo需安装并运行于背景音乐主机中

### 导入核心包
将demo内部两个核心包导入：jdbase-release.aar 和 jdmusic-release-X.X.XX.aar 导入项目 
``` xml
repositories{
    flatDir{
        dirs'libs'
    }
}
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile(name: 'jdbase-release', ext: 'aar')
    compile(name: 'jdmusic-release-X.X.XX', ext: 'aar')
}
```

### 添加权限
在AndroidManifest.xml中添加如下权限

``` xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

### 初始化
``` java
public class App extends Application{
    @Override
    public void onCreate() {
        super.onCreate();
        JdPlayPresenter.getInstance().init(this);
    }
}
```

基于开放SDK接入
------------------

### 关键类说明

| 类                        | 说明                     |
|---------------------------|--------------------------|
| JdOnlineMusicPresenter      | 获取主机在线音乐         |
| JdRecommendMusicPresenter     | 获取主机推荐音乐             |
| JdPlayPresenter | 播放控制与播放状态监听             |
| JdLocalMusicPresenter          | 获取主机本地歌曲列表，扫描场景歌曲文件夹 |
| JdVoicePresenter           | 语音合成和语义识别               |
| TTS           | 语音合成实体类               |


### 关键类接口使用说明

#### TTS
``` java 
public class TTS {
    //不操作
    public static final int TYPE_NONE = 0;
    //进入识别
    public static final int TYPE_ASR = 1;
    //进入唤醒
    public static final int TYPE_WAKE_UP = 2;
    //TTS 内容
    private String tts;
    //是否打断当前播放的
    private boolean canInterrupt;
    //TTS类型
    private int ttsType;
}
```

#### JdOnlineMusicPresenter
##### 回调函数 
``` java
public interface IOnlineCallBack {
    void setMusicResource(List<?> var1, boolean var2, boolean var3);
    void onOperationFail(int var1, String var2);
}

void setMusicResource(List<?> var1, boolean var2, boolean var3)
参数
* var1 歌单类型实体类
* var2 是否为最后一级目录歌单
* var3 是否能够加载更多

void onOperationFail(int var1, String var2)
参数
* var1 错误类型
* var2 错误提示
```

##### 主要方法
``` java
int getMusicResource(BCategory category)
获取下一级歌单 
参数
* category 如果第一级 参数传 null 后面传请求返回的实体类

int getMusicResourceMore()
返回参数表示有下一级列表时调用
```

#### JdRecommendMusicPresenter
##### 回调函数
``` java
public interface IRecommendCallBack {
    void onQuerySceneMusic(List<JDMusicChannel> var1);
}

void onQuerySceneMusic(List<JDMusicChannel> var1)
参数
* var1 主机推荐音乐集合
``` 

##### 主要方法 
``` java
void querySceneMusic()
 查询主机推荐音乐集合
 ``` 

#### JdPlayPresenter
##### 回调函数 
``` java
public interface IPlayCallBack {
    void onProgress(int var1, int var2);
    void onSongChange(EglSong var1);
    void onPlayStatus(int var1);
    void onListModeChange(String var1);
    void onPlaylist(List<JdSong> var1);
    void onPlaylistPosition(int var1);
}
public interface I485DataCallBack {
    void onReceiverData(String var1);
}

void onProgress(int var1, int var2)
参数
* var1 Progress
* var2 Max_Progress

void onSongChange(EglSong var1)
参数
* var1 改变后的歌曲

void onPlayStatus(int var1)
参数
* var1 播放状态

void onListModeChange(String var1)
参数
* var1 改变后的mode

void onPlaylist(List<JdSong> var1)
参数
* var1 歌曲集合

void onPlaylistPosition(int var1)
参数
* var1 播放歌曲在歌曲集合中的位置

void onReceiverData(String var1)
参数
* var1 485收到的数据

##### 主要方法
void queryMediaInfo()
 查询当前歌曲状态

void queryPlayList(final JdCallBack var1)
 获取播放列表
参数
* var1 获取播放列表成功与失败的回调

void clearPlayList(final JdCallBack var1)
 清空播放列表
参数
* var1 清空播放列表成功与失败的回调

void deletePlayListByPos(int var1, final JdCallBack var2)
 删除播放列表指定位置的歌曲
参数
* var1 删除播放列表指定位置
* var2 删除播放列表指定位置的歌曲成功与失败的回调

void editPlaylist(final String var1, final JdCallBack var2)
* 编辑播放列表歌曲
参数
* var1 编辑播放列表歌曲指定位置
* var2 编辑播放列表歌曲成功与失败的回调

void refreshListAndPos(JdPlayList var1)
* 刷新播放列表和播放位置
参数
* var1 播放列表

void clearMediaData()
* 清空数据

void updatePlaylistPosition(EglSong var1)
* 更新播放位置
参数
* var1 歌曲

void togglePlay()
* 播放状态切换

void play()
* 播放

void pause()
* 暂停

void next()
* 下一首

void prev()
* 上一首

void switchListMode()
* 切换播放mode

String queryListMode()
* 查询播放mode

void seekTo(long var1)
* 改变播放进度
参数
* var1 进度值

void release()
* 释放资源

void playLocalSceneMusic(int var1, int var2)
* 根据位置编号播放
参数
* var1 编号位置
* var2 歌曲位置

void playLocalSceneMusic(String var1, String var2)
* 根据文件标识码播放 md5
参数
* var1 文件标识码
* var2 歌曲md5

void playSceneMusic(List<JDMusicChannel> var1, String var2)
* 播放场景推荐列表指定歌曲
参数
* var1 场景推荐列表集合
* var2 场景id

void playSceneMusic(JDMusicChannel var1)
* 播放推荐歌单
参数
* var1 推荐歌单

void playMusic(EglSong var1)
* 播放单首音乐
参数
* var1 歌曲

void playMusicByIndex(List<EglSong> var1, int var2)
* 播放播放列表指定位置歌曲
参数
* var1 播放列表
* var2 播放列表指定位置

void playOnlineMusic(BCategory var1)
* 播放在线音乐
参数
* var1 播放歌曲类型

void playSongByIndex(int var1)
* 播放指定位置歌曲
参数
* var1 指定位置

void playTTS(String var1, boolean var2)  此方法已过时
* 播放TTS 
参数
* var1 播放文本
* var2 是否打断当前播放的TTS

void playTTS(String var1) 此方法已过时
* 播放TTS 
参数
* var1 播放文本

void playTTS(TTS var1)
* 播放TTS
参数
* var1 播放TTS 类型数据

void send485Data(String var1)
* 发送485控制指令
参数
* var1 485控制指令

void set485ReceiverListener(I485DataCallBack callBack) 
* 设置485数据接受回调
参数
* callBack I485DataCallBack  回调接口

void enableSmartDeviceGrammar(boolean var1)
* 禁用本地语法
参数
* var1是否禁用

void switchDeviceMode(String var1)
* 切换模式接口
参数
* var1模式
``` 

#### JdLocalMusicPresenter
##### 回调函数
``` java
public interface ILocalMusicCallBack {
    void onGetSdSongsSuccess(List<EglSong> var1);
    void onQueryLocalSceneMusic(List<LocalSceneMusic> var1);
}

void onGetSdSongsSuccess(List<EglSong> var1)
参数
* var1 Sd歌曲集合


void onQueryLocalSceneMusic(List<LocalSceneMusic> var1)
参数
* var1 本地场景歌曲集合

##### 主要方法
void querySdCardSongs()
* 查询sdcard歌曲

void queryLocalSceneMusic()
* 查询本地场景歌曲
``` 

#### JdVoicePresenter
##### 回调函数
``` java
public interface IVoiceCallBack {
    boolean onAsrResult(String var1, VoiceExeBack var2);
    void onVoiceState(int var1);
    interface VoiceExeBack{
        @Deprecated
        void onSuccess(String var1);
        @Deprecated
        void onFail(String var1);
        void onSuccess(TTS var1);
        void onFail(TTS var1);
    }
}

boolean onAsrResult(String var1, VoiceExeBack var2)
参数
* var1 识别内容
* var2 拦截语义处理成功或者失败后回调 (处理完成后通过 VoiceExeBack 的onSuccess(TTS tts) 或者 onFail(TTS tts) 返回自行处理需要播放的TTS)
返回值
* true 拦截语义自己处理 
* false 不拦截交给主机处理

void onVoiceState(int var1)
参数
* var1 voice_state 状态回调 状态对应如下：
    //语音对话开始
    public static final int VOICE_STATE_SESSION_START = 10001;
    //识别开始录音
    public static final int VOICE_STATE_ASR_START_SPEECH = 10003;
    //识别录音结束
    public static final int VOICE_STATE_ASR_END_SPEECH = 10004;
    //开始语义识别
    public static final int VOICE_STATE_ASR_RECOGNITION = 10005;
    //语音对话结束
    public static final int VOICE_STATE_SESSION_END = 10008;
    //TTS 播放开始
    public static final int VOICE_STATE_TTS_START = 10006;
    //TTS 播放结束
    public static final int VOICE_STATE_TTS_FINISH = 10007;
``` 

##### 主要方法
``` java
void setIVoiceCallBack(IVoiceCallBack callBack)
* 设置语音处理回调
参数
* callBack 回调方法
``` 







