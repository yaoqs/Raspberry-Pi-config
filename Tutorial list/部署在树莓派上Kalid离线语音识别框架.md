# 部署在树莓派上Kalid离线语音识别框架

Kaldi 语音识别总结

一、编译安装

1、下载：

建议使用kaldi-trunk 版本，来进行语音识别，下载代码：

```
git clonehttps://github.com/kaldi-asr/kaldi.git kaldi-trunk --origin golden
```

若没有安装git 请先安装，代码：

`sudo apt-get install git`

2、各目录功能：

```
./tools 目录下为kaldi的依赖包。

./src 目录下为kaldi 的源代码。

./egs 目录下为 kaldi 的一些例子。
```

3、kaldi 的编译安装：

安装前需要确保安装一下软件：

```
sudo apt-get install autoconf automakegcc libtool subversion libatlas-dev libatlas-base-dev gfortran g++ zlib1g-dev
```

进入./tool目录，输入make 也可make -j X （X为你电脑的核心数，可加大编译速度）之后切换到./src目录下，输入./configure进行配置，然后输入make depend，完成之后输入make进行编译。

至此，kaldi 基本包安装完成。

4、测试：

进入./egs/yesno/s5 目录下运行 sudo./run.sh 这是kaldi自带的一个识别yesno的一个例子。

训练完成的结果：WER为0.00 （注：WER（Word Error Rate）是字错误率，是一个衡量语音识别系统的准确程度的度量。其计算公式是WER=(I+D+S)/N，其中I代表被插入的单词个数，D代表被删除的单词个数，S代表被替换的单词个数。也就是说把识别出来的结果中，多认的，少认的，认错的全都加起来，除以总单词数。这个数字当然是越低越好。）

5、特别注意：

（1）Kaldi 中实例众多，但由于都不自带数据集的原因，绝大部分是不能直接运行的，有的下载数据集的方式集成在 run.sh 代码中，不自带的需要自行从网上下载。（每个脚本文件中的注释语句均为这个脚本的使用说明，请注意查看）

 

（2）有的示例在训练的过程中会提示缺少irstlm。是因为在新版的kaldi中irstlm不是默认编译的，需要自行编译，安装目录在tools/extras下。直接运行install_irstlm.sh脚本即可。并将安装完成的irstlm 复制一份放在tools目录下。

 

（3）在进行训练之前：需要更改一些环境变量。通常要更改的设置如下

     ①将cmd.sh中所有的queue.pl更改为 run.pl。（具体修改方式均在cmd.sh的注释语句中）
    
        ②确认数据集的位置：在run.sh中找到放置数据集的默认路径，将其改为你的数据集路径

二、进行语音识别

语音识别有两种模式，一为录制好的音频文件进行识别（注：音频文件格式必须为单声道，16000Hz，16bit模式，否者程序报错）。二为通过麦克风进行的实时识别。

1、音频文件识别：

（注：建议先安装如下软件后再执行以下步骤：

```
sudo apt-get install pulseaudio

sudo apt-get install libportaudio-dev

sudo apt-get install libportaudio0

sudo apt-get install libportaudio2

sudo apt-get install libportaudiocpp0

sudo apt-get install alsa-base

sudo apt-get install alsa-utils

sudo apt-get install libasound2

sudo apt-get install libasound-dev

sudo apt-get install libatlas3-base
```

这些事麦克风实时识别的依赖库，如不需要麦克风实时识别请跳过）

 

①找到需要用来识别的文件：在每一个示例中exp文件中存放的都是跟训练有关的目录，其中tri1，tri2…等等都是每个训练的文件夹。我们拿thchs30 的中文语料库的tri1的训练来举例子。在tri1中有final.mdl和40.mdl 这两个文件这是我们需要的模板文件，final.mdl 是指向40.mdl的快捷方式。其实就是一个文件。在graph_word中的word.txt和HCLG.fst，一个是字典，一个是有限状态机，有这三个文件就可以来进行识别了。

②在tools目录下 运行脚本install_portaudio.sh安装portaudio 再回到src目录下输入make ext 进行编译安装。在onlinebin文件夹中online-wav-gmm-decode-faster是识别wav文件的。online-gmm-decode-faster 是通过麦克风进行实时识别的。

③去egs下，打开voxforge，里面有个online_demo，在online_demo文件夹下新建两个文件夹online-data 和 work，在online-data下新建两个文件夹audio和models，audio下放的是你需要识别的wav音频文件，在models文件夹下放的是①中提到的模板。

④打开online_demo的run.sh

（a）将下面这些语句注释掉，或删掉：

if [ ! -s ${data_file}.tar.bz2 ]; then

    echo"Downloading test models and data ..."
    
    wget -T 10-t 3 $data_url;

 



    if [ ! -s${data_file}.tar.bz2 ]; then
    
        echo"Download of $data_file has failed!"
    
        exit 1
    
    fi

fi

 

if [ ! -d $ac_model ]; then

    echo"Extracting the models and data ..."
    
    tar xf${data_file}.tar.bz2

fi

 

（b）找的这句话：

Change this to "tri2a" if you like to test using aML-trained model

ac_model_type=tri1

将tri1改为你自己用的模板的文件夹名字

 

 

 

 

（c）更改下面这句话

online-wav-gmm-decode-faster --verbose=1--rt-min=0.8 --rt-max=0.85\

           --max-active=4000 --beam=12.0 --acoustic-scale=0.0769 \
    
           scp:$decode_dir/input.scp$ac_model/model $ac_model/HCLG.fst \
    
           $ac_model/words.txt '1:2:3:4:5'ark,t:$decode_dir/trans.txt \
    
           ark,t:$decode_dir/ali.txt $trans_matrix;;

（注：$ac_model/model这个mdl文件改为你的模板文件如$ac_model/final.mdl，

$ac_model/HCLG.fst为你的有限状态机，$ac_model/words.txt为你的词典）

（d）更改完成之后放入你需要识别的音频文件（注意格式），直接运行run.sh 即可

 

④如果你需要用到tri2、tri3、及以后的目录时会遇到如下报错

ERROR(online-wav-gmm-decode-faster:LogLikelihoods():diag-gmm.cc:533)DiagGmm::ComponentLogLikelihood, dimension mismatch 39vs. 40

解决方法。在run.sh中case前加入如下这句话，并将final.mat（若final.mat为快捷方式请将他链接的文件一并考入）考入你的模板中。

if [ -s $ac_model/model ]; then

          trans_matrix=$ac_model/matrix

fi

 

并添加两个参数（--left-context=3--right-context=3）如下所示：

online-wav-gmm-decode-faster --verbose=1--rt-min=0.8 --rt-max=0.85\

    --max-active=4000 --beam=12.0 --acoustic-scale=0.0769 --left-context=3--right-context=3\
    
     scp:$decode_dir/input.scp$ac_model/final.mdl $ac_model/HCLG.fst \
    
    $ac_model/words.txt '1:2:3:4:5' ark,t:$decode_dir/trans.txt \
    
    ark,t:$decode_dir/ali.txt $trans_matrix;;

保存退出，再次运行run.sh即可。

2、麦克风在线识别：

①安装前需要确保安装如下软件：

```
sudo apt-get installpulseaudio

sudo apt-get installlibportaudio-dev

sudo apt-get installlibportaudio0

sudo apt-get installlibportaudio2

sudo apt-get install libportaudiocpp0

sudo apt-get installalsa-base

sudo apt-get installalsa-utils

sudo apt-get installlibasound2

sudo apt-get installlibasound-dev

sudo apt-get installlibatlas3-base

 
```

如果你在安装portaudio前已经安装以上软件则可以跳过，建议如果你已经安装过portaudio而没有安装以上软件请将portaudio卸载重装，并且回到src 目录下重新编译online 和onlinbin文件夹下的内容。

 

②安装portaudio 并在 src 目录下make ext（若已安装过，请跳过）

（还需安装speex也在tools目录下）

③修改run.sh 参照音频识别的a,b,c,d.修改

在修改online-gmm-decode-faster 中

                online-gmm-decode-faster--rt-min=0.5 --rt-max=0.7 --max-active=4000 \
    
          --beam=12.0 --acoustic-scale=0.0769 $ac_model/final.mdl $ac_model/HCLG.fst\
    
          $ac_model/words.txt '1:2:3:4:5' $trans_matrix;;

将你的模板替换进去即可。

运行./run.sh --test-mode live 即可进行麦克风实时识别。

 

 

三、识别结果获取。

翻译识别的结果的输出函数在src/onlinbin/online-gmm-decode-faster.cc文件中若为音频文件识别则在src/onlinebin/online-wav-gmm-decode-faster.cc文件中。

PrintPartialResult(word_ids, word_syms, partial_res|| word_ids.size());

上述这句即为结果的输出函数。这个函数的定义在src/online/online-util.cc 和 sre/online/online-util.h 文件中。

在online-util.cc中PrintPartialResult的定义如下，其中的word即为识别的结果。可以自行修改函数为你需要的形式。

 

void PrintPartialResult(conststd::vector<int32>& words,

                        const fst::SymbolTable*word_syms,
    
                        bool line_break) {

 KALDI_ASSERT(word_syms != NULL);

  for (size_ti = 0; i < words.size(); i++) {

   std::string word = word_syms->Find(words[i]);

    if (word== "")
    
     KALDI_ERR << "Word-id " << words[i] <<"not in symbol table.";
    
    std::cout<< word << ' ';

  }

  if(line_break)

    std::cout<< "\n\n";

  else

   std::cout.flush();

}

 

注：若你在online-util.cc中添加了新的函数时，请在online-util.h中进行声明。

修改完代码后先重新编译online文件夹下的源码，再重新编译onlinebin文件夹下的源码。更改完成以后，再运行online_demo下的run.sh 即可得到你想要的运行结果。

 

四、linux（或树莓派）与arduino的串口通讯问题

1、首先，安装串口通讯软件

sudo apt-get install python-serial

2、连接电脑与arduino之后键入以下命令

ls /dev/tty*

是否有 ttyACM0 或 ttyUSB0 文件，这两个文件只有在连接arduino后才会生成。

3、编写用于串口通讯的python代码：

 

Import time

import serial

ser = serial.Serial('/dev/ttyACM0', 9600,timeout=1)     //若你的设备显示ttyUSB0就替换

time.sleep(2)           //延时两秒

ser.write("XXXX")             //XXXX为你要向arduino发送的东西

response = ser.readline()          //ser.readline()为读取arduino反馈回来的信号

print response

ser.close()

 

这个延时两秒，我试过很多次如果低于两秒信号发送不出去，可能是打开串口通讯需要时间的原因，故在此延时两秒发送信号。

 

4、如何在c++语言中调用命令行语句：

首先在头文件中加上 #include<stdlib.h> 头文件

之后system("sudo python test.py"); //引号内的即为命令行语句，直接调用之前编辑好的python脚本即可。

 

5、arduino端只要编辑好与之相配的代码即可，注意波特率需要一致。

 

 

 

 

 

 

Kaldi语音识别暑期实践总结

如有错误欢迎指正 
 ———————————————— 
版权声明：本文为CSDN博主「凯东」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_32950391/article/details/80766915