## 5856 
Update page - http://bbs.rdamicro.com/forum.php?mod=viewthread&tid=151&extra=page%3D2&page=3

### 5856Q32_codec_release_20171221.rar

主要改动：
1. 解决iphone配网失败问题；
2. 解决播放部分音频无响应问题；
3. 增加部分音频资源；
4. 增加设置EQ mode指令: AT+CSETEQ=<eq_mode>  
eq_mode参数如下：  
typedef enum  
{  
    NORMAL,   /*EQ=0*/  
    BASS,     /*EQ=1*/  
    DANCE,    /*EQ=2*/  
    CLASSICAL,/*EQ=3*/  
    TREBLE,   /*EQ=4*/  
    PARTY,    /*EQ=5*/  
    POP,      /*EQ=6*/  
    ROCK,     /*EQ=7*/  
    AUDIO_EQ_NUM  
} AUDIO_EQ;  


### 5856Q32_codec_release_20171031.tar.gz
主要改动：
1.增加了音乐播放出错时的异常处理
2.解决了长时间播放死机问题
3.修复了软件升级及bt bug

### 5856Q32_codec_release_flash.lod 和amr/MP3编码testcase
主要改动：
1.优化recorder流程
2.增加了amr及mp3编码流程，amr支持8k，MP3支持32k/44.1K/48K


### 5856Q32_codec_release_20170907.rar
<该版本兼容UNO_91H开发板>

主要改动：
1. 优化开机速度和模式切换速度；
2. 改进音频播放流程；
3. 完善HFP相关AT指令，HFP指令使用说明：
1）接听：AT+BTACCEPTCALL
2）拒接：AT+BTREJECTCALL
3）挂断：AT+BTTERMINALCALL
4）重拨：AT+BTCALL=7        // 拨通话记录中最后电话
5）拨出：AT+BTCALL=5,phone number        // 拨出指定电话
6）获取call status：AT+BTGETCALLSTATUS  // 返回：\r\n<call_status>\r\n\r\nOK\r\n
call_status value：
#define HFP_CALL_STATUS_NONE             0x0000        // idle状态
#define HFP_CALL_STATUS_INCOMING         0x0100        // 来电
#define HFP_CALL_STATUS_OUTGOING         0x0200        // 拨出电话对方未响铃前
#define HFP_CALL_STATUS_ALERT            0x0300        // 拨出电话对方响铃
#define HFP_CALL_STATUS_ACTIVE           0x0001        // 正在通话中
Note：多链接时可通过BT Addr指定要控制的BT设备，不指定则使用最后连接状态的bt addr。

4. 更新部分音频资源，去掉开机铃声等；
5. 优化蓝牙模式、播放内部音频和LINEIN模式下的音量调节机制；
6. 解决开机启动、模式切换等概率死机问题；
7. 增加通过5981 uart升级5856程序功能；
8. 增加外部音频PA控制逻辑；
9. 解决部分WAV文件不能播放问题；
10.更改部分配置以兼容UNO_91H开发板。


音频校准版本LOD（仅用于音频校准）：
5856Q32_codec_release_flash_20170729_aud_calib.lod


### 5856Q32_codec_release_flash_20170714.lod

主要改动：
1. 增加音频格式AMR的支持
2. 增加音频Fade In/Out效果
3. 解决部分小文件音频无法播放问题
4. 解决写BT MAC地址概率失败问题


### 5856Q32_codec_release_flash_20170703.lod

主要改动：
1. BLE相关需求完善
2. 解决部分MP3前面无法播出问题
3. 支持LINEIN，FM模式，切换命令：
   AT+CMODE=<MODE>  // MODE: LINEIN, FM


5856Q32_codec_release_flash_20170623.rar 

5856主要改动：
1. AT指令扩展：
1）设置/获取BT Name
AT+BTSETLNAME=<btname>  // btname为字符串
如设置BT Name为RDABT："AT+BTSETLNAME=RDABT\r\n"
获取BT Name：AT+BTSETLNAME?  // 返回\r\n<btname>\r\n\r\nOK\r\n

2）获取当前模式命令：AT+CMODE?
返回：\r\n<mode_name>\r\n\r\nOK\r\n                 // mode_name和切换模式时字串一样

3）获取蓝牙状态接口：AT+BTGETSTATUS         // 返回：\r\n<bt_status>\r\n\r\nOK\r\n
bt_status转换为UINT8型对应如下状态值：
typedef enum {
    MGR_CLOSED,                            // 已关闭
    MGR_CLOSE_PENDING,      // 正在关闭
    MGR_ACTIVE_PENDING,     // 正在激活
    MGR_ACTIVED,                           // 已激活
    MGR_INQUIRY_PENDNG,    // 查询状态
    MGR_CONNECTED,                    // 已连接
} app_mgr_state_t;

4）获取A2DP状态接口：AT+BTGETA2DPSTATUS          // 返回：\r\n<a2dp_status>\r\n\r\nOK\r\n
a2dp_status转换为UINT8型对应如下状态值：
typedef enum {
    A2DP_IDLE, 
    A2DP_PLAY_AUDIO,    // 播放提示音
    A2DP_CONNECTED,    // A2DP已连接
    A2DP_PLAY,           // 播放音乐
} app_a2dp_state_t;

5）播放内部音频资源
AT+MSGPLAY=<audio_id>  // audio_id为UINT8类型
audio_id 部分资源ID如下：
#define GUI_AUDIO_NUMBER_0                     0x32
#define GUI_AUDIO_NUMBER_1                     0x33
#define GUI_AUDIO_NUMBER_2                     0x34
#define GUI_AUDIO_NUMBER_3                     0x35
#define GUI_AUDIO_NUMBER_4                     0x36
#define GUI_AUDIO_NUMBER_5                     0x37
#define GUI_AUDIO_NUMBER_6                     0x38
#define GUI_AUDIO_NUMBER_7                     0x39
#define GUI_AUDIO_NUMBER_8                     0x3A
#define GUI_AUDIO_NUMBER_9                     0x3B

2. BLE相关需求修改
3. 完善TS格式支持
4. 完善WAV格式支持：ULAW，ALAW，Linear-PCM, MP3/AAC
5. 优化音乐播放流程、AT命令处理流程等
6. 解决各种死机问题



5856Q32_codec_release_flash_20170526.rar

5856主要改动：
1）增加设置BT MAC地址AT指令：AT+BTSETADDR=<BT Addr>
   // 如BT Addr为：FC:1A:118:668  命令格式为：AT+BTSETADDR=FC1A11D866D8

2）AVRCP AT指令实现，按如下方法调用：
        播放：AT+BTPLAY=<BT Addr>
        暂停：AT+BTPAUSE=<BT Addr>
        上一首：AT+BTBACKWARD=<BT Addr>
        下一首：AT+BTFORWARD=<BT Addr>
        音量+：AT+BTVOLUP
        音量-：AT+BTVOLDOWN
   Note：多链接时可通过BT Addr指定要控制的BT设备，默认不需要指定BT Addr。

3）调整音频参数，增加音量。
4）增加TS流格式的支持
5）优化音乐播放流程


5856Q32_codec_release_flash_20170517.rar

5856主要改动：
1）解决BT模式和codec模式切换异常问题
2）解决开机codec概率重启问题
3）优化音乐播放流畅

5856Q32_codec_release_flash_170510.rar

5856主要改动：
1）解决播放百度云音乐codec卡住问题
2）录音采样率支持8K/16K版本：AT+RUSTART=<data_type>, <frame_max_size>, <sample_rate>
    Note：第三个参数sample_rate设置录音采样率，没有第三个参数则默认采样率为8K

5856Q32_codec_release_flash_20170503.rar

测试BLE临时版本，5856主要改动：
1. 增加BLE支持，通过BLE实现WIFI配网功能按如下操作：
1）进入BT（BR）模式：AT+CMODE=BT
2)  进入LE模式：AT+BTSWITCH=1  // 1：进入LE模式，0：进入BR模式。 返回：\r\nOK\r\n
3）开启wifi配网服务：AT+LESWIFISCS  // 返回：\r\nOK\r\n
4）获取AP name：AT+LEGETSSID  // 返回：\r\n<ssid>\r\n\r\nOK\r\n        
Note:  ssid为字符串类型，初始值为空：\r\n\r\n\r\nOK\r\n ，通过LE写入后返回对应的值。
5）获取AP password：AT+LEGETPASSWD  // 返回：\r\n<passwd>\r\n\r\nOK\r\n
Note：passwd为字符串类型，初始值为空：\r\n\r\n\r\nOK\r\n        ，通过LE写入后返回对应的值。

2. 优化音乐播放流程


5856Q32_codec_release_flash_20170421.rar

5856主要改动：
1）解决新版本codec工厂自动测试（AT+FTCODEC）失败问题
2）增加读取BT RSSI接口：AT+BTGETRSSI  // 返回：\r\n<rssi value>\r\n\r\nOK\r\n
    Note：BT需要建立连接后才能读取有效的rssi值，否则返回的rssi值为0
3）增加MIC增益设置范围：AT+MUSETMICGAIN=<gain_value> // gain_value: 0 ~ 12
4）解决pause/resume时概率出现丢失语音数据的情况


5856Q32_codec_release_flash_20170414.rar

5856主要改动：
1）大版本升级，基础代码从2016.11版本升级到2017.04版本。
2）解决部分MP3开头没有播出来问题
3）解决部分MP3文件不能播放问题
4）增加codec/BT模式切换指令：
    切换codec模式：AT+CMODE=UART_PLAY  // 命令执行返回OK，切换codec成功后返回：CODEC_READY
    切换BT模式：   AT+CMODE=BT  // 返回OK
5）增加音量调节指令：AT+CVOLUME=<volume>  // volume: 0 ~ 15

5981端程序需要做相应改动：
上电：5856默认进入CODEC模式，等56回复\r\nCODEC_READY\r\n后codec可以工作。
切换CODEC模式：发送AT+CMODE=UART_PLAY\r\n，等56回复\r\nOK\r\n后，指令执行成功，再等56回复\r\nCODEC_READY\r\n后，方可开始工作。
切换BT模式：发送AT+CMODE=BT\r\n，等56回复\r\nOK\r\n后，说明切换成功。




5856T_nolcd_release_flash_20170328.rar

5856主要改动：
1）解决CME ERROR: 50错误问题
2）调整codec rx buffer size

5991程序改为smartconfig联网模式


5856T_nolcd_release_flash_20170325.rar

主要改动：
1）解决音乐播放过程中codec重启问题
2）软件控制外部Audio PA开关
3）增加获取codec播放状态的AT指令：AT+MUGETSTATUS  // 0：STOP， 1：PLAY， 2：PAUSE


TinyDu_U02_HF_20170324.rar

汉枫板子5981 U02测试程序：TinyDu_U02_HF_20170324.rar
Note：基于百度效率云20170324 版本出的测试程序


TinyDu_U02_HF_20170321.rar

汉枫板子5981 U02测试程序：TinyDu_U02_HF_20170321.rar
RDA开发板5981 U02测试程序：TinyDu_U02_20170321.rar
Note：基于百度效率云20170321 18:50 版本出的测试程序


5856T_nolcd_release_flash_20170321.rar

主要改动：
1）录音采样率改为8K
2）根据需求，91发送stopPlay(WITH_ENDING)后立即返回OK
Note：91 U01版芯片使用效率云0321版本验证WIFI连接、语音识别和歌曲播放正常。



5856T_nolcd_release_flash_20170308_2.rar


在5856出来稳定的版本之前，麻烦各位都使用0308_2的版本。
这个版本录音效果没问题。



5856T_nolcd_release_flash_20170317.zip


主要改动：1）完善工厂测试模式Codec自动测试AT指令：AT+FTCODEC=<mode>  // mode: 1-enable, 0-disable，返回OK即表示成功

2）增加版本查询指令 AT+CVERSION      //返回格式 CHIPID,SW_VERSION,BUILDTIME
3）16KHz 录音问题修复（待验证）

rda5856_player_hanfeng_20170311.rar


汉枫板子测试程序：
百度云测试程序：baidu_iot_hanfeng_20170310.rar    // ssid: splendid    password: 43214321
T卡播放测试程序：rda5856_player_hanfeng_20170311.rar
Note：基于百度效率云20170310版本出的测试程序


rda5856_player_20170310.rar

RDA5991H_HDK_V1.0 板子测试程序：
百度云测试程序：baidu_iot_20170310.rar    // ssid: splendid    password: 43214321
T卡播放测试程序：rda5856_player_20170310.rar
Note：基于百度效率云20170310版本出的测试程序


5856Q32_codec_release_flash_20170503.rar

测试BLE临时版本，5856主要改动：
1. 增加BLE支持，通过BLE实现WIFI配网功能按如下操作：
1）进入BT（BR）模式：AT+CMODE=BT
2)  进入LE模式：AT+BTSWITCH=1  // 1：进入LE模式，0：进入BR模式。 返回：\r\nOK\r\n
3）开启wifi配网服务：AT+LESWIFISCS  // 返回：\r\nOK\r\n
4）获取AP name：AT+LEGETSSID  // 返回：\r\n<ssid>\r\n\r\nOK\r\n        
Note:  ssid为字符串类型，初始值为空：\r\n\r\n\r\nOK\r\n ，通过LE写入后返回对应的值。
5）获取AP password：AT+LEGETPASSWD  // 返回：\r\n<passwd>\r\n\r\nOK\r\n
Note：passwd为字符串类型，初始值为空：\r\n\r\n\r\nOK\r\n        ，通过LE写入后返回对应的值。

2. 优化音乐播放流程

5856T_nolcd_release_flash_20170310.rar

5856T_nolcd_release_flash_20170310.rar
主要改动：
1）解决数据传输过程中触发按键中断时程序重启/音乐停止播放等问题；
2）增加MIC增益设置AT指令：AT+MUSETMICGAIN=<gain_value> // gain_value: 0 ~ 7
3）增加音乐播放暂停/恢复AT指令：AT+MUPAUSE，AT+MURESUME
4）增加工厂测试模式Codec自动测试AT指令：AT+FTCODEC=<mode>  // mode: 1-enable, 0-disable