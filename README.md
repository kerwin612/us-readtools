## 安装
https://openuserjs.org/install/kerwin612/ReadTools.user.js

## 辅助阅读工具  
**在 屏幕左侧 右击 触发 上一页 按钮，在 屏幕右侧 右击 触发 下一页 按钮；“扩大”翻页的区域，提高翻页效率，提升阅读体验**  
  
将屏幕 **纵向从下往上三分之二** 的区域划分为 **左右** 两部分，分别映射到 **上一页** 和 **下一页** 两个按钮上，用 **鼠标右键** 触发；  
当在屏幕 **左侧右击** 鼠标时，触发 **上一页** 按钮；  
当在屏幕 **右侧右击** 鼠标时，触发 **下一页** 按钮；  
本脚本通过 **支持自定义配置** 来扩展适配任何你需要的网站；  
配置项为：`funcConfig`，该值的格式为`object`：`key`为 **匹配网站URL的 正则表达式 或 前缀** ，`value`为 **函数字符串** ；  
默认适配了 [doukan.com](https://www.duokan.com/) & [gitbook.io](https://kerwin612.gitbook.io) ，如下：
```
funcConfig = {
	"^.*duokan.com/reader/www/app.html":
		"function() {\
			return {\
					prevLink: document.getElementsByClassName('j-pageup')[0],\
					nextLink: document.getElementsByClassName('j-pagedown')[0]\
			};\
		}",
	"^.*gitbook\\.io.*$": 
		"function() {\
			let loopEle = function(ele, classNamePattern) {\
				if (new RegExp(classNamePattern).test(ele.className)) return ele;\
				let rst = null;\
				for (let i = 0; i < ele.children.length; i++) {\
					if ((rst = loopEle(ele.children[i], classNamePattern))) break;\
				}\
				return rst;\
			};\
			let classBase;\
			let navPagesLinks;\
			let resultObj = {};\
			if ((classBase = document.getElementById('__GITBOOK__ROOT__CLIENT__').firstChild.className.split('--')[0]) && (navPagesLinks = loopEle(document.getElementById('__GITBOOK__ROOT__CLIENT__'), '^'+ classBase + '--navPagesLinks-.+$'))) {\
				navPagesLinks.children.forEach((val) => {\
					if (new RegExp('^'+ classBase + '.*--cardPrevious-.+$').test(val.className)) resultObj.prevLink = val;\
					if (new RegExp('^'+ classBase + '.*--cardNext-.+$').test(val.className)) resultObj.nextLink = val;\
				});\
			}\
			return resultObj;\
		}",
}
```  
如上，`value`为 **函数字符串** ，此函数必须返回网站的 **上一页** 和 **下一页** 两个按钮的 **节点对象** ，以便于脚本触发；
