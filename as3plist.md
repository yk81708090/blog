[一个基于AS3的plist库](http://zengrong.net/post/1982.htm)

本文并未全部完成，请耐心等待……
<hr>

为了在 [Sprite Sheet Editor][1] 中加入 plist 格式的 metadata 支持，我在 [f60k的as3plist库][2] 基础上进行了修改，实现了我的 [as3plist][3] 库。

Plist格式的本质是XML文件。由于AS3内置XML支持，所以这个库的实现还是比较容易的。

[Cocos2d-x 中大量使用了plist格式文件][4] ，因此实现plist的支持非常必要。目前我还没有找到软件能导入 plist+png 格式的 Sprite Sheet。大多数软件都只是能生成该格式。而 [Sprite Sheet Editor][1] 只需要稍加修改就能做到这一点。

由于精力有限，项目中的文档并不齐全且可能有错，直接编译 [sample][5] 会比较靠谱。

下面是范例代码：<!--more-->

<pre lang="Actionscript">
var __olist:Plist10  = new Plist10();

var __parr:PArray = new PArray();
var __pnum:PNumber = new PNumber();
__pnum.object = 3;
var __pstring:PString = new PString();
__pstring.object = "hello";
var __pbool:PBoolean = new PBoolean();
__pbool.object = true;

__parr.addValue(__pnum, __pbool, __pstring);
__parr.addValue(new PArray().addValue(3, 4, 5, 7));
__parr.addValue(new PDict().addValue("name", new PNumber()));
//trace(__parr.toXMLString());
__olist.root = __parr;
//trace(__olist.toString());

var __alist:Plist10 = new Plist10();
__alist.parse(__olist.toString());
trace(__alist.toString());
</pre>

输出内容：

<pre lang="XML">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <array>
    <integer>3</integer>
    <true/>
    <string>hello</string>
    <array>
      <integer>3</integer>
      <integer>4</integer>
      <integer>5</integer>
      <integer>7</integer>
    </array>
    <dict>
      <key>name</key>
      <integer>0</integer>
    </dict>
  </array>
</plist>
</pre>

ZwoptexFormat2File 是 Plist10 的子类，它实现了 Zwoptex 软件第二版的文件风格。目前 Cocos2d-x 支持的plist Sprite Sheet格式就是这种格式。

<pre lang="Actionscript">
var __zwoptex2:ZwoptexFormat2File = new ZwoptexFormat2File();
var __frame:ZwoptexFormat2Frame = new ZwoptexFormat2Frame();
__frame.setOffset(0, 0).setRotated(false).setSourceColorRect(0, 0, 100, 100).setSourceSize(100, 100).setFrame(50, 50, 100, 100);
__zwoptex2.setSize(100, 100).setRealTextureFileName("abc.png").setTextureFileName("abc.png").addFrame("test01", __frame);
trace(__zwoptex2.toString());
</pre>

输出内容：

<pre lang="XML">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>frames</key>
    <dict>
      <key>test01</key>
      <dict>
        <key>offset</key>
        <string>{0,0}</string>
        <key>rotated</key>
        <false/>
        <key>sourceColorRect</key>
        <string>{{0,0},{100,100}}</string>
        <key>sourceSize</key>
        <string>{100,100}</string>
        <key>frame</key>
        <string>{{50,50},{100,100}}</string>
      </dict>
    </dict>
    <key>metadata</key>
    <dict>
      <key>format</key>
      <integer>2</integer>
      <key>size</key>
      <string>{100,100}</string>
      <key>realTextureFileName</key>
      <string>abc.png</string>
      <key>textureFileName</key>
      <string>abc.png</string>
    </dict>
  </dict>
</plist>
</pre>

[1]: http://zengrong.net/sprite_sheet_editor
[2]: https://github.com/f60k/as3plist
[3]: https://github.com/zrong/as3plist
[4]: http://zengrong.net/post/1981.htm
[5]: https://github.com/zrong/as3plist/tree/master/sample
