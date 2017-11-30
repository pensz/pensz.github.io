---
layout: post
status: publish
published: true
title: "在windows下编译webkit"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 91
wordpress_url: http://www.zwsun.com/?p=91
date: '2011-08-03 20:33:56 +0000'
date_gmt: '2011-08-03 12:33:56 +0000'
categories:
- "技术记录"
tags:
- webkit
- "编译"
---
<p>这是半年前在windows上编译、调试webkit的一些记录，可能比较乱。</p>
<p><strong>windows下的webkit apple port编译的经验总结</strong></p>
<div>
<ol>
<li>主要参考webkit官网，在不清楚文档中每一步的作用前，老老实实的按照文档做</li>
<li>安装cygwin的时候要选择install from local directory， 否则会缺失包</li>
<li>在WebKitLibraries/win/tools/vsprops目录下面手动建立一个.svn文件夹（可以通过命令行建立）</li>
<li>BuildLog是很好的参考资料，一旦出错，参考该log是比较容易找出问题的</li>
<li>build-webkit 和 通过 vs编译事实上是一样的，因为build-webkit调用的是就是vs的命令行</li>
<li>另外dumprendertree 还是必须将vs编译的选项 treat warning as error改成 no，否则无法编译通过</li>
<li>在编译时还遇到一个问题，就是cl.exe报fatal错误，查了相关资料，后来没做任何处理，重新编译就可以了</li>
</ol>
</div>
<div>
<div></div>
<div><strong>debug调试</strong></div>
<div>用safari作为debug总是会出错，以下为错误信息：</div>
<blockquote>
<div>Unhandled exception at 0x78145215 (msvcr80.dll) in Safari.exe: 0xC0000005: Access violation reading location 0x091bfffc.</div>
<div>memcpy(data, characters, length * sizeof(UChar));</div>
<div>
<div>+		dst	0x091c0034 ""	unsigned char *</div>
</div>
<div>
<div>+		src	0x089e9b10 ",甓"	unsigned char *</div>
</div>
<div>
<div>count	68945734	unsigned long</div>
</div>
<div>memcpy(data, characters, length * sizeof(UChar));</div>
<div>+		data	0x091c0034 ""	wchar_t *</div>
<div>
<div>+		characters	0x089e9b10 "Ҷ"	const wchar_t *</div>
</div>
<div>
<div>length	34472867	unsigned int</div>
</div>
</blockquote>
<div>
<blockquote>
<div>+		string	{m_ptr=0x091c0020 }	WTF::PassRefPtr&lt;WTF::StringImpl&gt;</div>
</blockquote>
</div>
<div>后来使用winLancher作为debug的工具，还是不错的。</div>
<div>或者使用minibrowser作为debug的工具，需要主要的是，minibrowser要attach到webkit2的进程上面</div>
<div>运行这个需要apple的相关dll，这些dll的位置在 C:\Program Files\Common Files\Apple\Apple Application Support</div>
<div>注意，不要把webkit javascriptcore相关文件复制过去。</div>
<div>下面是点击一个按钮到提交form对应的堆栈信息：</div>
<div>
<blockquote>
<div>WebKit.dll!WebCore::HTMLFormElement::prepareSubmit(WebCore::Event * event=0x03417dc0)  Line 265	C++</div>
<div>WebKit.dll!WebCore::HTMLInputElement::defaultEventHandler(WebCore::Event * evt=0x03417dc0)  Line 2207 + 0x13 bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchGenericEvent(WTF::PassRefPtr&lt;WebCore::Event&gt; prpEvent={...})  Line 2747 + 0x1b bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchEvent(WTF::PassRefPtr&lt;WebCore::Event&gt; prpEvent={...})  Line 2631 + 0x12 bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchUIEvent(const  WTF::AtomicString &amp; eventType={...}, int detail=1,  WTF::PassRefPtr&lt;WebCore::Event&gt; underlyingEvent={...})  Line 2799 +  0x22 bytes	C++</div>
<div>WebKit.dll!WebCore::Node::defaultEventHandler(WebCore::Event * event=0x03ec3548)  Line 3021 + 0x23 bytes	C++</div>
<div>WebKit.dll!WebCore::HTMLFormControlElementWithState::defaultEventHandler(WebCore::Event * event=0x03ec3548)  Line 472	C++</div>
<div>WebKit.dll!WebCore::HTMLInputElement::defaultEventHandler(WebCore::Event * evt=0x03ec3548)  Line 2442	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchGenericEvent(WTF::PassRefPtr&lt;WebCore::Event&gt; prpEvent={...})  Line 2747 + 0x1b bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchEvent(WTF::PassRefPtr&lt;WebCore::Event&gt; prpEvent={...})  Line 2631 + 0x12 bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchMouseEvent(const  WTF::AtomicString &amp; eventType={...}, int button=0, int detail=1,  int pageX=435, int pageY=73, int screenX=531, int screenY=178, bool  ctrlKey=false, bool altKey=false, bool shiftKey=false, bool  metaKey=false, bool isSimulated=false, WebCore::Node *  relatedTargetArg=0x00000000, WTF::PassRefPtr&lt;WebCore::Event&gt;  underlyingEvent={...})  Line 2924	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchMouseEvent(const  WebCore::PlatformMouseEvent &amp; event={...}, const WTF::AtomicString  &amp; eventType={...}, int detail=1, WebCore::Node *  relatedTarget=0x00000000)  Line 2833	C++</div>
<div>WebKit.dll!WebCore::EventHandler::dispatchMouseEvent(const  WTF::AtomicString &amp; eventType={...}, WebCore::Node *  targetNode=0x03ee9b48, bool __formal=true, int clickCount=1, const  WebCore::PlatformMouseEvent &amp; mouseEvent={...}, bool setUnder=true)   Line 1845 + 0x23 bytes	C++</div>
<div>WebKit.dll!WebCore::EventHandler::handleMouseReleaseEvent(const  WebCore::PlatformMouseEvent &amp; mouseEvent={...})  Line 1573 + 0x2f  bytes	C++</div>
<div>WebKit.dll!WebView::handleMouseEvent(unsigned int message=514, unsigned int wParam=0, long lParam=4784563)  Line 1402	C++</div>
<div>WebKit.dll!WebView::WebViewWndProc(HWND__  * hWnd=0x000d02da, unsigned int message=514, unsigned int wParam=0,  long lParam=4784563)  Line 2052 + 0x14 bytes	C++</div>
<div>user32.dll!77d18734()</div>
<div>[Frames below may be incorrect and/or missing, no symbols loaded for user32.dll]</div>
<div>user32.dll!77d18816()</div>
<div>user32.dll!77d189cd()</div>
<div>user32.dll!77d191f1()</div>
<div>user32.dll!77d18a10()</div>
<div>WinLauncher_debug.exe!wWinMain(HINSTANCE__  * hInstance=0x00400000, HINSTANCE__ * hPrevInstance=0x00000000, wchar_t  * lpCmdLine=0x00020b14, int nCmdShow=1)  Line 241 + 0x10 bytes	C++</div>
<div>WinLauncher_debug.exe!__tmainCRTStartup()  Line 589 + 0x1c bytes	C</div>
<div>kernel32.dll!7c817077()</div>
</blockquote>
</div>
<div>更加详细的堆栈信息</div>
<div>
<blockquote>
<div>WebKit.dll!WebCore::FrameLoader::submitForm(WTF::PassRefPtr&lt;WebCore::FormSubmission&gt; submission={...})  Line 367	C++</div>
<div>WebKit.dll!WebCore::HTMLFormElement::submit(WebCore::Event  * event=0x03417dc0, bool activateSubmitButton=true, bool  lockHistory=false, WebCore::FormSubmissionTrigger  formSubmissionTrigger=NotSubmittedByJavaScript)  Line 312	C++</div>
<div>WebKit.dll!WebCore::HTMLFormElement::prepareSubmit(WebCore::Event * event=0x03417dc0)  Line 268	C++</div>
<div>WebKit.dll!WebCore::HTMLInputElement::defaultEventHandler(WebCore::Event * evt=0x03417dc0)  Line 2207 + 0x13 bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchGenericEvent(WTF::PassRefPtr&lt;WebCore::Event&gt; prpEvent={...})  Line 2747 + 0x1b bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchEvent(WTF::PassRefPtr&lt;WebCore::Event&gt; prpEvent={...})  Line 2631 + 0x12 bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchUIEvent(const  WTF::AtomicString &amp; eventType={...}, int detail=1,  WTF::PassRefPtr&lt;WebCore::Event&gt; underlyingEvent={...})  Line 2799 +  0x22 bytes	C++</div>
<div>WebKit.dll!WebCore::Node::defaultEventHandler(WebCore::Event * event=0x03ec3548)  Line 3021 + 0x23 bytes	C++</div>
<div>WebKit.dll!WebCore::HTMLFormControlElementWithState::defaultEventHandler(WebCore::Event * event=0x03ec3548)  Line 472	C++</div>
<div>WebKit.dll!WebCore::HTMLInputElement::defaultEventHandler(WebCore::Event * evt=0x03ec3548)  Line 2442	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchGenericEvent(WTF::PassRefPtr&lt;WebCore::Event&gt; prpEvent={...})  Line 2747 + 0x1b bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchEvent(WTF::PassRefPtr&lt;WebCore::Event&gt; prpEvent={...})  Line 2631 + 0x12 bytes	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchMouseEvent(const  WTF::AtomicString &amp; eventType={...}, int button=0, int detail=1,  int pageX=435, int pageY=73, int screenX=531, int screenY=178, bool  ctrlKey=false, bool altKey=false, bool shiftKey=false, bool  metaKey=false, bool isSimulated=false, WebCore::Node *  relatedTargetArg=0x00000000, WTF::PassRefPtr&lt;WebCore::Event&gt;  underlyingEvent={...})  Line 2924	C++</div>
<div>WebKit.dll!WebCore::Node::dispatchMouseEvent(const  WebCore::PlatformMouseEvent &amp; event={...}, const WTF::AtomicString  &amp; eventType={...}, int detail=1, WebCore::Node *  relatedTarget=0x00000000)  Line 2833	C++</div>
<div>WebKit.dll!WebCore::EventHandler::dispatchMouseEvent(const  WTF::AtomicString &amp; eventType={...}, WebCore::Node *  targetNode=0x03ee9b48, bool __formal=true, int clickCount=1, const  WebCore::PlatformMouseEvent &amp; mouseEvent={...}, bool setUnder=true)   Line 1845 + 0x23 bytes	C++</div>
<div>WebKit.dll!WebCore::EventHandler::handleMouseReleaseEvent(const  WebCore::PlatformMouseEvent &amp; mouseEvent={...})  Line 1573 + 0x2f  bytes	C++</div>
<div>WebKit.dll!WebView::handleMouseEvent(unsigned int message=514, unsigned int wParam=0, long lParam=4784563)  Line 1402	C++</div>
<div>WebKit.dll!WebView::WebViewWndProc(HWND__  * hWnd=0x000d02da, unsigned int message=514, unsigned int wParam=0,  long lParam=4784563)  Line 2052 + 0x14 bytes	C++</div>
<div>user32.dll!77d18734()</div>
<div>[Frames below may be incorrect and/or missing, no symbols loaded for user32.dll]</div>
<div>user32.dll!77d18816()</div>
<div>user32.dll!77d189cd()</div>
<div>user32.dll!77d191f1()</div>
<div>user32.dll!77d18a10()</div>
<div>WinLauncher_debug.exe!wWinMain(HINSTANCE__  * hInstance=0x00400000, HINSTANCE__ * hPrevInstance=0x00000000, wchar_t  * lpCmdLine=0x00020b14, int nCmdShow=1)  Line 241 + 0x10 bytes	C++</div>
</blockquote>
</div>
</div>
