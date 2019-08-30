# 树莓派的配置文件config.txt

由于树莓派并没有传统意义上的BIOS, 所以现在各种系统配置参数通常被存在”config.txt”这个文本文件中. 
 树莓派的config.txt文件会在ARM内核初始化之前被GPU读取. 
 这个文件存在引导分区上的.对于Linux, 路径通常是/boot/config.txt, 如果是Windows (或者OS X) 它会被识别为SD卡中可访问部分的一个普通文件.

你可以使用下列命令去获取当前激活的设置: 
- 列出指定的配置参数. 
- 例如: vcgencmd get_config arm_freq 
vcgencmd get_config  
- 列出所有已设置的整形配置参数(非零) 
vcgencmd get_config int 
- 列出所有已设置的字符型配置参数(非零) 
vcgencmd get_config str

文件格式 
 当值是整形时格式为”属性=值”. 每行只指定一个参数. 注释使用’#’井号作为一行开头. 
 注意: 在新版的树莓派里每行都有#注释, 要想使用该行参数只需移除#.

下面是示例文件 

```
 hdmi_drive=2 
 hdmi_group=2 
 hdmi_mode=16 
 overscan_left=20 
 overscan_right=12 
 overscan_top=10 
 overscan_bottom=10
```

**内存** 

```
 disable_l2cache 禁止ARM访问GPU的二级缓存. 相应的需要在内核中关闭二级缓存. 默认为0 
 gpu_mem GPU内存以兆为单位. 设置ARM和GPU之间的内存分配. ARM会获得剩余所有内存. 最小设为16. 默认为64 
 gpu_mem_256 对于有256MB内存的树莓派的GPU内存设置. 512MB的派请忽略. 会覆盖gpu_mem. 最大设为192. 默认不设置 
 gpu_mem_512 对于有512MB内存的树莓派的GPU内存设置. 256MB的派请忽略.  会覆盖gpu_mem. 最大设为448. 默认不设置 
 disable_pvt 禁止每500毫秒调整一次RAM的刷新率 (RAM温度测量).
```

**CMA – 动态内存分配** 
 自2012年11月19号, 固件和内核开始支持CMA, 这意味运行时可以动态管理ARM和GPU之间的内存分配. 这儿有相关config.txt示例. 

```
 cma_lwm 当GPU可用内存低于cma_lwm所设值, 将会向ARM请求一些内存. 
 cma_hwm 当GPU可用内存高于cma_hwm所设值, 将会向ARM释放一些内存. 
 要启用CMA，下面的参数需要添加到cmdline.txt文件里: 
 coherent_pool=6M smsc95xx.turbo_mode=N
```

**视频**

视频模式选项 

```
sdtv_mode 为复合信号输出设置视频制式(默认为0) 
 sdtv_mode=0    NTSC 
 sdtv_mode=1    日本版NTSC – 无基座 
 sdtv_mode=2    PAL 
 sdtv_mode=3    巴西版PAL – 副载波为525/60而不是625/50 
 sdtv_aspect 为复合信号输出设置宽高比(默认为1) 
 sdtv_aspect=1  4:3 
 sdtv_aspect=2  14:9 
 sdtv_aspect=3  16:9 
 sdtv_disable_colourburst 禁止复合信号输出彩色副载波群. 图片会显示为单色, 但是可能会更清晰 
 sdtv_disable_colourburst=1  禁止输出彩色副载波群 
 hdmi_safe 使用”安全模式”的设置去尝试用HDMI最大兼容性启动. 这和下面的组合是一个意思:  hdmi_force_hotplug=1, config_hdmi_boost=4, hdmi_group=2, hdmi_mode=4,  disable_overscan=0 
 hdmi_safe=1 
 hdmi_ignore_edid 如果你的显示器是天朝产的垃圾货, 允许系统忽略EDID显示数据 
 hdmi_ignore_edid=0xa5000080 
 hdmi_edid_file 当设为1时, 将会从edid.dat文件中读取EDID数据，而不是从显示器. 
 hdmi_edid_file=1 
 hdmi_force_edid_audio 伪装成支持所有音频格式播放, 即便报告不支持也允许通过DTS/AC3. 
 hdmi_force_edid_audio=1 
 hdmi_force_edid_3d 伪装成全部CEA模式都支持3D, 即便EDID并不支持. 
 hdmi_force_edid_3d=1 
 avoid_edid_fuzzy_match 禁止去模糊匹配EDID中描述的模式. 即便遮蔽错误, 也选用匹配分辨率和最接近帧率的标准模式. 
 avoid_edid_fuzzy_match=1 
 hdmi_ignore_cec_init 不发送初始化激活源消息. 避免在重启时使(启用CEC)TV结束待机并切换频道. 
 hdmi_ignore_cec_init=1 
 hdmi_ignore_cec 伪装成TV不支持CEC. 将不会支持任何CEC功能. 
 hdmi_ignore_cec=1 
 hdmi_force_hotplug 伪装成HDMI热插拔信号被检测到, 出现HDMI显示器被接入 
 hdmi_force_hotplug=1 即便没有检测到HDMI显示器也要使用HDMI模式 
 hdmi_ignore_hotplug 伪装成HDMI热插拔信号没有被检测到, 出现HDMI显示器未接入 
 hdmi_ignore_hotplug=1 即便检测到HDMI显示器也要使用混合模式 
 hdmi_pixel_encoding 强制像素编码模式. 默认情况下会使用EDID请求的模式, 所以不需要修改. 
 hdmi_pixel_encoding=0 default       (limited for CEA, full for DMT) 
 hdmi_pixel_encoding=1 RGB limited   (16-235) 
 hdmi_pixel_encoding=2 RGB full      ( 0-255) 
 hdmi_pixel_encoding=3 YCbCr limited (16-235) 
 hdmi_pixel_encoding=4 YCbCr limited ( 0-255) 
 hdmi_drive 选择HDMI还是DVI模式 
 hdmi_drive=1 DVI模式 (没声音) 
 hdmi_drive=2 HDMI模式 (如果支持并已启用将有声音输出) 
 hdmi_group 设置HDMI类型 
 不指定组, 或者设为0, 将会使用EDID报告的首选组. 
 hdmi_group=1   CEA 
 hdmi_group=2   DMT 
 hdmi_mode 设置在CEA或DMT格式下的屏幕分辨率 
 当hdmi_group=1 (CEA)时,下列值有效 
 hdmi_mode=1    VGA 
 hdmi_mode=2    480p  60Hz 
 hdmi_mode=3    480p  60Hz  H 
 hdmi_mode=4    720p  60Hz 
 hdmi_mode=5    1080i 60Hz 
 hdmi_mode=6    480i  60Hz 
 hdmi_mode=7    480i  60Hz  H 
 hdmi_mode=8    240p  60Hz 
 hdmi_mode=9    240p  60Hz  H 
 hdmi_mode=10   480i  60Hz  4x 
 hdmi_mode=11   480i  60Hz  4x H 
 hdmi_mode=12   240p  60Hz  4x 
 hdmi_mode=13   240p  60Hz  4x H 
 hdmi_mode=14   480p  60Hz  2x 
 hdmi_mode=15   480p  60Hz  2x H 
 hdmi_mode=16   1080p 60Hz 
 hdmi_mode=17   576p  50Hz 
 hdmi_mode=18   576p  50Hz  H 
 hdmi_mode=19   720p  50Hz 
 hdmi_mode=20   1080i 50Hz 
 hdmi_mode=21   576i  50Hz 
 hdmi_mode=22   576i  50Hz  H 
 hdmi_mode=23   288p  50Hz 
 hdmi_mode=24   288p  50Hz  H 
 hdmi_mode=25   576i  50Hz  4x 
 hdmi_mode=26   576i  50Hz  4x H 
 hdmi_mode=27   288p  50Hz  4x 
 hdmi_mode=28   288p  50Hz  4x H 
 hdmi_mode=29   576p  50Hz  2x 
 hdmi_mode=30   576p  50Hz  2x H 
 hdmi_mode=31   1080p 50Hz 
 hdmi_mode=32   1080p 24Hz 
 hdmi_mode=33   1080p 25Hz 
 hdmi_mode=34   1080p 30Hz 
 hdmi_mode=35   480p  60Hz  4x 
 hdmi_mode=36   480p  60Hz  4xH 
 hdmi_mode=37   576p  50Hz  4x 
 hdmi_mode=38   576p  50Hz  4x H 
 hdmi_mode=39   1080i 50Hz  reduced blanking 
 hdmi_mode=40   1080i 100Hz 
 hdmi_mode=41   720p  100Hz 
 hdmi_mode=42   576p  100Hz 
 hdmi_mode=43   576p  100Hz H 
 hdmi_mode=44   576i  100Hz 
 hdmi_mode=45   576i  100Hz H 
 hdmi_mode=46   1080i 120Hz 
 hdmi_mode=47   720p  120Hz 
 hdmi_mode=48   480p  120Hz 
 hdmi_mode=49   480p  120Hz H 
 hdmi_mode=50   480i  120Hz 
 hdmi_mode=51   480i  120Hz H 
 hdmi_mode=52   576p  200Hz 
 hdmi_mode=53   576p  200Hz H 
 hdmi_mode=54   576i  200Hz 
 hdmi_mode=55   576i  200Hz H 
 hdmi_mode=56   480p  240Hz 
 hdmi_mode=57   480p  240Hz H 
 hdmi_mode=58   480i  240Hz 
 hdmi_mode=59   480i  240Hz H 
 H表示16:9比例(正常是4:3). 
 2x表示双倍像素(即更高的像素时脉, 每个像素重复两次) 
 4x表示四倍像素(即更高的像素时脉, 每个像素重复四次) 
 当hdmi_group=2 (DMT)时,下列值有效 
 警告: 根据这篇帖子所述 
 像素时脉是有限制的, 最高支持的模式是1920x1200 @60Hz with reduced blanking. 
 hdmi_mode=1    640x350   85Hz 
 hdmi_mode=2    640x400   85Hz 
 hdmi_mode=3    720x400   85Hz 
 hdmi_mode=4    640x480   60Hz 
 hdmi_mode=5    640x480   72Hz 
 hdmi_mode=6    640x480   75Hz 
 hdmi_mode=7    640x480   85Hz 
 hdmi_mode=8    800x600   56Hz 
 hdmi_mode=9    800x600   60Hz 
 hdmi_mode=10   800x600   72Hz 
 hdmi_mode=11   800x600   75Hz 
 hdmi_mode=12   800x600   85Hz 
 hdmi_mode=13   800x600   120Hz 
 hdmi_mode=14   848x480   60Hz 
 hdmi_mode=15   1024x768  43Hz  DO NOT USE 
 hdmi_mode=16   1024x768  60Hz 
 hdmi_mode=17   1024x768  70Hz 
 hdmi_mode=18   1024x768  75Hz 
 hdmi_mode=19   1024x768  85Hz 
 hdmi_mode=20   1024x768  120Hz 
 hdmi_mode=21   1152x864  75Hz 
 hdmi_mode=22   1280x768        reduced blanking 
 hdmi_mode=23   1280x768  60Hz 
 hdmi_mode=24   1280x768  75Hz 
 hdmi_mode=25   1280x768  85Hz 
 hdmi_mode=26   1280x768  120Hz reduced blanking 
 hdmi_mode=27   1280x800        reduced blanking 
 hdmi_mode=28   1280x800  60Hz 
 hdmi_mode=29   1280x800  75Hz 
 hdmi_mode=30   1280x800  85Hz 
 hdmi_mode=31   1280x800  120Hz reduced blanking 
 hdmi_mode=32   1280x960  60Hz 
 hdmi_mode=33   1280x960  85Hz 
 hdmi_mode=34   1280x960  120Hz reduced blanking 
 hdmi_mode=35   1280x1024 60Hz 
 hdmi_mode=36   1280x1024 75Hz 
 hdmi_mode=37   1280x1024 85Hz 
 hdmi_mode=38   1280x1024 120Hz reduced blanking 
 hdmi_mode=39   1360x768  60Hz 
 hdmi_mode=40   1360x768  120Hz reduced blanking 
 hdmi_mode=41   1400x1050       reduced blanking 
 hdmi_mode=42   1400x1050 60Hz 
 hdmi_mode=43   1400x1050 75Hz 
 hdmi_mode=44   1400x1050 85Hz 
 hdmi_mode=45   1400x1050 120Hz reduced blanking 
 hdmi_mode=46   1440x900        reduced blanking 
 hdmi_mode=47   1440x900  60Hz 
 hdmi_mode=48   1440x900  75Hz 
 hdmi_mode=49   1440x900  85Hz 
 hdmi_mode=50   1440x900  120Hz reduced blanking 
 hdmi_mode=51   1600x1200 60Hz 
 hdmi_mode=52   1600x1200 65Hz 
 hdmi_mode=53   1600x1200 70Hz 
 hdmi_mode=54   1600x1200 75Hz 
 hdmi_mode=55   1600x1200 85Hz 
 hdmi_mode=56   1600x1200 120Hz reduced blanking 
 hdmi_mode=57   1680x1050       reduced blanking 
 hdmi_mode=58   1680x1050 60Hz 
 hdmi_mode=59   1680x1050 75Hz 
 hdmi_mode=60   1680x1050 85Hz 
 hdmi_mode=61   1680x1050 120Hz reduced blanking 
 hdmi_mode=62   1792x1344 60Hz 
 hdmi_mode=63   1792x1344 75Hz 
 hdmi_mode=64   1792x1344 120Hz reduced blanking 
 hdmi_mode=65   1856x1392 60Hz 
 hdmi_mode=66   1856x1392 75Hz 
 hdmi_mode=67   1856x1392 120Hz reduced blanking 
 hdmi_mode=68   1920x1200       reduced blanking 
 hdmi_mode=69   1920x1200 60Hz 
 hdmi_mode=70   1920x1200 75Hz 
 hdmi_mode=71   1920x1200 85Hz 
 hdmi_mode=72   1920x1200 120Hz reduced blanking 
 hdmi_mode=73   1920x1440 60Hz 
 hdmi_mode=74   1920x1440 75Hz 
 hdmi_mode=75   1920x1440 120Hz reduced blanking 
 hdmi_mode=76   2560x1600       reduced blanking 
 hdmi_mode=77   2560x1600 60Hz 
 hdmi_mode=78   2560x1600 75Hz 
 hdmi_mode=79   2560x1600 85Hz 
 hdmi_mode=80   2560x1600 120Hz reduced blanking 
 hdmi_mode=81   1366x768  60Hz 
 hdmi_mode=82   1080p     60Hz 
 hdmi_mode=83   1600x900        reduced blanking 
 hdmi_mode=84   2048x1152       reduced blanking 
 hdmi_mode=85   720p      60Hz 
 hdmi_mode=86   1366x768        reduced blanking 
 overscan_left 左侧跳过像素数 
 overscan_right 右侧跳过像素数 
 overscan_top 顶部跳过像素数 
 overscan_bottom 底部跳过像素数 
 framebuffer_width 控制台framebuffer宽度, 以像素为单位. 默认是显示器宽度减去超出扫描. 
 framebuffer_height 控制台framebuffer高度, 以像素为单位. 默认是显示器高度减去超出扫描. 
 framebuffer_depth 控制台framebuffer深度, 以位为单位. 默认是16位. 8位也是有效的,  但是默认RGB调色板会导致屏幕不可读. 24位效果更好 ,但是2012年6月15号发现有显示混乱问题. 32位没有混乱问题,  但是需要设置framebuffer_ignore_alpha=1, 并在2012年6月15号发现颜色显示错误. 
 framebuffer_ignore_alpha 设为1将禁用alpha通道. 仅对32位有效. 
 test_mode 允许在启动时做声音与图像测试. 
 disable_overscan 设为1将禁用超出扫描. 
 config_hdmi_boost 设置HDMI接口的信号强度. 默认为0. 如果出现HDMI干扰问题可以试试设为4. 最大为7. 
 display_rotate 顺时针旋转屏幕显示 (默认为0) 或者翻转显示. 
 display_rotate=0        正常 
 display_rotate=1        90度 
 display_rotate=2        180度 
 display_rotate=3        270度 
 display_rotate=0x10000  水平翻转 
 display_rotate=0x20000  垂直翻转 
```

 注意: 旋转90度或者270度额外需要GPU内存, 所以在GPU只分配到16M的时候旋转会无效. 可能的原因: 
 Crashes my RPI before Linux boots if set to “1” — REW 20120913.

 注意: 旋转90度或者270度额外需要GPU内存, 所以在GPU只分配到16M的时候旋转会无效. 可能的原因: 
 Crashes my RPI before Linux boots if set to “1” — REW 20120913.

哪些值对我的显示器有效?

你的HDMI显示器可能只支持一部分设置. 想要找出支持哪些设置, 可以使用下面的方法. 

```
把输出格式设为VGA 60Hz (hdmi_group=1 hdmi_mode=1) 然后启动树莓派 
 输入下列命令可以获取CEA支持模式的列表 
 /opt/vc/bin/tvservice -m CEA 
 输入下列命令可以获取DMT支持模式的列表 
 /opt/vc/bin/tvservice -m DMT 
 输入下列命令可以获取当前设置状态 
 /opt/vc/bin/tvservice -s 
 输入下列命令可以从显示器获取更多详细信息 
 /opt/vc/bin/tvservice -d edid.dat /opt/vc/bin/edidparser edid.dat 
 使用默认HDMI模式去排除问题时, edid.dat文件同样会提供信息
```

许可的解码器

你可以购买绑定树莓派CPU序列号的证书来使用额外的硬件解码器. 

```
decode_MPG2 可开启MPEG-2硬解的序列号. 
 decode_MPG2=0x12345678 
 decode_WVC1 可开启VC-1硬解的序列号. 
 decode_WVC1=0x12345678 
 可在多台树莓派间共享SD卡的序列号. 同时最多8个证书. 
 decode_XXXX=0x12345678,0xabcdabcd,0x87654321,…
```

**启动**

```
disable_commandline_tags 在启动内核前, 通过改写ATAGS (0x100处的内存)来阻止start.elf 
 cmdline (string) 命令行参数. 可用来代替cmdline.txt文件 
 kernel (string) 加载指定名称的内核镜像文件启动内核. 默认为”kernel.img” 
 kernel_address 加载kernel.img文件地址 
 kernel_old (bool) 为1时, 从0x0处加载内核 
 ramfsfile (string) 要的加载的ramfs文件 
 ramfsaddr 要加载的ramfs文件地址 
 initramfs (string address) 要加载的ramfs文件及其地址 (就是把ramfsfile+ramfsaddr合并为一项). 
 注意: 这项使用与其他项不同的语法 – 不要在这用”=”号. 正确示例: 
 initramfs initramf.gz 0x00800000 
 device_tree_address 加载device_tree的地址 
 init_uart_baud 初始化uart波特率. 默认为115200 
 init_uart_clock 初始化uart时序. 默认为3000000 (3Mhz) 
 init_emmc_clock 初始化emmc时序. 默认为100000000 (100MHz) 
 boot_delay 在加载内核前在start.elf等待指定秒. 总延迟=1000 * boot_delay + boot_delay_ms. 默认为1 
 boot_delay_ms 在加载内核前在start.elf等待指定毫秒. 默认为0 
 avoid_safe_mode 如果设为1, 将不以安全模式启动. 默认为0
```

**超频**

注意: 设置任何参数来超频树莓派都会在芯片中永久的储存一个保修位, 用于检测你的树莓派是否超频过. 如果设备超频过保修就无效了. 自2012年9月19号,你可以自由超频而不影响保修了. 
 最新的内核有一个默认开启”ondemand”调速器的cpu频率内核驱动. 未开启超频并不会有任何影响. 一旦你开超频,  ARM频率将随处理器负载而变化. 只有在调速器需要时才会使用非默认值. 你可以使用*_min配置选项来调整最低值,  或者使用force_turbo=1来禁用动态超频.

当芯片温度达到85°C运行时会关闭超频及超压, 直到冷却. 即使在25°C环境温度下使用最高设置, 也不要让温度达到极限.

超频选项

```
参数    说明 
 arm_freq    ARM频率,以MHz为单位. 默认为700 
 gpu_freq    同时设置core_freq, h264_freq, isp_freq, v3d_freq. 默认为250 
 core_freq    GPU处理器核心频率,以MHz为单位. 由于GPU要驱动二级缓存, 对ARM性能会造成影响. 默认为 250 
 h264_freq    视频硬解模块频率,以MHz为单位. 默认为250 
 isp_freq    图像传感器管道模块频率,以MHz为单位. 默认为250 
 v3d_freq    3D模块频率,以MHz为单位. 默认为250 
 avoid_pwm_pll    不要把锁相环用在PWM音频. 这会略微降低模拟音频的效果. 空闲的锁相环允许从剩余GPU独立设置core_freq, 这将会比超频有更多权限. 默认为0 
 sdram_freq    SDRAM频率,以MHz为单位.默认为400 
 over_voltage    ARM/GPU核心电压调节. [-16,8]用0.025V步进等同于[0.8V,1.4V]. 默认为0  (1.2V). 只有在指定 force_turbo或current_limit_override时 (会设置保修位), 才允许数值在6以上 
 over_voltage_sdram    同时设置over_voltage_sdram_c, over_voltage_sdram_i, over_voltage_sdram_p 
 over_voltage_sdram_c    SDRAM控制器电压调节. [-16,8]用0.025V步进等同于[0.8V,1.4V]. 默认为0 (1.2V) 
 over_voltage_sdram_i    SDRAM I/O电压调节. [-16,8]用0.025V步进等同于[0.8V,1.4V]. 默认为0 (1.2V) 
 over_voltage_sdram_p    SDRAM phy电压调节. [-16,8]用0.025V步进等同于[0.8V,1.4V]. 默认为0 (1.2V) 
 force_turbo    关闭动态CPU频率驱动及下面的最小设置. 开启h264/v3d/isp超频. 默认为0. 会设置保修位. 
 initial_turbo    在启动时以指定秒数 (上限为60) 或者以CPU频率来开启急速模式. 如果已经超频, 能对SD卡错误问题有改善. 默认为0 
 arm_freq_min    设置动态时序的最小arm_freq. 默认为700 
 core_freq_min    设置动态时序的最小core_freq. 默认为250 
 sdram_freq_min    设置动态时序的最小sdram_freq. 默认为400 
 over_voltage_min    设置动态时序的最小over_voltage. 默认为0 
 temp_limit    过热保护. 当芯片达到指定温度就把时序和电源切换会默认值. 把此值设高于默认值将影响保修. 默认为85 
 current_limit_override    当设为”0x5A000020″时, 禁止SMPS限流保护. 在超频过高无法重启时设置此项会有所帮助. 会设置保修位.
```

force_turbo模式 
 force_turbo=0 
 开启对ARM核心,GPU核心和SDRAM的动态时序及电压.  在忙的时候ARM频率会提高到”arm_freq”并在闲的时候降低到”arm_freq_min”. “core_freq”,  “sdram_freq”和”over_voltage”的行为都一样. “over_voltage”最高为6 (1.35V).  h264/v3d/isp部分的非默认值将被忽略. 
 force_turbo=1 
 关闭动态时序, 因此所有频率和电压会保持高值. h264/v3d/isp GPU部分的超频也会开启, 等同于设置”over_voltage”为8 (1.4V).

时序关系

GPU核心, h264, v3d和isp共享一个锁相环, 因此需要相关联的频率. ARM, SDRAM和GPU有各自独有的锁相环, 因此可以设为没有关联的频率.

当设了”avoid_pwm_pll=1″下列设置就没必要了. 
 pll_freq = floor(2400 / (2 * core_freq)) * (2 * core_freq) 
 gpu_freq = pll_freq / [偶数] 
 有效的gpu_freq会自动四舍五到到最接近的整型偶数, 所以请求core_freq为500, gpu_freq为300,算一下2000/300 = 6.666 => 6 ,结果就是333.33MHz.

已测试过的超频设置

下表显示了一些成功的超频尝试, 这些可以指导你进行超频. 这些设置不一定能在每台树莓派上都成功, 并且会缩短高通芯片的寿命.

arm_freq    gpu_freq    core_freq    h264_freq    isp_freq    v3d_freq    sdram_freq    over_voltage    over_voltage_sdram 
800 
900    275                    500 
900        450                450 
930    350                    500 
1000        500                500    6 
1050                            6 
1150        500                600    8 

 这是一个表明Hynix产的RAM在超频上表现不如三星产的RAM的报告.

超频时SD卡使用

设置SD卡: http://elinux.org/RPi_Easy_SD_Card_Setup 
 超频时使用6速或10速的SD卡(SHDC/SHDX)会导致在一些天后树莓派读取SD卡文件系统不稳定. 
 不管是ext4 , NTFS 或其他格式都一样. 
 不管是哪家SD卡生产商都一样. 
 不管是哪个版本的树莓派都一样. 
 这与SD卡容量无关 – 实际验证出现在16G或更大的SD卡上. 
 ! 关键是你何时让树莓派功率不足,也就是低于树莓派的基本设置需求 ! 
 popcornmix发表在https://github.com/raspberrypi/linux/issues/280: 
 “超频会导致SD卡错误.这情况往往是与板子相关(就是说有些树莓派超频后SD卡没事,有些不行). 
 我认为通常都是core_freq导致的SD卡问题(和arm_freq,sdram_freq比)” 
 在2013年4月写这个提示的时候在树莓派官方论坛上一共有137个有关于SD的问题, 绝大部分与超频有关. 
 如果你使用6速或10速SD卡, 还想要树莓派稳定运行:  不要尝试超频,否则很可能会丢失数据

监测温度及电压

要检测树莓派的温度, 看: /sys/class/thermal/thermal_zone0/temp 
 要检测树莓派当前的频率, 看: /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq 
 要检测树莓派电源装置的电压, 你需要一个万用电表, 接上电源测试点, 或者扩展头.

通常来说要保持核心温度低于70度, 电压高于4.8V. (另外请注意, 不要用那种便宜的USB电源, 那基本上是4.2V的,  这是因为那本来就是为充3.7V锂电池设计的, 根本无法为树莓派提供稳定的5V电压). 此外, 用散热片也是个好主意,  尤其是你把树莓派装到了壳子里. 一个合适的散热器是自带不干胶栅格状的 14x14x10 mm 散热片.

超频稳定性测试

大多数超频问题立马就会出现启动问题, 但还是会随时间而出现文件系统问题. 这是一个对系统,特别是SD卡进行压力测试的脚本. 如果脚本执行完成, dmesg中不提示任何错误, 你做的超频设置可能会比较稳定.

如果系统崩溃了, 在重启时按住shift键, 这会临时性关闭所有超频. 同样, 注意SD卡问题通常由core_freq造成,不要在raspi-config预设的高速(950 MHz)和超速(1 GHz)里来个大跳越(从250 MHz飞到500 MHz).