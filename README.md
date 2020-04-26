# Survey_Kagawa_PubComment
Survey_Kagawa＿PubComment

* 今のところ、pngは450dpiで取り出している。  

# Download
* 反対
https://github.com/GinSanaduki/Survey_Kagawa_PubComment/releases/download/PNG_1.0.0/hantai.7z  

* 事業者
https://github.com/GinSanaduki/Survey_Kagawa_PubComment/releases/download/PNG_1.0.0/jigyosha.7z  

* 賛成
https://github.com/GinSanaduki/Survey_Kagawa_PubComment/releases/download/PNG_1.0.0/sansei_01.7z  

https://github.com/GinSanaduki/Survey_Kagawa_PubComment/releases/download/PNG_1.0.0/sansei_02.7z  

* 提言
https://github.com/GinSanaduki/Survey_Kagawa_PubComment/releases/download/PNG_1.0.0/teigen.7z  




これを、popplerでpngとして取り出す。  
なにしろ、4000枚なので、整理が大変なので、PDFのファイル名で制御する。  

```bash
unzip drive.zip

mkdir -p img
mkdir -p text
ls -1 *.pdf | sed -e 's/.pdf//g' | awk '{print "mkdir -p img/"$0;}' | xargs -P 0 -I {} sh -c {}
ls -1 *.pdf | sed -e 's/.pdf//g' | awk '{print "mkdir -p text/"$0;}' | xargs -P 0 -I {} sh -c {}

```

* PDFからpngファイルへの変換は、以下を参照  
https://github.com/GinSanaduki/TB3DS_Termux/blob/master/AWKScripts/01_UPDATE/03_SubSystem/03_SubSystem_03.awk  

```bash

ls -1 *.pdf | awk '{Tex = $0; sub(".pdf","",Tex); print "pdftoppm -png -r 450 \""$0"\" \"img/"Tex"/"Tex"\"";}' | xargs -P 4 -I {} sh -c {}

```

pdftoppm -png -r 450 "賛成0123.pdf" "img/賛成0123/賛成0123"  
のようになるのだが、450は、DPIを示す。  
Tesseract OCRで、一番誤認識を起こさないのは、スキャンした際のDPI指定値になる。  
高すぎてもいけないのだ。  
ちなみに、官報のスキャンは、450DPIでされている（研究したからわかる）。  


```bash
find -name *.png | awk '{Tex = substr($0,3); Tex2 = Tex; sub("img","text", Tex2); sub(".png","", Tex2); print "tesseract \""Tex"\" \""Tex2"\" -l jpn --psm 3 --dpi 450 > /dev/null 2>&1";}' > cmd.txt
```

txtの拡張子をつけなくても、tesseract側でつけてくれるので、意図的に拡張子を指定していない。  
結構時間がかかる上にCPUをバカ喰いするので、実行する際は気をつけて。  

