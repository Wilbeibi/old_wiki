# Emacs 的 C/C++/Python 开发环境手册

Note: 本文环境为 OSX 下的 Emacs 24.5。

## 0x01 安装和基础设置
前段时间有空，把 Emacs 升级到24.5了，精简了很多配置，删改了很多依赖，做个 wiki 记录一下。我是通过`brew install emacs --with-cocoa --with-gnutls --with-imagemagick` 安装的。然后再设个软链接到 Application：`ln -s /usr/local/Cellar/emacs/24.x/Emacs.app /Applications`。

`.emacs` 文件主体就这几行，再把各个 wil- 打头的配置文件统一放在`.emacs.d/config` 下：
```python
(add-to-list 'load-path (expand-file-name "config" user-emacs-directory))
(require 'package)
(package-initialize)

(require 'wil-utils)
(require 'wil-org-mode)
(require 'wil-cpp)
(require 'wil-smartcopy)
(require 'wil-ido)
(require 'wil-python)
```
### 包管理
从 version 24 开始，Emacs加入了 elpa 包管理，但是，这是不够滴，常规配置还会加入这三个包:
```
(add-to-list 'package-archives '("org" . "http://orgmode.org/elpa/"))
(add-to-list 'package-archives
             '("marmalade" . "http://marmalade-repo.org/packages/"))
(add-to-list 'package-archives
             '("melpa" . "http://melpa.milkbox.net/packages/") t)
```
### 主题和字体
Emacs 自带 color-theme 里随便选一个。英文字体我习惯 Monaco（如果是 Linux 系统，得用 Monaco-linux）。
中文的话，OSX el capitan 新采用的苹方就挺好看的。
```
(load-theme 'deeper-blue t)
;; Setting English Font
(set-face-attribute
 'default nil :font "Monaco 13")
;; Chinese font
(dolist (charset '(kana han symbol cjk-misc bopomofo))
  (set-fontset-font (frame-parameter nil 'font)
                    charset
                    (font-spec :family "PingFang SC" :size 13)))
```
### 个性化 UI及 OSX 键位设置
这段比较无聊，几乎每个人的配置都有这些，去掉菜单栏，显示行号啊什么的。但官方还是舍不得弄成默认。
```
(tool-bar-mode -1)
(menu-bar-mode -1)
(scroll-bar-mode -1)

;; mode line settings
(global-linum-mode t)
(column-number-mode t)
(size-indication-mode t)
;; enable y/n answers
(fset 'yes-or-no-p 'y-or-n-p)

;; show buffer name
(setq frame-title-format "%b")

;; F2 Set mark
(global-set-key [(f2)] 'set-mark-command)
;; F5 goto line
(global-set-key [f5] 'goto-line)
```
此外，对于 OSX 用户，改下键位还是挺方便的，首先是系统设置里把 Caps lock 和 control 交换（解放小拇指），然后加上:
```
(setq mac-option-modifier 'none)
(setq mac-command-modifier 'meta)
(setq ns-function-modifier 'hyper)
```
这样，`C`就成了 Caps lock, `M` 就成了 command。

### 括号匹配，拼写检查及 org-mode
这些都是做笔记方面需要的。下面两行会自动匹配括号，并且输入左括号的时候自动补出右括号:
```
(electric-pair-mode t)
(show-paren-mode 1)
```

拼写检查我是先`brew install aspell`，再绑定到`ispell-word`上。
```
(setq ispell-program-name "/usr/local/bin/aspell")
(setq ispell-extra-args '("--sug-mode=ultra"))
(global-set-key (kbd "<f8>") 'ispell-word)
```

org-mode 可以配置地方挺多的。外观可以通过 `M-x customize-group RET org-appearance RET`来设置。
我这里只配了关键词高亮（比如 TODO），flycheck（边写边检查拼写）和 [writegood-mode](http://bnbeckwith.com/code/writegood-mode.html)（帮助更好写作）。
```
(setq org-log-done t
      org-todo-keywords '((sequence "TODO" "INPROGRESS" "DONE"))
      org-todo-keyword-faces '(("INPROGRESS" . (:foreground "blue" :weight bold))))
(add-hook 'org-mode-hook
          (lambda ()
            (flyspell-mode)))
(add-hook 'org-mode-hook
          (lambda ()
            (writegood-mode)))
```

此外我`M-x package-install` 装了ibuffer（增强 `C-x C-b`） 和 deft（org 笔记管理），make life a little bit easier。

## 0x02 提高效率的工具
[ido](https://www.masteringemacs.org/article/introduction-to-ido-mode) 是老牌的方便交互的package，自 Emacs 22开始加入默认包（此外，helm 也挺流行的）。我老的配置就是 ido+smex,所以现在延续这个配置。smex 是基于ido的方便找最近访问过的文件的工具，非常好用。此外，我用的不是官方 ido，是一个第三方的 ido 修改版：ido-ubiquitous。源码都在我 github 上.
配置是学 [purcell](https://github.com/purcell/emacs.d/blob/master/init-ido.el) 的。

smartcopy也是很方便的一个配置，这是来源于李杀写的函数：如果没有选中 region 进行复制或剪切，默认就操作当前行。这个非常常用，特方便（刚才找了下，找不到李杀写的这篇帖子了）。

## 0x03 C/C++ 环境配置
之前我用 CEDET 和 cc-mode 弄了个复杂到我自己都看不懂的配置。这回看到 Youtube上这段视频的配置，加一点点修改，就用的非常舒服。
需要说明一点，推荐视频里的 part1, part2 和part3。part4中的 irony 方法虽然很好，但相比 CEDET 少了不少功能，个人还是倾向 CEDET。

他这里代码补全的前端用到了 yasnippet 和 auto-complte。前者是关键词触发补全（比如敲个 main，自动把 main 函数框架补出来），还能自定义关键词及补全内容；
后者是根据上下文输入内容补全（比如给输入一个变量名一次，下次再输入时候就会补全）。

[](https://www.youtube.com/watch?v=HTUE03LnaXA)

Attention: 视频中 `(global-semantic-idle-scheduler-mode 1)`， 这里的 1 请改成 2，否则有一定几率 Emacs 启动时卡死，切记切记。

Hint: 此外，多光标编辑视频中用 iedit，其实 [multiple-cursor](https://github.com/magnars/multiple-cursors.el) 也挺好的。

## 0x04 Python 环境
我是参照[这篇文章](http://onthecode.com/post/2014/03/06/emacs-on-steroids-for-python-elpy-el.html) 配的。基本上 elpy，rope 和 jedi 能把所有事都干了。
以前我把 Emacs 下的 IPython 也配了，但感觉效果并不理想，也确实没啥必要。

## 0x05 常用 mode 快捷键及其他
我以上的配置都在 [github](https://github.com/Wilbeibi/Emacs_Cpp_Python_IDE) 上。
TODO...
### ibuffer
ibuffer是增强版的`C-x C-b`，
### Dired
### cua-mode
### Projectile
### Yasnippet

## emacs cli 版配置

## References
1. [Mastering Emacs By Mickey Petersen](https://www.masteringemacs.org/)
2. [Purcell's emacs.d](https://github.com/purcell/emacs.d)
3. [byuksel 的 Emacs配置](https://github.com/byuksel)
4. [叶文彬的 Elisp 入门](http://www.newsmth.net/bbsanc.php?path=%2Fgroups%2Fcomp.faq%2FEmacs%2Felisp%2Fhappierbee%2FM.1184679743.j0&ap=64311)
5. 早期配置不少来自于 Emacs 中文网，李杀的博客和各个独立博客。
[gimmick:ForkMeOnGitHub (position: 'right', color: 'red') ](https://github.com/Wilbeibi/Emacs_Cpp_Python_IDE)
