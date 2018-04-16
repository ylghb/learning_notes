# 对文件的处理

- 视频的清晰度前后不一
- 原视频中只有单声道有声音
- 最初的几集音频噪声严重

## 理想的处理思路

1. 分别下载视频与音频
2. 对音频进行降噪处理, 并合并音频声道
3. 合并视频音轨, 并且重命名

文件名的注意点:

1. 下载时以序号命名文件
2. 重命名时确保文件名不包含空格,括号等容易导致shell难以处理的字符

## 实际的处理方法

放弃对噪音的处理

```bash
youtube-dl.exe -f '135+bestaudio[ext=m4a]/best[ext=mp4]/best' https://www.youtube.com/playlist?list=PL8z8nfarlRRyZspCs7cRIONF1Vlr7lusI --no-progress  -o '%(playlist_index)s.%(ext)s' --playlist-start 29
# 135 是MP4格式的480p视频
# 音频格式前后非常不一致 bestaudio[ext=m4a]/best[ext=mp4]/best 优先选择最好的m4a,其次mp4,其次其他
# --playlist-start  偏移值, 从第n个视频开始下载
# -o 文件名

ffmpeg.exe -i input.mp4 -ac 1 output.mp4
# 基本的声道处理
```

### 基于SOX的消除视频噪声的处理方法

> sox       下载地址：[https://sourceforge.net/projects/sox/files/sox/](https://sourceforge.net/projects/sox/files/sox/)

- 噪音取样
- 视频音频分离
- 音频去噪
- 合并视频跟去噪的音频

```bash
#-an代表不要音频，可能是audio no的缩写
ffmpeg.exe -i 源视频.mp4 -an 输出视频.mp4
#-vn代表不要视频画面，可能是video no的缩写
ffmpeg.exe -i 源视频.mp4 -vn 输出音频.wav

#-ss代表起始时间，-t代表时间间隔，我们取一秒，最后的文件名都是输出文件名
ffmpeg -i 噪音视频.mp4 -vn -ss 00:00:00 -t 00:00:01 分离出来的噪音.wav
#通过sox我们将噪音的特征提取出来，然后就需要这个.prof文件
sox 分离出来的噪音.wav -n noiseprof 噪音样本.prof

#注意后面的0.21，根据google到说明，是说值最好在0.2到2.3之间，为什么0.21是因为别人试验后最好的效果，可以自己做相应的调试，取值在0.2到0.3即可
sox 输出音频.wav 去噪的音频.wav noisered 噪音样本.prof 0.21

ffmpeg.exe -i 去噪的音频.wav -i 输出视频.mp4 最终视频文件.mp4
```

脚本的形式

```bash
set -x
for i in `ls *mp4`;do
  f=$i
  filename=${f%.*}
  ffmpeg.exe -i $i -an -qscale 0 reduce/$filename.an.mp4;
  ffmpeg.exe -i $i -vn reduce/$filename.wav
  sox.exe reduce/$filename.wav reduce/$filename.clean.wav noisered samplenoise.prof 0.21
  ffmpeg.exe -i reduce/$filename.clean.wav -i reduce/$filename.an.mp4 reduce/$filename.clean.mp4
  done
```