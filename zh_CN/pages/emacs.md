# Emacs 的 C/C++/Python 开发环境搭建

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
Emacs 自带 color-theme

## References
1. [Mastering Emacs By Mickey Petersen](https://www.masteringemacs.org/)
2. [Purcell's emacs.d](https://github.com/purcell/emacs.d)
3. [byuksel 的 Emacs配置](https://github.com/byuksel)
4. [叶文彬的 Elisp 入门](http://www.newsmth.net/bbsanc.php?path=%2Fgroups%2Fcomp.faq%2FEmacs%2Felisp%2Fhappierbee%2FM.1184679743.j0&ap=64311)
5. 早期配置不少来自于 Emacs 中文网，李杀的博客和各个独立博客。
