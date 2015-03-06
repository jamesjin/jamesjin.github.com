javascript 是怎么在浏览器中加载的?
========================================

浏览器是如何加载 javascript 的? 看这篇文章 http://www.html5rocks.com/zh/tutorials/speed/script-loading/ 就可以基本明白了. 不过, 文章中还是有些地方对我来说讲的还不是很清楚. 下面把这些问题一一列出, 并试着一一解答.

1. css 为什么会阻塞 javascript 的执行?

  如果不管 css 有没有加载完成, 就执行 javascript, 那么会导致执行的 javascript 获取不到正确的样式, 比如颜色. 具体的解释可以参考 HTML5 spec 中的一段 http://www.w3.org/TR/html/single-page.html#a-style-sheet-that-is-blocking-scripts.

>>> Giving up on a style sheet before the style sheet loads, if the style sheet eventually does still load, means that the script might end up operating with incorrect information. For example, if a style sheet sets the color of an element to green, but a script that inspects the resulting style is executed before the sheet is loaded, the script will find that the element is black (or whatever the default color is), and might thus make poor choices (e.g. deciding to use black as the color elsewhere on the page, instead of green). Implementors have to balance the likelihood of a script using incorrect information with the performance impact of doing nothing while waiting for a slow network request to finish.

2. 什么时候可以认为 css 不再会阻塞 javascript 执行了?

  根据 HTML5 spec, 如果 css 的 `style sheet ready <http://www.w3.org/TR/html/single-page.html#style-sheet-ready>`_ 标识被设置为 true, 则该 css 就不再阻塞随后 javascript 的执行. 可惜的是, spec中并没有提及具体的 style sheet ready 标准. 通过对 Chrome 和 Firefox 的测试, 如果 css 下载完成而且其 import 的 css 也 style sheet ready 了, 则认为该 css style sheet ready 了. 也就是说, 即使 css 中引用到的背景图片和自定义字体没有下载完成, 该 css 也可以处于 style sheet ready 状态.

3. document.write 的 script 如何加载?

  根据 HTML5 spec 第 8.2 节 `Parsing HTML documents <http://www.w3.org/TR/html/single-page.html#parsing>`_, document.write 等同于在 script 所处的位置写上相同的 html 文本. 不过最好不要用 document.write, 主要原因有, 如果在 DOM 解析完成以后执行该语句, 会有意想不到的效果, 比如 a. 没效果, b. 页面被覆盖等, 而且 XHTML 是不支持该操作的. 当然, XHTML 也已经渐渐离我们远去了.

4. script-inserted script 会在什么时候执行?

  HTML5 spec 中没有提及此类脚本会在什么时候执行. 经过测试发现, Webkit 是在 DomContentLoaded 事件之后执行, 而 Gecko 是在脚本添加之后马上执行, 脚本的执行不会阻塞 DOM 解析.

5. script-inserted script 在当前主流浏览器的执行顺序到底如何?

  根据 https://wiki.whatwg.org/wiki/Dynamic_Script_Execution_Order 以及文中链接的 Webkit 和 Mozilla 的 bug 的解决情况, 我们可以认为, script-inserted script 在默认情况下是异步执行的, 即脚本执行的顺序与添加的顺序无关; 当 async 设置为 false 时, 是按添加顺序执行的.

6. 把 javascript 声明放在 body 的最后不会让 javascript 的下载延迟吗?

  现代浏览器有 preload 这一步, 在解析 DOM 之前, preload scanner 会扫描 HTML 文件中的所有外部资源链接, 在 HTML 解析器解析到这些资源之前, 就让浏览器先行加载. 与 preload 类似的技术还有 prefetch, preconnect, preresolve等, 具体可以看这篇文章: `Preconnect, prefetch, prerender ... <https://docs.google.com/presentation/d/18zlAdKAxnc51y_kj-6sWLmnjl6TLnaru_WH0LJTjP-o/edit#slide=id.g33a803cd_4_320>`_ .

7. 把 css 声明放在 javascript 之后会有什么问题?

  浏览器并不会等 css 处于 style sheet ready 以后才会触发 DomContentLoaded 事件. 这样 DomContentLoaded 事件发生时, 并不能确保 css 已经加载完成. 所以 css 声明之后至少得有一个 script 声明.

8. 为什么通过 requirejs 加载的 jquery 的 domReady 事件会在 load 事件之后触发?

  这是因为 jquery 在 DomContentLoaded 事件之后加载, jquery 没有监听到这个事件. 根据下面这段 jquery 的源码, 我们可以发现, 下一次触发要等 document.readyState === "complete", 也就是 load 事件之后.

.. code-block:: javascript

	jQuery.ready.promise = function( obj ) {
		if ( !readyList ) {

			readyList = jQuery.Deferred();

			// Catch cases where $(document).ready() is called after the browser event has already occurred.
			// We once tried to use readyState "interactive" here, but it caused issues like the one
			// discovered by ChrisS here: http://bugs.jquery.com/ticket/12282#comment:15
			if ( document.readyState === "complete" ) {
				// Handle it asynchronously to allow scripts the opportunity to delay ready
				setTimeout( jQuery.ready );

			} else {

				// Use the handy event callback
				document.addEventListener( "DOMContentLoaded", completed, false );

				// A fallback to window.onload, that will always work
				window.addEventListener( "load", completed, false );
			}
		}
		return readyList.promise( obj );
	};

总结下,

1. 非 async 和 defer 的 javascript 声明, 会阻塞 HTML 解析器解析 DOM, 从而阻塞页面渲染.
2. 对于需要兼容主流浏览器（包括老版本的IE）的 web 应用, 把 css 声明在 head, 把 javascript 声明在 body 的最后, 是目前最合适的做法了.
3. 对于针对 Chrome 做的应用, 可以继续使用上一条习惯, 但也可以使用 defer 和 async.
4. 现代浏览器很强大, 会采用各种预取技术来提高页面加载速度. 比如, prefetch, preload, preconnect等. 具体可以查看

.. author:: default
.. categories:: javascript
.. tags:: none
.. comments::
