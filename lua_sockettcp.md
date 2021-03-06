[一个LuaSocket封装](http://zengrong.net/post/1980.htm)

**2014-01-10更新：** SocketTCP 已经进入 quick-cocos2d-x 的 framework.
<hr>
在 [quick-cocos2d-x 中的 socket 技术选择：LuaSocket 和 WebSocket][1] 一文中，我提到了选择 [LuaSocket][2] 与服务器通信。

为了方便使用，我对LuaSocket进行了封装。封装主要做了这样几件事：

1. 封装基于 LuaSocket 的 TCP 模式，使用 settimeout 实现异步调用；
2. 利用 cocos2d-x 提供的定时器实现失败重连；
3. 利用 quick-cocos2d-x 提供的 [framework.api.EventProtocol][4] 实现了事件的注册和发布。

这个封装完全依赖 quick-cocos2d-x ，因此不能单独在lua环境中使用。

这个封装修改自 [quick 论坛的一个源码][3] ，感谢原作者！

下面的例子依赖 [ByteArray][5]，详情看这里：[用lua实现ByteArray和ByteArrayVarint][5] 。<!--more-->

	socket = SocketTCP.new("192.168.18.22", 12001, false)
	socket:addEventListener(SocketTCP.EVENT_CONNECTED, onStatus)
	socket:addEventListener(SocketTCP.EVENT_CLOSE, onStatus)
	socket:addEventListener(SocketTCP.EVENT_CLOSED, onStatus)
	socket:addEventListener(SocketTCP.EVENT_CONNECT_FAILURE, onStatus)
	socket:addEventListener(SocketTCP.EVENT_DATA, onData)
	
	socket:send(ByteArray.new():writeByte(0x59):getPack())

	function onStatus(__event)
		echoInfo("socket status: %s", __event.name)
	end

	function onData(__event)
		echoInfo("socket status: %s, data:%s", __event.name, ByteArray.toString(__event.data))
	end

SocketTCP 位于我的 [lua库][6] 项目中。

我在 [quick-cocos2d-x sample][9] 中提交了一个 sample 介绍用法。

我会把自己在游戏中常用的 lua 功能陆续整合进入这个开源库，就像我的 [as3库][7] 和 [java库][8] 一样。

[1]: http://zengrong.net/post/1965.htm
[2]: http://w3.impa.br/%7Ediego/software/luasocket/
[3]: http://cn.quick-x.com/?topic=quickkydsocketfzl
[4]: https://github.com/dualface/quick-cocos2d-x/blob/develop/framework/api/EventProtocol.lua
[5]: http://zengrong.net/post/1968.htm
[6]: https://github.com/zrong/lua
[7]: https://github.com/zrong/as3
[8]: https://github.com/zrong/java
[9]: https://github.com/dualface/quick-cocos2d-x/tree/develop/samples/luasocket
