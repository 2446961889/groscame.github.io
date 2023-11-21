+++
title = "emacs配置"
author = ["wacl"]
draft = false
+++

```emacs-lisp
(defun generate-package-list (packages)
  "Generate package! statements for a list of packages."
  (mapconcat (lambda (package) (format "(package! %s)" package))
             packages
             "\n"))
(defun write-package-statements-to-file (packages filepath)
  "Write package! statements for the given packages to the specified file."
  (let* ((result (generate-package-list packages))
         (header ";; -*- no-byte-compile: t; -*-\n;;; $DOOMDIR/packages.el\n"))
    (with-temp-file filepath
      (insert header)
      (insert result))))

(setq package-list '(js2-mode simple-httpd all-the-icons all-the-icons-completion all-the-icons-dired async autorevert avy beacon bind-key cal-china-x calfw calfw-org calibredb cape capf-autosuggest cnfonts company company-tabnine consult consult-notes corfu counsel counsel-projectile crux delsel denote diff-hl diminish diredfl dirvish djvu doom-modeline doom-themes editorconfig ef-themes elfeed elfeed-goodies elfeed-org embark embark-consult eshell eshell-git-prompt eshell-syntax-highlighting eshell-up evil evil-collection evil-goggles evil-matchit evil-traces eww exec-path-from-shell expand-region fanyi fontaine gnuplot go-translate helm helpful htmlize hydra indium ivy ivy-yasnippet js-comint keycast ledger-mode magit magit-delta marginalia minions multiple-cursors nerd-icons netease-cloud-music no-littering nov ob-http orderless org-ai org-appear org-auto-tangle org-contrib org-modern org-roam org-transclusion ox-gfm ox-hugo ox-pandoc ox-reveal pangu-spacing paren pass pinyinlib plantuml-mode popper posframe projectile py-autopep8 pyim pyim-basedict python quelpa quelpa-use-package rainbow-delimiters request sdcv sendmail sh-script shackle shr shrink-path skewer-mode smartparens super-save toc-org try undo-tree use-package use-package-ensure-system-package valign vertico web-mode which-key xr yasnippet yasnippet-snippets))
(write-package-statements-to-file package-list "~/.doom.d/packages.el")
(org-babel-tangle)
```

<div class="results">

("c:/Users/wacl/.spacemacs" "c:/Users/wacl/.doom.d/custom.el" "c:/Users/wacl/.doom.d/config.el" "c:/Users/wacl/.doom.d/init.el" "c:/Users/wacl/.emacs.d.purcell/lisp/init-local.el" "c:/Users/wacl/.emacs.d.centaur/custom.el" "c:/Users/wacl/.emacs.d/emacs-babel-dired.el" "c:/Users/wacl/.emacs.d/emacs-babel-ox-export.el" "c:/Users/wacl/.emacs.d/emacs-babel-plot.el" "c:/Users/wacl/.emacs.d/emacs-babel-js.el" "c:/Users/wacl/begin_new_life/org/eshell/aliases" "c:/Users/wacl/.custom.el" "c:/Users/wacl/emacs-babel.el" "c:/Users/wacl/.emacs-profile")

</div>

```emacs-lisp
d
```


## PACKAGE {#package}


### DISLIKE {#dislike}

```org
ol-notmuch notmuch doom-snippets
```


### LIKE {#like}

<a id="code-snippet--package-like"></a>
```org
js2-mode simple-httpd all-the-icons all-the-icons-completion all-the-icons-dired async autorevert avy beacon bind-key cal-china-x calfw calfw-org calibredb cape capf-autosuggest cnfonts company company-tabnine consult consult-notes corfu counsel counsel-projectile crux delsel denote diff-hl diminish diredfl dirvish djvu doom-modeline doom-themes editorconfig ef-themes elfeed elfeed-goodies elfeed-org embark embark-consult eshell eshell-git-prompt eshell-syntax-highlighting eshell-up evil evil-collection evil-goggles evil-matchit evil-traces eww exec-path-from-shell expand-region fanyi fontaine gnuplot go-translate helm helpful htmlize hydra indium ivy ivy-yasnippet js-comint keycast ledger-mode magit magit-delta marginalia minions multiple-cursors nerd-icons netease-cloud-music no-littering nov ob-http orderless org-ai org-appear org-auto-tangle org-contrib org-modern org-roam org-transclusion ox-gfm ox-hugo ox-pandoc ox-reveal pangu-spacing paren pass pinyinlib plantuml-mode popper posframe projectile py-autopep8 pyim pyim-basedict python quelpa quelpa-use-package rainbow-delimiters request sdcv sendmail sh-script shackle shr shrink-path skewer-mode smartparens super-save toc-org try undo-tree use-package use-package-ensure-system-package valign vertico web-mode which-key xr yasnippet yasnippet-snippets
```


### IN {#in}

```org
appt autorevert calendar delsel dired dired-aux dired-x elisp-mode em-hist em-rebind em-term emacs esh-mode eshell eww lilypond-mode message notifications ol org org-agenda org-capture org-habit org-src ox ox-extra ox-html ox-latex ox-publish ox-reveal paren parse-time python recentf savehist saveplace sendmail sh-script shr simple vc winner
```


### GITHUB {#github}

<a id="code-snippet--package-github"></a>
```org
org-super-links sdcv copilot
```


## <span class="org-todo done NEXT">NEXT</span> OL-NOTMUCH tangle ~/emacs-babel.el {#ol-notmuch-tangle-emacs-babel-dot-el}

```emacs-lisp
(use-package ol-notmuch
  :ensure t)
```


## <span class="org-todo done NEXT">NEXT</span> NOTMUCH 邮件系统配置。 {#notmuch-邮件系统配置}

```emacs-lisp
(use-package notmuch
  :ensure t
  :commands notmuch
  :hook ((window-setup . notmuch)
         (notmuch-hello-refresh . notmuch-unread-count))
  :bind (("\e\em" . notmuch)
         ("C-x m" . notmuch-mua-new-mail) ; override `compose mail'
         :map notmuch-hello-mode-map
         ("q"     . quit-window)
         :map notmuch-show-mode-map
         ("C-c f" . my/capture-mail-follow-up)
         ("C-c r" . my/capture-mail-read-later)
         :map notmuch-search-mode-map
         ("/"     . notmuch-search-filter)
         ("C-c f" . my/capture-mail-follow-up))
  :init
  (setq notmuch-search-oldest-first nil) ; newest on the top
  :config
  ;; 设置notmuch邮件快速记录的模板
  (require 'org-capture)
  (add-to-list 'org-capture-templates
               '("e" "Email follow up" entry
                 (file+headline "mail.org" "Follow Up")
                 "* TODO [#A] %:subject :mail:\nSCHEDULED: %t\nDEADLINE: %(org-insert-time-stamp (org-read-date nil t \"+2d\"))\n\n%a%?"
                 :empty-lines-after 1
                 :prepend t
                 :immediate-finish t
                 :jump-to-captured t
                 ))
  (add-to-list 'org-capture-templates-contexts
               '("e" ((in-mode . "notmuch-search-mode")
                      (in-mode . "notmuch-show-mode"))))

  ;; 在邮件界面快速记录需要跟进的邮件
  (defun my/capture-mail-follow-up ()
    (interactive)
    (call-interactively 'org-store-link)
    (org-capture nil "e"))

  ;; custom UI
  (setq notmuch-show-logo t)
  (setq notmuch-column-control 1.0)
  (setq notmuch-hello-recent-searches-max 20)
  (setq notmuch-hello-thousands-separator ",")
  (setq notmuch-hello-sections '(notmuch-hello-insert-header
                                 notmuch-hello-insert-saved-searches
                                 notmuch-hello-insert-alltags
                                 notmuch-hello-insert-footer))
  (setq notmuch-show-all-tags-list t)

  ;; set saved searches, use `j' to jump to the search
  (setq notmuch-saved-searches '(
                                 (:name "all"
                                        :query "*"
                                        :sort-order newest-first
                                        :key "l")

                                 (:name "archived"
                                        :query "tag:archived"
                                        :sort-order newest-first
                                        :key "A")

                                 (:name "inbox"
                                        :query "tag:inbox"
                                        :sort-order newest-first
                                        :key "i")

                                 (:name "sent"
                                        :query "tag:sent"
                                        :sort-order newest-first
                                        :key "s")
                                 ))
  (setq notmuch-archive-tags '("-inbox" "+archived"))

  ;; Email composition
  (setq notmuch-mua-user-agent-function #'notmuch-mua-user-agent-full)

  ;; Reading messages
  (setq notmuch-wash-citation-lines-prefix 6) ; default is 3
  (setq notmuch-wash-citation-lines-suffix 6)
  (setq notmuch-unthreaded-show-out nil)
  (setq notmuch-message-headers '("To" "Cc" "Subject" "Date"))

  ;; notmuch notifications on mode-line based from notmuch-unread-mode, refer to:
  ;; https://www.reddit.com/r/emacs/comments/esxjh5/my_notmuch_email_count_display_in_modeline/
  (defvar notmuch-unread-mode-line-string "")
  (defvar notmuch-unread-email-count nil)
  (defconst my-mode-line-map (make-sparse-keymap))

  (defun notmuch-unread-count ()
    (setq notmuch-unread-email-count (string-to-number (replace-regexp-in-string "\n" "" (notmuch-command-to-string "count" "tag:unread"))))
    (if (eq notmuch-unread-email-count 0)
        (setq notmuch-unread-mode-line-string (format "? 0 "))
      (setq notmuch-unread-mode-line-string (format "? %d " notmuch-unread-email-count)))
    (force-mode-line-update))

  (defun notmuch-open-emails ()
    (interactive)
    (if (eq notmuch-unread-email-count 0) (notmuch-search "tag:inbox") (notmuch-search "tag:unread")))

  (setq global-mode-string
        (append global-mode-string (list '(:eval (propertize notmuch-unread-mode-line-string 'help-echo "notmuch emails" 'mouse-face 'mode-line-highlight 'local-map my-mode-line-map)))))

  (define-key my-mode-line-map
              (vconcat [mode-line down-mouse-1])
              (cons "hello" 'notmuch-open-emails))

  ;; 每三分钟刷新一下notmuch，在刷新的时候会执行 `notmuch-unread-count' hook
  ;; 来执行状态栏的邮件数量更新
  (run-at-time t 180 #'notmuch-refresh-all-buffers)
  )
```


## <span class="org-todo todo TODO">TODO</span> 低龄阶段 {#低龄阶段}

在 Emacs 刚启动，还未加载主要配置文件时的配置文件。
:tangle ~/emacs-babel.el

```elisp
;; 启动早期不加载`package.el'包管理器
(setq package-enable-at-startup nil)
;; 不从包缓存中加载
(setq package-quickstart nil)

;; 禁止展示菜单栏、工具栏和纵向滚动条
(push '(menu-bar-lines . 0) default-frame-alist)
(push '(tool-bar-lines . 0) default-frame-alist)
(push '(vertical-scroll-bars) default-frame-alist)

;; 禁止自动缩放窗口先
(setq frame-inhibit-implied-resize t)

;; 禁止菜单栏、工具栏、滚动条模式，禁止启动屏幕和文件对话框
(menu-bar-mode -1)
(tool-bar-mode -1)
(scroll-bar-mode -1)
(setq inhibit-splash-screen t)
(setq use-file-dialog nil)

;; 在这个阶段不编译
;;(setq comp-deferred-compilation nil)
```


## init.el 是 Emacs 的主要配置文件。 {#init-dot-el-是-emacs-的主要配置文件}

FIRST_第一阶段，用 PACKAGE 下载 USEPACKAGE


### STANDARD 文件头 {#standard-文件头}

```emacs-lisp
;;; init.el --- The main init entry for Emacs -*- lexical-binding: t -*-
;;; Commentary:

;;; Code:
```


### PACKAGE 包管理配置 {#package-包管理配置}

```emacs-lisp
(require 'package)
;;(setq package-enable-at-startup nil)
(setq package-archives
      '(("melpa"  . "https://melpa.org/packages/")
        ("gnu"    . "https://elpa.gnu.org/packages/")
        ("nongnu" . "https://elpa.nongnu.org/nongnu/")))

(add-to-list 'package-archives
             '("melpa" . "https://melpa.org/packages/"))

(package-initialize)
```


### USE-PACKAGE 插件安装 {#use-package-插件安装}

[use-package](https://github.com/jwiegley/use-package) 是一个让 Emacs 配置更加结构化更加清晰的一个宏插件。

```emacs-lisp
;; 安装 `use-package'
;;(when (not package-archive-contents)
;;(package-refresh-contents)
;;)
(unless (package-installed-p 'use-package)
  (package-refresh-contents)
  (package-install 'use-package))

;;(unless (and (package-installed-p 'delight)
;; (package-installed-p 'use-package)))
;; 配置 `use-package'
(eval-and-compile
  (setq use-package-always-ensure nil)
  (setq use-package-always-defer nil)
  (setq use-package-expand-minimally nil)
  (setq use-package-enable-imenu-support t)
  (if (daemonp)
      (setq use-package-always-demand t)))

(eval-when-compile
  (require 'use-package))

;; 安装 `use-package' 的集成模块
(use-package use-package-ensure-system-package
  :ensure t)

(use-package diminish
  :ensure t);;(require 'diminish)

(use-package bind-key
  :ensure t)
```


### 包管理器 QUELPA {#包管理器-quelpa}

[quelpa](https://github.com/quelpa/quelpa) 是配合 `package.el` 使用的，基于 git 的一个包管理器。

```emacs-lisp
(unless (package-installed-p 'quelpa)
  (with-temp-buffer
    (url-insert-file-contents "https://raw.githubusercontent.com/quelpa/quelpa/master/quelpa.el")
    (eval-buffer)
    (quelpa-self-upgrade)))

;; 安装 `quelpa'
(use-package quelpa
  :ensure t
  :commands quelpa
  :config
  :custom
  (quelpa-git-clone-depth 1)
  (quelpa-update-melpa-p nil)
  (quelpa-self-upgrade-p nil)
  (quelpa-checkout-melpa-p nil))

;; `quelpa' 与 `use-package' 集成

(use-package quelpa-use-package
  :ensure t
  :init
  (setq quelpa-use-package-inhibit-loading-quelpa t)
  :demand t)
```

```emacs-lisp
(unless (package-installed-p 'quelpa-use-package)
  (quelpa
   '(quelpa-use-package
     :fetcher git
     :url "https://github.com/quelpa/quelpa-use-package.git")))
```


## FONTAINE 字体设置 {#fontaine-字体设置}

<https://github.com/search?q=FONTAINE>
[fontaine](https://protesilaos.com/emacs/fontaine) 插件可以根据需要高度定制字体。

> 这篇文章可以作为字体设置的参考：
> <http://xahlee.info/emacs/emacs/emacs_list_and_set_font.html>

```emacs-lisp
(use-package fontaine
  :ensure t)

(setq fontaine-latest-state-file
      (locate-user-emacs-file "fontaine-latest-state.eld"))

;; Iosevka Comfy is my highly customised build of Iosevka with
;; monospaced and duospaced (quasi-proportional) variants as well as
;; support or no support for ligatures:
;; <https://git.sr.ht/~protesilaos/iosevka-comfy>.
;;
;; Iosevka Comfy            == monospaced, supports ligatures
;; Iosevka Comfy Fixed      == monospaced, no ligatures
;; Iosevka Comfy Duo        == quasi-proportional, supports ligatures
;; Iosevka Comfy Wide       == like Iosevka Comfy, but wider
;; Iosevka Comfy Wide Fixed == like Iosevka Comfy Fixed, but wider
(setq fontaine-presets
      '((tiny
         :default-family "Iosevka Comfy Wide Fixed"
         :default-height 90)
        (small
         :default-family "Iosevka Comfy Fixed"
         :default-height 100)
        (medium
         :default-family "Iosevka Comfy"
         :default-weight semilight
         :default-height 140
         :fixed-pitch-family nil ; falls back to :default-family
         :fixed-pitch-weight nil ; falls back to :default-weight
         :fixed-pitch-height 1.0
         :variable-pitch-family "FiraGO"
         :variable-pitch-weight normal
         :variable-pitch-height 1.05
         :bold-family nil ; use whatever the underlying face has
         :bold-weight bold
         :italic-family nil
         :italic-slant italic
         :line-spacing nil)
        (regular
         :default-family "Hack"
         :default-weight regular
         :default-height 140
         :fixed-pitch-family "Fira Code"
         :fixed-pitch-weight nil ; falls back to :default-weight
         :fixed-pitch-height 1.0
         :variable-pitch-family "Noto Sans"
         :variable-pitch-weight normal
         :variable-pitch-height 1.0
         :bold-family nil ; use whatever the underlying face has
         :bold-weight bold
         :italic-family "Source Code Pro"
         :italic-slant italic
         :line-spacing 1)
        (large
         :default-family "Iosevka"
         :default-weight normal
         :default-height 180
         :fixed-pitch-family nil ; falls back to :default-family
         :fixed-pitch-weight nil ; falls back to :default-weight
         :fixed-pitch-height 1.0
         :variable-pitch-family "FiraGO"
         :variable-pitch-weight normal
         :variable-pitch-height 1.05
         :bold-family nil ; use whatever the underlying face has
         :bold-weight bold
         :italic-family nil ; use whatever the underlying face has
         :italic-slant italic
         :line-spacing 1)
        (presentation
         :default-weight semilight
         :default-height 170
         :bold-weight extrabold)
        (jumbo
         :default-weight semilight
         :default-height 220
         :bold-weight extrabold)
        (t
         ;; I keep all properties for didactic purposes, but most can be
         ;; omitted.  See the fontaine manual for the technicalities:
         ;; <https://protesilaos.com/emacs/fontaine>.
         ;; :default-family "Iosevka Comfy"
         :default-family "Source Code Pro"
         :fixed-pitch-family "Source Code Pro"
         :variable-pitch-family "Source Code Pro"
         :italic-family "Source Code Pro"
         :variable-pitch-weight normal
         :bold-weight normal
         :italic-slant italic
         :default-weight regular
         :default-height 100
         :fixed-pitch-family nil ; falls back to :default-family
         :fixed-pitch-weight nil ; falls back to :default-weight
         :fixed-pitch-height 1.0
         :fixed-pitch-serif-family nil ; falls back to :default-family
         :fixed-pitch-serif-weight nil ; falls back to :default-weight
         :fixed-pitch-serif-height 1.0
         :variable-pitch-family "Iosevka Comfy Duo"
         :variable-pitch-weight nil
         :variable-pitch-height 1.0
         :bold-family nil ; use whatever the underlying face has
         :bold-weight bold
         :italic-family nil
         :italic-slant italic
         :line-spacing nil)))

;; Recover last preset or fall back to desired style from
;; `fontaine-presets'.
(fontaine-set-preset (or (fontaine-restore-latest-preset) 'regular))

;; The other side of `fontaine-restore-latest-preset'.
(add-hook 'kill-emacs-hook #'fontaine-store-latest-preset)

;; fontaine does not define any key bindings.  This is just a sample that
;; respects the key binding conventions.  Evaluate:
;;
;;     (info "(elisp) Key Binding Conventions")
(define-key global-map (kbd "C-c f") #'fontaine-set-preset)
(define-key global-map (kbd "C-c F") #'fontaine-set-face-font)

(create-fontset-from-fontset-spec
     (font-xlfd-name
      (font-spec :family "InconsolataGo QiHei NF"
                 :registry "fontset-modeline fontset")))

    (dolist (charset '(kana han cjk-misc bopomofo unicode))
      (set-fontset-font "fontset-modeline fontset"  charset
                        (font-spec :family "InconsolataGo QiHei NF")))
    (set-fontset-font "fontset-modeline fontset" 'ascii
                      (font-spec :family "Cascadia Mono"))

    (dolist (face '(mode-line mode-line-inactive mode-line-buffer-id
                              mode-line-highlight mode-line-active
                              mode-line-emphasis))
      (set-face-attribute face nil :fontset "fontset-modeline fontset"))
    (dolist (sym '(?● ?■ ?◢))
      (set-fontset-font "fontset-modeline fontset" sym
                        (font-spec :family "Sarasa Mono SC Nerd" :size 12)))

;; set emoji font
(set-fontset-font t
                  (if (version< emacs-version "28.1")
                      '(#x1f300 . #x1fad0)
                    'emoji)
                  (cond
                   ((member "Noto Emoji" (font-family-list)) "Noto Emoji")
                   ((member "Symbola" (font-family-list)) "Symbola")
                   ((member "Apple Color Emoji" (font-family-list)) "Apple Color Emoji")
                   ((member "Noto Color Emoji" (font-family-list)) "Noto Color Emoji")
                   ((member "Segoe UI Emoji" (font-family-list)) "Segoe UI Emoji")))

(dolist (charset '(kana han cjk-misc bopomofo))
  (set-fontset-font (frame-parameter nil 'font) charset
           (font-spec :family "Sarasa Mono SC Nerd")))
(set-face-attribute 'font-lock-comment-face nil :font "LXGW WenKai")

;; set Chinese font
(dolist
  (charset '(kana han symbol cjk-misc bopomofo))
  (set-fontset-font
   (frame-parameter nil 'font)
   charset
   (font-spec :family
              (cond
               ((eq system-type 'darwin)
                (cond
                 ((member "Sarasa Mono SC Nerd" (font-family-list)) "Sarasa Mono SC Nerd")
                 ((member "PingFang SC" (font-family-list)) "PingFang SC")
                 ((member "WenQuanYi Zen Hei" (font-family-list)) "WenQuanYi Zen Hei")
                 ((member "Microsoft YaHei" (font-family-list)) "Microsoft YaHei")))

               ((eq system-type 'gnu/linux)
                (cond
                 ((member "Sarasa Mono SC Nerd" (font-family-list)) "Sarasa Mono SC Nerd")
                 ((member "WenQuanYi Micro Hei" (font-family-list)) "WenQuanYi Micro Hei")
                 ((member "WenQuanYi Zen Hei" (font-family-list)) "WenQuanYi Zen Hei")
                 ((member "Microsoft YaHei" (font-family-list)) "Microsoft YaHei")))

               (t
                (cond
                 ((member "Sarasa Mono SC Nerd" (font-family-list)) "Sarasa Mono SC Nerd")
                 ((member "Microsoft YaHei" (font-family-list)) "Microsoft YaHei")))))))


;; set Chinese font scale
(setq face-font-rescale-alist `(
                                ("Symbola"             . 1.3)
                                ("Microsoft YaHei"     . 1.2)
                                ("WenQuanYi Zen Hei"   . 1.2)
                                ("Sarasa Mono SC Nerd" . 1.2)
                                ("PingFang SC"         . 1.16)
                                ("Lantinghei SC"       . 1.16)
                                ("Kaiti SC"            . 1.16)
                                ("Yuanti SC"           . 1.16)
                                ("Apple Color Emoji"   . 0.91)))
```

```emacs-lisp
(use-package fontaine
  :ensure t
  :when (display-graphic-p)
  ;; :hook (kill-emacs . fontaine-store-latest-preset)
  :config
  ;;(setq fontaine-latest-state-file (locate-user-emacs-file "etc/fontaine-latest-state.eld"))

  ;; (fontaine-set-preset (or (fontaine-restore-latest-preset) 'regular))
  (fontaine-set-preset 'regular)
```

<a id="table--测试中英文字体对齐"></a>
<div class="table-caption">
  <span class="table-number"><a href="#table--测试中英文字体对齐">Table 1</a>:</span>
  测试中英文字体对齐
</div>

| 中文 |   |
|----|---|
| abcd |   |


## <span class="org-todo done DONE">DONE</span> RESTART-EMACS {#restart-emacs}

<https://github.com/search?q=restart-emacs>

```emacs-lisp
(use-package restart-emacs
  :ensure t)
```


## <span class="org-todo done DONE">DONE</span> TRY {#try}

```emacs-lisp
(use-package try
  :ensure t)
```


## <span class="org-todo done DONE">DONE</span> ALL-THE-ICONS 图标设置 {#all-the-icons-图标设置}

```emacs-lisp
(use-package all-the-icons
  :ensure t
  :when (display-graphic-p)
  :commands all-the-icons-install-fonts)
```

```emacs-lisp
(when (display-graphic-p)
  (require 'all-the-icons))
;; or
(use-package all-the-icons
  :if (display-graphic-p))
```


## <span class="org-todo done DONE">DONE</span> PAREN 高亮匹配的括号 {#paren-高亮匹配的括号}

```emacs-lisp
(use-package paren
  :ensure nil
  :hook (after-init . show-paren-mode)
  :custom
  (show-paren-when-point-inside-paren t)
  (show-paren-when-point-in-periphery t))
;; (show-paren-mode t)
```


## <span class="org-todo done DONE">DONE</span> RAINBOW-DELIMETERS 多彩括号 {#rainbow-delimeters-多彩括号}

[rainbow-delimiters](https://github.com/Fanael/rainbow-delimiters) 插件将多彩显示括号等分隔符。

```emacs-lisp
(use-package rainbow-delimiters
  :ensure t
  :hook (prog-mode . rainbow-delimiters-mode))
;; (add-hook 'prog-mode-hook #'rainbow-delimiters-mode)
```


## AUTOREVERT 自动重载设置 {#autorevert-自动重载设置}

当我们的文件发生了改变后，我们希望 Emacs 里打开的永远是最新的文件，这个时候，我们需要对自动重载进行设置，让我们的 Emacs 在文件发生改变的时候自动重载文件。

```emacs-lisp
(use-package autorevert
  :ensure nil
  :hook (after-init . global-auto-revert-mode)
  :bind ("s-u" . revert-buffer)
  :custom
  (auto-revert-interval 10)
  (auto-revert-avoid-polling t)
  (auto-revert-verbose t)
  (auto-revert-remote-files nil)
  (auto-revert-check-vc-info t)
  (global-auto-revert-non-file-buffers t))
;; (global-auto-revert-mode 1)
```


## COMMON-LISP {#common-lisp}

```emacs-lisp
(setq inferior-lisp-program "~/scoop/apps/win-portacle/current/win/bin/sbcl.exe")
(setq image-use-external-converter t)
(setq org-hugo-base-dir "~/begin_new_life/org/org/quickstart")
```


## CRUX 系统增强 {#crux-系统增强}

[crux](https://github.com/bbatsov/crux) 插件提供一系列的增强，如移动增强、删除增强等优化功能。

```emacs-lisp
(use-package crux
  :ensure t
  :bind (("C-a" . crux-move-beginning-of-line)
         ("C-x 4 t" . crux-transpose-windows)
         ("C-x K" . crux-kill-other-buffers)
         ("C-k" . crux-smart-kill-line)
         ("C-c r" . crux-rename-file-and-buffer)
         ("C-x DEL" . crux-kill-line-backwards))
  :config
  (crux-with-region-or-buffer indent-region)
  (crux-with-region-or-buffer untabify)
  (crux-with-region-or-point-to-eol kill-ring-save)
  (defalias 'rename-file-and-buffer #'crux-rename-file-and-buffer))
```


## <span class="org-todo todo WAIT">WAIT</span> 在 Mac 下，我的默认启动窗口大小 {#在-mac-下-我的默认启动窗口大小}

窗口设置调整启动窗口大小 tangle ~/emacs-babel.el

```emacs-lisp
;; 设置窗口大小，仅仅在图形界面需要设置
(when (display-graphic-p)
  (let ((top    0)                                     ; 顶不留空
        (left   (/ (x-display-pixel-width) 10))        ; 左边空10%
        (height (round (* 0.8                          ; 窗体高度为0.8倍的显示高度
                          (/ (x-display-pixel-height)
                             (frame-char-height))))))
    (let ((width  (round (* 2.5 height))))             ; 窗体宽度为2.5倍高度
      (setq default-frame-alist nil)
      (add-to-list 'default-frame-alist (cons 'top top))
      (add-to-list 'default-frame-alist (cons 'left left))
      (add-to-list 'default-frame-alist (cons 'height height))
      (add-to-list 'default-frame-alist (cons 'width width)))))
```


## NO-LITTERING 让配置目录简洁 {#no-littering-让配置目录简洁}

[no-littering](https://github.com/emacscollective/no-littering) 插件将原本放在 `.emacs.d` 目录下的一些配置信息或动态信息，转移到 `etc` 或 `var` 子目录里，让配置目录更加简洁清爽。

```emacs-lisp
(use-package no-littering
  :ensure t)
```


## 个人函数定义 {#个人函数定义}

以下是一些个人的函数定义，配置文件的其他部分会用到这些函数。

```emacs-lisp
;; 将列表加入到列表
(defun add-list-to-list (dst src)
  "Similar to `add-to-list', but accepts a list as 2nd argument"
  (set dst
       (append (eval dst) src)))

;; 打开当前配置Org文件
(defun open-emacs-config ()
  "Opens emacs config org file."
  (interactive)
  (find-file (locate-user-emacs-file "emacs-config.org")))

;; 从剪贴板获取内容
(defun clipboard/get ()
  "return the content of clipboard as string"
  (interactive)
  (with-temp-buffer
    (clipboard-yank)
    (buffer-substring-no-properties (point-min) (point-max))))
```


## PINYINLIB {#pinyinlib}

support Pinyin first character match for orderless, avy etc.

```emacs-lisp
(use-package pinyinlib
  :ensure t)
```


## <span class="org-todo todo TODO">TODO</span> TEX {#tex}

```emacs-lisp
(defun tex-view() (interactive) (tex-send-command "evince" (tex-append tex-print-file ".pdf")))
;;(use-package tex
 ;; :ensure auctex)
;;(use-package auctex
  ;;:ensure t)
```


## VERTICO Emacs 的补全设置。 {#vertico-emacs-的补全设置}

[vertico](https://github.com/minad/vertico) 插件提供了一个垂直样式的补全系统。

```emacs-lisp
(use-package vertico
  :ensure t
  :hook (after-init . vertico-mode)
  :bind (:map minibuffer-local-map
              ("M-<DEL>" . my/minibuffer-backward-kill)
              :map vertico-map
              ("M-q" . vertico-quick-insert)) ; use C-g to exit
  :config
  (defun my/minibuffer-backward-kill (arg)
    "When minibuffer is completing a file name delete up to parent
 folder, otherwise delete a word"
    (interactive "p")
    (if minibuffer-completing-file-name
        ;; Borrowed from https://github.com/raxod502/selectrum/issues/498#issuecomment-803283608
        (if (string-match-p "/." (minibuffer-contents))
            (zap-up-to-char (- arg) ?/)
          (delete-minibuffer-contents))
      (backward-kill-word arg)))

  ;; Do not allow the cursor in the minibuffer prompt
  (setq minibuffer-prompt-properties
        '(read-only t cursor-intangible t face minibuffer-prompt))
  (add-hook 'minibuffer-setup-hook #'cursor-intangible-mode)

  (setq vertico-cycle t)                ; cycle from last to first
  :custom
  (vertico-count 15))                    ; number of candidates to display, default is 10
```


## ORDERLESS 是一种哲学思想 {#orderless-是一种哲学思想}

[oderless](https://github.com/oantolin/orderless) 插件提供一种无序的补全新姿势，将一个搜索的范式变成数个以空格分隔的部分，各部分之间没有顺序，你要做的就是根据记忆输入关键词、空格、关键词。

```emacs-lisp
;; orderless 是一种哲学思想
(use-package orderless
;; support Pinyin first character match for orderless, avy etc.
  :ensure t
    :custom
    (completion-styles '(orderless basic))
    (completion-category-overrides '((file (styles basic partial-completion))))
  :init
  (setq completion-styles '(orderless partial-completion basic))
  (setq orderless-component-separator "[ &]") ; & is for company because space will break completion
  (setq completion-category-defaults nil)
  (setq completion-category-overrides nil)
  :config
  ;; make completion support pinyin, refer to
  ;; https://emacs-china.org/t/vertico/17913/2
  (defun completion--regex-pinyin (str)
    (orderless-regexp (pinyinlib-build-regexp-string str)))
  (add-to-list 'orderless-matching-styles 'completion--regex-pinyin))
```


## MARGINALIA {#marginalia}

[marginalia](https://github.com/minad/marginalia) 插件给迷你缓冲区的补全候选条目添加一些提示。

```emacs-lisp
;; minibuffer helpful annotations
(use-package marginalia
  :ensure t
  :hook (after-init . marginalia-mode)
  :custom
  (marginalia-annotators '(marginalia-annotators-heavy marginalia-annotators-light nil))
  :config
  (marginalia-mode 1))
```


## SHELL {#shell}

;; 获取当前的环境变量列表
(setq my-path (split-string (getenv "PATH") ":" t))
;; 删除 Python 3.7.3 的路径
(setq my-path (delete "\\\Users\\\wacl\\\AppData\\\Local\\\Programs\\\Python\\\Python37\\\Scripts;c" my-path))
(setq my-path (delete "\\\Users\\\wacl\\\AppData\\\Local\\\Programs\\\Python\\\Python37;d" my-path))
;; 将 pip 的路径添加到环境变量列表中
(add-to-list 'my-path "\\\Users\\\wacl\\\scoop\\\apps\\\python310\\\current\\\lib\\\site-packages\\\pip;")
(add-to-list 'my-path "c;")
;; 将新的环境变量列表设置回 PATH 变量
(setenv "PATH" (mapconcat 'identity my-path ":"))
在这个代码示例中，您需要将 \`/path/to/Python/3.7.3\` 替换为您要删除的 Python 3.7.3 的路径。这段代码首先获取当前的环境变量列表，然后使用 \`delete\` 函数从列表中删除 Python 3.7.3 的路径。最后，它将新的环境变量列表设置回 PATH 变量中。
请注意，在使用此代码之前，您需要了解 Emacs Lisp 的基础知识，并且需要知道如何在 Emacs 中运行 Lisp 代码。您可以使用 \`M-x eval-buffer\` 命令来运行当前缓冲区中的所有 Lisp 代码。或者，您可以将上面的代码添加到您的 Emacs 配置文件中，并重新启动 Emacs，以使更改生效。


## PYTHON {#python}

```emacs-lisp
;;py-python-command is a variable without a source file. (setq py-python-command "python3")
(setq python-shell-interpreter "python3")
;; sudo pip install pylint
;; Jedi.el is a Python auto-completion package for Emacs. It aims at helping your Python coding in a non-destructive way. It also helps you to find information about Python objects, such as docstring, function arguments and code location.
(use-package jedi
  :ensure t
  :init
  (add-hook 'python-mode-hook 'jedi:setup)
  (add-hook 'python-mode-hook 'jedi:ac-setup)
  )

;; Elpy, the Emacs Python IDE
(use-package elpy
  :ensure t
  ;;:config
  ;;(elpy-enable))
  :init
  (elpy-enable))
;; A featureful virtualenv tool for Emacs. Emulates much of the functionality of Doug Hellmann's virtualenvwrapper.
(use-package virtualenvwrapper
  :ensure t
  :config
  (venv-initialize-interactive-shells)
  (venv-initialize-eshell)

  (venv-initialize-interactive-shells) ;; if you want interactive shell support
  (venv-initialize-eshell) ;; if you want eshell support
  ;; note that setting `venv-location` is not necessary if you
  ;; use the default location (`~/.virtualenvs`), or if the
  ;; the environment variable `WORKON_HOME` points to the right place
  ;;(setq venv-location "~/virtualenvs/")
  (setq venv-location '("~/virtualenvs/project1-env/"
                        "~/virtualenvs/ptoject2-env/"))
  )
```


## FLYCHECK {#flycheck}

```elisp
(use-package flycheck
  :ensure t
  :init (global-flycheck-mode)
  :config
  (global-flycheck-mode t))
```


## COUNSEL {#counsel}

```emacs-lisp
(use-package counsel
  :bind (("M-y" . counsel-yank-pop)
         :map ivy-minibuffer-map ("M-y" . ivy-next-line)))
```


## MYSELF {#myself}


### GAME {#game}

1.  俄罗斯方块
    ```elisp
    (tetris)
    ```
2.  贪吃蛇
    ```elisp
    (snake)
    ```


### NAME EMAIL {#name-email}

个性自定义

```emacs-lisp
(setq user-full-name "wacl" user-mail-address "2863296436@qq.com")
```


### WINDOWS PATH {#windows-path}

修改 windows 版本的 PATH 路径。

```emacs-lisp
(if (eq system-type 'windows-nt)
    (setenv "PATH"
            (concat
             "C:/Users/wacl/scoop/shims" ";"
             (getenv "PATH")))
  nil)
```


### 中文字体优化 {#中文字体优化}

```emacs-lisp
(setq read-process-output-max (* 1024 1024)) ;; 1mb

(setq inhibit-compacting-font-caches t)
;; 似乎不用单独设置垃圾回收方式，用这个配置就可以。这个能解决几乎所有字体卡顿的问题。 试试看
;; 设置垃圾回收，在Windows下，emacs25版本会频繁出发垃圾回收，所以需要设置

(when (eq system-type 'windows-nt)
  (setq gc-cons-threshold (* 512 1024 1024))
;; 设置垃圾回收参数
(setq gc-cons-threshold most-positive-fixnum)
  (setq gc-cons-percentage 0.6)
  (run-with-idle-timer 5 t #'garbage-collect)
  ;; 显示垃圾回收信息，这个可以作为调试用 ;; (setq garbage-collection-messages t)
  (setq garbage-collection-messages nil))
```


### BASIC CUSTOM {#basic-custom}

```css
#80ff80
isg
;; Normal
(setq evil-normal-state-cursor '("block" "#80ff80"))
(setq evil-normal-state-cursor '("box" "#80ff80"))

;; Insert
(setq evil-insert-state-cursor '("bar" "#80ff80"))
```

```emacs-lisp
;;让鼠标滚动更好用
(setq mouse-wheel-scroll-amount '(1 ((shift) . 1) ((control) . nil)))
(setq mouse-wheel-progressive-speed nil)

(tool-bar-mode -1)
(scroll-bar-mode -1)

(setq-default cursor-type '(bar . 5))
(tab-bar-mode -1)

(prefer-coding-system 'utf-8)
(set-default-coding-systems 'utf-8)
(setq-default buffer-file-coding-system 'utf-8)
(setq-default coding-system-for-read 'utf-8)
(setq file-name-coding-system 'utf-8)
(setq locale-coding-system 'utf-8)
(set-language-environment "UTF-8")
(prefer-coding-system 'utf-8)

(electric-indent-mode t)
(electric-pair-mode t)
(icomplete-mode nil)
(server-mode 1)

(delete-selection-mode nil)

(when (fboundp 'set-charset-priority)
  (set-charset-priority 'unicode))
(setq auto-save-default nil)
(setq ring-bell-function 'ignore)
;;(global-linum-mode 1)

;; 设置窗口透明度
;;(set-frame-parameter (selected-frame) 'alpha '(96 . 90))
;;(add-to-list 'default-frame-alist '(alpha . (95 . 85)))
;; 设置默认 frame 的透明度为 90
;;(set-frame-parameter (selected-frame) 'alpha '(90 00))
;;(add-to-list 'default-frame-alist '(alpha . (90 90)))
;;其中第一个数字表示未选定时窗口的透明度，第二个数字表示当前窗口选定时的透明度。
;; 设置默认 frame 的背景图片和透明度
;;(setq default-frame-alist
;;      '((background-image . "~/begin_new_life/org/org/1.png")
;;        (alpha . (100 100))))
```


### SHORTCUT 快捷键 TANGLE ~/EMACS-BABEL.EL {#shortcut-快捷键-tangle-emacs-babel-dot-el}

```emacs-lisp
(global-set-key (kbd "<f4>") 'open-my-file)
(global-set-key (kbd "<f5>") 'revert-buffer)
(global-set-key (kbd "C-;") 'embark-act)
(global-set-key (kbd "C-c C-'") #'consult-directory-externally)
(global-set-key (kbd "C-c C-'") 'consult-directory-externally)
(global-set-key (kbd "C-c a") #'org-agenda)
(global-set-key (kbd "C-c a") 'org-agenda)
(global-set-key (kbd "C-c c") #'org-capture)
(global-set-key (kbd "C-c c") 'org-capture)
(global-set-key (kbd "C-c l") #'org-store-link)
(global-set-key (kbd "C-c l") 'org-store-link)
(global-set-key (kbd "C-c p") #'org-previous-visible-heading)
(global-set-key (kbd "C-c p") 'org-previous-visible-heading)
(global-set-key (kbd "C-x r d") 'bookmark-delete)
;;(define-key function-key-map (kbd "ESC") (kbd "C-g"))
;;(define-key key-translation-map (kbd "C-x C-c") (kbd "C-g"))
;;(global-set-key (kbd "<escape>") 'keyboard-escape-quit)
;;(global-set-key (kbd "<f5>") 'reload-emacs-config)
;;(setq prefix-help-command 'embark-prefix-help-command)
;;bookmark
;;doublecmd
;;embark
;;evilnc-copy-and-comment-lines(global-set-key (kbd "C-c c") 'org-capture)
;;evilnc-quick-comment-or-uncomment-to-the-line
;;org_heading-temp
```

```elisp
(global-set-key (kbd "\e\ei") ;;\e\ec \e\el
                (lambda () (interactive) (find-file "~/begin_new_life/org/emacs.org")))
```


### FUNCTION {#function}

```emacs-lisp
(defun org-agenda-show-agenda-and-todo (&optional arg)
  (interactive "P")
  (org-agenda arg "c")
  (org-agenda-fortnight-view))

```

```emacs-lisp
(defun remove-duplicate-words-and-sort ()
  "Remove duplicate words and sort them alphabetically, treating hyphenated words as one word."
  (interactive)
  (let ((words (split-string (buffer-substring-no-properties (point-min) (point-max)) "[ \t\n]+")))
    (erase-buffer)
    (insert (mapconcat 'identity (sort (delete-dups words) 'string<) " "))))

;;(defun my-company-capf-wrapper (func &optional arg &rest _)
;;  (funcall func arg)) ;; 将前缀传递给compan-capf
;;(setq company-backends '(my-company-capf-wrapper))

(defun my-org-company-insert ()
  "插入local设置"
  (interactive)
  (insert "(setq-local company-backends '(company-yasnippet company-bbdb company-dabbrev company-dabbrev-code company-files company-gtags company-keywords company-oddmuse company-semantic))"))
;; company-tabnine company-capf
(defun my-org-company-setup ()
  "直接设置company-backends后端"
  (interactive)
  (setq company-backends '((company-yasnippet :separate company-dabbrev company-dabbrev-code)
                           (company-lsp :separate company-files company-keywords)(company-gtags company-etags company-oddmuse company-semantic company-cmake company-clang))))

;;(add-hook 'inferior-maxima-mode-hook 'my-imaxima-hook)
;; fontsize 更改显示字体大小 16pt
;; http://stackoverflow.com/questions/294664/how-to-set-the-font-size-in-emacs
;;(set-face-attribute 'default nil :font "Hack 24" :font (font-spec :weight 'normal :slant 'normal :size 36))

(defun my-font-size ()
  "设置我的字体为Hack 16"
  (interactive)
  (set-face-attribute 'default nil :font "Hack 16"))

(require 'cl)
(defvar my-imaxima-font-size 12
  "The font size to use in `my-imaxima' function.")
(defvar my-imaxima-counter 1
  "Counter for `my-imaxima' function.")

(defun my-for-imaxima-font ()
  (interactive)
  (setq my-imaxima-counter (1+ my-imaxima-counter))
  (let ((font-size (if (evenp my-imaxima-counter)
                       16
                     my-imaxima-font-size)))
    (set-face-attribute 'default nil :font (concat "Hack " (number-to-string font-size)))))

(defun my-browse-url ()
  "选择浏览器打开"
  (interactive)
  (let ((browser (completing-read "Select browser: " '("browse-url-default-browser" "eaf-open-browser"))))
    (funcall (intern browser) (read-string "URL: "))))
;;(setq browse-url-browser-function 'my-browse-url)
(setq browse-url-browser-function 'browse-url-default-browser)

(defun reload-emacs-config ()
  "Reload the emacs-babel.el file."
  (interactive)
  (load-file "~/emacs-babel.el"))

;;(defun open-ob-babel-file (filename)
;;"Open the specified Org Babel file"
;;(interactive "选择语言: ")
;;(find-file filename))
;; (add-to-list 'load-path "~/.emacs.d.spacemacs/elpa/28.2/develop/org-9.6.7/")
;;(defvar ob-babel-files
;;'((ob-maxima . "~/.emacs.d.spacemacs/elpa/29.1/develop/org-9.6.7/ob-maxima.el")
;;(ob-python . "~/.emacs.d.spacemacs/elpa/28.2/develop/org-9.6.7/ob-python.el")))

(defvar ob-directory "~/.emacs.d.spacemacs/elpa/29.1/develop/org-9.6.7")

;;(let ((file-path (concat org-directory "/" filename ".org")))
;;(when (file-exists-p file-path)
;;(find-file file-path)))

(defun open-ob-babel-file (filename)
  "Open the ob-Babel file"
  (interactive "sEnter file name: ")
  (let ((file-path (concat ob-directory "/ob-" filename ".el")))
    (when (file-exists-p file-path)
      (find-file file-path)
      (progn
        (message "File %s not found, opening directory %s" filename ob-directory)
        (dired ob-directory)))))

(defun reload-doom-ob-babel-file ()
  (interactive)
  (dired "~/.emacs.d.doomemacs/.local/straight/links/"))


(defun open-org-org-file (filename)
  "Open the specified Org Babel file"
  (interactive "sEnter file name: ")
  (let ((file-path (concat org-directory "/" filename ".org")))
    (if (file-exists-p file-path)
        (find-file file-path)
      (progn
        (message "File %s not found, opening directory %s" filename org-directory
                 (dired org-directory))))))
;; (dired-find-file-other-window org-directory)
;; (dired-other-window ob-directory)
;; (dired-jump org-directory)
;; (dired-jump-other-window ob-directory)

;; find-file-other-window
;;(defun open-ob-babel-file (lang)
;;"Open the specified Org Babel file"
;;(interactive
;;(list (completing-read "Enter language: " (mapcar #'car ob-babel-files))))
;;(let ((filename (cdr (assoc lang ob-babel-files))))
;;(when filename
;;(find-file filename))))

;;doublecmd
(defun consult-doublemcd-directory-externally (file)
  "Open FILE externally using the default application of the system."
  (interactive "fOpen externally: ")
  (if (and (eq system-type 'windows-nt)
           (fboundp 'w32-shell-execute))
      (start-process "doublecmd" nil "doublecmd.exe"
                     (encode-coding-string (replace-regexp-in-string "/" "\\\\"
                                                                     (file-name-directory (expand-file-name file))) 'gbk))
    (call-process (pcase system-type
                    ('darwin "open")
                    ('cygwin "cygstart")
                    (_ "xdg-open"))
                  nil 0 nil
                  (encode-coding-string (replace-regexp-in-string "/" "\\\\"
                                                                  (file-name-directory (expand-file-name file))) 'gbk))))

;; keycast
;;(defun turn-on-keycast () (add-to-list 'global-mode-string '("" mode-line-keycast " ")))
;;(defun turn-off-keycast () (setq global-mode-string (delete '("" mode-line-keycast " ") global-mode-string)))

;; add display image to C-c C-c
(with-eval-after-load 'org
  (define-key org-mode-map (kbd "C-c C-c")
    (lambda ()
      (interactive)
      (org-ctrl-c-ctrl-c)))
  (define-key org-mode-map (kbd "<tab>") 'org-cycle)
  ;; (global-set-key (kbd "<S-tab>") 'org-global-cycle)
  (define-key org-mode-map (kbd "<S-tab>") 'org-shifttab)
  (require 'org-clock)
  (add-hook 'org-mode-hook 'org-clock-load))
;;(org-redisplay-inline-images)
```

```emacs-lisp
;; alt + num to insert special symbol
(let ((map company-active-map))
  (mapc
   (lambda (x)
     (define-key map (kbd (format "M-%d" x))
       `(lambda ()
          (interactive)
          (when (company-manual-begin)
            (company-abort))
          (insert ,(number-to-string x)))))
   (number-sequence 0 9)))

;;(defun my-company-enter ()
;;  "Enter the selected completion candidate."
;;  (interactive)
;;  (company-complete-selection) ;; (newline-and-indent)
;;  )

;;(let ((map company-active-map))
;;  (define-key map (kbd "SPC") 'my-company-enter))
;; (define-key map (kbd "SPC") (lambda () (interactive) (insert " ")))

```


## EMACS {#emacs}


### eshell {#eshell}

alias 设置

```shell
alias d dired $1
alias f find-file $1
alias ff find-file $1
alias fo find-file-other-window $1
alias l ls -lh
alias l. ls -dh .*
alias la ls -lAh
alias less view-file $1
alias ll ls -alh
alias ll ls -l
alias lr ls -tRh
alias lrt ls -lcrt
alias lsa ls -lah
alias lt ls -lth
alias more view-file $1
alias pk eshell-up-peek $1
alias up eshell-up $1
```


### 限制代码块结果长度 {#限制代码块结果长度}

参考 [twlz0ne 大佬在这篇贴子的回复](https://emacs-china.org/t/org-babel/18399/4)。

```emacs-lisp
;; limit the babel result length
(defvar org-babel-result-lines-limit 40)
(defvar org-babel-result-length-limit 6000)

(defun org-babel-insert-result@limit (orig-fn result &rest args)
  (if (not (member (car (org-babel-get-src-block-info)) '("jupyter-python"))) ; not for jupyter-python etc.
      (if (and result (or org-babel-result-lines-limit org-babel-result-length-limit))
          (let (new-result plines plenght limit)
            (with-temp-buffer
              (insert result)
              (setq plines (if org-babel-result-lines-limit
                               (goto-line org-babel-result-lines-limit)
                             (point-max)))
              (setq plenght (if org-babel-result-length-limit
                                (min org-babel-result-length-limit (point-max))
                              (point-max)))
              (setq limit (min plines plenght))
              (setq new-result (concat (buffer-substring (point-min) limit)
                                       (if (< limit (point-max)) "..."))))
            (apply orig-fn result args))
        (apply orig-fn result args))
    (apply orig-fn result args)))

(advice-add 'org-babel-insert-result :around #'org-babel-insert-result@limit)
```


## SHACKLE 窗口管理 {#shackle-窗口管理}

[shackle](https://depp.brause.cc/shackle/) 插件能自定义窗口的弹出方式。

```emacs-lisp
(use-package shackle
  :ensure t
  :hook (after-init . shackle-mode)
  :init
  (setq shackle-lighter "")
  (setq shackle-select-reused-windows nil) ; default nil
  (setq shackle-default-alignment 'below)  ; default below
  (setq shackle-default-size 0.4)          ; default 0.5
  (setq shackle-rules
        ;; CONDITION(:regexp)            :select     :inhibit-window-quit   :size+:align|:other     :same|:popup
        '((compilation-mode              :ignore t)
          ("\\*Async Shell.*\\*" :regexp t :ignore t)
          ("\\*corfu.*\\*"       :regexp t :ignore t)
          ("*eshell*"                    :select t                          :size 0.4  :align t     :popup t)
          (helpful-mode                  :select t                          :size 0.6  :align right :popup t)
          ("*Messages*"                  :select t                          :size 0.4  :align t     :popup t)
          ("*Calendar*"                  :select t                          :size 0.3  :align t     :popup t)
          ("*info*"                      :select t                                                  :same t)
          (magit-status-mode             :select t   :inhibit-window-quit t                         :same t)
          (magit-log-mode                :select t   :inhibit-window-quit t                         :same t)
          ))
  )
```


## POPPER 窗口弹出管理 {#popper-窗口弹出管理}

我们通过 [popper](https://github.com/karthink/popper) 插件，来控制窗口的弹出行为，与 [shackle](https://depp.brause.cc/shackle/) 一起配合使用。

```emacs-lisp
(use-package popper
  :ensure t
  :bind (("M-`"     . popper-toggle-latest)
         ("M-<tab>" . popper-cycle)
         ("M-\\"    . popper-toggle-type))

  :init
  (setq popper-reference-buffers
        '("\\*Messages\\*"
          "\\*Async Shell Command\\*"
          help-mode
          helpful-mode
          occur-mode
          pass-view-mode
          "^\\*eshell.*\\*$" eshell-mode ;; eshell as a popup
          "^\\*shell.*\\*$"  shell-mode  ;; shell as a popup
          ("\\*corfu\\*" . hide)
          (compilation-mode . hide)
          ;; derived from `fundamental-mode' and fewer than 10 lines will be considered a popup
          (lambda (buf) (with-current-buffer buf
                          (and (derived-mode-p 'fundamental-mode)
                               (< (count-lines (point-min) (point-max))
                                  10))))))


  (popper-mode +1)
  (popper-echo-mode +1)
  :config
  ;; group by project.el, projectile, directory or perspective
  (setq popper-group-function nil)

  ;; pop in child frame or not
  (setq popper-display-function #'display-buffer-in-child-frame)

  ;; use `shackle.el' to control popup
  (setq popper-display-control nil))
```


## <span class="org-todo todo TODO">TODO</span> TAB-BAR :tangle ~/emacs-babel.el {#tab-bar-tangle-emacs-babel-dot-el}

```emacs-lisp
;;(global-tab-line-mode 1)
(use-package tab-bar
  :ensure nil
  :init
  (tab-bar-mode t)
  (setq tab-bar-new-tab-choice "*scratch*") ;; buffer to show in new tabs
  (setq tab-bar-close-button-show nil)      ;; hide tab close / X button
  (setq tab-bar-show 1)                     ;; hide bar if <= 1 tabs open
  (setq tab-bar-format '(tab-bar-format-tabs tab-bar-separator))

  (custom-set-faces
   '(tab-bar ((t (:inherit mode-line))))
   '(tab-bar-tab ((t (:inherit mode-line :foreground "#993644"))))
   '(tab-bar-tab-inactive ((t (:inherit mode-line-inactive :foreground "black")))))

  (defvar ct/circle-numbers-alist
    '((0 . "⓪")
      (1 . "①")
      (2 . "②")
      (3 . "③")
      (4 . "④")
      (5 . "⑤")
      (6 . "⑥")
      (7 . "⑦")
      (8 . "⑧")
      (9 . "⑨"))
    "Alist of integers to strings of circled unicode numbers.")

  (defun ct/tab-bar-tab-name-format-default (tab i)
    (let ((current-p (eq (car tab) 'current-tab))
      (tab-num (if (and tab-bar-tab-hints (< i 10))
               (alist-get i ct/circle-numbers-alist) "")))
      (propertize
       (concat tab-num
           " "
           (alist-get 'name tab)
           (or (and tab-bar-close-button-show
            (not (eq tab-bar-close-button-show
                 (if current-p 'non-selected 'selected)))
            tab-bar-close-button)
           "")
           " ")
       'face (funcall tab-bar-tab-face-function tab))))
  (setq tab-bar-tab-name-format-function #'ct/tab-bar-tab-name-format-default)
  (setq tab-bar-tab-hints t))
```

```emacs-lisp
(defun ct/tab-bar-tab-name-format-default (tab i)
  (let* ((current-p (eq (car tab) 'current-tab))
         (tab-num (if (and tab-bar-tab-hints (< i 10))
                      (alist-get i ct/circle-numbers-alist) ""))
         (close-button (if (and tab-bar-close-button-show
                                (not (eq tab-bar-close-button-show
                                         (if current-p 'non-selected 'selected))))
                           tab-bar-close-button
                         "")))
    (propertize
     (concat tab-num
             " "
             (alist-get 'name tab)
             close-button
             " ")
     'face (funcall tab-bar-tab-face-function tab))))
```


## <span class="org-todo todo TODO">TODO</span> WINNER 窗口管理 {#winner-窗口管理}

内置的 `winner` 插件是一个窗口管理器，可以通过 `winner-undo` 和 `winner-redo` 命令恢复或重做窗口布局。

```emacs-lisp
(use-package winner
  :ensure nil
  :commands (winner-undo winner-redo)
  :hook (after-init . winner-mode)
  :init  ;; config
  (setq winner-boring-buffers
        '("*Completions*"
          "*Compile-Log*"
          "*inferior-lisp*"
          "*Fuzzy Completions*"
          "*Apropos*"
          "*Help*"
          "*cvs*"
          "*Buffer List*"
          "*Ibuffer*"
          "*esh command on file*")))
```


## 别名定义 {#别名定义}

定义了别名后，可以通过 `M-x 别名` 的方式来调用某个命令。

```emacs-lisp
(defalias 'e #'eshell)
(defalias 's #'scratch)
(defalias 'conf #'open-emacsconfig)
```


## <span class="org-todo todo TODO">TODO</span> 解除一些不常用的快捷键 {#解除一些不常用的快捷键}

将一些不常用的快捷键解除，防止误操作。

```emacs-lisp
;; 解除不常用的快捷键定义
(global-set-key (kbd "C-z") nil)
(global-set-key (kbd "s-q") nil)
(global-set-key (kbd "M-z") nil)
(global-set-key (kbd "M-m") nil)
(global-set-key (kbd "C-x C-z") nil)
;;(global-set-key [mouse-2] nil)
```


## DELSEL 选择文本输入时直接替换 {#delsel-选择文本输入时直接替换}

Emacs 默认选择文本后直接输入，是不会直接删除所选择的文本进行替换的。通过内置的 `delsel` 插件来实现这个行为。

```emacs-lisp
;; Directly modify when selecting text
;;(use-package delsel :ensure nil :hook (after-init . delete-selection-mode))
(use-package delsel
  :ensure nil
  :config
  (delete-selection-mode -1))
```


## <span class="org-todo todo TODO">TODO</span> AVY 光标移动 {#avy-光标移动}

[avy](https://github.com/abo-abo/avy) 是一个光标移动插件，能快速将光标移动到屏幕上的任意字符，非常强大！

```emacs-lisp
(use-package avy
  :ensure t
  :bind (("C-." . my/avy-goto-char-timer)
         ;;("C-。" . my/avy-goto-char-timer)
         :map isearch-mode-map
         ("C-." . avy-isearch))
  :config
  ;; Make `avy-goto-char-timer' support pinyin, refer to:
  ;; https://emacs-china.org/t/avy-avy-goto-char-timer/20900/2
  (defun my/avy-goto-char-timer (&optional arg)
    "Make avy-goto-char-timer support pinyin"
    (interactive "P")
    (let ((avy-all-windows (if arg
                               (not avy-all-windows)
                             avy-all-windows)))
      (avy-with avy-goto-char-timer
        (setq avy--old-cands (avy--read-candidates
                              'pinyinlib-build-regexp-string))
        (avy-process avy--old-cands))))

  (defun avy-action-kill-whole-line (pt)
    "avy action: kill the whole line where avy selection is"
    (save-excursion
      (goto-char pt)
      (kill-whole-line))
    (select-window
     (cdr
      (ring-ref avy-ring 0)))
    t)

  (defun avy-action-copy-whole-line (pt)
    "avy action: copy the whole line where avy selection is"
    (save-excursion
      (goto-char pt)
      (cl-destructuring-bind (start . end)
          (bounds-of-thing-at-point 'line)
        (copy-region-as-kill start end)))
    (select-window
     (cdr
      (ring-ref avy-ring 0)))
    t)

  (defun avy-action-yank-whole-line (pt)
    "avy action: copy the line where avy selection is and paste to current point"
    (avy-action-copy-whole-line pt)
    (save-excursion (yank))
    t)

  (defun avy-action-teleport-whole-line (pt)
    "avy action: kill the line where avy selection is and paste to current point"
    (avy-action-kill-whole-line pt)
    (save-excursion (yank)) t)

  (defun avy-action-helpful (pt)
    "avy action: get helpful information at point"
    (save-excursion
      (goto-char pt)
      (helpful-at-point))
    ;; (select-window
    ;;  (cdr (ring-ref avy-ring 0)))
    t)

  (defun avy-action-mark-to-char (pt)
    "avy action: mark from current point to avy selection"
    (activate-mark)
    (goto-char pt))

  (defun avy-action-flyspell (pt)
    "avy action: flyspell the word where avy selection is"
    (save-excursion
      (goto-char pt)
      (when (require 'flyspell nil t)
        (flyspell-correct-wrapper))))

  (defun avy-action-define (pt)
    "avy action: define the word in dictionary where avy selection is"
    (save-excursion
      (goto-char pt)
      (fanyi-dwim2)))

  (defun avy-action-embark (pt)
    "avy action: embark where avy selection is"
    (unwind-protect
        (save-excursion
          (goto-char pt)
          (embark-act))
      (select-window
       (cdr (ring-ref avy-ring 0))))
    t)

  (defun avy-action-google (pt)
    "avy action: google the avy selection when it is a word or browse it when it is a link"
    (save-excursion
      (goto-char pt)
      (my/search-or-browse)))

  (setf (alist-get ?k avy-dispatch-alist) 'avy-action-kill-stay
        (alist-get ?K avy-dispatch-alist) 'avy-action-kill-whole-line
        (alist-get ?w avy-dispatch-alist) 'avy-action-copy
        (alist-get ?W avy-dispatch-alist) 'avy-action-copy-whole-line
        (alist-get ?y avy-dispatch-alist) 'avy-action-yank
        (alist-get ?Y avy-dispatch-alist) 'avy-action-yank-whole-line
        (alist-get ?t avy-dispatch-alist) 'avy-action-teleport
        (alist-get ?T avy-dispatch-alist) 'avy-action-teleport-whole-line
        (alist-get ?H avy-dispatch-alist) 'avy-action-helpful
        (alist-get ?  avy-dispatch-alist) 'avy-action-mark-to-char
        (alist-get ?\; avy-dispatch-alist) 'avy-action-flyspell
        (alist-get ?= avy-dispatch-alist) 'avy-action-define
        (alist-get ?o avy-dispatch-alist) 'avy-action-embark
        (alist-get ?G avy-dispatch-alist) 'avy-action-google)


  :custom
  ;; (avy-case-fold-search t)              ; default is t
  (avy-timeout-seconds 1.0)
  (avy-all-windows t)
  (avy-background t)
  (avy-keys '(?a ?s ?d ?f ?g ?h ?j ?l ?q ?e ?r ?u ?i ?p ?n)))
```


## SAVEHIST {#savehist}

记住迷你缓冲区历史
让 Emacs 记住一些历史信息
我们打开一些文件，输入一些命令，这些命令的历史都是数据，而数据都是有价值的，一个最浅显的价值就是通过让 Emacs 记住一些历史信息，让我们直接从历史信息中再次执行。
让 Emacs 记住迷你缓冲区历史信息
我们通过 Emacs 自带的 savehist 插件，来记住迷你缓冲区的历史，如执行过的命令等。在设置这个插件之前，无论何时，我们按下 M-x 显示的命令都是一样的：
让 Emacs 记住关闭文件时的位置
我们通过 Emacs 自带的 save-place 插件，来记住每个文件关闭时光标所在的位置，当我们再次打开这个文件时，自动跳转到记住的位置。
自动记住每个文件的最后一次访问的光标位置

```emacs-lisp
(use-package savehist
  :ensure nil
  :hook (after-init . savehist-mode)
  :config
  ;; Allow commands in minibuffers, will affect 'dired-do-dired-do-find-regexp-and-replace' command:
  (setq enable-recursive-minibuffers t)
  (setq history-length 1000)
  (setq savehist-additional-variables '(mark-ring
                                        global-mark-ring
                                        search-ring
                                        regexp-search-ring
                                        extended-command-history))
  (setq savehist-autosave-interval 300)

  :init
  (setq enable-recursive-minibuffers t ;; Allow commands in minibuffers
        history-length 1000
        savehist-additional-variables '(mark-ring
                                        global-mark-ring
                                        search-ring
                                        regexp-search-ring
                                        extended-command-history)
        savehist-autosave-interval 300)
  )
```


## SAVEPLACE {#saveplace}

记住每个文件的光标位置
自动记住每个文件的最后一次访问的光标位置。

```emacs-lisp
(use-package saveplace
  :ensure nil
  :hook (after-init . save-place-mode))
```


## <span class="org-todo todo TODO">TODO</span> RECENTF :tangle ~/emacs-babel.el {#recentf-tangle-emacs-babel-dot-el}

<https://github.com/search?q=recentf>
最近打开的文件历史
记住最近打开的文件历史。
让 Emacs 记住最近打开的文件历史
我们通过 Emacs 自带的 recentf 插件，来记住 Emacs 最近打开的文件历史。
(recentf-mode 1) 启用最近打开文件功能
(setq recentf-max-menu-items 25) 最多显示 25 个文件

```emacs-lisp
(use-package recentf
  :ensure nil
  :defines no-littering-etc-directory no-littering-var-directory
  :hook (after-init . recentf-mode)
  :custom
  (recentf-max-saved-items 100)
  (recentf-auto-cleanup 'never))
  ;; `recentf-add-file' will apply handlers first, then call `string-prefix-p'
  ;; to check if it can be pushed to recentf list.
```


### <span class="org-todo todo WAIT">WAIT</span> MacOS {#macos}

```emacs-lisp
(
 (recentf-filename-handlers '(abbreviate-file-name))
 (recentf-exclude `(,@(cl-loop for f in `(,package-user-dir
                                          ,no-littering-var-directory
                                          ,no-littering-etc-directory)
                               collect (abbreviate-file-name f))
                    ;; Folders on MacOS start
                    "^/private/tmp/"
                    "^/var/folders/"
                    ;; Folders on MacOS end
                    ".cache"
                    ".cask"
                    ".elfeed"
                    "elfeed"
                    "bookmarks"
                    "cache"
                    "ido.*"
                    "persp-confs"
                    "recentf"
                    "undo-tree-hist"
                    "url"
                    "^/tmp/"
                    "/ssh\\(x\\)?:"
                    "/su\\(do\\)?:"
                    "^/usr/include/"
                    "/TAGS\\'"
                    "COMMIT_EDITMSG\\'")))
```


### <span class="org-todo todo WAIT">WAIT</span> complete tangle ~/emacs-babel.el {#complete-tangle-emacs-babel-dot-el}

```emacs-lisp
(use-package recentf
  :ensure nil
  :defines no-littering-etc-directory no-littering-var-directory
  :hook (after-init . recentf-mode)
  :custom
  (recentf-max-saved-items 300)
  (recentf-auto-cleanup 'never)
  ;; `recentf-add-file' will apply handlers first, then call `string-prefix-p'
  ;; to check if it can be pushed to recentf list.
  (recentf-filename-handlers '(abbreviate-file-name))
  (recentf-exclude `(,@(cl-loop for f in `(,package-user-dir
                                           ,no-littering-var-directory
                                           ,no-littering-etc-directory)
                                collect (abbreviate-file-name f))
                     ;; Folders on MacOS start
                     "^/private/tmp/"
                     "^/var/folders/"
                     ;; Folders on MacOS end
                     ".cache"
                     ".cask"
                     ".elfeed"
                     "elfeed"
                     "bookmarks"
                     "cache"
                     "ido.*"
                     "persp-confs"
                     "recentf"
                     "undo-tree-hist"
                     "url"
                     "^/tmp/"
                     "/ssh\\(x\\)?:"
                     "/su\\(do\\)?:"
                     "^/usr/include/"
                     "/TAGS\\'"
                     "COMMIT_EDITMSG\\'")))
```


## UNDO-TREE 撤销设置 {#undo-tree-撤销设置}

[undo-tree](https://www.dr-qubit.org/undo-tree.html) 插件可以提供一个可视化的撤销、重做系统，我们使用 `C-/` 来撤销，使用 `M-_` 来重做。

```emacs-lisp
(use-package undo-tree
  :ensure t
  :diminish
  ;;:hook (after-init . global-undo-tree-mode)
  ;;:init
  ;;(global-undo-tree-mode 1)
  :config
  (global-undo-tree-mode 1)
  ;; don't save undo history to local files
  (setq undo-tree-auto-save-history nil))
;; (evil-set-undo-system 'undo-tree)
```


## BEACON ― NEVER LOSE YOUR CURSOR AGAIN {#beacon-never-lose-your-cursor-again}

```emacs-lisp
(use-package beacon
  :ensure t
  :config
  (beacon-mode 1))
;;(setq beacon-color "#666600")
```


## EXPAND-REGION {#expand-region}

increases the selected region by semantic units. Just keep pressing the key until it selects what you want.

```emacs-lisp
(use-package expand-region
  :ensure t
  :config
  (global-set-key (kbd "C-=") 'er/expand-region)
  )
;;(use-package expand-region
;;:bind ("C-=" . er/expand-region))
```


## WEB-MODE {#web-mode}

is an autonomous emacs major-mode for editing web templates.
HTML documents can embed parts (CSS / JavaScript) and blocks (client / server side).

```emacs-lisp
(use-package web-mode
  :ensure t
  :config
  (add-to-list 'auto-mode-alist '("\\.html?\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("\\.phtml\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("\\.tpl\\.php\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("\\.[agj]sp\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("\\.as[cp]x\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("\\.erb\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("\\.mustache\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("\\.djhtml\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("\\.api\\'" . web-mode))
  (add-to-list 'auto-mode-alist '("/some/react/path/.*\\.js[x]?\\'" . web-mode))
  (setq web-mode-content-types-alist
        '(("json" . "/some/path/.*\\.api\\'")
          ("xml"  . "/other/path/.*\\.api\\'")
          ("jsx"  . "/some/react/path/.*\\.js[x]?\\'")))
  (setq web-mode-engines-alist
        '(("php"    . "\\.phtml\\'")
          ("blade"  . "\\.blade\\.")
          ("django" . "\\.html\\'")))


  (setq web-mode-ac-sources-alist
        '(("css" . (ac-source-css-property))
          ("html" . (ac-source-words-in-buffer ac-source-abbrev))))
  (setq web-mode-enable-auto-closing t)
  (setq web-mode-enable-auto-pairing t))
```


## SUPER-SAVE 自动保存 {#super-save-自动保存}

EMACS 备份设置 使用 Emacs 的自动备份设置。
[super-save](https://github.com/bbatsov/super-save) 插件能自动保存缓冲区。它可以设置在某些行为（如窗口切换、或窗口空闲一段时间）下自动保存。

```emacs-lisp
(use-package super-save
  :ensure t
  :hook (after-init . super-save-mode)
  :config
  ;; Emacs空闲是否自动保存，这里不设置
  (setq super-save-auto-save-when-idle t)
  ;; 切换窗口自动保存
  (add-to-list 'super-save-triggers 'other-window)
  ;; 查找文件时自动保存
  (add-to-list 'super-save-hook-triggers 'find-file-hook)
  ;; 远程文件编辑不自动保存
  (setq super-save-remote-files nil)
  ;; 特定后缀名的文件不自动保存
  (setq super-save-exclude '(".gpg"))
  ;; 自动保存时，保存所有缓冲区
  (defun super-save/save-all-buffers ()
    (save-excursion
      (dolist (buf (buffer-list))
        (set-buffer buf)
        (when (and buffer-file-name
                   (buffer-modified-p (current-buffer))
                   (file-writable-p buffer-file-name)
                   (if (file-remote-p buffer-file-name) super-save-remote-files t))
          (save-buffer)))))
  (advice-add 'super-save-command :override 'super-save/save-all-buffers))
```

```emacs-lisp
(setq auto-save-default t)                                  ; 使用Emacs自带的自动保存
(setq make-backup-files t)                                  ; 自动备份
```


## COMPANY {#company}

```emacs-lisp
(defun smarter-yas-expand-next-field-complete ()
  "Try to `yas-expand' and `yas-next-field' at current cursor position.

If failed try to complete the common part with `company-complete-common'"
  (interactive)
  (if yas-minor-mode
      (let ((old-point (point))
            (old-tick (buffer-chars-modified-tick)))
        (yas-expand)
        (when (and (eq old-point (point))
                   (eq old-tick (buffer-chars-modified-tick)))
          (ignore-errors (yas-next-field))
          (when (and (eq old-point (point))
                     (eq old-tick (buffer-chars-modified-tick)))
            (company-complete-common-or-show-delayed-tooltip))))
    (company-complete-common-or-show-delayed-tooltip)))

;; company-complete-common
(use-package company
  :ensure t
  :config
  ;;(require 'company-capf)
  ;;(add-to-list 'company-backends 'company-capf)
  (setq company-transformers '(company-sort-by-backend-importance company-sort-prefer-same-case-prefix company-sort-by-occurrence))
  (add-hook 'org-mode-hook
            (lambda ()
              (setq-local company-backends
                          (delete 'company-capf company-backends))))
  (delete 'company-capf company-backends)
  (setq company-backends (delete 'company-capf company-backends))
  (setq company-backends
        '((company-yasnippet :separate ;; 使用 yasnippet 补全的后端。
           company-files ;; files & directory;; 补全文件系统的路径后端。
           company-keywords       ;; keywords;; 使用当前文件所属编程语言的语法关键词补全。
          ;; company-capf ;; regular too big
           company-dabbrev)
          (company-abbrev company-dabbrev)))
;; 5.1 company-capf
;; 使用 completion-at-point-functions 的后端，但是在 cc-mode 中做补全时，发现会导致 buffer 中输入卡顿，暂时不使用该后端。
;; 5.2 company-dabbrev
;; 使用 dabbrev（Dynamic Abbreviation ）的后端，主要用来补全当前 buffer 中出现的 word。很有用。
;; 5.3 company-dabbrev-code
;; 类似 company-dabbrev，但是不在注释或者字符串中查找符号。编程很有用。
;; 5.4 company-eclim
;; eclipse 补全后端。
;; 5.6 company-ispell
;; ispell 使用的后端。
;; 5.9 company-lsp
;; lsp-mode 配合使用补全后端。
  ;; company-tabnine
  ;; (setq company-backends '(company-capf company-files))
  (setq company-backends '((company-yasnippet :separate company-dabbrev company-dabbrev-code)
                           (;; company-lsp :separate
                            company-files company-keywords)))

  (add-hook 'after-init-hook 'global-company-mode)
  (setq-default company-minimum-prefix-length 3)
  ;; Trigger completion immediately.
  ;; Number the candidates (use M-1, M-2 etc to select completions).
  (setq company-complete-number nil) ;; 禁止数字字符单独触发自动补全
  (setq company-idle-delay 0) ;; 没有延迟弹出补全菜单
  (setq company-show-numbers t) ;; 显示数字提示
  ;;(global-company-mode t) ;; 全局开启

  :bind
  (:map company-active-map
        ([tab] . smarter-yas-expand-next-field-complete)
        ("TAB" . smarter-yas-expand-next-field-complete)))

;;(add-to-list 'auto-mode-alist '("\\.js\\'" . react-mode))
;; 在 Spacemacs 中，我们可以用 auto-mode-alist 来设置某一类文件默认的主模式。
;; 我们只需要在 ~/.spacemacs 中的 user-config 中加入下面代码即可：
;; 上面代码会用 react-mode 打开所有 .js 文件。
;; 设置主模式的 hook return company-complete-selection
  (delete 'company-capf company-backends)
```


## <span class="org-todo todo TODO">TODO</span> COMPANY-TABNINE :tangle ~/emacs-babel.el {#company-tabnine-tangle-emacs-babel-dot-el}

<https://github.com/TommyX12/company-tabnine>

```emacs-lisp
(use-package company-tabnine
  :ensure t
  :after company-mode
  :config
  (add-to-list 'company-backends #'company-tabnine))
```


## <span class="org-todo todo TODO">TODO</span> COPILOT tangle ~/.custom.el {#copilot-tangle-dot-custom-dot-el}

```elisp
(add-to-list 'load-path "~/begin_new_life/my-packages/straight/repos/copilot.el")
(use-package copilot
  :straight (:host github :repo "zerolfx/copilot.el" :files ("dist" "*.el"))
  :ensure t
  :config
  (evil-define-key 'insert 'company-active-map (kbd "C-v") 'copilot-accept-completion-by-word)
  (evil-define-key 'insert 'company-active-map (kbd "C-l") 'copilot-accept-completion-by-line)
  )

;;(setq browse-url-generic-program "firefox")
;;(setq browse-url-browser-function 'browse-url-generic)
;;(setq gnus-button-url 'browse-url-generic
;;      browse-url-generic-program "firefox"
;;      browse-url-browser-function gnus-button-url)
;;(setq browse-url-browser-function 'browse-url-firefox)
;;(straight-use-package
;; '(copilot :host github :repo "zerolfx/copilot.el")
;; :config
;; (require 'copilot))
;;(add-to-list 'load-path "C:/Users/wacl/begin_new_life/my-packages/straight/repos/")
;;(add-to-list 'load-path "C:/Users/wacl/begin_new_life/my-packages/straight/build/")
;;(add-to-list 'load-path "C:/Users/wacl/begin_new_life/my-packages/straight/links/")
;; you can utilize :map :hook and :config to customize copilot

;;  (evil-define-key 'insert 'company-active-map (kbd "TAB") 'copilot-accept-completion)
;;  (evil-define-key 'insert 'company-active-map (kbd "<tab>") 'copilot-accept-completion)
```

```emacs-lisp
(use-package copilot
  :hook (prog-mode . copilot-mode)
  :quelpa (copilot :fetcher github
                   :repo "zerolfx/copilot.el"
                   :branch "main"
                   :files ("dist" "*.el"))
  :config
  (evil-define-key 'insert 'company-active-map (kbd "C-l") 'copilot-accept-completion-by-line))
;; you can utilize :map :hook and :config to customize copilot
```


## PYIM-basedict {#pyim-basedict}

```emacs-lisp
;; 激活 basedict 拼音词库
(use-package pyim-basedict
  :ensure t
  :after pyim
  :config (pyim-basedict-enable)) ;; 拼音词库，五笔用户 *不需要* 此行设置
;; 五笔用户使用 wbdict 词库
;; (use-package pyim-wbdict
;;   :ensure nil
;;   :config (pyim-wbdict-gbk-enable))
```


## POSFRAME {#posframe}

```emacs-lisp
;(use-package posframe
;  :ensure t)
(require 'posframe)
```


## PYIM 拼音 XIAOHE <span class="tag"><span class="after">after</span></span> {#pyim-拼音-xiaohe}

(require 'posframe)

```emacs-lisp
;;(require 'pyim)
;;(require 'pyim-basedict) ;; 拼音词库设置，五笔用户 *不需要* 此行设置
;;(require 'popon)
(use-package pyim
  :ensure t
  :config
  (setq default-input-method "pyim")
  ;; 我使用全拼
  (setq pyim-default-scheme 'xiaohe-shuangpin)
  (pyim-default-scheme 'xiaohe-shuangpin)

  ;; (setq default-input-method "pyim")
  ;; (pyim-default-scheme 'quanpin)
  ;; 设置 pyim 探针设置，这是 pyim 高级功能设置，可以实现 *无痛* 中英文切换 (:-)
  ;; 我自己使用的中英文动态切换规则是：
  ;; 1. 光标只有在注释里面时，才可以输入中文。
  ;; 2. 光标前是汉字字符时，才能输入中。
  ;; 3. 使用 M-j 快捷键，强制将光标前的拼音字符串转换为中文。
  (setq-default pyim-english-input-switch-functions
                '(pyim-probe-dynamic-english
                  pyim-probe-isearch-mode
                  pyim-probe-program-mode
                  pyim-probe-org-structure-template))
  (setq-default pyim-punctuation-half-width-functions
                '(pyim-probe-punctuation-line-beginning
                  pyim-probe-punctuation-after-punctuation))

  ;; 开启拼音搜索功能
  (pyim-isearch-mode 1)
  ;; 使用 pupup-el 来绘制选词框
  (require 'popup)
  (setq pyim-page-tooltip 'popup)
  ;;(setq pyim-page-tooltip 'posframe)
  ;; 选词框显示5个候选词
  (setq pyim-page-length 9)
  ;; 让 Emacs 启动时自动加载 pyim 词库
  (add-hook 'emacs-startup-hook
            #'(lambda () (pyim-restart-1 t))))
(setq pyim-dicts '((:name "default" :file "c:/Users/wacl/begin_new_life/pyim/pyim-bigdict.pyim")
                   (:name "rime-ice" :file "c:/Users/wacl/begin_new_life/pyim/pyim-tsinghua-dict.pyim")))

;;:bind
;;(("M-j" . pyim-convert-code-at-point) ;与 pyim-probe-dynamic-english wouigedauabi
;; ("C-;" . pyim-delete-word-from-personal-buffer))

;;(global-set-key (kbd "C-\\") 'toggle-input-method)
```


## <span class="org-todo todo TODO">TODO</span> EVERYTING 配置搜索中文 CONSULT-LOCATE {#everyting-配置搜索中文-consult-locate}

```emacs-lisp
(require 'delight)
;;使用ripgrep来进行搜索
;;consult-ripgrep
(progn
  (setq consult-locate-args (encode-coding-string "es.exe -i -p -r" 'utf-8))
  (add-to-list 'process-coding-system-alist '("es" gbk . utf-8)))

(eval-after-load 'consult
  (progn
    (setq
     consult-narrow-key "<"
     consult-line-numbers-widen t
     consult-async-min-input 2
     consult-async-refresh-delay  0.1
     consult-async-input-throttle 0.2
     consult-async-input-debounce 0.1)
    ))
```


## <span class="org-todo todo TODO">TODO</span> NOUSINGTOSORT {#nousingtosort}

```emacs-lisp
;;consult-imenu
;;(setq prefix-help-command 'embark-prefix-help-command)
;;(setq completion-styles '(orderless))
(eval-after-load
    'consult
  '(eval-after-load
       'embark
     '(progn
        (require 'embark-consult)
        (add-hook
         'embark-collect-mode-hook
         #'consult-preview-at-point-mode))))
;;(define-key minibuffer-local-map (kbd "C-c C-e") 'embark-export-write)
;;(define-key minibuffer-local-map (kbd "C-c C-e") 'consult-directory-externally)
```

org-bullets tabbar avy


## <span class="org-todo todo TODO">TODO</span> ALT+NUM {#alt-plus-num}

```elisp
(dotimes (n 10)
  (global-unset-key (kbd (format "C-%d" n)))
  (global-unset-key (kbd (format "M-%d" n)))
  )
;; set up my own map
(define-prefix-command 'z-map')
(global-set-key (kbd "C-l" 'z-map))

(define-key z-map (kbd "m") 'mu4e)
(define-key z-map (kbd "e") 'bjm/elfeed-load-db-and-open)
(define-key z-map (kbd "l") 'org-global-cycle)
(define-key z-map (kbd "a") 'org-agenda-show-agenda-and-todo)
(define-key z-map (kbd "s") 'flyspell-correct-word-before-point)
(define-key z-map (kbd "i") (lambda() (interactive) (find-file "~/emacs.org")))
```


## <span class="org-todo todo TODO">TODO</span> KEYCAST {#keycast}

按键展示
[keycast mode](https://github.com/tarsius/keycast) 插件可以在模式栏上展示所有的按键，以及对应的函数。
在模式栏上显示按键详情
我们可以通过 keycast mode 插件在模式栏上展示所有的按键详情（如按键对应的函数）：

```emacs-lisp
(use-package keycast
  ;;:straight (:host github :repo "tarsius/keycast" :files ("dist" "*.el"))
  :ensure t
  :hook (after-init . keycast-mode)
  :config
  ;; set for doom-modeline support
  ;; With the latest change 72d9add, mode-line-keycast needs to be modified to keycast-mode-line.
  ;; :custom-face
  ;; (keycast-key ((t (:background "#0030b4" :weight bold))))
  ;; (keycast-command ((t (:foreground "#0030b4" :weight bold))))

  (define-minor-mode keycast-mode
    "Show current command and its key binding in the mode line (fix for use with doom-mode-line)."
    :global t
    (if keycast-mode
        (progn
          (add-hook 'pre-command-hook 'keycast--update t)
          (add-to-list 'global-mode-string '("" keycast-mode-line "  ")))
      (remove-hook 'pre-command-hook 'keycast--update)
      (setq global-mode-string (delete '("" keycast-mode-line "  ") global-mode-string))
      ))

  (dolist (input '(self-insert-command
                   org-self-insert-command))
    (add-to-list 'keycast-substitute-alist `(,input "." "Typing…")))

  (dolist (event '(mouse-event-p
                   mouse-movement-p
                   mwheel-scroll))
    (add-to-list 'keycast-substitute-alist `(,event nil)))

  (setq keycast-log-format "%-20K%C\n")
  (setq keycast-log-frame-alist
        '((minibuffer . nil)))
  (setq keycast-log-newest-first t)
  )

```

```elisp
(with-eval-after-load 'keycast
    (define-minor-mode keycast-mode
      "Show current command and its key binding in the mode line."
      :global t
      (if keycast-mode
          (add-hook 'pre-command-hook 'keycast--update t)
        (remove-hook 'pre-command-hook 'keycast--update)))
    (add-to-list 'global-mode-string '("" mode-line-keycast))
    )
```


## VALIGN {#valign}

```emacs-lisp
(use-package valign
  ;;:straight (:host github :repo "casouri/valign" :files ("dist" "*.el"))
  :ensure t
  ;;:hook (prog-mode . copilot-mode)
  :init
  (add-hook 'org-mode-hook #'valign-mode))
;;(add-hook 'after-init-hook #'doom-modeline-mode)
```


## OB-HTTP {#ob-http}

```emacs-lisp
(use-package ob-http
  :ensure t)
```


## EF 主题 {#ef-主题}

(setq ef-themes-headings
      (quote ((1 . (variable-pitch 1.1))
              (4 . (regular)))))
             (use-package ef-themes
:init
(setq ef-themes-headings
      (quote ((1 . (variable-pitch 1.1))
              (2 . (regular))
              (4 . (regular)))))
:custom-face
(org-scheduled-today ((t (:inherit org-level-3)))))
       ;;(0 bold variable-pitch 1.4)
       ;;k(i1 . (bold 1.35))
       ;;(2 variable-pitch regular 1.3)
       ;;(3 regular 1.25)
       ;;(4 .(rainbow bold 1.20))
       ;;(5 variable-pitch light 1.15) ;
       ;;(6 variable-pitch 1.1)
       ;;(7 .  (rainbow bold 1.05))
       ;;(t variable-pitch)

[ef themes](https://protesilaos.com/emacs/ef-themes) 是我非常喜欢的一个主题包。

```emacs-lisp
;; Make customisations that affect Emacs faces BEFORE loading a theme
;; (any change needs a theme re-load to take effect).
(use-package ef-themes
  :ensure t
  :bind ("C-c t" . ef-themes-toggle)
  :init
  ;; set two specific themes and switch between them
  ;; If you like two specific themes and want to switch between them, you
  ;; can specify them in `ef-themes-to-toggle' and then invoke the command
  ;; `ef-themes-toggle'.  All the themes are included in the variable
  ;; `ef-themes-collection'.
  (setq ef-themes-to-toggle '(ef-summer ef-winter))
  ;; set org headings and function syntax
  ;;(setq ef-themes-headings
  ;;      '((0 . (bold 1))
  ;;        (1 . (bold 1))
  ;;        (2 . (rainbow bold 1))
  ;;        (3 . (rainbow bold 1))
  ;;        (4 . (rainbow bold 1))
  ;;        (t .)))
  ;; Read the doc string or ;;  absence of weight means `bold'
  (setq ef-themes-headings ; read the manual's entry or the doc string manual for this one.  The symbols can be
        '(
          (0 ultrabold variable-pitch 1.4)
          (1 extrabold variable-pitch 1.35)
          (2 bold variable-pitch 1.3)
          (3 semibold variable-pitch 1.25)
          (4 medium variable-pitch 1.20)
          (5 regular variable-pitch 1.15)
          (6 light variable-pitch 1.10)
          (t semilight variable-pitch 1.05)))

  ;; combined in any order.
  (setq ef-themes-region '(intense no-extend neutral))

  ;; Disable all other themes to avoid awkward blending:
  (mapc #'disable-theme custom-enabled-themes)

  ;; Load the theme of choice:
  ;;(load-theme 'ef-summer :no-confirm)
  ;; The themes we provide are recorded in the `ef-themes-dark-themes',
  ;; `ef-themes-light-themes'.

  ;; 如果你不喜欢随机主题，也可以直接固定选择一个主题，如下：
  ;; OR use this to load the theme which also calls `ef-themes-post-load-hook':
  ;; (ef-themes-select 'ef-summer)

  ;; 随机挑选一款主题，如果是命令行打开Emacs，则随机挑选一款黑色主题
  (if (display-graphic-p)
      (ef-themes-load-random)
    (ef-themes-load-random 'dark))
 ;; (ef-themes-load-random 'dark)
  :config
  ;; auto change theme, aligning with system themes.
  (defun my/apply-theme (appearance)
    "Load theme, taking current system APPEARANCE into consideration."
    (mapc #'disable-theme custom-enabled-themes)
    (pcase appearance
      ('light (if (display-graphic-p) (ef-themes-load-random 'light) (ef-themes-load-random 'dark)))
      ('dark (ef-themes-load-random 'dark))))

  (if (eq system-type 'darwin)
      ;; only for emacs-plus
      (add-hook 'ns-system-appearance-change-functions #'my/apply-theme)
    ;;(ef-themes-select 'ef-summer)
    (ef-themes-load-random 'dark)))

;; They are nil by default...
;;(setq ef-themes-mixed-fonts t ef-themes-variable-pitch-ui t)

;; The themes we provide are recorded in the `ef-themes-dark-themes',
;; `ef-themes-light-themes'.

;; We also provide these commands, but do not assign them to any key:
;;
;; - `ef-themes-toggle'
;; - `ef-themes-select'
;; - `ef-themes-select-dark'
;; - `ef-themes-select-light'
;; - `ef-themes-load-random'
;; - `ef-themes-preview-colors'
;; - `ef-themes-preview-colors-current'
```


## MINIONS {#minions}

插件
[minions](https://github.com/tarsius/minions) 插件能让模式栏变得清爽，将次要模式隐藏起来。

```emacs-lisp
(use-package minions
  :ensure t
  :hook (after-init . minions-mode))
```


## SIMPLE {#simple}

```emacs-lisp
(use-package simple
  :ensure nil
  :hook (after-init . size-indication-mode)
  :init
  (progn
    (setq column-number-mode t)))
```


## CONSULT {#consult}

[consult](https://github.com/minad/consult) 插件基于 Emacs 自带的补全机制，提供了一系列的补全命令。
;;replace swiper :config (global-set-key (kbd "C-s") 'consult-line)

For locate on MacOS:

1.  `locate` is not enabled in MacOS by default. We need to enable it via:
    sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.locate.plist
2.  Then we need to wait `locate` to build db for the whole file system.
3.  If there is something wrong with updating locate db, we can update it manually via:

chomd 755 ~/Library ~/Downloads ~/Documents ~/Desktop
sudo /usr/libexec/locate.updatedb

```emacs-lisp
;; Example configuration for Consult
(use-package consult
  ;; Replace bindings. Lazily loaded due by `use-package'.
  :bind (
         ([remap goto-line]                     . consult-goto-line)
         ([remap isearch-forward]               . consult-line-symbol-at-point) ; my-consult-ripgrep-or-line
         ([remap switch-to-buffer]              . consult-buffer)
         ([remap switch-to-buffer-other-window] . consult-buffer-other-window)
         ([remap switch-to-buffer-other-frame]  . consult-buffer-other-frame)
         ([remap yank-pop]                      . consult-yank-pop)
         ([remap apropos]                       . consult-apropos)
         ([remap bookmark-jump]                 . consult-bookmark)
         ([remap goto-line]                     . consult-goto-line)
         ([remap imenu]                         . consult-imenu)
         ([remap multi-occur]                   . consult-multi-occur)
         ([remap recentf-open-files]            . consult-recent-file)
         ;;("C-x j"                               . consult-mark)
         ;;("C-c g"                               . consult-ripgrep)
         ;;("C-c f"                               . consult-find)
         ;;("\e\ef"                               . consult-locate) ; need to enable locate first
         ;;("C-c n h"                             . my/consult-find-org-headings)
         :map org-mode-map
         ("C-c C-j"                             . consult-org-heading)
         :map minibuffer-local-map
         ("C-r"                                 . consult-history)
         :map isearch-mode-map
         ("C-;"                                 . consult-line)
         :map prog-mode-map
         ("C-c C-j"                             . consult-outline)


         ;; C-c bindings in `mode-specific-map'
         ("C-c M-x" . consult-mode-command)
         ("C-c h" . consult-history)
         ("C-c k" . consult-kmacro)
         ("C-c m" . consult-man)
         ("C-c i" . consult-info)
         ([remap Info-search] . consult-info)
         ;; C-x bindings in `ctl-x-map'
         ("C-x M-:" . consult-complex-command)     ;; orig. repeat-complex-command
         ("C-x b" . consult-buffer)                ;; orig. switch-to-buffer
         ("C-x 4 b" . consult-buffer-other-window) ;; orig. switch-to-buffer-other-window
         ("C-x 5 b" . consult-buffer-other-frame)  ;; orig. switch-to-buffer-other-frame
         ("C-x r b" . consult-bookmark)            ;; orig. bookmark-jump (consult-bookmark NAME)
         ("C-x p b" . consult-project-buffer)      ;; orig. project-switch-to-buffer
         ;; Custom M-# bindings for fast register access
         ("M-#" . consult-register-load)
         ("M-'" . consult-register-store)          ;; orig. abbrev-prefix-mark (unrelated)
         ("C-M-#" . consult-register)
         ;; Other custom bindings
         ("M-y" . consult-yank-pop)                ;; orig. yank-pop
         ;; M-g bindings in `goto-map'
         ("M-g e" . consult-compile-error)
         ("M-g f" . consult-flymake)               ;; Alternative: consult-flycheck
         ("M-g g" . consult-goto-line)             ;; orig. goto-line
         ("M-g M-g" . consult-goto-line)           ;; orig. goto-line
         ("M-g o" . consult-outline)               ;; Alternative: consult-org-heading
         ("M-g m" . consult-mark)
         ("M-g k" . consult-global-mark)
         ("M-g i" . consult-imenu)
         ("M-g I" . consult-imenu-multi)
         ;; M-s bindings in `search-map'
         ("M-s d" . consult-find)
         ("M-s D" . consult-locate)
         ("M-s g" . consult-grep)
         ("M-s G" . consult-git-grep)
         ("M-s r" . consult-ripgrep)
         ("M-s l" . consult-line)
         ("M-s L" . consult-line-multi)
         ("M-s k" . consult-keep-lines)
         ("M-s u" . consult-focus-lines)
         ;; Isearch integration
         ;;("M-s e" . consult-isearch-history)
         :map isearch-mode-map
         ("M-e" . consult-isearch-history)         ;; orig. isearch-edit-string
         ;;("M-s e" . consult-isearch-history)       ;; orig. isearch-edit-string
         ;;("M-s l" . consult-line)                  ;; needed by consult-line to detect isearch
         ;;("M-s L" . consult-line-multi)            ;; needed by consult-line to detect isearch
         ;; Minibuffer history
         :map minibuffer-local-map
         ;;("M-s" . consult-history)                 ;; orig. next-matching-history-element
         ("M-r" . consult-history))                ;; orig. previous-matching-history-element

  ;; Enable automatic preview at point in the *Completions* buffer. This is
  ;; relevant when you use the default completion UI.
  :hook (completion-list-mode . consult-preview-at-point-mode)

  ;; The :init configuration is always executed (Not lazy)
  :init

  ;; Optionally configure the register formatting. This improves the register
  ;; preview for `consult-register', `consult-register-load',
  ;; `consult-register-store' and the Emacs built-ins.
  (setq register-preview-delay 0.1
        register-preview-function #'consult-register-format)

  ;; Optionally tweak the register preview window.
  ;; This adds thin lines, sorting and hides the mode line of the window.
  (advice-add #'register-preview :override #'consult-register-window)

  ;; MacOS locate doesn't support `--ignore-case --existing' args.
  (setq consult-locate-args (pcase system-type
                              ('gnu/linux "locate --ignore-case --existing --regex")
                              ('darwin "mdfind -name")))

  ;; Use Consult to select xref locations with preview
  (setq xref-show-xrefs-function #'consult-xref
        xref-show-definitions-function #'consult-xref)

  ;; Configure other variables and modes in the :config section,
  ;; after lazily loading the package.
  :config

  ;; Optionally configure preview. The default value
  ;; is 'any, such that any key triggers the preview.
  ;; (setq consult-preview-key 'any)
  ;; (setq consult-preview-key "M-.")
  ;; (setq consult-preview-key '("S-<down>" "S-<up>"))
  ;; For some commands and buffer sources it is useful to configure the
  ;; :preview-key on a per-command basis using the `consult-customize' macro.
  (consult-customize
   consult-theme :preview-key '(:debounce 0.2 any)
   consult-ripgrep consult-git-grep consult-grep
   consult-bookmark consult-recent-file consult-xref
   consult--source-bookmark consult--source-file-register
   consult--source-recent-file consult--source-project-recent-file
   ;; :preview-key "M-."
   :preview-key '(:debounce 0.4 any))

  ;; Optionally configure the narrowing key.
  ;; Both < and C-+ work reasonably well.
  (setq consult-narrow-key "<") ;; "C-+";; (kbd "C-+")

  ;; Optionally make narrowing help available in the minibuffer.
  ;; You may want to use `embark-prefix-help-command' or which-key instead.
  ;; (define-key consult-narrow-map (vconcat consult-narrow-key "?") #'consult-narrow-help)

  ;; By default `consult-project-function' uses `project-root' from project.el.
  ;; Optionally configure a different project root function.
  ;;;; 1. project.el (the default)
  ;; (setq consult-project-function #'consult--default-project--function)
  ;;;; 2. vc.el (vc-root-dir)
  ;; (setq consult-project-function (lambda (_) (vc-root-dir)))
  ;;;; 3. locate-dominating-file
  ;; (setq consult-project-function (lambda (_) (locate-dominating-file "." ".git")))
  ;;;; 4. projectile.el (projectile-project-root)
  ;; (autoload 'projectile-project-root "projectile")
  ;; (setq consult-project-function (lambda (_) (projectile-project-root)))
  ;;;; 5. No project support
  ;; (setq consult-project-function nil)


  (autoload 'projectile-project-root "projectile")
  (setq consult-project-root-function #'projectile-project-root)

  ;; search all org file headings under a directory, see:
  ;; https://emacs-china.org/t/org-files-heading-entry/20830/4
  (defun my/consult-find-org-headings (&optional match)
    "find headngs in all org files."
    (interactive)
    (consult-org-heading match (directory-files org-directory t "^[0-9]\\{8\\}.+\\.org$")))

  ;; Use `consult-ripgrep' instead of `consult-line' in large buffers
  (defun consult-line-symbol-at-point ()
    "Consult line the synbol where the point is"
    (interactive)
    (consult-line (thing-at-point 'symbol))))
```


## ALL-THE-ICONS-COMPLETION {#all-the-icons-completion}

补全图标美化
[all-the-icons-completion](https://github.com/iyefrat/all-the-icons-completion) 插件给候选添上漂亮的图标。

```emacs-lisp
(use-package all-the-icons-completion
  :ensure t
  :hook ((after-init . all-the-icons-completion-mode)
         (marginalia-mode . all-the-icons-completion-marginalia-setup))
  )
```


## EMBARK-CONSULT {#embark-consult}

```emacs-lisp
(use-package embark-consult
  :ensure t
  :after embark
  :hook (embark-collect-mode . consult-preview-at-point-mode))
```


## EMBARK {#embark}

[embark](https://github.com/oantolin/embark) 插件提供了一系列的迷你缓冲区的类似右键机制的增强。

```emacs-lisp
(use-package embark
  :ensure t
  :bind (([remap describe-bindings] . embark-bindings)
         ("C-'" . embark-act)
         :map minibuffer-local-map
         :map minibuffer-local-completion-map
         ("TAB" . minibuffer-force-complete)
         :map embark-file-map
         ("E" . consult-file-externally)      ; Open file externally, or `we' in Ranger
         ("O" . consult-directory-externally) ; Open directory externally
         )
  :init
  ;; Optionally replace the key help with a completing-read interface
  (setq prefix-help-command #'embark-prefix-help-command)
  :config
  ;; Show Embark actions via which-key
  (setq embark-action-indicator
        (lambda (map)
          (which-key--show-keymap "Embark" map nil nil 'no-paging)
          #'which-key--hide-popup-ignore-command)
        embark-become-indicator embark-action-indicator)

  ;; open directory
  (defun consult-directory-externally (file)
    "Open directory externally using the default application of the system."
    (interactive "fOpen externally: ")
    (if (and (eq system-type 'windows-nt)
             (fboundp 'w32-shell-execute))
        (shell-command-to-string (encode-coding-string (replace-regexp-in-string "/" "\\\\"
                                                                                 (format "explorer.exe %s" (file-name-directory (expand-file-name file)))) 'gbk))
      (call-process (pcase system-type
                      ('darwin "open")
                      ('cygwin "cygstart")
                      (_ "xdg-open"))
                    nil 0 nil
                    (file-name-directory (expand-file-name file)))))

  ;; Hide the mode line of the Embark live/completions buffers
  (add-to-list 'display-buffer-alist
               '("\\`\\*Embark Collect \\(Live\\|Completions\\)\\*"
                 nil
                 (window-parameters (mode-line-format . none))))
  )
```


## SMARTPARENS {#smartparens}

<https://github.com/Fuco1/smartparens>

```elisp
(use-package smartparens
  :ensure t
  :diminish smartparens-mode
  :config
  (smartparens-global-mode)
  (require 'smartparens-config)
  )
```


## GO-TRANLATE {#go-tranlate}

```emacs-lisp
(use-package go-translate
  :ensure t
  :config
  (setq gts-translate-list '(("en" "zh")))
  ;; (setq gts-default-translator (gts-translator :engines (gts-bing-engine)))
  (setq gts-default-translator
        (gts-translator
         :picker (gts-prompt-picker)
         :engines (list (gts-google-engine) (gts-google-rpc-engine))
         :render (gts-buffer-render)))
  )
```


## DOOM-MODELINE 模式栏设置 {#doom-modeline-模式栏设置}

[doom-modeline](https://github.com/seagle0128/doom-modeline) 是一个模式栏美化插件。

```emacs-lisp
(use-package doom-modeline
  :ensure t
  :hook (after-init . doom-modeline-mode)
  ;;:init (doom-modeline-mode t)(doom-modeline-mode 1)
  :custom
  (doom-modeline-irc nil)
  (doom-modeline-mu4e nil)
  (doom-modeline-gnus nil)
  (doom-modeline-github nil)
  (doom-modeline-buffer-file-name-style 'truncate-upto-root) ; : auto
  (doom-modeline-persp-name nil)
  (doom-modeline-unicode-fallback t)
  (doom-modeline-enable-word-count nil)
  ;;fix doom modeline
  :custom-face
  (mode-line ((t (:height 0.75))))
  (mode-line-inactive ((t (:height 0.8)))))
```


## <span class="org-todo todo TODO">TODO</span> DOOM-MODELINE tangle ~/emacs-babel.el {#doom-modeline-tangle-emacs-babel-dot-el}

```emacs-lisp
(use-package doom-modeline
  :config
  (set-face-attribute 'mode-line nil :font
                      (format   "%s:size=%d"  "Hack" 32))
  (set-face-attribute 'mode-line-inactive nil :font
                      (format   "%s:size=%d"  "Hack" 32))
  (setq inhibit-compacting-font-caches t
        doom-modeline-buffer-file-name-style 'auto
        doom-modeline-buffer-encoding nil))
;;(require 'doom-modeline)
;;(setq doom-modeline-height 36)
;;(set-face-attribute 'mode-line nil :height 160)
```


## <span class="org-todo todo TODO">TODO</span> LEDGER :tangle ~/emacs-babel.el {#ledger-tangle-emacs-babel-dot-el}

ledger-mode
This Emacs library provides a major mode for editing files in the format used by the ledger command-line accounting system.
It also provides automated support for some ledger workflows, such as reconciling transactions, or running certain reports.
Installation
If you choose not to use one of the convenient packages in MELPA or MELPA Stable, you'll need to add the directory containing ledger-mode.el to your load-path, and then (require 'ledger-mode).
Configuring completion

Earlier ledger-mode versions had an always-on TAB completion system, but now the code uses the standard Emacs completion-at-point system for compatibility with all completion UIs, e.g. company or helm.

See the "Adding Transactions" section of the ledger-mode Info manual for more information.
Getting started

ledger-mode will automatically associate itself with .ledger files when installed as a package. ledger-mode includes documentation in info format, accessible through Emacs with C-h i. The info chapter includes a quick demo as well as more extensive documentation.
Related packages

In-buffer checking of formatting and balancing of transactions is available built-in for Emacs version 26 and later using flymake-mode. For flycheck users (and users of Emacs 25 and earlier), flycheck-ledger is available.
<https://github.com/ledger/ledger-mode>

```emacs-lisp
(use-package ledger-mode
  :ensure t
  :after evil
  :init
  (setq ledger-clear-whole-transations 1)

  :config
  (add-to-list 'evil-emacs-state-modes 'ledger-report-mode)
  :mode "\\.dat'")
```


## EVIL {#evil}

(define-key org-mode-map (kbd "&lt;tab&gt;") 'org-cycle)

```emacs-lisp
(use-package evil
  :ensure t
  :init
  ;;(setq evil-want-integration t) ;; This is optional since it's already set to t by default.
  ;;(setq evil-want-keybinding nil) ;; 禁用 Evil 的默认按键绑定
  :config
  (evil-mode) ;; evil-mode 1
  (progn
    ;;   (eval-after-load "evil" '(define-key evil-normal-state-map "<tab>" 'org-cycle))
    ;;(with-eval-after-load 'evil (define-key evil-normal-state-map "<tab>" 'org-cycle))
    ;;(define-key copilot-completion-map (kbd "<tab>") 'copilot-accept-completion)
    ;;(define-key copilot-completion-map (kbd "TAB") 'copilot-accept-completion)
    ;;(eval-after-load "evil" '(define-key evil-motion-state-map "TAB" 'org-cycle))
    (eval-after-load "evil" '(define-key evil-normal-state-map "j" 'evil-next-visual-line))
    (eval-after-load "evil" '(define-key evil-visual-state-map "j" 'evil-next-visual-line))
    (eval-after-load "evil" '(define-key evil-normal-state-map "k" 'evil-previous-visual-line))
    (eval-after-load "evil" '(define-key evil-visual-state-map "k" 'evil-previous-visual-line))
    (eval-after-load "evil" '(define-key evil-normal-state-map "M" 'evil-join-whitespace))
    (eval-after-load "evil" '(define-key evil-normal-state-map "s" 'nil))
    (eval-after-load "evil" '(define-key evil-motion-state-map "s" 'nil))
    (eval-after-load "evil" '(define-key evil-normal-state-map "sc" 'evil-window-delete))
    (eval-after-load "evil" '(define-key evil-motion-state-map "sc" 'evil-window-delete))
    (eval-after-load "evil" '(define-key evil-normal-state-map "sj" 'evil-window-down))
    (eval-after-load "evil" '(define-key evil-motion-state-map "sj" 'evil-window-down))
    (eval-after-load "evil" '(define-key evil-normal-state-map "sk" 'evil-window-up))
    (eval-after-load "evil" '(define-key evil-motion-state-map "sk" 'evil-window-up))
    (eval-after-load "evil" '(define-key evil-normal-state-map "so" 'delete-other-windows))
    (eval-after-load "evil" '(define-key evil-motion-state-map "so" 'delete-other-windows))
    ;;(eval-after-load "evil" '(define-key evil-normal-state-map "so" 'spacemacs/toggle-maximize-buffer))
    (eval-after-load "evil" '(define-key evil-motion-state-map "sh" 'evil-window-left))
    (eval-after-load "evil" '(define-key evil-motion-state-map "sl" 'evil-window-right))

    (eval-after-load "evil" '(define-key evil-insert-state-map "\C-a" 'beginning-of-visual-line))
    (eval-after-load "evil" '(define-key evil-insert-state-map "\C-e" 'end-of-visual-line))
    (eval-after-load "evil" '(define-key evil-insert-state-map "\C-f" 'right-char))
    (eval-after-load "evil" '(define-key evil-insert-state-map "\C-b" 'left-char))
    (eval-after-load "evil" '(define-key evil-normal-state-map "\C-d" 'evil-delete-char))
    (eval-after-load "evil" '(define-key evil-insert-state-map "\C-d" 'evil-delete-char))
    (eval-after-load "evil" '(define-key evil-motion-state-map "\C-d" 'evil-delete-char))

    (eval-after-load "evil" '(define-key evil-motion-state-map  "zl" 'next-buffer))
    (eval-after-load "evil" '(define-key evil-normal-state-map  "zl" 'next-buffer))
    (eval-after-load "evil" '(define-key evil-motion-state-map  "zh" 'previous-buffer))
    (eval-after-load "evil" '(define-key evil-normal-state-map  "zh" 'previous-buffer))))
;;(eval-after-load 'evil-vars
;;'(define-key evil-ex-completion-map (kbd "C-j") 'next-complete-history-element))
;;(eval-after-load 'evil-vars
;;'(define-key evil-ex-completion-map (kbd "C-k") 'previous-complete-history-element))
;;(define-key evil-ex-completion-map (kbd "C-j") 'next-complete-history-element)
;;(define-key evil-ex-completion-map (kbd "C-k") 'previous-complete-history-element)
```


## <span class="org-todo todo TODO">TODO</span> EVIL-COLLECTION :tangle ~/emacs-babel.el {#evil-collection-tangle-emacs-babel-dot-el}

```emacs-lisp
(use-package evil-collection
  :after evil
  :ensure t
  :config
  (evil-collection-init))
```


## EVIL-MATCHIT {#evil-matchit}

```emacs-lisp
(use-package evil-matchit
  :ensure t
  :after evil
  :init
  (global-evil-matchit-mode 1))
```


## EVIL-TRACES {#evil-traces}

```emacs-lisp
(use-package evil-traces
  :after evil
  :ensure t
  :config
  (evil-traces-use-diff-faces) ; if you want to use diff's faces
  (evil-traces-mode))
```


## EVIL-GOGGLES {#evil-goggles}

```emacs-lisp
(use-package evil-goggles
  :ensure t
  :after evil
  :config
  (evil-goggles-mode)

  ;; optionally use diff-mode's faces; as a result, deleted text
  ;; will be highlighed with `diff-removed` face which is typically
  ;; some red color (as defined by the color theme)
  ;; other faces such as `diff-added` will be used for other actions
  (evil-goggles-use-diff-faces))
```


## <span class="org-todo todo TODO">TODO</span> EVIL-REPEAT-FIND-CHAR-PINYIN {#evil-repeat-find-char-pinyin}

tangle ~/.custom.el

```emacs-lisp
(eval-after-load "evil" '(define-key evil-normal-state-map  ";" 'evil-repeat-find-char-pinyin))
(eval-after-load "evil" '(define-key evil-motion-state-map  ";" 'evil-repeat-find-char-pinyin))
(define-key evil-motion-state-map (kbd ";") 'evil-repeat-find-char-pinyin)
(define-key evil-normal-state-map (kbd ";") 'evil-repeat-find-char-pinyin)
;;(define-key evil-normal-state-map (kbd ",") 'evil-repeat-find-char-pinyin-reverse)
;;(define-key evil-motion-state-map (kbd ",") 'evil-repeat-find-char-pinyin-reverse)
;;(global-set-key (kbd ";") 'evil-repeat-find-char-pinyin)
;;(evil-define-key 'normal global-map (kbd ";") 'evil-repeat-find-char-pinyin)
;;(global-set-key (kbd ";") nil)
;;(global-unset-key (kbd ";f"))
;;(global-set-key (kbd ";g") 'nil)
;;(define-key evil-normal-state-map (kbd "H") 'previous-buffer)
;;(define-key evil-normal-state-map (kbd ";") (kbd ";"))
;;(eval-after-load "shell" '(define-key evil-insert-state-map "\C-k" 'nil))
;;(eval-after-load "shell" '(define-key evil-insert-state-map "\C-k" 'previous-line))
;;(eval-after-load "shell" '(define-key evil-insert-state-map "\C-j" 'next-line))
;;(eval-after-load "eshell" '(define-key eshell-hist-mode-map "\C-j" 'eshell-next-matching-input-from-input))
;;(eval-after-load "eshell" '(define-key evil-insert-state-map "\C-k" 'eshell-previous-matching-input-from-input))
;;(eval-after-load "eshell" '(define-key eshell-hist-mode-map "\C-k" 'eshell-previous-matching-input-from-input))
```


## <span class="org-todo todo TODO">TODO</span> IVY :search {#ivy-search}

```emacs-lisp
(use-package ivy
  :ensure t
  :diminish (ivy-mode)
  :bind (("C-x b" . ivy-switch-buffer))
  :config
  (ivy-mode)
  (setq ivy-display-style 'fancy)
  (setq ivy-use-virtual-buffers t)
  (setq enable-recursive-minibuffers t)
  ;; enable this if you want `swiper' to use it
  (setq search-default-mode #'char-fold-to-regexp)
  ;; (global-set-key "\C-s" 'swiper)
  (global-set-key (kbd "C-c C-r") 'ivy-resume)
  (global-set-key (kbd "<f6>") 'ivy-resume)
  (global-set-key (kbd "M-x") 'counsel-M-x)
  (global-set-key (kbd "C-x C-f") 'counsel-find-file)
  (global-set-key (kbd "<f1> f") 'counsel-describe-function)
  (global-set-key (kbd "<f1> v") 'counsel-describe-variable)
  (global-set-key (kbd "<f1> o") 'counsel-describe-symbol)
  (global-set-key (kbd "<f1> l") 'counsel-find-library)
  ;;(global-set-key (kbd "<f2> i") 'counsel-info-lookup-symbol)
  ;;(global-set-key (kbd "<f2> u") 'counsel-unicode-char)
  (global-set-key (kbd "C-c g") 'counsel-git)
  (global-set-key (kbd "C-c j") 'counsel-git-grep)
  (global-set-key (kbd "C-c k") 'counsel-rg) ;; counsel-rg
  (global-set-key (kbd "C-x l") 'counsel-locate)
  (global-set-key (kbd "C-S-o") 'counsel-rhythmbox)
  (define-key minibuffer-local-map (kbd "C-r") 'counsel-minibuffer-history))
```


## <span class="org-todo todo TODO">TODO</span> AUTO-COMPLETE {#auto-complete}

```emacs-lisp
(use-package auto-comlete
  :ensure t
  :init
  (progn
    ac-config-default)
  (global-auto-complete-mode 1))
```


## SWIPER {#swiper}

```emacs-lisp
(use-package swiper
  :ensure try
  :bind (("C-s" . swiper)
         ("C-r" . swiper)
         ("C-c C-r" . ivy-resume)
         ("M-x" . counsel-M-x)
         ("C-x C-f" . counsel-find-file))
  :config
  (progn
    (ivy-mode 1)
    (setq ivy-use-virtual-buffers t)
    (setq ivy-display-style 'fancy)
    (define-key read-expression-map (kbd "C-r") 'counsel-expression-history)
    ))
```


## <span class="org-todo todo TODO">TODO</span> simple-httpd {#simple-httpd}

```emacs-lisp
(use-package simple-httpd
  :ensure t)
(require 'simple-httpd)
;(setq httpd-root "/var/www")
;;(httpd-start)
```


## <span class="org-todo todo TODO">TODO</span> js2-mode {#js2-mode}


## YASNIPPET 模板补全 {#yasnippet-模板补全}

[yasnippet](https://github.com/joaotavora/yasnippet) 插件是一个非常强大的模板补全系统。

```emacs-lisp
;; yasnippet settings
(use-package yasnippet
  :ensure t
  :diminish yas-minor-mode
  ;;:hook ((after-init . yas-reload-all)
  ;;       ((prog-mode LaTeX-mode org-mode) . yas-minor-mode))
  :hook ((prog-mode . yas-minor-mode)
         (org-mode . yas-minor-mode))
  ;; Suppress warning for yasnippet code.
  :config
  (require 'warnings)
  (add-hook 'org-mode-hook #'yas-minor-mode)
  ;;(add-hook 'prog-mode-hook #'yas-minor-mode)
  (add-to-list 'warning-suppress-types '(yasnippet backquote-change))
  (add-to-list 'yas-snippet-dirs "~/.emacs.d/snippets/")
  (yas-global-mode)
  (yas-reload-all)
   ;;(setq yas-snippet-dirs '("~/.emacs.d/snippets/"))
   ;;(setq yas-prompt-functions '(yas-x-prompt yas-dropdown-prompt))
  (setq yas-prompt-functions '(yas-x-prompt yas-dropdown-prompt))
  (progn
    ;;(yas-recompile-all) (yas-reload-all)
    ;;(define-key yas-keymap (kbd "TAB") nil)
    ;;(define-key yas-keymap [(tab)] nil)
    ;;(define-key yas-keymap [(shift tab)] nil)
    ;;(define-key yas-keymap [backtab] nil)
    (define-key yas-keymap (kbd "C-j") 'yas-next-field-or-maybe-expand)
    (define-key yas-keymap (kbd "C-k") 'yas-prev-field)
    (define-key yas-keymap (kbd "M-j") 'yas-expand))

  (defun smarter-yas-expand-next-field ()
    "Try to `yas-expand' then `yas-next-field' at current cursor position."
    (interactive)
    (let ((old-point (point))
          (old-tick (buffer-chars-modified-tick)))
      (yas-expand)
      (when (and (eq old-point (point))
                 (eq old-tick (buffer-chars-modified-tick)))
        (ignore-errors (yas-next-field))))))
;;(setq company-auto-complete-chars '((kbd "RET")))
;;(define-key company-active-map (kbd [tab]) #'smarter-yas-expand-next-field-complete)
```

company-complete-common-or-show-delayed-tooltip
(with-eval-after-load 'company
    (define-key company-active-map [tab] #'yas-expand)
    (define-key company-active-map (kbd "TAB") #'yas-expand)
    (define-key company-active-map (kbd "TAB") #'yas-expand))
  ;;(define-key company-active-map (kbd "M-n") nil)
  ;;(define-key company-active-map (kbd "M-p") nil)
  ;;(define-key company-active-map (kbd "C-n") #'company-select-next)
  ;;(define-key company-active-map (kbd "C-p") #'company-select-previous)
(with-eval-after-load 'company
(:map company-active-map
( . smarter-yas-expand-next-field-complete)
("TAB" . smarter-yas-expand-next-field-complete))')
(defun smarter-yas-expand-next-field-complete ()
    "Try to \`yas-expand' and \`yas-next-field' at current cursor position.

If failed try to complete the common part with \`company-complete-common'"
    (interactive)
    (if yas-minor-mode
        (let ((old-point (point))
              (old-tick (buffer-chars-modified-tick)))
          (yas-expand)
          (when (and (eq old-point (point))
                     (eq old-tick (buffer-chars-modified-tick)))
            (ignore-errors (yas-next-field))
            (when (and (eq old-point (point))
                       (eq old-tick (buffer-chars-modified-tick)))
              (company-complete-common))))
      (company-complete-common)))


## IVY-YASNIPPET {#ivy-yasnippet}

```emacs-lisp
(use-package ivy-yasnippet
  :after ivy
  :ensure t)
```


## ORG mode 基本配置 对 Org mode 基本配置进行修改。 {#org-mode-基本配置-对-org-mode-基本配置进行修改}

```emacs-lisp
(require 'org)
(require 'org-duration)
(require 'org-tempo)
(require 'org-checklist)
(use-package org
  :ensure nil
                                        ;:init
  ;:mode ("\\.org\\'" . org-mode)
  :hook ((org-mode . visual-line-mode)
         (org-mode . my/org-prettify-symbols))
  :commands (org-find-exact-headline-in-buffer org-set-tags)
  :custom-face
  ;; 设置Org mode标题以及每级标题行的大小

  ;;(org-document-title ((t (:height 1.5 :weight bold))))
  ;;(org-level-1 ((t (:height 1.2 :weight bold))))
  ;;(org-level-2 ((t (:height 1.15 :weight bold))))
  ;;(org-level-3 ((t (:height 1.1 :weight bold))))
  ;;(org-level-4 ((t (:height 1.05 :weight bold))))
  ;;(org-level-5 ((t (:height 1.0 :weight bold))))
  ;;(org-level-6 ((t (:height 1.0 :weight bold))))
  ;;(org-level-7 ((t (:height 1.0 :weight bold))))
  ;;(org-level-8 ((t (:height 1.0 :weight bold))))
  ;;(org-level-9 ((t (:height 1.0 :weight bold))))

  ;; 设置代码块用上下边线包裹
  (org-block-begin-line ((t (:underline t :background unspecified))))
  (org-block-end-line ((t (:overline t :underline nil :background unspecified))))
  :config
  ;; ================================
  ;; 在org mode里美化字符串
  ;; ================================
  (defun my/org-prettify-symbols ()
    (setq prettify-symbols-alist
          (mapcan
           (lambda (x)
             (list x (cons (upcase (car x)) (cdr x))))
           '(
             ("[ ]" . 9744)         ; ☐
             ("[X]" . 9745)         ; ☑
             ("[-]" . 8863)         ; ⊟
             ("<->" . ?⭤)
             ("->" . 8594)    ; →
             ("=>" . 8658)    ; ⇒
             ("->" . ?→)
             ("<-" . ?⭠)
             (">>" . ?»)
             ("<<" . ?«))))

    (setq prettify-symbols-unprettify-at-point t)
    (prettify-symbols-mode 1))

  ;; 提升latex预览的图片清晰度
  (plist-put org-format-latex-options :scale 1.8)

  ;; 设置标题行之间总是有空格；列表之间根据情况自动加空格
  (setq org-blank-before-new-entry '((heading . t)
                                     (plain-list-item . auto)))


  ;; ======================================
  ;; 设置打开Org links的程序
  ;; ======================================
  (defun my-func/open-and-play-gif-image (file &optional link)
    "Open and play GIF image `FILE' in Emacs buffer.

Optional for Org-mode file: `LINK'."
    (let ((gif-image (create-image file))
          (tmp-buf (get-buffer-create "*Org-mode GIF image animation*")))
      (switch-to-buffer tmp-buf)
      (erase-buffer)
      (insert-image gif-image)
      (image-animate gif-image nil t)
      (local-set-key (kbd "q") 'bury-buffer)))

  (setq org-file-apps '(("\\.png\\'"     . default)
                        (auto-mode       . emacs)
                        (directory       . emacs)
                        ("\\.mm\\'"      . default)
                        ("\\.x?html?\\'" . default)
                        ("\\.pdf\\'"     . emacs)
                        ("\\.md\\'"      . emacs)
                        ("\\.gif\\'"     . my-func/open-and-play-gif-image)
                        ("\\.xlsx\\'"    . default)
                        ("\\.svg\\'"     . default)
                        ("\\.pptx\\'"    . default)
                        ("\\.docx\\'"    . default)))

  :custom
  ;; 设置Org mode的目录
  (org-directory "~/begin_new_life/org/org/")
  ;; 设置笔记的默认存储位置
  (org-default-notes-file (expand-file-name "capture.org" org-directory))
  ;;(org-default-notes-file (concat org-directory "notes.org"))
  ;; 启用一些子模块
  (org-modules '(ol-bibtex ol-info ol-eww org-habit org-protocol))
  ;;(org-modules '(ol-doi ol-w3m ol-bbdb ol-bibtex ol-docview ol-gnus ol-info ol-irc ol-mhe ol-rmail ol-eww))
  ;;(ol-bibtex ol-gnus ol-info ol-eww org-habit org-protocol)
  ;; 在按M-RET时，是否根据光标所在的位置分行，这里设置为是
  ;; (org-M-RET-may-split-line '((default . nil)))
  ;; 一些Org mode自带的美化设置
  ;; 标题行美化
  (org-fontify-whole-heading-line t)
  ;; 设置标题行折叠符号 (org-ellipsis " ▾")  (org-ellipsis " ⭍")
  ;; 在活动区域内的所有标题栏执行某些命令
  (org-loop-over-headlines-in-active-region t)
  ;; TODO标签美化
  (org-fontify-todo-headline t)
  ;; DONE标签美化
  (org-fontify-done-headline t)
  ;; 引用块美化
  (org-fontify-quote-and-verse-blocks t)
  ;; 隐藏宏标记
  (org-hide-macro-markers t)
  ;; 隐藏强调标签
  (org-hide-emphasis-markers t)
  ;; 高亮latex语法
  (org-highlight-latex-and-related '(native script entities))
  ;; 以UTF-8显示
  (org-pretty-entities t)
  ;; 是否隐藏标题栏的前置星号，这里我们通过org-modern来隐藏
  ;; (org-hide-leading-stars t)
  ;; 当启用缩进模式时自动隐藏前置星号
  (org-indent-mode-turns-on-hiding-stars t)
  ;; 自动启用缩进
  (org-startup-indented nil)
  ;; 根据标题栏自动缩进文本
  (org-adapt-indentation nil)
  ;; 自动显示图片
  (org-startup-with-inline-images nil)
  ;; 默认以Overview的模式展示标题行
  ;;(defun turn-on-org-show-all-inline-images () (org-display-inline-images t t))
  ;;(add-hook 'org-mode-hook 'turn-on-org-show-all-inline-images)
  ;; 默认以Overview的模式展示标题行
  (org-startup-folded 'overview)
  ;; 允许字母列表
  (org-list-allow-alphabetical t)
  ;; 列表的下一级设置
  (org-list-demote-modify-bullet '(
                                   ("-"  . "+")
                                   ("+"  . "1.")
                                   ("1." . "a.")))

  ;; 编辑时检查是否在折叠的不可见区域
  (org-fold-catch-invisible-edits 'smart)
  ;; 在当前位置插入新标题行还是在当前标题行后插入，这里设置为当前位置
  (org-insert-heading-respect-content nil)
  ;; 设置图片的最大宽度，如果有imagemagick支持将会改变图片实际宽度
  ;; 四种设置方法：(1080), 1080, t, nil
  (org-image-actual-width nil)
  ;; imenu的最大深度，默认为2
  (org-imenu-depth 4)
  ;; 回车要不要触发链接，这里设置不触发
  (org-return-follows-link t)
  ;; 上标^下标_是否需要特殊字符包裹，这里设置需要用大括号包裹
  (org-use-sub-superscripts '{})
  ;; 复制粘贴标题行的时候删除id
  (org-clone-delete-id t)
  ;; 粘贴时调整标题行的级别
  (org-yank-adjusted-subtrees t)

  ;; TOOD的关键词设置，可以设置不同的组
  ;;(org-todo-keywords '((sequence "TODO(t)" "HOLD(h!)" "WIP(i!)" "WAIT(w!)" "|" "DONE(d!)" "CANCELLED(c@/!)")
  ;;                     (sequence "REPORT(r)" "BUG(b)" "KNOWNCAUSE(k)" "|" "FIXED(f!)")))
  ;; TODO关键词的样式设置
  ;;(org-todo-keyword-faces '(("TODO"       :foreground "#7c7c75" :weight bold)
  ;;                          ("HOLD"       :foreground "#feb24c" :weight bold)
  ;;                          ("WAIT"       :foreground "#9f7efe" :weight bold)
  ;;                          ("DONE"       :foreground "#50a14f" :weight bold)))
  ;; 当标题行状态变化时标签同步发生的变化
  ;; Moving a task to CANCELLED adds a CANCELLED tag
  ;; Moving a task to WAIT adds a WAIT tag
  ;; Moving a task to HOLD adds WAIT and HOLD tags
  ;; Moving a task to a done state removes WAIT and HOLD tags
  ;; Moving a task to TODO removes WAIT, CANCELLED, and HOLD tags
  ;; Moving a task to DONE removes WAIT, CANCELLED, and HOLD tags
  (org-todo-state-tags-triggers
   (quote (("CANCELLED" ("CANCELLED" . t))
           ("WAIT" ("WAIT" . t))
           ("HOLD" ("WAIT") ("HOLD" . t))
           (done ("WAIT") ("HOLD"))
           ("TODO" ("WAIT") ("CANCELLED") ("HOLD"))
           ("DONE" ("WAIT") ("CANCELLED") ("HOLD")))))
  ;; 使用专家模式选择标题栏状态
  (org-use-fast-todo-selection 'expert)
  ;; 父子标题栏状态有依赖
  (org-enforce-todo-dependencies t)
  ;; 标题栏和任务复选框有依赖
  (org-enforce-todo-checkbox-dependencies t)
  ;; 优先级样式设置
  (org-priority-faces '((?A :foreground "red")
                        (?B :foreground "orange")
                        (?C :foreground "yellow")))
  ;; 标题行全局属性设置
  (org-global-properties '(("EFFORT_ALL" . "0:15 0:30 0:45 1:00 2:00 3:00 4:00 5:00 6:00 7:00 8:00")
                           ("APPT_WARNTIME_ALL" . "0 5 10 15 20 25 30 45 60")
                           ("RISK_ALL" . "Low Medium High")
                           ("STYLE_ALL" . "habit")))
  ;; Org columns的默认格式
  (org-columns-default-format "%25ITEM %TODO %SCHEDULED %DEADLINE %3PRIORITY %TAGS %CLOCKSUM %EFFORT{:}")
  ;; 当状态从DONE改成其他状态时，移除 CLOSED: [timestamp]
  (org-closed-keep-when-no-todo t)
  ;; DONE时加上时间戳
  (org-log-done 'time)
  ;; 重复执行时加上时间戳
  (org-log-repeat 'time)
  ;; Deadline修改时加上一条记录
  (org-log-redeadline 'note)
  ;; Schedule修改时加上一条记录
  (org-log-reschedule 'note)
  ;; 以抽屉的方式记录
  (org-log-into-drawer t)
  ;; 紧接着标题行或者计划/截止时间戳后加上记录抽屉
  (org-log-state-notes-insert-after-drawers nil)


  ;; refile使用缓存
  (org-refile-use-cache t)
  ;; refile的目的地，这里设置的是agenda文件的所有标题
  (org-refile-targets '((nil :maxlevel . 9) (org-agenda-files . (:maxlevel . 9))))
  ;; 将文件名加入到路径
  (org-refile-use-outline-path 'full-file-path)
  ;; 是否按步骤refile
  (org-outline-path-complete-in-steps nil) ; 新宝不能用
  ;; 允许创建新的标题行，但需要确认
  (org-refile-allow-creating-parent-nodes 'confirm)

  ;; 设置标签的默认位置，默认是第77列右对齐
  ;; (org-tags-column -77)
  ;; 自动对齐标签
  (org-auto-align-tags t)
  ;; 标签不继承
  (org-use-tag-inheritance nil)
  ;; 在日程视图的标签不继承
  (org-agenda-use-tag-inheritance nil)
  ;; 标签快速选择
  (org-use-fast-tag-selection t)
  ;; 标签选择不需要回车确认
  (org-fast-tag-selection-single-key t)
  ;; 定义了有序属性的标题行也加上 OREDERD 标签
  (org-track-ordered-property-with-tag t))
;; 始终存在的的标签 (org-tag-persistent-alist '(("read"     . ?r) ("home"     . ?h) ("emacs"    . ?e) ("study"    . ?s) ("work"     . ?w)))
;; 预定义好的标签 (org-tag-alist '((:startgroup)
;;("crypt"    . ?c)
;;("linux"    . ?l) ("apple"    . ?a)
;;("noexport" . ?n) ("ignore"   . ?i)
;;("TOC"      . ?t) (:endgroup) (:startgroup . nil) ("@work" . ?w)
;; 工作 ("@home" . ?h)
;; 父母 ("@game" . ?g)
;; 隐私 (:endgroup . nil)))
;; 归档设置(org-archive-location "%s_archive::datetree/")
```

```emacs-lisp
(use-package emacs
  :ensure nil
  :config
  ;;关闭启动画面
  ;;禁止在 Emacs 启动时显示欢迎界面。 一个包含版本号、版权信息和一些提示的消息。
  (setq inhibit-startup-screen t)
  (setq inhibit-startup-message t)
  (add-to-list 'default-frame-alist '(fullscreen . maximized))
  (add-hook 'window-setup-hook 'toggle-frame-fullscreen)
  (add-hook 'window-setup-hook 'toggle-frame-maximized)
  (fset 'yes-or-no-p 'y-or-n-p)

  (setq save-interprogram-paste-before-kill t)
  (show-paren-mode t)
  (global-auto-revert-mode t)
  (global-auto-revert-mode 1)

  (setq auto-revert-verbose nil)

  (setq-default buffer-file-coding-system 'utf-8)
  (setq x-select-enable-clipboard t)
  (setq x-select-enable-primary t)


  ;;(setq-default fill-column 90)
  (global-visual-line-mode)

  ;;font-size
  (setq confirm-kill-processes nil)
  ;;(add-to-list 'default-frame-alist '(font . "Hack"))
  ;;(set-face-attribute 'default nil :font "Hack" :height 160)
  ;;(set-face-attribute 'default nil :font "Hack 16")
  (set-face-attribute 'default nil :height 160)

  ;;(set-face-attribute 'default nil :height 200)

  (global-hl-line-mode)
  (global-eldoc-mode)
  ;; 设置自动补全和括号匹配

  (setq-default
   cursor-type 'box
   load-prefer-newer t
   package-enable-at-startup nil
   use-package-always-defer t
   use-package-always-ensure t)

  ;; buffer
  ;;(setq ido-enable-flex-matching t)
  ;;(setq ido-everywhere t)
  (ido-mode nil)
  (defalias 'list-buffers 'ibuffer) ;;(defalias 'list-buffers 'ibuffer-other-window)

  ;;(org-deadline-warning-days 30)
  (setq bookmark-default-file "~/begin_new_life/org/bookmarks")
  (setq diary-file "~/begin_new_life/org/diary")
  (setq bbdb-file "~/begin_new_life/org/bbdb"))
;;(add-to-list 'org-agenda-files "~/begin_new_life/org/org/gtd.org")
```

```emacs-lisp
;;Linux
;;(setq org-agenda-files '("~/begin_new_life/org/org/" "\\.org$"))
;;windows
;;
;; (eval-after-load 'org (setq org-image-actual-width 512))
;(setq org-refile-targets '(
;                           ("~/begin_new_life/org/org/note.org" :level . 1)
;                           ("~/begin_new_life/org/family/cal.org" :maxlevel . 2)))
;;(org-refile-targets '((org-agenda-files . (:maxlevel . 9))))
(setq org-log-done 'note)
(setq org-log-into-drawer t)
```


### <span class="org-todo todo TODO">TODO</span> 邮件系统通知 {#邮件系统通知}

```emacs-lisp
(use-package emacs
  :ensure nil
  :hook (notmuch-hello-refresh . notmuch-hello-refresh-status-message)
  :config
  (defvar notmuch-hello-refresh-count 0)
  (defun notmuch-hello-refresh-status-message ()
    (let* ((new-count
            (string-to-number
             (car (process-lines notmuch-command "count"))))
           (diff-count (- new-count notmuch-hello-refresh-count)))
      (cond
       ((= notmuch-hello-refresh-count 0)
        (progn
          (message "邮件系统通知You have new messages.")
          (notify-send :title "邮件系统通知Notmuch email"
                       :body (concat "You have " (notmuch-hello-nice-number new-count) " messages.")
                       :timeout 5
                       :urgency 'critical)))

       ((> diff-count 0)
        (progn
          (message "邮件系统通知You have new messages.")
          (notify-send :title "邮件系统通知Notmuch email"
                       :body (concat "You have " (notmuch-hello-nice-number diff-count) " more messages since last refresh.")
                       :timeout 5
                       :urgency 'critical)))

       ((< diff-count 0)
        (progn
          (message "邮件系统通知You have new messages.")
          (notify-send :title "邮件系统通知Notmuch email"
                       :body (concat "You have " (notmuch-hello-nice-number (- diff-count)) " fewer messages since last refresh.")
                       :timeout 5
                       :urgency 'critical))))

      (setq notmuch-hello-refresh-count new-count))))
```


### 编码设置 统一使用 UTF-8 编码。 {#编码设置-统一使用-utf-8-编码}

```emacs-lisp
;; 配置所有的编码为UTF-8，参考：
;; https://thraxys.wordpress.com/2016/01/13/utf-8-in-emacs-everywhere-forever/
(setq locale-coding-system 'utf-8)
(set-terminal-coding-system 'utf-8)
(set-keyboard-coding-system 'utf-8)
(set-selection-coding-system 'utf-8)
(set-default-coding-systems 'utf-8)
(set-language-environment 'utf-8)
(set-clipboard-coding-system 'utf-8)
(set-file-name-coding-system 'utf-8)
(set-buffer-file-coding-system 'utf-8)
(prefer-coding-system 'utf-8)
(modify-coding-system-alist 'process "*" 'utf-8)
(when (display-graphic-p)
  (setq x-select-request-type '(UTF8_STRING COMPOUND_TEXT TEXT STRING)))
```


### <span class="org-todo todo TODO">TODO</span> 其他 UI 零散设置项 {#其他-ui-零散设置项}

tangle ~/emacs-babel.el

```emacs-lisp
;; 禁用一些GUI特性
;; (setq use-dialog-box nil)               ; 鼠标操作不使用对话框
(setq max-lisp-eval-depth 10000)

(setq inhibit-default-init t)           ; 不加载 `default' 库
(setq inhibit-startup-screen t)         ; 不加载启动画面
(setq inhibit-startup-message t)        ; 不加载启动消息
(setq inhibit-startup-buffer-menu t)    ; 不显示缓冲区列表

;; 草稿缓冲区默认文字设置
(setq initial-scratch-message (concat ";; Happy hacking, "
                                      (capitalize user-login-name) " - Emacs ? you!\n\n"))

;; 设置缓冲区的文字方向为从左到右
(setq bidi-paragraph-direction 'left-to-right)
;; 禁止使用双向括号算法
;; (setq bidi-inhibit-bpa t)

;; 设置自动折行宽度为80个字符，默认值为70
(setq-default fill-column 80)

;; 设置大文件阈值为100MB，默认10MB
(setq large-file-warning-threshold 100000000)

;; 以16进制显示字节数
(setq display-raw-bytes-as-hex t)
;; 有输入时禁止 `fontification' 相关的函数钩子，能让滚动更顺滑
(setq redisplay-skip-fontification-on-input t)

;; 禁止响铃
(setq ring-bell-function 'ignore)

;; 禁止闪烁光标
(blink-cursor-mode -1)

;; 在光标处而非鼠标所在位置粘贴
(setq mouse-yank-at-point t)

;; 拷贝粘贴设置
(setq select-enable-primary nil)        ; 选择文字时不拷贝
(setq select-enable-clipboard t)        ; 拷贝时使用剪贴板

;; 鼠标滚动设置
(setq scroll-step 2)
(setq scroll-margin 2)
(setq hscroll-step 2)
(setq hscroll-margin 2)
(setq scroll-conservatively 101)
(setq scroll-up-aggressively 0.01)
(setq scroll-down-aggressively 0.01)
(setq scroll-preserve-screen-position 'always)

;; 对于高的行禁止自动垂直滚动
(setq auto-window-vscroll nil)

;; 设置新分屏打开的位置的阈值
(setq split-width-threshold (assoc-default 'width default-frame-alist))
(setq split-height-threshold nil)

;; TAB键设置，在Emacs里不使用TAB键，所有的TAB默认为4个空格
(setq-default indent-tabs-mode nil)
(setq-default tab-width 4)

;; yes或no提示设置，通过下面这个函数设置当缓冲区名字匹配到预设的字符串时自动回答yes
(setq original-y-or-n-p 'y-or-n-p)
(defalias 'original-y-or-n-p (symbol-function 'y-or-n-p))
(defun default-yes-sometimes (prompt)
  "automatically say y when buffer name match following string"
  (if (or
       (string-match "has a running process" prompt)
       (string-match "does not exist; create" prompt)
       (string-match "modified; kill anyway" prompt)
       (string-match "Delete buffer using" prompt)
       (string-match "Kill buffer of" prompt)
       (string-match "still connected.  Kill it?" prompt)
       (string-match "Shutdown the client's kernel" prompt)
       (string-match "kill them and exit anyway" prompt)
       (string-match "Revert buffer from file" prompt)
       (string-match "Kill Dired buffer of" prompt)
       (string-match "delete buffer using" prompt)
       (string-match "Kill all pass entry" prompt)
       (string-match "for all cursors" prompt)
       (string-match "Do you want edit the entry" prompt))
      t
    (original-y-or-n-p prompt)))
(defalias 'yes-or-no-p 'default-yes-sometimes)
(defalias 'y-or-n-p 'default-yes-sometimes)

;; 设置剪贴板历史长度300，默认为60
(setq kill-ring-max 200)

;; 在剪贴板里不存储重复内容
(setq kill-do-not-save-duplicates t)

;; 设置位置记录长度为6，默认为16
;; 可以使用 `counsel-mark-ring' or `consult-mark' (C-x j) 来访问光标位置记录
;; 使用 C-x C-SPC 执行 `pop-global-mark' 直接跳转到上一个全局位置处
;; 使用 C-u C-SPC 跳转到本地位置处
(setq mark-ring-max 6)
(setq global-mark-ring-max 6)

;; 设置 emacs-lisp 的限制
(setq max-lisp-eval-depth 10000)        ; 默认值为 800
(setq max-specpdl-size 10000)           ; 默认值为 1600

;; 启用 `list-timers', `list-threads' 这两个命令
(put 'list-timers 'disabled nil)
(put 'list-threads 'disabled nil)

;; 在命令行里支持鼠标
(xterm-mouse-mode 1)

;; 退出Emacs时进行确认
(setq confirm-kill-emacs 'y-or-n-p)

;; 在模式栏上显示当前光标的列号
(column-number-mode t)
```


## ORG-CONTRIB {#org-contrib}

Org mode 的附加包，有诸多附加功能

```emacs-lisp
(use-package org-contrib
  :after org
  :ensure t)
```


## ORG-SRC {#org-src}

org-src 代码块基础配置
Org mode 代码块的基本配置。
babel

```emacs-lisp
(use-package org-src
  :after org
  :ensure nil
  :hook (org-babel-after-execute . org-redisplay-inline-images)
  :init
  (setq org-babel-default-header-args
         '((:eval  . "never-export")    ; 导出时不执行代码块
           (:session . "none")
           (:results . "replace")             ; 执行结果替换
           (:exports . "both")                ; 导出代码和结果
           (:cache   . "no")
           (:noweb   . "no")
           (:hlines  . "no")
           (:wrap    . "results")            ; 结果通过#+begin_results包裹
           (:tangle  . "no")))                ; 不写入文件

  :config
  (defun display-ansi-colors ()
    (ansi-color-apply-on-region (point-min) (point-max)))
  (add-hook 'org-babel-after-execute-hook #'display-ansi-colors)
  (setq original-image-width-before-del "960") ; 设置图片的默认宽度为400
  (setq original-caption-before-del "")        ; 设置默认的图示文本为空

  :custom
  ;; 代码块语法高亮
  (org-src-fontify-natively t)
  ;; 使用编程语言的TAB绑定设置
  (org-src-tab-acts-natively t)
  ;; 保留代码块前面的空格 ;; For languages with significant whitespace like Python:
  ;;(setq org-src-preserve-indentation t)
  (org-src-preserve-indentation nil)
  ;; 代码块编辑窗口的打开方式：当前窗口+代码块编辑窗口
  (org-src-window-setup 'reorganize-frame)
  ;; 执行前是否需要确认 (setq org-confirm-babel-evaluate nil)
  (org-confirm-babel-evaluate nil)
  ;; 代码块默认前置多少空格
  (org-edit-src-content-indentation 0)
  ;; 代码块的语言模式设置，设置之后才能正确语法高亮
  (org-src-lang-modes '(("C"            . c)
                        ("C++"          . c++)
                        ("bash"         . sh)
                        ("cpp"          . c++)
                        ("elisp"        . emacs-lisp)
                        ("python"       . python)
                        ("shell"        . sh)
                        ("mysql"        . sql)
                        ;;("dot" . graphviz-dot) ;; was `fundamental-mode'
                        ("asymptote" . asy)
                        ("beamer" . latex)
                        ("calc" . fundamental)
                        ("cpp" . c++)
                        ("ditaa" . artist)
                        ("desktop" . conf-desktop)
                        ("dot" . fundamental)
                        ("elisp" . emacs-lisp)
                        ("ocaml" . tuareg)
                        ("screen" . shell-script)
                        ("shell" . sh)
                        ("sqlite" . sql)
                        ("toml" . conf-toml)))
  )
  ;; 在这个阶段，只需要加载默认支持的语言;;(org-babel-do-load-languages 'org-babel-load-languages)
  ;;(org-babel-load-languages ') (;;(python . t) ;;(awk . t) ;;(C  . t) ;;(calc   . t) ;;(clojure . t) ;;(eshell . t) ;;(shell  . t) ;;(sql  . t) ;;(css  . t) ;;(ditaa . t) ;;(dot . t) ;;(java . t) ;;(js . t) ;;(julia . t) ;;(latex . t) ;;(elixir .t) ;;(lilypond . t) ;;(lisp . t) ;;(octave . t) ;;(org . t) ;;(plantuml . t) ;;(ruby . t) ;;(R . t) (emacs-lisp . t))

;; https://emacs-china.org/t/org-babel/18699/10 org-babel: 按需加载所有的语言
(defun my/org-babel-execute-src-block (&optional _arg info _params)
  "Load language if needed"
  (let* ((lang (nth 0 info))
         (sym (if (member (downcase lang) '("c" "cpp" "c++")) 'C (intern lang)))
         (backup-languages org-babel-load-languages))
    ;; - (LANG . nil) 明确禁止的语言，不加载。
    ;; - (LANG . t) 已加载过的语言，不重复载。
    (unless (assoc sym backup-languages)
      (condition-case err
          (progn
            (org-babel-do-load-languages 'org-babel-load-languages (list (cons sym t)))
            (setq-default org-babel-load-languages (append (list (cons sym t)) backup-languages)))
        (file-missing
         (setq-default org-babel-load-languages backup-languages)
         err)))))

(advice-add 'org-babel-execute-src-block :before #'my/org-babel-execute-src-block)
```

```emacs-lisp
(use-package org-src
  :after org
  :ensure nil
  :hook (org-babel-after-execute . org-redisplay-inline-images)
  :bind (("s-l" . show-line-number-in-src-block)
         :map org-src-mode-map
         ("C-c C-c" . org-edit-src-exit))
  :init
  ;; 设置代码块的默认头参数
  (setq org-babel-default-header-args
        '(
          (:eval    . "never-export")     ; 导出时不执行代码块
          (:session . "none")
          (:results . "replace")          ; 执行结果替换
          (:exports . "both")             ; 导出代码和结果
          (:cache   . "no")
          (:noweb   . "no")
          (:hlines  . "no")
          (:wrap    . "results")          ; 结果通过#+begin_results包裹
          (:tangle  . "no")               ; 不写入文件
          ))
  :config
  ;; ==================================
  ;; 如果出现代码运行结果为乱码，可以参考：
  ;; https://github.com/nnicandro/emacs-jupyter/issues/366
  ;; ==================================
  (defun display-ansi-colors ()
    (ansi-color-apply-on-region (point-min) (point-max)))
  (add-hook 'org-babel-after-execute-hook #'display-ansi-colors)

  ;; ==============================================
  ;; 通过overlay在代码块里显示行号，s-l显示，任意键关闭
  ;; ==============================================
  (defvar number-line-overlays '()
    "List of overlays for line numbers.")

  (defun show-line-number-in-src-block ()
    (interactive)
    (save-excursion
      (let* ((src-block (org-element-context))
             (nlines (- (length
                         (s-split
                          "\n"
                          (org-element-property :value src-block)))
                        1)))
        (goto-char (org-element-property :begin src-block))
        (re-search-forward (regexp-quote (org-element-property :value src-block)))
        (goto-char (match-beginning 0))

        (cl-loop for i from 1 to nlines
                 do
                 (beginning-of-line)
                 (let (ov)
                   (setq ov (make-overlay (point) (point)))
                   (overlay-put ov 'before-string (format "%3s | " (number-to-string i)))
                   (add-to-list 'number-line-overlays ov))
                 (next-line))))

    ;; now read a char to clear them
    (read-key "Press a key to clear numbers.")
    (mapc 'delete-overlay number-line-overlays)
    (setq number-line-overlays '()))

  ;; =================================================
  ;; 执行结果后，如果结果所在的文件夹不存在将自动创建
  ;; =================================================
  (defun check-directory-exists-before-src-execution (orig-fun
                                                      &optional arg
                                                      info
                                                      params)
    (when (and (assq ':file (cadr (cdr (org-babel-get-src-block-info))))
               (member (car (org-babel-get-src-block-info)) '("mermaid" "ditaa" "dot" "lilypond" "plantuml" "gnuplot" "d2")))
      (let ((foldername (file-name-directory (alist-get :file (nth 2 (org-babel-get-src-block-info))))))
        (if (not (file-exists-p foldername))
            (mkdir foldername)))))
  (advice-add 'org-babel-execute-src-block :before #'check-directory-exists-before-src-execution)

  ;; =================================================
  ;; 自动给结果的图片加上相关属性
  ;; =================================================
  (setq original-image-width-before-del "960") ; 设置图片的默认宽度为400
  (setq original-caption-before-del "")        ; 设置默认的图示文本为空

  (defun insert-attr-decls ()
    "insert string before babel execution results"
    (insert (concat "\n#+CAPTION:"
                    original-caption-before-del
                    "\n#+ATTR_ORG: :width "
                    original-image-width-before-del
                    "\n#+ATTR_LATEX: :width "
                    (if (>= (/ (string-to-number original-image-width-before-del) 800.0) 1)
                        "1.0"
                      (number-to-string (/ (string-to-number original-image-width-before-del) 800.0)))
                    "\\linewidth :float nil"
                    "\n#+ATTR_HTML: :width "
                    original-image-width-before-del
                    )))

  (defun insert-attr-decls-at (s)
    "insert string right after specific string"
    (let ((case-fold-search t))
      (if (search-forward s nil t)
          (progn
            ;; (search-backward s nil t)
            (insert-attr-decls)))))

  (defun insert-attr-decls-at-results (orig-fun
                                       &optional arg
                                       info
                                       param)
    "insert extra image attributes after babel execution"
    (interactive)
    (progn
      (when (member (car (org-babel-get-src-block-info)) '("mermaid" "ditaa" "dot" "lilypond" "plantuml" "gnuplot" "d2"))
        (setq original-image-width-before-del (number-to-string (if-let* ((babel-width (alist-get :width (nth 2 (org-babel-get-src-block-info))))) babel-width (string-to-number original-image-width-before-del))))
        (save-excursion
          ;; `#+begin_results' for :wrap results, `#+RESULTS:' for non :wrap results
          (insert-attr-decls-at "#+begin_results")))
      (org-redisplay-inline-images)))
  (advice-add 'org-babel-execute-src-block :after #'insert-attr-decls-at-results)

  ;; 再次执行时需要将旧的图片相关参数行删除，并从中头参数中获得宽度参数，参考
  ;; https://emacs.stackexchange.com/questions/57710/how-to-set-image-size-in-result-of-src-block-in-org-mode
  (defun get-attributes-from-src-block-result (&rest args)
    "get information via last babel execution"
    (let ((location (org-babel-where-is-src-block-result))
          ;; 主要获取的是图示文字和宽度信息，下面这个正则就是为了捕获这两个信息
          (attr-regexp "[:blank:]*#\\+\\(ATTR_ORG: :width \\([0-9]\\{3\\}\\)\\|CAPTION:\\(.*\\)\\)"))
      (setq original-caption-before-del "") ; 重置为空
      (when location
        (save-excursion
          (goto-char location)
          (when (looking-at (concat org-babel-result-regexp ".*$"))
            (next-line 2)               ; 因为有个begin_result的抽屉，所以往下2行
            ;; 通过正则表达式来捕获需要的信息
            (while (looking-at attr-regexp)
              (when (match-string 2)
                (setq original-image-width-before-del (match-string 2)))
              (when (match-string 3)
                (setq original-caption-before-del (match-string 3)))
              (next-line)               ; 因为设置了:wrap，所以这里不需要删除这一行
              )
            )))))
  (advice-add 'org-babel-execute-src-block :before #'get-attributes-from-src-block-result)

  :custom
  ;; 代码块语法高亮
  (org-src-fontify-natively t)
  ;; 使用编程语言的TAB绑定设置
  (org-src-tab-acts-natively t)
  ;; 保留代码块前面的空格
  (org-src-preserve-indentation t)
  ;; 代码块编辑窗口的打开方式：当前窗口+代码块编辑窗口
  (org-src-window-setup 'reorganize-frame)
  ;; 执行前是否需要确认 (setq org-confirm-babel-evaluate nil)
  (org-confirm-babel-evaluate nil)
  ;; 代码块默认前置多少空格
  (org-edit-src-content-indentation 0)
  ;; 代码块的语言模式设置，设置之后才能正确语法高亮
  (org-src-lang-modes '(("C"            . c)
                        ("C++"          . c++)
                        ("bash"         . sh)
                        ("cpp"          . c++)
                        ("elisp"        . emacs-lisp)
                        ("python"       . python)
                        ("shell"        . sh)
                        ("mysql"        . sql)
                        ;;("dot" . graphviz-dot) ;; was `fundamental-mode'
                        ;;("elisp" . emacs-lisp)
                        ;;("ocaml" . tuareg)
                        ))
  ;; 在这个阶段，只需要加载默认支持的语言;;(org-babel-do-load-languages 'org-babel-load-languages)
  (org-babel-load-languages '((python . t)
                              (awk . t)
                              (C  . t)
                              (calc   . t)
                              (emacs-lisp . t)
                              (eshell . t)
                              (shell  . t)
                              (sql  . t)
                              (css  . t)
                              (ditaa . t)
                              (dot . t)
                              (java . t)
                              (js . t)
                              (julia . t)
                              (latex . t)
                              (lilypond . t)
                              (lisp . t)
                              (octave . t)
                              (org . t)
                              (plantuml . t)
                              (ruby . t)
                              (R . t)
                              (clojure . t)
                              ))
  )
```

```emacs-lisp
;; (org-babel-do-load-languages 'org-babel-load-languages '((emacs-lisp . t)))
;;(setq org-babel-load-languages '((python . t)))
;;(dolist (lang '(R ditaa dot emacs-lisp java)) (add-to-list 'org-babel-load-languages `(,lang . t)))
(add-to-list 'org-babel-load-languages '(emacs-lisp . t))
;;(clojure . t)
;;(R . t)
;;(ditaa . t)
;;(dot . t)
;;(emacs-lisp . t)
;;(java . t)
;;(js . t)
;;(julia . t)
;;(latex . t)
;;(lilypond . t)
;;(lua . t)
;;(lisp . t)
;;(ledger . t)
;;(maxima . t)
;;(octave . t)
;;(gnuplot . t)
;;(org . t)
;;(plantuml . t)
;;(python . t)
;;(ruby . t)
;;(shell . t)
;;(sqlite . t)
;;(http . t)
;;(awk . t)
;;(sed . t)
(setq org-babel-no-eval-on-ctrl-c-ctrl-c nil)
(setq org-ditaa-jar-path "~/begin_new_life/ditaa0_9/ditaa.jar")
(setq org-plantuml-jar-path "~/scoop/apps/plantuml/current/plantuml.jar")
(setq plantuml-jar-path org-plantuml-jar-path)
(setq org-babel-R-command "C:/Users/wacl/scoop/apps/R/current/bin/x64/R --slave --no-save")
(add-to-list 'load-path "~/scoop/apps/lilypond/current/share/emacs/site-lisp/")
(autoload 'LilyPond-mode "lilypond-mode")
(setq auto-mode-alist (cons '("\\.ly$" . LilyPond-mode) auto-mode-alist))
(add-hook 'LilyPond-mode-hook (lambda () (turn-on-font-lock)))
(add-to-list 'load-path "~/scoop/apps/maxima/current/share/emacs/site-lisp/")
(autoload 'imath-mode "imath" "Imath mode for math formula input" t)
(autoload 'imath "imath" "Imath mode for math formula input" t)
(autoload 'imaxima "imaxima" "Frontend for maxima with Image support" t)
(autoload 'maxima-mode "maxima" "Maxima mode" t)
(autoload 'maxima "maxima" "Maxima interaction" t)
(setq imaxima-use-maxima-mode-flag t)
(defun ob-js-insert-session-header-arg (session)
  "Insert ob-js `SESSION' header argument.
- `js-comint'
- `skewer-mode'
- `Indium'
"
  (interactive (list (completing-read "ob-js session: "
                                      '("js-comint" "skewer-mode" "indium"))))
  (org-babel-insert-header-arg
   "session"
   (pcase session
     ("js-comint" "\"*Javascript REPL*\"")
     ("skewer-mode" "\"*skewer-repl*\"")
     ("indium" "\"*JS REPL*\""))))
(add-to-list 'org-babel-load-languages '(js . t))
(org-babel-do-load-languages 'org-babel-load-languages org-babel-load-languages)
(add-to-list 'org-babel-tangle-lang-exts '("js" . "js"))


;;(require 'ob-clojure-literate) (require 'ob-php) (require 'ob-redis) (require 'ob-sclang)
```


## ORG-MODERN {#org-modern}

下面，我们通过 [org-modern](https://github.com/minad/org-modern) 插件对 Org mode 进行进一步的美化。

```emacs-lisp
(use-package org-modern
  :after org
  :ensure t
  :hook (after-init . (lambda ()
                        (setq org-modern-hide-stars 'leading)
                        (global-org-modern-mode t)))
  :config
  ;; 标题行型号字符
  (setq org-modern-star ["◉" "○" "✸" "✳" "◈" "◇" "✿" "❀" "✜"])
  ;;(setq org-modern-star [ "𝄞"  "♩"  "♪"  "♫"  "♬"])

  ;; 额外的行间距，0.1表示10%，1表示1px
  (setq-default line-spacing 0.1)
  ;; tag边框宽度，还可以设置为 `auto' 即自动计算
  (setq org-modern-label-border 1)
  ;; 设置表格竖线宽度，默认为3
  (setq org-modern-table-vertical 2)
  ;; 设置表格横线为0，默认为0.1
  (setq org-modern-table-horizontal 0)
  ;; 复选框美化
  (setq org-modern-checkbox
        '((?X . #("▢✓" 0 2 (composition ((2)))))
          (?- . #("▢–" 0 2 (composition ((2)))))
          (?\s . #("▢" 0 1 (composition ((1)))))))
  ;; 列表符号美化
  (setq org-modern-list
        '((?- . "•")
          (?+ . "◦")
          (?* . "▹")))
  ;; 代码块左边加上一条竖边线（需要Org mode顶头，如果启用了 `visual-fill-column-mode' 会很难看）
  (setq org-modern-block-fringe t)
  ;; 代码块类型美化，我们使用了 `prettify-symbols-mode'
  (setq org-modern-block-name nil)
  ;; #+关键字美化，我们使用了 `prettify-symbols-mode'
  (setq org-modern-keyword nil)
  )

```


## ORG-APPEAR {#org-appear}

自动展开强调链接
通过 [org-appear](https://github.com/awth13/org-appear) 插件，当我们的光标移动到 Org mode 里的强调、链接上时，会自动展开，这样方便进行编辑。

```emacs-lisp
(use-package org-appear
  :after org
  :ensure t
  :hook (org-mode . org-appear-mode)
  :config
  (setq org-appear-autolinks t)
  (setq org-appear-autosubmarkers t)
  (setq org-appear-autoentities t)
  (setq org-appear-autokeywords t)
  (setq org-appear-inside-latex t))

;;(setq org-appear-trigger 'manual)
(add-hook 'org-mode-hook (lambda ()
                           (add-hook 'evil-insert-state-entry-hook
                                     #'org-appear-manual-start
                                     nil
                                     t)
                           (add-hook 'evil-insert-state-exit-hook
                                     #'org-appear-manual-stop
                                     nil
                                     t)))
```


## ORG-AUTO-TANGLE {#org-auto-tangle}

自动 tangle 设置
[org-auto-tangle](https://github.com/yilkalargaw/org-auto-tangle) 插件可以在 Org mode 下自动进行 tangle。

```emacs-lisp
(use-package org-auto-tangle
  :after org
  :ensure t
  :hook (org-mode . org-auto-tangle-mode)
  :config
  (setq org-auto-tangle-default nil))
```


## ORG-AGENDA {#org-agenda}

```emacs-lisp
(use-package org-agenda
  :ensure nil
  :hook (org-agenda-finalize . org-agenda-to-appt)
  :bind (("\e\e a" . org-agenda)
         :map org-agenda-mode-map
         ("i" . (lambda () (interactive) (org-capture nil "d")))
         ("J" . consult-org-agenda))
  :config
  ;; 日程模式的日期格式设置
  (setq org-agenda-format-date 'org-agenda-format-date-aligned)
  (defun org-agenda-format-date-aligned (date)
    "Format a DATE string for display in the daily/weekly agenda, or timeline.

This function makes sure that dates are aligned for easy reading."
    (require 'cal-iso)
    (let* ((dayname (aref cal-china-x-days
                          (calendar-day-of-week date)))
           (day (cadr date))
           (month (car date))
           (year (nth 2 date))
           (day-of-week (calendar-day-of-week date))
           (iso-week (org-days-to-iso-week
                      (calendar-absolute-from-gregorian date)))
           (cn-date (calendar-chinese-from-absolute (calendar-absolute-from-gregorian date)))
           (cn-month (cl-caddr cn-date))
           (cn-day (cl-cadddr cn-date))
           (cn-month-string (concat (aref cal-china-x-month-name
                                          (1- (floor cn-month)))
                                    (if (integerp cn-month)
                                        ""
                                      "（闰月）")))
           (cn-day-string (aref cal-china-x-day-name
                                (1- cn-day)))
           (extra (format " 农历%s%s%s%s"
                          (if (or (eq org-agenda-current-span 'day)
                                  (= day-of-week 1)
                                  (= cn-day 1))
                              cn-month-string
                            "")
                          (if (or (= day-of-week 1)
                                  (= cn-day 1))
                              (if (integerp cn-month) "" "[闰]")
                            "")
                          cn-day-string
                          (if (or (= day-of-week 1)
                                  (eq org-agenda-current-span 'day))
                              (format " 今年第%02d周" iso-week)
                            ""))))


      (format "%04d-%02d-%02d 星期%s%s%s\n" year month
              day dayname extra (concat " 第" (format-time-string "%j") "天"))))

  ;; 显示时间线
  (setq org-agenda-use-time-grid t)
  ;; 设置面包屑分隔符
  ;; (setq org-agenda-breadcrumbs-separator " ❱ ")
  ;; 设置时间线的当前时间指示串
  (setq org-agenda-current-time-string "⏰------------now")
  ;; 时间线范围和颗粒度设置
  (setq org-agenda-time-grid (quote ((daily today)
                                     (0600 0800 1000 1200
                                           1400 1600 1800
                                           2000 2200 2400)
                                     "......" "----------------")))
  ;; 日程视图的前缀设置
  (setq org-agenda-prefix-format '((agenda . " %i %-25:c %5t %s")
                                   (todo   . " %i %-25:c ")
                                   (tags   . " %i %-25:c ")
                                   (search . " %i %-25:c ")))
  ;; 对于计划中的任务在视图里的显示
  (setq org-agenda-scheduled-leaders
        '("计划 " "应在%02d天前开始 "))
  ;; 对于截止日期的任务在视图里的显示
  (setq org-agenda-deadline-leaders
        '("截止 " "还有%02d天到期 " "已经过期%02d天 "))

  ;; =====================
  ;; 自定义日程视图，分别显示TODO，WIP，WIAT中的任务
  ;; n键显示自定义视图，p键纯文本视图，a键默认视图
  ;; =====================
  (defvar my-org-custom-daily-agenda
    `((todo "TODO"
       ((org-agenda-block-separator nil)
        (org-agenda-overriding-header "所有待办任务\n")))
      (todo "HOLD"
            ((org-agenda-block-separator nil)
             (org-agenda-overriding-header "\n重复性的任务\n")))
      (todo "WAIT"
            ((org-agenda-block-separator nil)
             (org-agenda-overriding-header "\n等待中的任务\n")))
      (todo "保期内"
            ((org-agenda-block-separator nil)
             (org-agenda-overriding-header "\n保质期内的东西\n")))
      (todo "TODO"
            ((org-agenda-overriding-header "\n无效待办事项时间")
             (org-agenda-prefix-format "  %-12:c")
             (org-agenda-sorting-strategy '(time-up todo tag-up priority-down))
             (org-agenda-todo-ignore-deadlines 'past)
             (org-agenda-todo-ignore-scheduled 'past)
             (org-agenda-todo-ignore-timestamp 'past)
             (org-agenda-todo-ignore-with-date nil)
             (org-agenda-todo-ignore-todo 'done)))

      (agenda "" ((org-agenda-block-separator nil)
                  (org-agenda-overriding-header "\n今日日程\n"))))

    "Custom agenda for use in `org-agenda-custom-commands'.")
  (setq org-agenda-custom-commands
        `(("n" "Daily agenda and top priority tasks"
           ,my-org-custom-daily-agenda)
          ("p" "Plain text daily agenda and top priorities"
           ,my-org-custom-daily-agenda
           ((org-agenda-with-colors nil)
            (org-agenda-prefix-format "%t %s")
            (org-agenda-current-time-string ,(car (last org-agenda-time-grid)))
            (org-agenda-fontify-priorities nil)
            (org-agenda-remove-tags t))
           ("agenda.txt"))))

  ;; 时间戳格式设置，会影响到 `svg-tag' 等基于正则的设置
  ;; 这里设置完后是 <2022-12-24 星期六> 或 <2022-12-24 星期六 06:53>
  (setq system-time-locale "zh_CN.UTF-8")
  (setq org-time-stamp-formats '("<%Y-%m-%d %A>" . "<%Y-%m-%d %A %H:%M>"))
  ;; 不同日程类别间的间隔
  (setq org-cycle-separator-lines 2)
  :custom
  ;; 设置需要被日程监控的org文件
  (org-agenda-files
   (list ;;(expand-file-name "tasks.org" org-directory)
    ;;(expand-file-name "diary.org" org-directory)
    (expand-file-name "inbox.org" org-directory)))
  ;; org agenda
  (org-agenda-files (directory-files-recursively "~/begin_new_life/org/org/" ".*\\.org$"))
  (org-agenda-files (append org-agenda-files (directory-files-recursively "~/begin_new_life/org/family/" ".*\\.org$")))
  ;;(expand-file-name "habits.org" org-directory)
  ;;(expand-file-name "mail.org" org-directory)
  ;;(expand-file-name "emacs-config.org" user-emacs-directory)

  ;; 设置org的日记文件
  (org-agenda-diary-file (expand-file-name "diary.org" org-directory))
  ;; 日记插入精确时间戳
  (org-agenda-insert-diary-extract-time t)
  ;; 设置日程视图更加紧凑
  ;; (org-agenda-compact-blocks t)
  ;; 日程视图的块分隔符
  (org-agenda-block-separator ?─)
  ;; 日视图还是周视图，通过 v-d, v-w, v-m, v-y 切换视图，默认周视图
  (org-agenda-span 'week)
  ;; q退出时删除agenda缓冲区
  (org-agenda-sticky t)
  ;; 是否包含直接日期
  (org-agenda-include-deadlines t)
  ;; 禁止日程启动画面
  (org-agenda-inhibit-startup t)
  ;; 显示每一天，不管有没有条目
  (org-agenda-show-all-dates t)
  ;; 时间不足位时前面加0
  (org-agenda-time-leading-zero t)
  ;; 日程同时启动log mode
  (org-agenda-start-with-log-mode t)
  ;; 日程同时启动任务时间记录报告模式
  (org-agenda-start-with-clockreport-mode t)
  ;; 截止的任务完成后不显示
  (org-agenda-skip-deadline-if-done t)
  ;; 当计划的任务完成后不显示
  (org-agenda-skip-scheduled-if-done t)
  ;; 计划过期上限
  (org-scheduled-past-days 365)
  ;; 计划截止上限
  (org-deadline-past-days 365)
  ;; 计划中的任务不提醒截止时间
  (org-agenda-skip-deadline-prewarning-if-scheduled 1)
  (org-agenda-skip-scheduled-if-deadline-is-shown t)
  (org-agenda-skip-timestamp-if-deadline-is-shown t)
  ;; 设置工时记录报告格式
  (org-agenda-clockreport-parameter-plist
   '(:link t :maxlevel 5 :fileskip0 t :compact nil :narrow 80))
  (org-agenda-columns-add-appointments-to-effort-sum t)
  (org-agenda-restore-windows-after-quit t)
  (org-agenda-window-setup 'current-window)
  ;; 标签显示的位置，第100列往前右对齐
  (org-agenda-tags-column -100)
  ;; 从星期一开始作为一周第一天
  (org-agenda-start-on-weekday 1)
  ;; 是否使用am/pm
  ;; (org-agenda-timegrid-use-ampm nil)
  ;; 搜索是不看时间
  (org-agenda-search-headline-for-time t)
  ;; 提前3天截止日期到期告警
  (org-deadline-warning-days 30))
```

加上了根据主题背景是黑还是白来自动选择颜色配色

```emacs-lisp
;;(setq abbrev-file-name "~/begin_new_life/org/abbrev_defs")
;;(setq save-abbrevs "~/begin_new_life/org/abbrev_defs")
;; 我加上了根据主题背景是黑还是白来自动选择颜色配色。
;; agenda 里面时间块彩色显示
;; From: https://emacs-china.org/t/org-agenda/8679/3
(defun my:org-agenda-time-grid-spacing ()
  "Set different line spacing w.r.t. time duration."
  (save-excursion
    (let* ((background (alist-get 'background-mode (frame-parameters)))
           (background-dark-p (string= background "dark"))
           (colors (if background-dark-p
                       (list "#aa557f" "DarkGreen" "DarkSlateGray" "DarkSlateBlue")
                     (list "#F6B1C3" "#FFFF9D" "#BEEB9F" "#ADD5F7")))
           pos
           duration)
      (nconc colors colors)
      (goto-char (point-min))
      (while (setq pos (next-single-property-change (point) 'duration))
        (goto-char pos)
        (when (and (not (equal pos (point-at-eol)))
                   (setq duration (org-get-at-bol 'duration)))
          (let ((line-height (if (< duration 30) 1.0 (+ 0.5 (/ duration 60))))
                (ov (make-overlay (point-at-bol) (1+ (point-at-eol)))))
            (overlay-put ov 'face `(:background ,(car colors)
                                                :foreground
                                                ,(if background-dark-p "black" "white")))
            (setq colors (cdr colors))
            (overlay-put ov 'line-height line-height)
            (overlay-put ov 'line-spacing (1- line-height))))))))

(add-hook 'org-agenda-finalize-hook #'my:org-agenda-time-grid-spacing)
```


### <span class="org-todo todo TODO">TODO</span> custom view {#custom-view}

```elisp
(setq org-agenda-custom-commands
      '(("c" "重要且紧急的事"
     ((tags-todo "+PRIORITY=\"A\"")))
    ;; ...other commands here
    ))
(setq org-agenda-custom-commands '(
   ("p" "Past deadlines and scheduled items" (
        (agenda "" (
                (org-agenda-overriding-header "scheduled和deadline时间无效")
                (org-agenda-show-log t)
                (org-agenda-log-mode-items '(state))
                (org-agenda-prefix-format '((agenda . "  %-12:c%?-12t% s")))
                (org-agenda-skip-deadline-if-done nil)
                (org-agenda-skip-scheduled-if-done nil)
                (org-agenda-skip-deadline-if-not-today nil)
                (org-agenda-skip-scheduled-if-not-today nil)
                ;;(org-agenda-deadline-faces org-agenda-deadline-faces)
                ;;(org-agenda-scheduled-faces org-agenda-scheduled-faces)
))))
   ("r" "日的议程，但同时显示待办事项" (
         (agenda "" (
                (org-agenda-start-day "+0d")
                (org-agenda-prefix-format "  %-12:c")
                (org-agenda-overriding-header "Today's tasks:")
                (org-agenda-todo-ignore-deadlines 'all)
                (org-agenda-todo-ignore-scheduled 'all)
                (org-agenda-todo-ignore-timestamp 'all)
                (org-agenda-todo-ignore-with-date nil)
                (org-agenda-sorting-strategy '(time-up todo tag-up priority-down))))
         (todo "TODO" (
                (org-agenda-overriding-header "\n\n进行中的待办事项:")
                (org-agenda-prefix-format "  %-12:c")
                (org-agenda-sorting-strategy '(time-up priority-down))
                (org-agenda-todo-ignore-with-date nil)
                (org-agenda-sorting-strategy '(time-up todo tag-up priority-down))))))))
```


## ORG-HABIT 习惯管理 :tangle ~/emacs-babel.el {#org-habit-习惯管理-tangle-emacs-babel-dot-el}

```emacs-lisp
(use-package org-habit
  :ensure nil
  :defer t
  :custom
  (org-habit-show-habits t)
  (org-habit-graph-column 70)
  (org-habit-show-all-today t)
  (org-habit-show-done-always-green t)
  (org-habit-scheduled-past-days t)
  ;; org habit show 7 days before today and 7 days after today. ! means not done. * means done.
  (org-habit-preceding-days 7))
```


## <span class="org-todo todo TODO">TODO</span> ORG-ROAM {#org-roam}

<https://github.com/org-roam/org-roam> Org-roam is a plain-text knowledge management system. It brings some of Roam's more powerful features into the Org-mode ecosystem. Org-roam borrows principles from the Zettelkasten method, providing a solution for non-hierarchical note-taking. It should also work as a plug-and-play solution for anyone already using Org-mode for their personal wiki. Private and Secure: Edit your personal wiki completely offline, entirely in your control. Encrypt your notes with GPG. Take lasting notes in plain-text. Networked Thought: Connect notes and thoughts together with ease using backlinks. Discover surprising and previously unseen connections in your notes with the built-in graph visualization. Extensible and Powerful: Leverage Emacs' fantastic text-editing interface, and the mature Emacs and Org-mode ecosystem of packages. Free and Open Source: Org-roam is licensed under the GNU General Public License version 3 or later.
<https://github.com/org-roam/org-roam> Org-roam 是一个纯文本知识管理系统。它将 Roam 的一些更强大的功能带入 Org 模式生态系统。 Org-roam 借鉴了 Zettelkasten 方法的原理，为非分层笔记提供了解决方案。对于已经在其个人 wiki 中使用 Org 模式的任何人来说，它也应该作为即插即用的解决方案。私密且安全：完全离线编辑您的个人维基，完全由您掌控。使用 GPG 加密您的笔记。用纯文本记录持久的笔记。网络思想：使用反向链接轻松将笔记和想法连接在一起。通过内置的图形可视化，发现笔记中令人惊讶的和以前未见过的联系。可扩展且功能强大：利用 Emacs 出色的文本编辑界面以及成熟的 Emacs 和 Org-mode 软件包生态系统。免费和开源：Org-roam 根据 GNU 通用公共许可证版本 3 或更高版本获得许可。

```emacs-lisp
(use-package org-roam
  :ensure t
  :init (setq org-roam-v2-ack t)
  :custom
  (org-roam-directory (file-truename "~/begin_new_life/org/roamnotes/"))
  ;;(org-roam-directory "~/begin_new_life/org/roamnotes/")
  ;;
  (org-roam-completion-everywhere t)
  :bind (
         ("C-c n l" . org-roam-buffer-toggle)
         ("C-c n f" . org-roam-node-find)
         ("C-c n g" . org-roam-graph)
         ("C-c n i" . org-roam-node-insert)
         ("C-c n c" . org-roam-capture)
         ;; Dailies
         ("C-c n j" . org-roam-dailies-capture-today)
         ;;:map org-mode-map
         ("C-M-i" . completion-at-point))
  :config
  ;; If you're using a vertical completion framework, you might want a more informative completion interface
  (setq org-roam-node-display-template (concat "${title:*} " (propertize "${tags:10}" 'face 'org-tag))) (org-roam-db-autosync-mode)
  ;;(org-roam-setup)
  ;; If using org-roam-protocol
  (require 'org-roam-protocol))
```


## <span class="org-todo todo TODO">TODO</span> ORG-AI 智能助手 {#org-ai-智能助手}

[org-ai](https://github.com/rksm/org-ai) 插件可以访问 ChatGPT 并成为 Emacs 的智能助手。

```emacs-lisp
(use-package org-ai
  :ensure t
  :bind (
         ("C-c q" . org-ai-prompt)
         ("C-c x" . org-ai-on-region))

  :hook (org-mode . org-ai-mode)
  :config
  ;; (setq org-ai-openai-api-token "Your Key") ; 以明文的方式存储key，或者放入到 ~/.authinfo.gpg 文件里
  (setq org-ai-default-max-tokens 4096)
 ;;(setq org-ai-openai-api-token "pk-xpgeDQoUreIZTkgJcMxGinBMKqoccDgRaHeHYpBUyJHuPrtG")
 ;;(setq org-ai-openai-chat-endpoint "https://api.pawan.krd/v1/chat/completions")
 (setq org-ai-openai-chat-endpoint "https://api.chatanywhere.com.cn")
 (setq org-ai-default-chat-system-prompt "你是一个Emacs助手，请以Org-mode的格式来回复我"))
```

(setq org-ai-sd-endpoint-base "<http://localhost:7861/sdapi/v1/>")
org-ai-openai-chat-endpoint is a variable defined in org-ai-openai.el.

Value
"<https://api.openai.com/v1/chat/completions>"
org-ai-openai-completion-endpoint is a variable defined in
org-ai-openai.el.

Value
"<https://api.openai.com/v1/completions>"


## <span class="org-todo todo TODO">TODO</span> ORG-SUPER-LINKS 反链设置 {#org-super-links-反链设置}

[org-super-links](https://github.com/toshism/org-super-links) 插件可以设置反向链接。

```emacs-lisp
(use-package org-super-links
  :quelpa (org-super-links :fetcher github :repo "toshism/org-super-links")
  :bind (("C-c s s"   . org-super-links-link)
         ("C-c s l"   . org-super-links-store-link)
         ("C-c s C-l" . org-super-links-insert-link)
         ("C-c s d"   . org-super-links-quick-insert-drawer-link)
         ("C-c s i"   . org-super-links-quick-insert-inline-link)
         ("C-c s C-d" . org-super-links-delete-link))
  :config
  (setq org-super-links-related-into-drawer t)
  (setq org-super-links-link-prefix 'org-super-links-link-prefix-timestamp))
```


## <span class="org-todo todo TODO">TODO</span> ORG-CAPTURE {#org-capture}

快速记录设置
(org-capture-templates \`(
))

```emacs-lisp
(use-package org-capture
  :ensure nil
  ;; :bind ("\e\e c" . (lambda () (interactive) (org-capture)))
  :hook ((org-capture-mode . (lambda ()
                               (setq-local org-complete-tags-always-offer-all-agenda-tags t)))
         (org-capture-mode . delete-other-windows))
  :custom
  ;; automatically created 书签 org-capture-last-stored
  (org-capture-bookmark t)
  (org-capture-use-agenda-date nil)
  ;; define common template
  (org-capture-templates '(
                           ("t" "TODO要做的[gtd.org]" entry (file+headline "~/begin_new_life/org/org/gtd.org" "ToGetDone")
                            "* TODO %i%?"
                            :prepend t)
                           ;;("N" "Notes" entry (file+headline "capture.org" "Notes")
                           ;; "* %? %^g\n%i\n"
                           ;; :empty-lines-after 1)
                           ("n" "Note" entry (file+headline "~/begin_new_life/org/org/notes.org" "Notes")
                            "* %? \n%U\n  %i\n %a")
                           ;; For EWW
                           ("b" "计算学校和家里生活用品的过期时限")
                           ("bj" "家里" entry (file+headline "~/begin_new_life/org/family/cal.org" "屋里的过期时限")
                            "* 保期内 %:description\n%a%?")
                           ("bx" "学校" entry (file+headline "~/begin_new_life/org/family/cal.org" "TODO_before_deadline")
                            "* 保期内 ")
;;:immediate-finish t %:description\n%a%? :empty-lines-after 1

                           ("d" "Diary")
                           ("dt" "Today's TODO list" entry (file+olp+datetree "diary.org")
                            "* Today's TODO list [/]\n%T\n\n** TODO %?"
                            :empty-lines 1
                            :jump-to-captured t)
                           ("do" "Other stuff" entry (file+olp+datetree "diary.org")
                            "* %?\n%T\n\n%i"
                            :empty-lines 1
                            :jump-to-captured t)
                           ("j" "Journal" entry (file+datetree "~/begin_new_life/org/org/journal.org")
                            "* %? \n%U\n %i\n %a")
                           ("c" "Code-inbox" entry (file+headline "~/begin_new_life/org/org/inbox.org" "ToSort")
                            "* %?\n#+BEGIN_SRC %^{language}\n\n#+END_SRC\nentered on %u\n %i\n %a")
                           ("g" "Game")
                           ("gs" "游戏存档" entry (file+headline "~/begin_new_life/org/org/galsave.org" "SaveData")
                            "** TODO %^{游戏名称}\n游戏结束DEADLINE: %^t")
                           ("gg" "GalGame" entry (file+headline "~/begin_new_life/org/org/galsave.org" "愿望清单")
                            "** TODO %^{游戏名称|%(org-ql--query-all-headlines)}\n游戏结束DEADLINE: %^t\n:PROPERTIES:\n:存档路径:%x\n:END:\n:LOGBOOK:\nCLOCK: [%U]\n:END:"))))
```

<div class="results">

((lambda nil
   (setq-local org-complete-tags-always-offer-all-agenda-tags t))
 delete-other-windows
 (closure
     (t)
     nil
   (set
    (make-local-variable 'org-complete-tags-always-offer-all-agenda-tags)
    t))
 +org-show-target-in-capture-header-h evil-insert-state)

</div>

("b" "beorgno" entry
 "\* %? %^L \n%U\n %i\n %a")


### <span class="org-todo todo TODO">TODO</span> SETTING CAPTURE-TEMPLATES {#setting-capture-templates}

;;%x：插入当前的剪贴板内容。
task with a TODO headline and a timestamp. The task is added to the "Tasks" headline in the ~/org/inbox.org file.
note with a custom heading and a timestamp. The note is added to the "Notes" headline
link with a custom heading and a timestamp. The link is added to the "Links" headline
journal entry with a custom heading and a timestamp. The entry is added to a datetree
code snippet with a custom heading, language, and timestamp. The code snippet is added to the "Code Snippets" headline
;;"\* TODO [#B] %?\n  %i\n %U %T"
;;"\* TODO %i%?")
;;"\* %i%? \n %U"          "\* TODO %?\n %i\n %U %T %a" :empty-lines 1
;;(file+headline "~/begin_new_life/org/org/inbox.org" "Code-Snippets")


## TOC-ORG 目录自动生成 {#toc-org-目录自动生成}

[toc-org](https://github.com/snosov1/toc-org) 插件可以在 Org 文件里自动生成目录，只需给一个标题行设置一个标签为 `toc` 或 `toc_2` 即可（后者只生成 2 层）。

```emacs-lisp
(use-package toc-org
  :after org
  :ensure t
  :hook (org-mode . toc-org-mode))
```


## <span class="org-todo todo TODO">TODO</span> bIT DENOTE 笔记设置 {#bit-denote-笔记设置}

[denote](https://protesilaos.com/emacs/denote) 是一个轻量级的笔记插件，拥有良好的文件名命名模板。

```emacs-lisp
(use-package denote
  :ensure t
  :hook (dired-mode . denote-dired-mode-in-directories)
  :bind (("C-c d n" . denote)
         ("C-c d d" . denote-date)
         ("C-c d t" . denote-type)
         ("C-c d s" . denote-subdirectory)
         ("C-c d f" . denote-open-or-create)
         ("C-c d r" . denote-dired-rename-file))
  :init
  (with-eval-after-load 'org-capture
    (setq denote-org-capture-specifiers "%l\n%i\n%?")
    (add-to-list 'org-capture-templates
                 '("N" "New note (with denote.el)" plain
                   (file denote-last-path)
                   #'denote-org-capture
                   :no-save t
                   :immediate-finish nil
                   :kill-buffer t
                   :jump-to-captured t)))
  :config
  (setq denote-directory (expand-file-name "~/org/"))
  (setq denote-known-keywords '("emacs" "entertainment" "reading" "studying"))
  (setq denote-infer-keywords t)
  (setq denote-sort-keywords t)
  ;; org is default, set others such as text, markdown-yaml, markdown-toml
  (setq denote-file-type nil)
  (setq denote-prompts '(title keywords))

  ;; We allow multi-word keywords by default.  The author's personal
  ;; preference is for single-word keywords for a more rigid workflow.
  (setq denote-allow-multi-word-keywords t)
  (setq denote-date-format nil)

  ;; If you use Markdown or plain text files (Org renders links as buttons
  ;; right away)
  (add-hook 'find-file-hook #'denote-link-buttonize-buffer)
  (setq denote-dired-rename-expert nil)

  ;; OR if only want it in `denote-dired-directories':
  (add-hook 'dired-mode-hook #'denote-dired-mode-in-directories))
```


## CONSULT-NOTES 查找笔记 {#consult-notes-查找笔记}

[consult-notes](https://github.com/mclear-tools/consult-notes) 插件可以通过 consult 快速找到笔记。

```emacs-lisp
(use-package consult-notes
  :ensure t
  :commands (consult-notes
             consult-notes-search-in-all-notes)
  :bind (("C-c n f" . consult-notes)
         ("C-c n c" . consult-notes-search-in-all-notes))
  :config
  (setq consult-notes-file-dir-sources
        `(
          ("work"    ?w ,(concat org-directory "/midea/"))
          ("article" ?a ,(concat org-directory "/article/"))
          ("org"     ?o ,(concat org-directory "/"))
          ("hugo"    ?h ,(concat org-directory "/hugo/"))
          ("books"   ?b ,(concat (getenv "HOME") "/Books/"))
          ))

  ;; embark support
  (with-eval-after-load 'embark
    (defun consult-notes-open-dired (cand)
      "Open notes directory dired with point on file CAND."
      (interactive "fNote: ")
      ;; dired-jump is in dired-x.el but is moved to dired in Emacs 28
      (dired-jump nil cand))

    (defun consult-notes-marked (cand)
      "Open a notes file CAND in Marked 2.
Marked 2 is a mac app that renders markdown."
      (interactive "fNote: ")
      (call-process-shell-command (format "open -a \"Marked 2\" \"%s\"" (expand-file-name cand))))

    (defun consult-notes-grep (cand)
      "Run grep in directory of notes file CAND."
      (interactive "fNote: ")
      (consult-grep (file-name-directory cand)))

    (embark-define-keymap consult-notes-map
                          "Keymap for Embark notes actions."
                          :parent embark-file-map
                          ("d" consult-notes-dired)
                          ("g" consult-notes-grep)
                          ("m" consult-notes-marked))

    (add-to-list 'embark-keymap-alist `(,consult-notes-category . consult-notes-map))

    ;; make embark-export use dired for notes
    (setf (alist-get consult-notes-category embark-exporters-alist) #'embark-export-dired)
    )
  )
```


## OL 新增链接类型 {#ol-新增链接类型}

[google Org mode](https://google.com/search?q=Org%20mode)

```emacs-lisp
(use-package ol
  :ensure nil
  :defer t
  :custom
  (org-link-keep-stored-after-insertion t)
  (org-link-abbrev-alist '(("github"        . "https://github.com/search?q=")
                           ("google" . "https://google.com/search?q=")
                           ("baidu"         . "https://baidu.com/s?wd=")
                           ("rfc"           . "https://tools.ietf.org/html/")
                           ("wiki"          . "https://en.wikipedia.org/wiki/")
                           ("youtube" . "https://www.youtube.com/results?search_query=%s")
                           ("bilibli" . "https://search.bilibili.com/all?keyword=%s")
                           ("zhihu"         . "https://www.zhihu.com/search?type=content&q=")
                           ("doomdir" . "c:/Users/wacl/.doom.d/%s")
                           ("doomemacs" . "c:/Users/wacl/.emacs.d.doomemacs/%s")
                           ("spacemacs" . "c:/Users/wacl/.emacs.d.spacemacs/%s")
                           ("doom-repo" . "https://github.com/doomemacs/doomemacs/%s")
                           ("wolfram" . "https://wolframalpha.com/input/?i=%s")
                           ("wikipedia" . "https://en.wikipedia.org/wiki/%s")
                           ("duckduckgo" . "https://duckduckgo.com/?q=%s")
                           ("gmap" . "https://maps.google.com/maps?q=%s")
                           ("gimages" . "https://google.com/images?q=%s")
                           ("vndb" .  "https://vndb.org/v?sq=")
                           ("perl" . "https://perldoc.perl.org/search.html?q=%s")
                           ;; search
                           ("bing" . "https://www.bing.com/search?q=%s")
                           ("wiki" . "https://www.wikipedia.org/w/index.php?search=%s")
                           ("ryu" . "https://www.ryuugames.com/?s=%s") ;; ryuugames
                           ("babel" . "https://orgmode.org/worg/org-contrib/babel/languages/ob-doc-%s.html"))))
```

("github" . "<https://github.com/%s>")
("youtube"       . "<https://youtube.com/watch?v>=")
<c:/Users/wacl/.doom.d/packages.el>
<https://www.acgndog.com/category/game/lvg>
<https://acgngames.net/index.php/search/?phrase=mme>#
<https:/www.sf-express.com/we/ow/chn/sc/waybill/>
;; Wikipedia

```emacs-lisp
("bing" . "https://www.bing.com/search?q=")
("brave" . "https://search.brave.com/search?q=")
("duckduckgo" . "https://duckduckgo.com/?q=")
("github" . "https://github.com/search?q=")
("google" . "https://google.com/search?q=")
("mojeek" . "https://www.mojeek.com/search?q=")
("yahoo" . "https://search.yahoo.com/search?p=")
("yandex" . "https://yandex.com/search/?text=")
("duckduckgo" . "https://duckduckgo.com/ac/?q=")
("google" . "https://www.google.com/complete/search?client=chrome&q=")
("yahoo" . "https://search.yahoo.com/sugg/gossip/gossip-us-ura/?output=sd1&command=")
("ecosia" . "https://www.ecosia.org/search?q=")
("metager" . "https://metager.org/meta/meta.ger3?eingabe=")
("swisscows" . "https://swisscows.com/web?query=")
("mojeek" . "https://www.mojeek.com/search?q=")
("peekier" . "https://peekier.com/#!")
("wikipedia" . "https://en.wikipedia.org/w/?search=")
("stackoverflow" . "https://stackoverflow.com/search?q=")
("wolframalpha" . "https://www.wolframalpha.com/input/?i=")
("reddit" . "https://www.reddit.com/search/?q=")
("youtube" . "https://youtube.com/results?q=")
("github" . "https://github.com/search?q=")
("bbc" . "https://www.bbc.co.uk/search?q=")
("duckduckgo" . "https://duckduckgo.com/?q=")
("google" . "https://google.com/search?q=")
("whoogle" . "https://whoogle.sdf.org/search?q=")
("qwant" . "https://www.qwant.com/?q=")
("startpage" . "https://www.startpage.com/do/search?query=")
(//Niche)
("searx-bar" . "https://searx.bar/search?q=")
("searx-info" . "https://searx.info/search?q=")
("searx-tiekoetter" . "https://searx.tiekoetter.com/search?q=")
("searx-bissisoft" . "https://searx.bissisoft.com/search?q=")

    searchUrl: UseZhLang_ ? "https://www.baidu.com/s?ie=utf-8&wd=%s \u767e\u5ea6"
      : "https://www.google.com/search?q=%s Google",
? `b|ba|baidu|Baidu|: https://www.baidu.com/s?ie=utf-8&wd=%s
  blank=https://www.baidu.com/ \u767e\u5ea6
bi|bing|Bing|\u5fc5\u5e94: https://cn.bing.com/search?q=%s
  blank=https://cn.bing.com/ \u5fc5\u5e94
g|go|gg|google|Google|\u8c37\u6b4c: https://www.google.com/search?q=%s
  www.google.com re=/^(?:.[a-z]{2,4})?/searchb.*?[#&?]q=([^#&]*)/i
  blank=https://www.google.com/ Google
br|brave: https://search.brave.com/search?q=%s Brave
d|dd|ddg|duckduckgo: https://duckduckgo.com/?q=%s DuckDuckGo
ec|ecosia: https://www.ecosia.org/search?q=%s Ecosia
qw|qwant: https://www.qwant.com/?q=%s Qwant
ya|yd|yandex: https://yandex.com/search/?text=%s Yandex
yh|yahoo: https://search.yahoo.com/search?p=%s Yahoo
maru|mailru|mail.ru: https://go.mail.ru/search?q=%s Mail.ru

b.m|bm|map|b.map|bmap|\u5730\u56fe|\u767e\u5ea6\u5730\u56fe:
  https://api.map.baidu.com/geocoder?output=html&address=%s&src=vimium-c
  blank=https://map.baidu.com/
gd|gaode|\u9ad8\u5fb7\u5730\u56fe: https://www.gaode.com/search?query=%s
  blank=https://www.gaode.com
g.m|gm|g.map|gmap: https://www.google.com/maps?q=%s
  blank=https://www.google.com/maps \u8c37\u6b4c\u5730\u56fe
y|yt: https://www.youtube.com/results?search_query=%s

b.x|b.xs|bx|bxs|bxueshu: https://xueshu.baidu.com/s?ie=utf-8&wd=%s
  blank=https://xueshu.baidu.com/ \u767e\u5ea6\u5b66\u672f
gs|g.s|gscholar|g.x|gx|gxs: https://scholar.google.com/scholar?q=$s
  scholar.google.com re=/^(?:.[a-z]{2,4})?/scholarb.*?[#&?]q=([^#&]*)/i
  blank=https://scholar.google.com/ \u8c37\u6b4c\u5b66\u672f

t|tb|taobao|ali|\u6dd8\u5b9d: https://s.taobao.com/search?ie=utf8&q=%s
  blank=https://www.taobao.com/ \u6dd8\u5b9d
j|jd|jingdong|\u4eac\u4e1c: https://search.jd.com/Search?enc=utf-8&keyword=%s
  blank=https://jd.com/ \u4eac\u4e1c
az|amazon: https://www.amazon.com/s?k=%s
  blank=https://www.amazon.com/ \u4e9a\u9a6c\u900a

gh|github: https://github.com/search?q=$s
  blank=https://github.com/ GitHub 仓库
ge|gitee: https://search.gitee.com/?type=repository&q=$s
  blank=https://gitee.com/ Gitee 仓库
js:|Js: javascript: $S; JavaScript`

: `bi: https://cn.bing.com/search?q=$s
bi|bing: https://www.bing.com/search?q=%s
  blank=https://www.bing.com/ Bing
b|ba|baidu|\u767e\u5ea6: https://www.baidu.com/s?ie=utf-8&wd=%s
  blank=https://www.baidu.com/ \u767e\u5ea6
g|go|gg|google|Google: https://www.google.com/search?q=%s
  www.google.com re=/^(?:.[a-z]{2,4})?/searchb.*?[#&?]q=([^#&]*)/i
  blank=https://www.google.com/ Google
br|brave: https://search.brave.com/search?q=%s Brave
d|dd|ddg|duckduckgo: https://duckduckgo.com/?q=%s DuckDuckGo
ec|ecosia: https://www.ecosia.org/search?q=%s Ecosia
qw|qwant: https://www.qwant.com/?q=%s Qwant
ya|yd|yandex: https://yandex.com/search/?text=%s Yandex
yh|yahoo: https://search.yahoo.com/search?p=%s Yahoo
maru|mailru|mail.ru: https://go.mail.ru/search?q=%s Mail.ru

g.m|gm|g.map|gmap: https://www.google.com/maps?q=%s
  blank=https://www.google.com/maps Google Maps
b.m|bm|map|b.map|bmap|\u767e\u5ea6\u5730\u56fe:
  https://api.map.baidu.com/geocoder?output=html&address=%s&src=vimium-c
y|yt: https://www.youtube.com/results?search_query=%s
  blank=https://www.youtube.com/ YouTube
w|wiki: https://www.wikipedia.org/w/index.php?search=%s Wikipedia
g.s|gs|gscholar: https://scholar.google.com/scholar?q=$s
  scholar.google.com re=/^(?:.[a-z]{2,4})?/scholarb.*?[#&?]q=([^#&]*)/i
  blank=https://scholar.google.com/ Google Scholar

a|ae|ali|alie|aliexp: https://www.aliexpress.com/wholesale?SearchText=%s
  blank=https://www.aliexpress.com/ AliExpress
j|jd|jb|joy|joybuy: https://www.joybuy.com/search?keywords=%s
  blank=https://www.joybuy.com/ Joybuy
az|amazon: https://www.amazon.com/s?k=%s

gh|github: https://github.com/search?q=$s
  blank=https://github.com/ GitHub Repo
ge|gitee: https://search.gitee.com/?type=repository&q=$s
  blank=https://gitee.com/ Gitee 仓库
(defjump *jumps* "g" "https://google.com/search?q=~a")
(defjump *jumps* "gh" "https://github.com/search?&q=~a")
(defjump *jumps* "y" "https://www.youtube.com/results?search_query=~a")
(defjump *jumps* "r" "https://www.reddit.com/r/~a/")

```


## CALENDAR {#calendar}

```emacs-lisp
(use-package calendar
  :ensure nil
  :hook (calendar-today-visible . calendar-mark-today)
  :custom
  ;; 是否显示中国节日，我们使用 `cal-chinese-x' 插件
  (calendar-chinese-all-holidays-flag nil)
  ;; 是否显示节日
  (calendar-mark-holidays-flag t)
  ;; 是否显示 Emacs 的日记，我们使用 org 的日记
  (calendar-mark-diary-entries-flag t)
  ;; 数字方式显示时区，如 +0800，默认是字符方式如 CST
  (calendar-time-zone-style 'numeric)
  ;; 日期显示方式：year/month/day
  (calendar-date-style 'iso)
  ;; 中文天干地支设置
  (calendar-chinese-celestial-stem ["甲" "乙" "丙" "丁" "戊" "己" "庚" "辛" "壬" "癸"])
  (calendar-chinese-terrestrial-branch ["子" "丑" "寅" "卯" "辰" "巳" "午" "未" "申" "酉" "戌" "亥"])
  ;; 设置中文月份
  (calendar-month-name-array ["一月" "二月" "三月" "四月" "五月" "六月" "七月" "八月" "九月" "十月" "十一月" "十二月"])
  ;; 设置星期标题显示
  (calendar-day-name-array ["日" "一" "二" "三" "四" "五" "六"])
  ;; 周一作为一周第一天
  (calendar-week-start-day 1))
```


## PARSE-TIME {#parse-time}

时间解析增加中文拼音
parse-time before cal-china-x

```emacs-lisp
(use-package parse-time
  :ensure nil
  :defer t
  :config
  (setq parse-time-months
        (append '(("yiy" . 1) ("ery" . 2) ("sany" . 3)
                  ("siy" . 4) ("wuy" . 5) ("liuy" . 6)
                  ("qiy" . 7) ("bay" . 8) ("jiuy" . 9)
                  ("shiy" . 10) ("shiyiy" . 11) ("shiery" . 12)
                  ("yiyue" . 1) ("eryue" . 2) ("sanyue" . 3)
                  ("siyue" . 4) ("wuyue" . 5) ("liuyue" . 6)
                  ("qiyue" . 7) ("bayue" . 8) ("jiuyue" . 9)
                  ("shiyue" . 10) ("shiyiyue" . 11) ("shieryue" . 12))
                parse-time-months))

  (setq parse-time-weekdays
        (append '(("zri" . 0) ("zqi" . 0)
                  ("zyi" . 1) ("zer" . 2) ("zsan" . 3)
                  ("zsi" . 4) ("zwu" . 5) ("zliu" . 6)
                  ("zr" . 0) ("zq" . 0)
                  ("zy" . 1) ("ze" . 2) ("zs" . 3)
                  ("zsi" . 4) ("zw" . 5) ("zl" . 6))
                parse-time-weekdays)))
```


## CAL-CHINA-X 日历中文增强 {#cal-china-x-日历中文增强}

;;no (use-package cal-china :ensure t)
我们通过 [cal-china-x](https://github.com/xwl/cal-china-x) 插件进一步地增强中文日历，显示农历等信息。
中国节日设置

```emacs-lisp
(require 'cal-china)
(require 'cal-china-x)
(use-package cal-china-x
  :ensure t
  :commands cal-china-x-setup
  :hook (after-init . cal-china-x-setup)
  :config
  ;; 重要节日设置
  (require 'cal-china-x)

  (setq cal-china-x-important-holidays cal-china-x-chinese-holidays)
  ;; 所有节日设置
  (setq cal-china-x-general-holidays
        '(;;公历节日
          (holiday-fixed 1 1 "元旦")
          (holiday-fixed 2 14 "情人节")
          (holiday-fixed 3 8 "妇女节")
          (holiday-fixed 3 14 "白色情人节")
          (holiday-fixed 4 1 "愚人节")
          (holiday-fixed 5 1 "劳动节")
          (holiday-fixed 5 4 "青年节")
          (holiday-float 5 0 2 "母亲节")
          (holiday-fixed 6 1 "儿童节")
          (holiday-float 6 0 3 "父亲节")
          (holiday-fixed 9 10 "教师节")
          (holiday-fixed 10 1 "国庆节")
          (holiday-fixed 10 2 "国庆节")
          (holiday-fixed 10 3 "国庆节")
          (holiday-fixed 10 24 "程序员节")
          (holiday-fixed 11 11 "双11购物节")
          (holiday-fixed 12 25 "圣诞节")
          ;; 农历节日
          (holiday-lunar 12 30 "春节" 0)
          (holiday-lunar 1 1 "春节" 0)
          (holiday-lunar 1 2 "春节" 0)
          (holiday-lunar 1 15 "元宵节" 0)
          (holiday-solar-term "清明" "清明节")
          (holiday-solar-term "小寒" "小寒")
          (holiday-solar-term "大寒" "大寒")
          (holiday-solar-term "立春" "立春")
          (holiday-solar-term "雨水" "雨水")
          (holiday-solar-term "惊蛰" "惊蛰")
          (holiday-solar-term "春分" "春分")
          (holiday-solar-term "谷雨" "谷雨")
          (holiday-solar-term "立夏" "立夏")
          (holiday-solar-term "小满" "小满")
          (holiday-solar-term "芒种" "芒种")
          (holiday-solar-term "夏至" "夏至")
          (holiday-solar-term "小暑" "小暑")
          (holiday-solar-term "大暑" "大暑")
          (holiday-solar-term "立秋" "立秋")
          (holiday-solar-term "处暑" "处暑")
          (holiday-solar-term "白露" "白露")
          (holiday-solar-term "秋分" "秋分")
          (holiday-solar-term "寒露" "寒露")
          (holiday-solar-term "霜降" "霜降")
          (holiday-solar-term "立冬" "立冬")
          (holiday-solar-term "小雪" "小雪")
          (holiday-solar-term "大雪" "大雪")
          (holiday-solar-term "冬至" "冬至")
          (holiday-lunar 5 5 "端午节" 0)
          (holiday-lunar 8 15 "中秋节" 0)
          (holiday-lunar 7 7 "七夕情人节" 0)
          (holiday-lunar 12 8 "腊八节" 0)
          (holiday-lunar 9 9 "重阳节" 0)))
  ;; 设置日历的节日，通用节日已经包含了所有节日
  (setq calendar-holidays (append cal-china-x-general-holidays)))
```


## <span class="org-todo todo TODO">TODO</span> CLEAN {#clean}

;; :commands cal-china-x-setup
;; :hook (after-init . cal-china-x-setup)
;; :config (setq calendar-holidays (append cal-china-x-chinese-holidays holiday-local-holidays))
;;(autoload 'gmail2bbdb-import-file "gmail2bbdb" nil t nil)
;;(setq gmail2bbdb-bbdb-file "~/begin_new_life/org/bbdb")
;;(add-to-list 'gmail2bbdb-excluded-email-regex-list "noreply@mycompany.com")
;;(add-to-list 'load-path "~/begin_new_life/org/chinese-calendar.el")
;;(require 'chinese-calendar)


## CALFW {#calfw}

```emacs-lisp
(use-package calfw
  :ensure t)

(require 'calfw)
```


## CALFW-ORG {#calfw-org}

```emacs-lisp
(use-package calfw-org
   :ensure t)
```


## 设置任务样式 (setq org-todo-keywords-1 org-todo-keywords) {#设置任务样式--setq-org-todo-keywords-1-org-todo-keywords}

;; TODO 关键词的样式设置

```css
#0098dd
#ff6480
magenta
#7c7c75
#feb24c
#0098dd
#9f7efe
red
yellow
green
purple
blue
#0030b4
#dae7ff
#7cc77f
```

```emacs-lisp
(setq org-todo-keywords
      '((sequence "TODO(t)" "HOLD(h!)" "WAIT(w!)"  "|" "DONE(d)" "NEXT(n!)")
        (sequence "没进(m)"  "线单(x)" "残党(c)" "剩一(s)"  "|"  "全通(q!)" "放弃(f)")
        (sequence "保期内(b)"  "|" "已过期(y)")))
(setq org-todo-keywords-for-agenda org-todo-keywords)
(setq org-todo-keyword-faces '(
                               ("[-]" . +org-todo-active)
                               ("STRT" . +org-todo-active)
                               ("[?]" . +org-todo-onhold)
                               ("WAIT" . +org-todo-onhold)
                               ("HOLD" . +org-todo-onhold)
                               ("PROJ" . +org-todo-project)
                               ("NO" . +org-todo-cancel)
                               ("KILL" . +org-todo-cancel)
                               ("没进" . +org-todo-active)
                               ("线单" . +org-todo-onhold)
                               ("残党" . +org-todo-onhold)
                               ("剩一" . +org-todo-project)
                               ("全通" . +org-todo-cancel)
                               ("放弃" . +org-todo-cancel)
                               ("WAIT" . (:foreground "orange" :weight bold))
                               ("TODO" . (:foreground "red" :weight bold))
                               ("HOLD" . (:foreground "magenta" :weight bold))
                               ("DONE" . (:background "grey" :weight bold))
                               ("NEXT" . (:foreground "brown" :weight bold))

                               ("保期内" . (:foreground "green" :weight bold))
                               ("已过期" . (:foreground "purple" :weight bold))

                               ("取消" .    (:background "gray" :foreground "black"))
                               ("落幕" . (:background "yellow" :weight bold))))

```

```emacs-lisp
;; 设置Org-mode日程视图过期事件颜色 (setq org-agenda-scheduled-faces '((0.0 . (:background "red"))   ; overdue deadlines (0.5 . (:background "orange")) ; approaching deadlines (1.0 . (:background "green"))  ; scheduled items)) (setq org-agenda-deadline-faces '(
;;(1.001 . org-warning) ; upcoming scheduled item (0.0 . (:foreground "red" :weight bold))    ;过期的截止日期 (0.2 . (:foreground "orange" :weight bold)) ;将要过期的截止日期 (0.4 . (:foreground "yellow" :weight bold)) ;即将到期的截止日期 (0.6 . (:foreground "green" :weight bold)) ;截止日期还有一定时间 (0.8 . (:foreground "blue" :weight bold))  ;截止日期遥远 (1.0 . (:foreground "purple" :weight bold))  ;截止日期很遥远))
;;(1.0 . org-imminent-deadline)
;;(0.5 . org-upcoming-deadline)
;;(0.0 . org-upcoming-distant-deadline)
;;(custom-set-faces '(org-imminent-deadline ((t (:background "red")))))
;;"REPORT" "KNOWNCAUSE" "FIXED"(sequence "TODO" "STARTED" "WAITING" "|" "DONE" "CANCELLED")
;;(setq org-todo-keywords (quote ()))
;; drug; medicinal; medicines and chemical reagents; pharmaceutical

;; home
;; (sequence "WAIT(w@/!)" "SOMEDAY(S)" "|" "CANCELLED(c@/!)" "MEETING(m)" "PHONE(p)")
;; TODO的关键词设置，可以设置不同的组
;;(sequence "REPORT(r)" "BUG(b)" "KNOWNCAUSE(k)" "|" "FIXED(f!)")"CANCELLED(c@/!)" "WAITING(w@/!)" "SOMEDAY(S)" "|" "CANCELLED(c@/!)" "MEETING(m)" "PHONE(p)"
;;M-x occur #\+begin_\|#+end_ 这个正则表达式会搜索 org-mode 文件中所有包含 `#+begin_` 或 `#+end_` 的行，并将它们显示在一个新的 buffer 中。
;; (sequence "TODO(t)" "INDO(i)" "|" "DONE(d!/!)")
;;(add-to-list 'org-todo-keywords '(sequence "保期内(b)" "|" "已过期(y)"))
;;'("WAITING(w@/!)" "SOMEDAY(S)" "|" "CANCELLED(c@/!)" "MEETING(m)" "PHONE(p)"))
;;(sequence "TODO(t)" "STARTED(s)" "|" "DONE(d!/!)")
;;(setq org-todo-keywords '((sequence "未开始(k!)" "进行中(j!)" "阻塞中(z!)" "|" "已完成(f!)" "已过期(a@/!)")))
```


## <span class="org-todo todo ___">保期内</span> TEST {#test}

```text
("NEXT" "DONE" "已过期" "放弃" "全通" "保期内" "剩一" "残党" "线单" "没进" "WAIT" "HOLD" "TODO")
;;"
;;("TODO" . warning)
;;("DOING" . success)
;;("WAITING" . error)
;;("VERIFY" . error)
;;("DONE" . shadow)
;;("CANCEL" . shadow)
;;TODO(t)
;;HOLD(d)
;;WAIT(w)
;;未过期 未开始
;;DONE(d)
;;NEXT(n)
;;
;;In-Progress(p)
;;REVIEW
;;INPROGRESS(i)
;;(C)(d!)(a@/!)
;;开始(k)
;;进行(j)
;;计划
;;暂停(z)
;;阻塞(z)
;;落幕(l)
;;取消(c)
;;
```

```emacs-lisp
;; 设置Org-mode默认TAGS (setq org-tag-alist '((:startgroup . nil) ("@work" . ?w)
;; 工作 ("@home" . ?h)
;; 父母 ("@game" . ?g)
;; 隐私 (:endgroup . nil)))
```


## <span class="org-todo todo TODO">TODO</span> EXPORT {#export}

latex &amp; html 在 orgmode 中，Validate 功能是用于在导出为 HTML 文件时，自动添加一个链接到 W3C Validator 的网站，可以让你快速验证生成的 HTML 代码是否符合标准。如果你的 HTML 代码被 Validator 检测到不符合标准，那么这个链接会显示错误信息，给你修复错误提供帮助。 禁用 Validate 功能可以使得导出的 HTML 文件更加简洁、清晰，不会包含 Validate 链接，避免了一些人视觉上的干扰。但是，这也意味着你需要手动去验证你生成的 HTML 代码是否符合标准；因此，在实际应用过程中，你需要根据具体需求和情况，决定是否需要启用 Validate 功能。

```emacs-lisp
(setq org-html-validation-link nil)
(setq org-format-latex-options (plist-put org-format-latex-options :scale 3))
(setq org-format-latex-options '(:foreground default
                                             :background "Transparent"
                                             :scale 3.0 :html-foreground "Black" :html-background "Transparent"
                                             :html-scale 1.5
                                             :matchers '("begin" "$1" "$" "$$" "\\(" "\\[")))
(add-hook 'org-mode-hook (lambda () (setq-local org-format-latex-options (plist-put org-format-latex-options :scale 3.0))))
;;P25 00:42
;;把这个背景设置成透明的.但若是频繁在亮/暗主题切换的话, 这个涉及到要让 LaTeX 渲染图的本体 (foreground) 颜色在黑/白之间反复切换. 这避免不了重新渲染.
;; P25 1:37
;;find-file:/ssh:www.wacl.org:sc/tfsodF
;;find-file:/ssh:wacl2399@gmail.com@www.wacl.org:sc/tfsodf
;;/ssh:www.wacl.org|sudo:root@www.wacl.org:/home/wacl/sr/rootfile
```


## TEX {#tex}

tangle ~/emacs-babel.el

```emacs-lisp
  ;;(defun tex-view() (interactive) (tex-send-command "evince" (tex-append tex-print-file ".pdf")))
;;  (use-package tex
;;    :ensure auctex)
  (use-package auctex
    :ensure t)
```


## COUNSEL-PROJECTILE {#counsel-projectile}

```emacs-lisp
(use-package counsel-projectile :ensure t)
```


## PROJECTILE {#projectile}

```emacs-lisp
(use-package projectile
  :ensure t
  :config
  (projectile-global-mode)
  (setq projectile-completion-system 'ivy))
```


## <span class="org-todo todo TODO">TODO</span> MAXIMAtangle ~/emacs-babel.el {#maximatangle-emacs-babel-dot-el}

```emacs-lisp
(setq imaxima-inline-image-style (concat "<style type=\"text/css\">\n" (with-temp-buffer (insert-file-contents "~/begin_new_life/org/custom-imaxima.css") (buffer-string)) "</style>"))
```

```emacs-lisp
(add-to-list 'load-path "~/.emacs.d.doomemacs/.local/straight/links/ob-elixir/")
(add-to-list 'load-path "~/.emacs.d.doomemacs/.local/straight/build-29.1/ob-elixir/")
(require 'ob-C)
(require 'ob-calc)
;;(require 'ob-csharp)
(require 'ob-css)
(require 'ob-java)
(require 'ob-emacs-lisp)
(require 'ob-eshell)
(require 'ob-eval)
(require 'ob-exp)
(require 'ob-js)
;;(add-to-list 'org-babel-load-languages '(js . t))
(require 'ob-maxima)
(require 'ob-org)
(require 'ob-python)
(require 'ob-sass)
(require 'ob-shell)
(require 'ob-sql)
(require 'ob-sqlite)
(require 'ob-table)
(require 'ob-tangle)
(require 'ob-dot)
(require 'ob-ditaa)
(require 'ob-gnuplot)
(require 'ob-perl)
(require 'ob-R)
(require 'ob-plantuml)
```

```emacs-lisp
;(require 'ob-hledger)
;(require 'ob-julia)
;(require 'ob-latex)
;(require 'ob-ledger)
;(require 'ob-lilypond)
;(require 'ob-lisp)
;(require 'ob-lua)
;(require 'ob-tcl)
;(require 'ob-vala)
;(require 'ob-vbnet)
;(require 'org-lint)
;;(require 'ob-J)
;;(require 'ob-abc)
;;(require 'ob-ammonite)
;;(require 'ob-asymptote)
;;(require 'ob-awk)
;;(require 'ob-clojure)
;;(require 'ob-comint)
;;(require 'ob-coq)
;;(require 'ob-core)
;;(require 'ob-ebnf)
;;(require 'ob-eukleides)
;;(require 'ob-fomus)
;;(require 'ob-forth)
;;(require 'ob-fortran)
;;(require 'ob-groovy)
;;(require 'ob-haskell)
;;(require 'ob-io)
;;(require 'ob-lob)
;;(require 'ob-makefile)
;;(require 'ob-mathomatic)
;;(require 'ob-matlab)
;;(require 'ob-mscgen)
;;(require 'ob-nim)
;;(require 'ob-ocaml)
;;(require 'ob-octave)
;;(require 'ob-oz)
;;(require 'ob-picolisp)
;;(require 'ob-processing)
;;(require 'ob-ref)
;;(require 'ob-restclient)
;;(require 'ob-ruby)
;;(require 'ob-scheme)
;;(require 'ob-screen)
;;(require 'ob-sed)
;;(require 'ob-shen)
;;(require 'ob-spice)
```

```elisp
;;(require 'imaxima)
;;(defcustom imaxima-create-image-options '(:ascent center :mask ((:float "left") (heuristic (color-values imaxima-bg-color)))) "Optional arguments passed to `imaxima-create-image'" :group 'imaxima :type '(alist))
;; change default (defun org-babel-execute:maxima (body params) "Execute a block of Maxima entries with org-babel. This function is called by `org-babel-execute-src-block'." (message "executing Maxima source code block") (let ((result-params (split-string (or (cdr (assq :results params)) ""))) (result
;;(let* ((cmdline (or (cdr (assq :cmdline params)) ""))
       ;;(in-file (org-babel-temp-file "maxima-" ".max"))
       ;;(cmd (format "%s --very-quiet -r 'batchload(%S)$' %s"
                    ;;org-babel-maxima-command in-file cmdline)))
;;(let* ((cmdline (or (cdr (assoc :cmdline params)) ""))
         ;;(in-file (org-babel-temp-file "maxima-" ".max"))
         ;;(cmd (format "%s --very-quiet -r \"batchload(\\\"%s\\\")$\" %s"
                      ;;org-babel-maxima-command in-file cmdline)))

;; (add-to-list 'load-path "~/.emacs.d.spacemacs/elpa/28.2/develop/org-9.6.7/")
;; C:\Users\wacl\.emacs.d.doomemacs\.local\straight\links\ob-http
;; ob-elixir.el
;;(load "~/.emacs.d.spacemacs/elpa/28.2/develop/org-contrib-0.4.1/ob-ledger.el")
;;(load "~/.emacs.d.spacemacs/elpa/28.2/develop/org-9.6.7/ob-maxima.el")
;;~/.emacs.d.doomemacs/.local/straight/repos/org/lisp/ob-python.el
;;~/.emacs.d.doomemacs/.local/straight/build-28.2/org/ob-css.el
;;(unload-feature 'ob-python)
;;(load "~/.emacs.d.spacemacs/elpa/28.2/develop/org-9.6.7/ob-python.el")
;;(load "~/.emacs.d.doomemacs/.local/straight/repos/ob-elixir/ob-elixir.el")
;; (add-to-list 'load-path "~/.emacs.d.spacemacs/elpa/28.2/develop/org-9.6.7/")

    ;;(with-temp-file in-file (insert (org-babel-maxima-expand body params)))
    ;;(message cmd)
    ;; " | grep -v batch | grep -v 'replaced' | sed '/^$/d' " (let ((raw (org-babel-eval cmd ""))) (mapconcat #'identity (delq nil (mapcar (lambda (line) (unless (or (string-match "batch" line) (string-match "^rat: replaced .*$" line) (string-match "^
  ;;; Loading #P" line) (= 0 (length line))) line)) (split-string raw "[\r\n]"))) "\n"))))) (if (ignore-errors (org-babel-graphical-output-file params)) nil (org-babel-result-cond result-params result (let ((tmp-file (org-babel-temp-file "maxima-res-"))) (with-temp-file tmp-file (insert result)) (org-babel-import-elisp-from-file tmp-file))))))

```


## APPT {#appt}

Emacs 的邀约提醒，并集成 org-agenda 提醒：

```emacs-lisp
(use-package appt
  :ensure nil
  :hook ((after-init . (lambda () (appt-activate 1)))
         (org-finalize-agenda . org-agenda-to-appt))
  :config
  ;; 通知提醒
  (defun appt-display-with-notification (min-to-app new-time appt-msg)
    (notify-send :title (format "Appointment in %s minutes" min-to-app)
                 :body appt-msg
                 :urgency 'critical)
    (appt-disp-window min-to-app new-time appt-msg))

  ;; 每15分钟更新一次appt
  (run-at-time t 900 #'org-agenda-to-appt)

  ;;(shut-up! #'org-agenda-to-appt)

  :custom
  ;; 是否显示日记
  (appt-display-diary t)
  ;; 提醒间隔时间，每15分钟提醒一次
  (appt-display-interval 15)
  ;; 模式栏显示提醒
  (appt-display-mode-line t)
  ;; 设置提醒响铃
  (appt-audible t)
  ;; 提前30分钟提醒
  (appt-message-warning-time 30)
  ;; 通知提醒函数
  ;;(appt-disp-window-function #'appt-display-with-notification)

  :config
  (require 'appt)
  ;; 每小时同步一次appt,并且现在就开始同步
  (run-at-time nil 3600 'org-agenda-to-appt)
  ;; 更新agenda时，同步appt
  (add-hook 'org-finalize-agenda-hook 'org-agenda-to-appt)
  ;; 激活提醒
  (appt-activate 1)
   ;;(appt-check)
  ;;   我希望appt在提醒时会通过beep发声提醒我:
  (setq appt-audible t)
  ;; appt默认每隔三分钟就弹出一次提醒，这个也太频繁了，我们可以通过修改 apt-display-interval 来修改这个间隔时间
  (setq appt-display-interval 5)
  (setq appt-display-format 'window))
;;表示在新窗口中显示提醒信息。其他的显示格式，例如 'echo、'frame 等。
```


### <span class="org-todo todo TODO">TODO</span> APPT-WINDOW-FORMAT {#appt-window-format}

```emacs-lisp
;;(add-hook 'diary-hook 'diary-remind-function)
;;(remove-hook 'diary-hook 'diary-remind-function)
;;(remove-hook 'diary-hook 'all)
;;(add-hook 'diary-hook 'appt-make-list)
;;(remove-hook 'diary-hook 'appt-make-list)
;;(add-hook 'org-agenda-finalize-hook
;;          (lambda ()
;;            (when (org-agenda-span 'year)
;;              (setq org-agenda-skip-scheduled-if-done t))))
;;(lambda ()
;;  (when (org-agenda-span 'year)
;;    (setq org-agenda-skip-scheduled-if-done t)
;;))
;; 提前半小时提醒
(setq appt-message-warning-time 30)
;; (setq appt-message-warning-time '(list 30 15 1))
;; (setq appt-message-warning-time '(30 15 1))
(require 'notifications)
(defun appt-disp-window-and-notification (min-to-appt current-time appt-msg)
  (let ((title (format "%s分钟内有新的任务" min-to-appt)))
    (notifications-notify :timeout (* appt-display-interval 60000) ;一直持续到下一次提醒
                          :title title
                          :body appt-msg
                          )
    (appt-disp-window min-to-appt current-time appt-msg))) ;同时也调用原有的提醒函数
(setq appt-display-format 'window) ;; 只有这样才能使用自定义的通知函数
(setq appt-disp-window-function #'appt-disp-window-and-notification)
```

:<span class="timestamp-wrapper"><span class="timestamp">&lt;2023-05-31 Wednesday 18:36&gt;</span></span>
(require 'dbus) (dbus-init-busybox)
(setq appt-window-format '(:title "Appointment Reminder" :point-either t :size 6 :noselect t :no-focus t :minibuffer t :screen-position ,(quote center)))
(run-at-time "00:01" (\* 60 60 24) 'appt-activate) 没有设置 Appt Timer。Appt 模式需要一个定时器来检查日程事件和任务。如果没有设置 Appt Timer，appt-check 命令将无法检查日程事件和任务。您可以使用以下代码来设置 Appt Timer： (setq appt-display-duration 30) (setq appt-file "/file") 上述代码将在每天凌晨 12:01 激活 Appt 模式。 没有添加日程事件或任务。如果没有添加任何日程事件或任务，appt-check 命令将无法检查任何内容。您可以在 Org 日程文件中添加一些测试任务来测试 Appt 窗口是否正常工作。 没有启用 Appt 模式。如果没有启用 Appt 模式，appt-check 命令将无法检查日程事件和任务。您可以使用以下代码来启用 Appt 模式： (appt-activate 1) (appt-check) 上述代码将启用 Appt 模式并检查日程事件和任务。


## <span class="org-todo todo TODO">TODO</span> APPT-DISPLAY-FORMAT 变量用于设置提醒窗口的显示格式。 {#appt-display-format-变量用于设置提醒窗口的显示格式}

```emacs-lisp
(setq appt-time-msg-list '(
                           ((diary-float t 9 0) "Call Jane")
                           ((diary-float t 14 0) "Meeting with John")
                           ((diary-float t 16 0) "Send report to boss")
                           (diary-float t 14 0) "Meeting with John"))
```


## <span class="org-todo todo TODO">TODO</span> BBDB {#bbdb}

```emacs-lisp
(require 'bbdb) (bbdb-initialize)
```


## HELPFUL {#helpful}

[helpful](https://github.com/Wilfred/helpful) 插件提供了帮助增强。

```emacs-lisp
(use-package helpful
  :ensure t
  :commands (helpful-callable helpful-variable helpful-command helpful-key helpful-mode)
  :bind (([remap describe-command] . helpful-command)
         ("C-h f" . helpful-callable)
         ("C-h v" . helpful-variable)
         ("C-h s" . helpful-symbol)
         ("C-h S" . describe-syntax)
         ("C-h m" . describe-mode)
         ("C-h F" . describe-face)
         ([remap describe-key] . helpful-key)))
```


## WHICH-KEY {#which-key}

[which-key](https://github.com/justbur/emacs-which-key) 插件将提示快捷键。

```emacs-lisp
(use-package which-key
  :ensure t
  :diminish which-key-mode
  :hook (after-init . which-key-mode)
  :config
  (which-key-mode)
  (which-key-setup-minibuffer)
  (which-key-add-key-based-replacements
    "C-c !" "flycheck"
    "C-c @" "hideshow"
    "C-c i" "ispell"
    "C-c t" "hl-todo"
    "C-x a" "abbrev"
    "C-x n" "narrow"
    "C-x t" "tab")
  :custom
  (which-key-idle-delay 0.5)
  (which-key-add-column-padding 1))
```


## SDCV 本地词典 <span class="tag"><span class="after">after</span></span> {#sdcv-本地词典}

[sdcv](https://github.com/manateelazycat/sdcv) 是一个基于本地 stardict 词典的客户端，可以快速查询一些单词释义。
;;:after posframe

```emacs-lisp
;; 这个插件依赖于 `posframe' 这个插件
(use-package sdcv
  :quelpa (sdcv :fetcher github :repo "manateelazycat/sdcv")
  :commands (sdcv-search-pointer+)
  :bind ("C-," . sdcv-search-pointer+)
  :config
  (setq sdcv-say-word-p t)
  (setq sdcv-dictionary-data-dir (expand-file-name "~/Backup/stardict/"))
  (setq sdcv-dictionary-simple-list
        '("懒虫简明英汉词典"
          "懒虫简明汉英词典"))
  (setq sdcv-dictionary-complete-list
        '("朗道英汉字典5.0"
          "牛津英汉双解美化版"
          "21世纪双语科技词典"
          "quick_eng-zh_CN"
          "新世纪英汉科技大词典"))
  (setq sdcv-tooltip-timeout 10)
  (setq sdcv-fail-notify-string "没找到释义")
  (setq sdcv-tooltip-border-width 2))
```


## FANYI 在线词典 {#fanyi-在线词典}

[fanyi](https://github.com/condy0919/fanyi.el) 是一个线上的多词典系统。安装 `mpv` 来播放读音。

```emacs-lisp
(use-package fanyi
  :ensure t
  :bind-keymap ("\e\e =" . fanyi-map)
  :bind (:map fanyi-map
              ("w" . fanyi-dwim2)
              ("i" . fanyi-dwim))
  :init
  ;; to support `org-store-link' and `org-insert-link'
  (require 'ol-fanyi)
  ;; 如果当前指针下有单词，选择当前单词，否则选择剪贴板
  (with-eval-after-load 'org-capture
    (add-to-list 'org-capture-templates
                 '("w" "New word" entry (file+olp+datetree "20221001T221032--vocabulary__studying.org" "New")
                   "* %^{Input the new word:|%(cond ((with-current-buffer (org-capture-get :original-buffer) (thing-at-point 'word 'no-properties))) ((clipboard/get)))}\n\n[[fanyi:%\\1][%\\1]]\n\n[[http://dict.cn/%\\1][海词：%\\1]]%?"
                   :tree-type day
                   :empty-lines 1
                   :jump-to-captured t)))
  :config
  (defvar fanyi-map nil "keymap for `fanyi")
  (setq fanyi-map (make-sparse-keymap))
  (setq fanyi-sound-player "mpv")
  :custom
  (fanyi-providers '(;; 海词
                     fanyi-haici-provider
                     ;; 有道同义词词典
                     fanyi-youdao-thesaurus-provider
                     ;; ;; Etymonline
                     ;; fanyi-etymon-provider
                     ;; Longman
                     fanyi-longman-provider
                     ;; ;; LibreTranslate
                     ;; fanyi-libre-provider
                     )))
```


## <span class="org-todo todo TODO">TODO</span> PASS 密码管理 {#pass-密码管理}

通过 [pass](https://github.com/NicolasPetton/pass) 插件来进行密码管理。

```emacs-lisp
(use-package pass
  :ensure t
  :commands (pass))
```


## NOV epub 阅读器 {#nov-epub-阅读器}

通过 [nov](https://depp.brause.cc/nov.el/) 对 `epub` 进行阅读。

```emacs-lisp
(use-package nov
  :ensure t
  :mode ("\\.epub\\'" . nov-mode)
  :bind (:map nov-mode-map
              ("j" . scroll-up-line)
              ("k" . scroll-down-line)))
```


## <span class="org-todo todo TODO">TODO</span> CALIBREDB {#calibredb}

通过 [calibredb.el](https://github.com/chenyanming/calibredb.el) 来进行图书管理。

```emacs-lisp
(use-package calibredb
  :ensure t
  :commands calibredb
  :bind ("\e\e b" . calibredb)
  :config
  (setq calibredb-root-dir "~/Calibre")
  (setq calibredb-db-dir (expand-file-name "metadata.db" calibredb-root-dir))
  (setq calibredb-library-alist '(("~/Books/books"))))
```


## VC 设置 Emacs 自带的 vc 设置。 {#vc-设置-emacs-自带的-vc-设置}

```emacs-lisp
(use-package vc
  :ensure nil
  :custom
  ;; 打开链接文件时，不进行追问
  (vc-follow-symlinks t)
  (vc-allow-async-revert t)
  (vc-handled-backends '(Git)))
```


## MAGIT 版本管理 {#magit-版本管理}

[magit](https://github.com/magit/magit) 是 Emacs 里的另一个杀手级应用！可以直接在 Emacs 里进行基于 git 的版本管理。

```emacs-lisp
(use-package magit
  :ensure t
  :hook (git-commit-mode . flyspell-mode)
  :bind (("C-x g"   . magit-status)
         ("C-x M-g" . magit-dispatch)
         ("C-c M-g" . magit-file-dispatch))
  :custom
  (magit-diff-refine-hunk t)
  (magit-ediff-dwim-show-on-hunks t))
```


## DIFF-HL 高亮显示修改的部分 {#diff-hl-高亮显示修改的部分}

[diff-hl](https://github.com/dgutov/diff-hl) 插件可以在左侧高亮显示相对于远程仓库的修改部分。

```emacs-lisp
(use-package diff-hl
  :ensure t
  :hook ((dired-mode         . diff-hl-dired-mode-unless-remote)
         (magit-pre-refresh  . diff-hl-magit-pre-refresh)
         (magit-post-refresh . diff-hl-magit-post-refresh))
  :init
  (global-diff-hl-mode t)
  :config
  ;; When Emacs runs in terminal, show the indicators in margin instead.
  (unless (display-graphic-p)
    (diff-hl-margin-mode)))
```


## MAGIT-DELTA 增强 git diff {#magit-delta-增强-git-diff}

[magit-delta](https://github.com/dandavison/magit-delta) 插件可以通过 `git-delta` 来更优化的方式显示 diff 内容（需要提前安装 `brew install git-delta` ）。

```emacs-lisp
(use-package magit-delta
  :ensure t
  :after magit-mode
  :hook (magit-mode . magit-delta-mode)
  :config
  (setq magit-delta-hide-plus-minus-markers nil))
```


## language {#language}


## ELISP-MODE emacs-lisp 语言设置 {#elisp-mode-emacs-lisp-语言设置}

```emacs-lisp
(use-package elisp-mode
  :ensure nil
  :bind (:map emacs-lisp-mode-map
              ("C-c C-b" . eval-buffer)
              ("C-c C-c" . eval-to-comment)
              :map lisp-interaction-mode-map
              ("C-c C-c" . eval-to-comment)
              :map org-mode-map
              ("C-c C-;" . eval-to-comment))

  :init
  ;; for emacs-lisp org babel
  (add-to-list 'org-babel-default-header-args:emacs-lisp
             '(:results . "value pp"))
  :config
  (defconst eval-as-comment-prefix " ? ")
  (defun eval-to-comment (&optional arg)
    (interactive "P")
    ;; (if (not (looking-back ";\\s*"))
    ;;     (call-interactively 'comment-dwim))
    (call-interactively 'comment-dwim)
    (progn
      (search-backward ";")
      (forward-char 1))
    (delete-region (point) (line-end-position))
    (save-excursion
      (let ((current-prefix-arg '(4)))
        (call-interactively 'eval-last-sexp)))
    (insert eval-as-comment-prefix)
    (end-of-line 1)))
```


## PYTHON 语言设置 {#python-语言设置}

```emacs-lisp
(use-package python
  :ensure nil
  :mode ("\\.py\\'" . python-mode)
  :init
  (add-list-to-list 'org-babel-default-header-args:python '(;(:results . "output pp")
                                                            ;(:noweb   . "yes")
                                                            ;(:session . "py")
                                                            ;(:async   . "yes")
                                                            (:exports . "both")))

  :config
  (setq python-indent-offset 4)
  ;; Disable readline based native completion
  (setq python-shell-completion-native-enable nil)
  (setq python-indent-guess-indent-offset-verbose nil)
  (setq python-shell-interpreter "python3"
        python-shell-prompt-detect-failure-warning nil)
  ;; disable native completion
  (add-to-list 'python-shell-completion-native-disabled-interpreters "python3")
  ;; Env vars
  (with-eval-after-load 'exec-path-from-shell
    (exec-path-from-shell-copy-env "PYTHONPATH")))
```


## PY-AUTOPEP8 {#py-autopep8}

[py-autopep8.el](https://github.com/paetzke/py-autopep8.el) 插件可以让我们在编写 Python 保存文件的时候，自动以 `PEP8` 标准做格式美化（需要提前 `brew install autopep8` ）。

```emacs-lisp
;; need to pip install autopep8 first
(use-package py-autopep8
  :ensure t
  :hook (python-mode . py-autopep8-mode))
```


## <span class="org-todo todo TODO">TODO</span> SH-SCRIPT {#sh-script}

Shell 语言设置
tangle ~/emacs-babel.el

```emacs-lisp
(use-package sh-script
  :ensure nil
  :mode (("\\.sh\\'"     . sh-mode)
         ("zshrc"        . sh-mode)
         ("zshenv"       . sh-mode)
         ("/PKGBUILD\\'" . sh-mode))
  :hook (sh-mode . sh-mode-setup)
  :bind (:map sh-mode-map
         ("C-c C-e" . sh-execute-region))
  :init
  ;; for org babel
  (add-list-to-list 'org-babel-default-header-args:shell
                  '((:results . "output")
                    (:tangle  . "no")))
  :config
  (defun sh-mode-setup ()
    (add-hook 'after-save-hook #'executable-make-buffer-file-executable-if-script-p nil t))
  :custom
  (sh-basic-offset 2)
  (sh-indentation 2))
```


## MESSAGE {#message}

```emacs-lisp
(use-package message
  :ensure nil
  :hook ((message-mode . auto-fill-mode)
         (message-mode . (lambda () (display-line-numbers-mode 0)))
         (message-mode . turn-on-orgtbl))

  ;; :bind (:map message-mode-map ("TAB" . message-display-abbrev))
  :config
  ;; add Cc and Bcc headers to the message buffer
  (setq message-default-mail-headers "Cc: \nBcc: \n")
  ;; change the directory to store the sent mail and mkdir sent ahead
  (setq message-directory "~/Mail/")
  (setq message-auto-save-directory "~/Mail/drafts/")
  :custom
  ;; make sure `user-full-name' and `user-mail-address' are configed
  (message-kill-buffer-on-exit t)
  (message-mail-alias-type 'ecomplete)
  (message-send-mail-function #'message-use-send-mail-function)
  (message-signature (concat "Best regards,\n" user-full-name)))
```


## SENDMAIL {#sendmail}

邮件发送配置

```emacs-lisp
(use-package sendmail
  :ensure nil
  :defer t
  :custom
  (send-mail-function 'sendmail-send-it)
  ;; msmtp config
  (sendmail-program "/usr/local/bin/msmtp")
  (mail-specify-envelope-from t)
  (message-sendmail-envelope-from 'header)
  (mail-envelope-from 'header)
  )
```


## ESHELL 基本配置 {#eshell-基本配置}

```emacs-lisp
(use-package eshell
  :ensure nil
  :functions eshell/alias
  :hook ((eshell-mode . (lambda ()
                          (term-mode-common-init)
                          ;; Remove cmd args word by word
                          (modify-syntax-entry ?- "w")
                          (visual-line-mode 1)
                          (setenv "PAGER" "cat")))
         )
  :config
  (defun term-mode-common-init ()
  "The common initialization for term."
  (setq-local scroll-margin 0)
  (setq-local truncate-lines t)
  )

  ;; 在Emacs里输入vi，直接在buffer里打开文件
  (defalias 'eshell/vi 'find-file)
  (defalias 'eshell/vim 'find-file)

  ;; 语法高亮显示
  (defun eshell/bat (file)
    "cat FILE with syntax highlight."
    (with-temp-buffer
      (insert-file-contents file)
      (let ((buffer-file-name file))
        (delay-mode-hooks
          (set-auto-mode)
          (font-lock-ensure)))
      (buffer-string)))
  (defalias 'eshell/cat 'eshell/bat)

  ;; 交互式进入目录
  (defun eshell/z ()
    "cd to directory with completion."
    (let ((dir (completing-read "Directory: " (ring-elements eshell-last-dir-ring) nil t)))
      (eshell/cd dir)))

  ;; 查找文件
  (defun eshell/f (filename &optional dir)
    "Search for files matching FILENAME in either DIR or the
current directory."
    (let ((cmd (concat
                ;; using find
                (executable-find "find")
                " " (or dir ".")
                " -not -path '*/.git*'"            ; ignore .git directory
                " -and -not -path 'build'"         ; ignore cmake build directory
                " -and -not -path '*/eln-cache*'"  ; ignore eln cache
                " -and -type f -and -iname "
                "'*" filename "*'")))
      (eshell-command-result cmd)))

  :custom
  (eshell-banner-message
   '(format "%s %s\n"
            (propertize (format " %s " (string-trim (buffer-name)))
                        'face 'mode-line-highlight)
            (propertize (current-time-string)
                        'face 'font-lock-keyword-face)))
  (eshell-scroll-to-bottom-on-input 'all)
  (eshell-scroll-to-bottom-on-output 'all)
  (eshell-kill-on-exit t)
  (eshell-kill-processes-on-exit t)
  ;; Don't record command in history if starts with whitespace
  (eshell-input-filter 'eshell-input-filter-initial-space)
  (eshell-error-if-no-glob t)
  (eshell-glob-case-insensitive t)
  ;; set scripts
  (eshell-rc-script (locate-user-emacs-file "etc/eshell/profile"))
  (eshell-login-script (locate-user-emacs-file "etc/eshell/login"))
  )
```


## EM-REBIND {#em-rebind}

eshell 里的 C-d

1.  让 `C-d` 更智能：

<!--listend-->

```emacs-lisp
(use-package em-rebind
  :ensure nil
  :after eshell
  :commands eshell-delchar-or-maybe-eof)

```


## ESH-MODE {#esh-mode}

```emacs-lisp
(use-package esh-mode
  :ensure nil
  :after eshell
  :bind (:map eshell-mode-map
              ("C-d" . eshell-delchar-or-maybe-eof)
              ("C-r" . consult-history)
              ("C-l" . eshell/clear))
  )
```


## EM-HIST {#em-hist}

Eshell 的命令历史

```emacs-lisp
(use-package em-hist
  :ensure nil
  :after eshell
  :defer t
  :custom
  (eshell-history-size 1024)
  (eshell-hist-ignoredups t)
  (eshell-save-history-on-exit t))
```


## EM-TERM {#em-term}

有一些命令如 top，我们还是使用 term：
有些命令使用 term

```emacs-lisp
;; following commands will run on term instead
(use-package em-term
  :ensure nil
:after eshell-mode
  :defer t
  :custom
  (eshell-visual-commands '("top" "htop" "less" "more"))
  (eshell-visual-subcommands '(("git" "help" "lg" "log" "diff" "show")))
  (eshell-visual-options '(("git" "--help" "--paginate")))
  (eshell-destroy-buffer-when-process-dies t))
```


## ESHELL-GIT-PROMPT {#eshell-git-prompt}

[eshell-git-prompt](https://github.com/xuchunyang/eshell-git-prompt) 插件提供了数个好看的 Eshell 命令行主题。

```emacs-lisp
(use-package eshell-git-prompt
  :ensure t
  :after esh-mode
  :custom-face
  (eshell-git-prompt-multiline2-dir-face ((t (:foreground "#c09035" :bold t))))
  :config
  (eshell-git-prompt-use-theme 'multiline2)
  )

```


## ESHELL-SYNTAX-HIGHLIGHTING {#eshell-syntax-highlighting}

[eshell-syntax-highlighting](https://github.com/akreisher/eshell-syntax-highlighting) 插件为 Eshell 提供语法高亮。

```emacs-lisp
(use-package eshell-syntax-highlighting
  :after esh-mode
  :ensure t
  :hook (eshell-mode . eshell-syntax-highlighting-global-mode)
  :custom-face
  (eshell-syntax-highlighting-shell-command-face ((t (:foreground "#7cc77f" :bold t)))))
```


## CAPF-AUTOSUGGEST 自动补全 {#capf-autosuggest-自动补全}

[capf-autosuggest](https://github.com/emacsmirror/capf-autosuggest) 提供 Fish 类似的 Eshell 命令自动补全功能。类似的插件还有 [esh-autosuggest](https://github.com/dieggsy/esh-autosuggest)。

```emacs-lisp
(use-package capf-autosuggest
  :ensure t
  :after eshell
  :hook ((eshell-mode comint-mode) . capf-autosuggest-mode)
  :custom-face
  (capf-autosuggest-face ((t (:foreground "#dae7ff")))))
```

```css
#dae7ff
#7cc77f
```


## ESHELL-UP 快速进入父级文件夹 {#eshell-up-快速进入父级文件夹}

[eshell-up](https://github.com/peterwvj/eshell-up) 插件可以快速进入当前文件夹的任何一个父级文件夹。通过 `up` 命令（已经设置了 up 在 eshell 里的 alias）进入当前文件夹的任何一级父目录。

```emacs-lisp
(use-package eshell-up
  :ensure t
  :after eshell
  :commands (eshell-up eshell-up-peek)
  :config
  ;; to print the matching parent directory before changing to it
  (setq eshell-up-print-parent-dir t))
```


## SHR {#shr}

```emacs-lisp
(use-package shr
  :ensure nil
  :defer t
  :custom
  (shr-inhibit-images nil)                ; 显示图片
  (shr-image-animate t))               ; 显示 gif
```


## EWW Emacs 内置 EWW 浏览器配置。 {#eww-emacs-内置-eww-浏览器配置}

```emacs-lisp
(use-package eww
  :ensure nil
  :commands eww eww-follow-link
  :hook (eww-mode . visual-line-mode)
  :bind (("\e\e w" . eww)
         :map eww-mode-map
         ("o" . eww-browse-with-external-browser)
         ("D" . eww-forward-url)
         ("S" . eww-back-url)
         ("f" . link-hint-open-link)
         ("TAB" . shr-next-link)
         ("<backtab>" . shr-previous-link)
         ("j" . scroll-up-line)
         ("k" . scroll-down-line))

  :config
  (setq eww-download-directory (expand-file-name "~/begin_new_life/emacs/.eww"))
  (setq eww-form-checkbox-symbol "☐")
  (setq eww-form-checkbox-selected-symbol "☑"))
```


## <span class="org-todo todo TODO">TODO</span> EAF 配置 :tangle ~/emacs-babel.el {#eaf-配置-tangle-emacs-babel-dot-el}

[EAF (Emacs Application Framework)](https://github.com/emacs-eaf/emacs-application-framework) 配置。

```emacs-lisp
;;Check the full list of configurable variables with C-h v eaf-var-list
(add-to-list 'load-path "~/.emacs.d/emacs-application-framework/")
(require 'eaf)
(setq frame-title-format "Emacs")
(setq eaf-frame-title-format "Emacs")
(use-package eaf
  ;;:ensure nil
  :load-path "~/.emacs.d/emacs-application-framework/"
  :custom
  ;; See https://github.com/emacs-eaf/emacs-application-framework/wiki/Customization
  (eaf-browser-continue-where-left-off t)
  (eaf-browser-enable-adblocker t)
  ;;(setq browse-url-browser-function 'eaf-open-browser)
  ;;(browse-url-browser-function 'my-browse-url)
  :config
  (require 'eaf-2048)
  (require 'eaf-airshare)
  (require 'eaf-browser)
  (require 'eaf-camera)
  (require 'eaf-demo)
  ;;(require 'eaf-file-browser)
  (require 'eaf-file-sender)
  (require 'eaf-git)
  (require 'eaf-image-viewer)
  (require 'eaf-js-video-player)
  (require 'eaf-jupyter)
  (require 'eaf-map)
  (require 'eaf-music-player)
  (require 'eaf-org-previewer)
  (require 'eaf-pdf-viewer)
  (require 'eaf-pyqterminal)
  (require 'eaf-rss-reader)
  (require 'eaf-system-monitor)
  (require 'eaf-video-player)
  (require 'eaf-vue-demo)
  (require 'eaf-vue-tailwindcss)
  (require 'eaf-evil)
  ;;(require 'eaf-file-manager)
  ;;(require 'eaf-netease-cloud-music)
  ;;(require 'eaf-terminal)

  (defalias 'browse-web #'eaf-open-browser)

  (setq frame-title-format "Emacs")
  (setq eaf-browser-remember-history nil)
  (setq eaf-browse-blank-page-url "https://duckduckgo.com")
  (setq eaf-webengine-default-zoom 1.75) ;; (setq eaf-webengine-default-zoom "2")
  (setq eaf-webengine-font-size 20)
  (setq eaf-webengine-fixed-font-size 20)

  (eaf-bind-key scroll_up "C-n" eaf-pdf-viewer-keybinding)
  (eaf-bind-key scroll_down "C-p" eaf-pdf-viewer-keybinding)
  (eaf-bind-key take_photo "p" eaf-camera-keybinding)
  (eaf-bind-key nil "M-q" eaf-browser-keybinding)
  (setq eaf-browser-enable-plugin t)
  (setq eaf-browser-enable-javascript t)
  (setq eaf-mindmap-dark-mode "follow") ; default option
  (setq eaf-browser-dark-mode "force")
  (setq eaf-terminal-dark-mode nil)
  (setq eaf-browser-continue-where-left-off t)
  (setq eaf-pdf-dark-mode "ignore")
  (setq eaf-webengine-pc-user-agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0"))


;;In EAF PDF Viewer, since you can interactively toggle inverted mode using i , it has an additional option to be more flexible:
;;When eaf-pdf-dark-mode is set to ignore, opening a PDF file for the first time will follow the current Emacs theme. Toggle inverted mode however you want, closing the buffer will record the final state of the dark mode. Next time the PDF file opens, it will restore the previous state. Therefore eaf-pdf-dark-mode will differ per file

;;(require 'eaf-terminal) ;;(require 'eaf-file-manager) ;;(require 'eaf-netease-cloud-music)
;;
(when (memq window-system '(mac ns x)) (exec-path-from-shell-initialize))
(setq exec-path (append exec-path '("~/AppData/Roaming/Python/Python310/Scripts/")))
(setq exec-path (append exec-path '("~/AppData/Roaming/Python/Python311/Scripts/")))
(setq exec-path (append exec-path '("~/AppData/Roaming/Python/Python39/Scripts/")))
(setq exec-path (append exec-path '("~/AppData/Roaming/Python/Python37/Scripts/")))
;;(setq exec-path (append exec-path '("~/AppData/Roaming/Python/Python310/Scripts/")))
;;(setq python-shell-interpreter "~/scoop/apps/python310/current/python.exe")
;;(setenv "PYTHONPATH" "C:/path/to/your/python/modules")
;; unbind, see more in the Wiki
;; Require it
```


## RSS {#rss}


### <span class="org-todo todo TODO">TODO</span> ELFEED Emacs 的 RSS 新闻阅读设置 {#elfeed-emacs-的-rss-新闻阅读设置}

[elfeed](https://github.com/skeeto/elfeed) 插件是一个非常棒的 RSS 新闻阅读客户端。
 :tangle ~/emacs-babel.el

```emacs-lisp
(use-package elfeed
  :ensure t
  :hook ((elfeed-new-entry . (lambda () (elfeed-make-tagger :feed-url "video" :add '(video))
                               (elfeed-make-tagger :entry-title "图卦" :add '(pic)))))
  :bind (("\e\e n" . elfeed)
         :map elfeed-search-mode-map
         ("g" . elfeed-update)
         ("G" . elfeed-search-update--force)
         ("o" . elfeed-default-browser-open)
         :map elfeed-show-mode-map
         ("M-v" . scroll-down-command)
         ("j" . scroll-up-line)
         ("k" . scroll-down-line))
  :config
  (setq elfeed-db-directory "~/.elfeed")
  ;; capture template for elfeed
  (with-eval-after-load 'org-capture
    (add-to-list 'org-capture-templates '("r" "Elfeed RSS" entry (file+headline "capture.org" "Elfeed")
                                          "* %:elfeed-entry-title :READ:\n%?\n%a"
                                          :empty-lines-after 1
                                          :prepend t))
    (add-to-list 'org-capture-templates-contexts '("r" ((in-mode . "elfeed-show-mode")
                                                        (in-mode . "elfeed-search-mode")))))
  ;; ================================
  ;; open entry with browser
  ;; ================================
  (defun elfeed-default-browser-open (&optional use-generic-p)
    "open with default browser"
    (interactive "P")
    (let ((entries (elfeed-search-selected)))
      (cl-loop for entry in entries
               do (elfeed-untag entry 'unread)
               when (elfeed-entry-link entry)
               do (browse-url it))
      (mapc #'elfeed-search-update-entry entries)
      (unless (use-region-p) (forward-line))))
  :custom
  (elfeed-feeds '(
                  ("https://planet.emacslife.com/atom.xml" emacs)
                  ("http://www.dapenti.com/blog/rss2.asp?name=xilei" news)
                  ("https://remacs.cc/index.xml" emacs product)
                  ))
  (elfeed-use-curl t)
  (elfeed-curl-max-connections 10)
  (elfeed-enclosure-default-dir "~/Downloads/")
  (elfeed-search-filter "@4-months-ago +")
  (elfeed-sort-order 'descending)
  (elfeed-search-clipboard-type 'CLIPBOARD)
  (elfeed-search-title-max-width 100)
  (elfeed-search-title-min-width 30)
  (elfeed-search-trailing-width 25)
  (elfeed-show-truncate-long-urls t)
  (elfeed-show-unique-buffers t)
  (elfeed-search-date-format '("%F %R" 16 :left))
  )
```


### ELFFED :tangle ~/emacs-babel.el {#elffed-tangle-emacs-babel-dot-el}

```emacs-lisp
(require 'elfeed)
(setq elfeed-db-directory "~/begin_new_life/emacs/.elfeed")
(setq elfeed-feeds '(
                     ("https://planet.emacslife.com/atom.xml" emacs)
                     ("http://www.dapenti.com/blog/rss2.asp?name=xilei" news)
                     ("https://remacs.cc/index.xml" emacs product)))
(setq elfeed-db-directory "~/begin_new_life/emacs/.elfeed")
(use-package elfeed-org
  :ensure t
  :config
  (elfeed-org)
  (setq rmh-elfeed-org-files (list "~/begin_new_life/rss/elfeed.org")))
;; (require 'elfeed-org)
;;(elfeed-org)
;;(setq rmh-elfeed-org-files "C:/Users/wacl/begin_new_life/rss/elfeed.org")
```


### <span class="org-todo todo TODO">TODO</span> ELFEED-GOODIES 给 elfeed 优化增强 :tangle ~/emacs-babel.el {#elfeed-goodies-给-elfeed-优化增强-tangle-emacs-babel-dot-el}

我们通过 [elfeed-goodies](https://github.com/jeetelongname/elfeed-goodies) 插件给 elfeed 进行优化增强：

```emacs-lisp
(use-package elfeed-goodies
  :ensure t
  :hook (after-init . elfeed-goodies/setup)
  :config
  ;; set elfeed show entry switch function
  (setq elfeed-show-0ntry-switch #'elfeed-goodies/switch-pane)) ; switch-to-buffer, pop-to-buffer
```


### <span class="org-todo todo TODO">TODO</span> RSS {#rss}

tangle ~/emacs-babel.el

```emacs-lisp
  (defun bjm/elfeed-load-db-and-open ()
    "Warpper to load the elfeed db from disk before opening"
    (interactive)
    (elfeed-db-load)
    (elfeed)
    (elfeed-search-update--force))
  ;; write to disk when quiting
  (defun bjm/elfeed-save-db-and-bury ()
    "Wrapper to save the elfeed db to disk before burying buffer"
    (interactive)
    (elfeed-db-save)
    (quit-window))


  ;;(defalias 'elfeed-toggle-star
  ;;(elfeed-expose #'elfeed-search-toggle-all 'star))

  (use-package elfeed
    :ensure t
    :hook ((elfeed-new-entry . (lambda () (elfeed-make-tagger :feed-url "video" :add '(video))
                                 (elfeed-make-tagger :entry-title "图卦" :add '(pic)))))
    :bind (("\e\e n" . elfeed)
           :map elfeed-search-mode-map
           ("g" . elfeed-update)
           ("G" . elfeed-search-update--force)
           ("o" . elfeed-default-browser-open)
           ("q" . bjm/elfeed-save-db-and-bury)
           ("Q" . bjm/elfeed-save-db-and-bury)
           ("m" . elfeed-toggle-star)
           ("M" . elfeed-toggle-star)
           :map elfeed-show-mode-map
           ("M-v" . scroll-down-command)
           ("j" . scroll-up-line)
           ("k" . scroll-down-line))
    :config
    (setq elfeed-db-directory "~/begin_new_life/emacs/.elfeed")
    ;; capture template for elfeed
    (with-eval-after-load 'org-capture
      (add-to-list 'org-capture-templates '("r" "Elfeed RSS" entry (file+headline "capture.org" "Elfeed")
                                            "* %:elfeed-entry-title :READ:\n%?\n%a"
                                            :empty-lines-after 1
                                            :prepend t))
      (add-to-list 'org-capture-templates-contexts '("r" ((in-mode . "elfeed-show-mode")
                                                          (in-mode . "elfeed-search-mode")))))
    ;; ================================
    ;; open entry with browser
    ;; ================================
    (defun elfeed-default-browser-open (&optional use-generic-p)
      "open with default browser"
      (interactive "P")
      (let ((entries (elfeed-search-selected)))
        (cl-loop for entry in entries
                 do (elfeed-untag entry 'unread)
                 when (elfeed-entry-link entry)
                 do (browse-url it))
        (mapc #'elfeed-search-update-entry entries)
        (unless (use-region-p) (forward-line))))
    :custom
    (elfeed-feeds '(
                    ("https://planet.emacslife.com/atom.xml" emacs)
                    ("http://www.dapenti.com/blog/rss2.asp?name=xilei" news)
                    ("https://remacs.cc/index.xml" emacs product)
                    ))
    (elfeed-use-curl t)
    (elfeed-curl-max-connections 10)
    (elfeed-enclosure-default-dir "~/Downloads/")
    (elfeed-search-filter "@4-months-ago +")
    (elfeed-sort-order 'descending)
    (elfeed-search-clipboard-type 'CLIPBOARD)
    (elfeed-search-title-max-width 100)
    (elfeed-search-title-min-width 30)
    (elfeed-search-trailing-width 25)
    (elfeed-show-truncate-long-urls t)
    (elfeed-show-unique-buffers t)
    (elfeed-search-date-format '("%F %R" 16 :left))
    )

  (use-package elfeed-goodies
    :ensure t
    :hook (after-init . elfeed-goodies/setup) ;; :config (elfeed-goodies/setup)
    :config
    ;; set elfeed show entry switch function
    (setq elfeed-show-entry-switch #'elfeed-goodies/switch-pane) ; switch-to-buffer, pop-to-buffer
    )
  ;;(use-package elfeed-goodies
  ;;:ensure t
  ;;:config
  ;;(elfeed-goodies/setup))


  ;;part2
  ;;This is a package for GNU Emacs that can be used to tie related commands into a family of short bindings with a common prefix - a Hydra.
  (use-package hydra
    :ensure t
;;    :init
)
    ;; Hydra for modes that toggle on and off
    (global-set-key
     (kbd "C-x t")
     (defhydra toggle (:color blue)
       "toggle"
       ("a" abbrev-mode "abbrev")
       ("s" flyspell-mode "flyspell")
       ("d" toggle-debug-on-error "debug")
       ("c" fci-mode "fCi")
       ("f" auto-fill-mode "fill")
      ;; ("t" toggle-truncate-lines "truncate")  no for c emacs
       ("w" whitespace-mode "whitespace")
       ("q" nil "cancel")))

    ;; Hydra for navigation
    (global-set-key
     (kbd "C-x j")
     (defhydra gotoline
       ( :pre (linum-mode 1)
         :post (linum-mode -1))
       "goto"
       ("t" (lambda () (interactive)(move-to-window-line-top-bottom 0)) "top")
       ("b" (lambda () (interactive)(move-to-window-line-top-bottom -1)) "bottom")
       ("m" (lambda () (interactive)(move-to-window-line-top-bottom)) "middle")
       ("e" (lambda () (interactive)(end-of-buffer)) "end")
       ("c" recenter-top-bottom "recenter")
       ("n" next-line "down")
       ("p" (lambda () (interactive) (forward-line -1))  "up")
       ("g" goto-line "goto-line")
       ))

    ;; Hydra for some org-mode stuff
    (global-set-key
     (kbd "C-c t")
     (defhydra hydra-global-org (:color blue)
       "Org"
       ("t" org-timer-start "Start Timer")
       ("s" org-timer-stop "Stop Timer")
       ("r" org-timer-set-timer "Set Timer") ; This one requires you be in an orgmode doc, as it sets the timer for the header
       ("p" org-timer "Print Timer") ; output timer value to buffer
       ("w" (org-clock-in '(4)) "Clock-In") ; used with (org-clock-persistence-insinuate) (setq org-clock-persist t)
       ("o" org-clock-out "Clock-Out") ; you might also want (setq org-log-note-clock-out t)
       ("j" org-clock-goto "Clock Goto") ; global visit the clocked task
       ("c" org-capture "Capture") ; Don't forget to define the captures you want http://orgmode.org/manual/Capture.html
       ("l" (or )rg-capture-goto-last-stored "Last Capture"))
     )
  ;;part2_1
  (defhydra mz/hydra-elfeed ()
    "filter"
    ("c" (elfeed-search-set-filter "@6-months-ago +cs") "cs")
    ("b" (elfeed-search-set-filter "@6-months-ago +blogs") "blog")
    ("e" (elfeed-search-set-filter "@6-months-ago +emacs") "emacs")
    ("d" (elfeed-search-set-filter "@6-months-ago +education") "education")
    ("*" (elfeed-search-set-filter "@6-months-ago +star") "Starred")
    ("M" elfeed-toggle-star "Mark")
    ("A" (elfeed-search-set-filter "@6-months-ago") "All")
    ("T" (elfeed-search-set-filter "@1-day-ago") "Today")
    ("Q" bjm/elfeed-save-db-and-bury "Quit Elfeed" :color blue)
    ("q" nil "quit" :color blue)
    )


  (defun mz/make-and-run-elfeed-hydra ()
    "elfeed-search-mode-map J"
    (interactive)
                                          ;(mz/make-elfeed-hydra)
    (mz/hydra-elfeed/body))
  ;;part2
  (use-package elfeed
    :ensure t
    :bind (:map elfeed-search-mode-map
                ("q" . bjm/elfeed-save-db-and-bury)
                ("Q" . bjm/elfeed-save-db-and-bury)
                ("m" . elfeed-toggle-star)
                ("M" . elfeed-toggle-star)
                ("j" . mz/hydra-elfeed/body)
                ("J" . mz/hydra-elfeed/body)))

  ;; part1
  (setq elfeed-db-directory "~/Dropbox/shared/elfeeddb")


  (defun elfeed-mark-all-as-read ()
    (interactive)
    (mark-whole-buffer)
    (elfeed-search-untag-all-unread))


  ;;functions to support syncing .elfeed between machines
  ;;makes sure elfeed reads index from disk before launching
  (defun bjm/elfeed-load-db-and-open ()
    "Wrapper to load the elfeed db from disk before opening"
    (interactive)
    (elfeed-db-load)
    (elfeed)
    (elfeed-search-update--force))

  ;;write to disk when quiting
  (defun bjm/elfeed-save-db-and-bury ()
    "Wrapper to save the elfeed db to disk before burying buffer"
    (interactive)
    (elfeed-db-save)
    (quit-window))


  (use-package elfeed
    :ensure t
    :bind (:map elfeed-search-mode-map
                ("q" . bjm/elfeed-save-db-and-bury)
                ("Q" . bjm/elfeed-save-db-and-bury)
                ("m" . elfeed-toggle-star)
                ("M" . elfeed-toggle-star)
                )
    )


```


## <span class="org-todo todo WAIT">WAIT</span> LILYPOND 乐谱绘图 {#lilypond-乐谱绘图}

通过 `lilypond` 乐谱画图，需要提前安装 `lilypond` 和 `mactex-no-gui` 。

```emacs-lisp
(use-package lilypond-mode
  :ensure nil
  :mode ("\\.i?ly\\'" . LilyPond-mode)
  :init
  (add-to-list 'org-src-lang-modes '("lilypond" . lilypond))
  ;; add support for org babel
  ;;(org-babel-do-load-languages 'org-babel-load-languages
                               ;;(append org-babel-load-languages
                                       ;;'((lilypond . t))))
  ;; set lilypond binary directory
  (setq org-babel-lilypond-ly-command "/usr/local/bin/lilypond -dpreview")
  :config
  ;; ;; trim extra space for generated image
  ;; (defun my/trim-lilypond-png (orig-fun
  ;;                              &optional arg
  ;;                              info
  ;;                              param)
  ;;   (when (member (car (org-babel-get-src-block-info)) '("lilypond"))
  ;;     (let ((ly-file (alist-get :file (nth 2 (org-babel-get-src-block-info)))))
  ;;       (let ((ly-preview-file (replace-regexp-in-string "\\.png" ".preview.png" ly-file)))
  ;;         (when (file-exists-p ly-preview-file)
  ;;           (shell-command (concat "mv " ly-preview-file " " ly-file)))
  ;;         (org-redisplay-inline-images)))))
  ;; (advice-add 'org-babel-execute-src-block :after #'my/trim-lilypond-png)
  (setq ob-lilypond-header-args
        '((:results . "file replace")
          (:exports . "results")
          ))
  )
```


## <span class="org-todo todo WAIT">WAIT</span> COMPANY-BOX {#company-box}

```emacs-lisp
(use-package company-box
  :hook (company-mode . company-box-mode))
(require 'company-box)
(add-hook 'company-mode-hook 'company-box-mode)
```


## <span class="org-todo todo WAIT">WAIT</span> PNGPASTE {#pngpaste}

图片粘贴
通过 `pngpaste` 这个命令行工具，将系统剪贴板里的图片，输出到当前文件同名的 `assets` 文件夹下，然后自动在当前 org 文件的光标处插入图片链接，并设置图片链接的宽度属性。

```emacs-lisp
(use-package emacs
  :ensure nil
  :after org
  :bind (:map org-mode-map
              ("s-V" . my/org-insert-clipboard-image))
  :config
  (defun my/org-insert-clipboard-image (width)
    "create a time stamped unique-named file from the clipboard in the sub-directory
 (%filename.assets) as the org-buffer and insert a link to this file."
    (interactive (list
                  (read-string (format "Input image width, default is 800: ")
                               nil nil "800")))
    ;; 设置图片存放的文件夹位置为 `当前Org文件同名.assets'
    (setq foldername (concat (file-name-base (buffer-file-name)) ".assets/"))
    (if (not (file-exists-p foldername))
        (mkdir foldername))
    ;; 设置图片的文件名，格式为 `img_年月日_时分秒.png'
    (setq imgName (concat "img_" (format-time-string "%Y%m%d_%H%M%S") ".png"))
    ;; 图片文件的相对路径
    (setq relativeFilename (concat (file-name-base (buffer-name)) ".assets/" imgName))
    ;; 根据不同的操作系统设置不同的命令行工具
    (cond ((string-equal system-type "gnu/linux")
           (shell-command (concat "xclip -selection clipboard -t image/png -o > " relativeFilename)))
          ((string-equal system-type "darwin")
           (shell-command (concat "pngpaste " relativeFilename))))
    ;; 给粘贴好的图片链接加上宽度属性，方便导出
    (insert (concat "\n#+DOWNLOADED: screenshot @ "
                    (format-time-string "%Y-%m-%d %a %H:%M:%S" (current-time))
                    "\n#+CAPTION: \n#+ATTR_ORG: :width "
                    width
                    "\n#+ATTR_LATEX: :width "
                    (if (>= (/ (string-to-number width) 800.0) 1.0)
                        "1.0"
                      (number-to-string (/ (string-to-number width) 800.0)))
                    "\\linewidth :float nil\n"
                    "#+ATTR_HTML: :width "
                    width
                    "\n[[file:" relativeFilename "]]\n"))
    ;; 重新显示一下图片
    (org-redisplay-inline-images)))
```


## <span class="org-todo todo WAIT">WAIT</span> HUGRY-DELETE {#hugry-delete}

```elisp
(use-package hungry-delete
  :ensure t
  :config
  (require 'hungry-delete)
  (global-hungry-delete-mode))
```


## <span class="org-todo todo WAIT">WAIT</span> NOTIFICATIONS tangle ~/emacs-babel.el 通知管理 {#notifications-tangle-emacs-babel-dot-el-通知管理}

需要先通过 `brew install terminal-notifier` 命令安装 [terminal-notifier](https://github.com/julienXX/terminal-notifier) 。

```emacs-lisp
(use-package notifications
  :ensure nil
  :commands notify-send
  :config
  (cond ((eq system-type 'darwin)
         (defun notify-send (&rest params)
           "Send notifications via `terminal-notifier'."
           (let ((title (plist-get params :title))
                 (body (plist-get params :body)))
             (start-process "terminal-notifier"
                            nil
                            "terminal-notifier"
                            "-group" "Emacs"
                            "-title" title
                            "-message" body
                            "-activate" "org.gnu.Emacs"))))
        (t
         (defalias 'notify-send 'notifications-notify))))
```


## <span class="org-todo todo WAIT">WAIT</span> COPILOT-COMPLETION {#copilot-completion}

```emacs-lisp
;;company
;; you can utilize :map :hook and :config to customize copilot
;;(require 'copilot)
;;2. Configure completion
;;Option 1: Use copilot-mode to automatically provide completions
(add-hook 'prog-mode-hook 'copilot-mode)
;;To customize the behavior of copilot-mode, please check copilot-enable-predicates and copilot-disable-predicates.
;;3. Configure completion acceptation
;;In general, you need to bind copilot-accept-completion to some key in order to accept the completion. Also, you may find copilot-accept-completion-by-word is useful.
;;Example of using tab with company-mode
(with-eval-after-load 'company
  ;; disable inline previews
  (delq 'company-preview-if-just-one-frontend company-frontends))
;;Example of defining tab in copilot-mode
;;This is useful if you don't want to depend on a particular completion framework.

;;(defun my/copilot-tab ()
;;  (interactive)
;;  (or (copilot-accept-completion)
;;      (indent-for-tab-command)))

;;(with-eval-after-load 'copilot
;;  (define-key copilot-mode-map (kbd "<tab>") #'my/copilot-tab))
;;Or with evil-mode:
;;(setq tls-checktrust nil)
;;(setq gnutls-verify-error nil)

;;(with-eval-after-load 'copilot
;;  evil-define-key 'insert copilot-mode-map
;;    (kbd "<tab>") #'my/copilot-tab))

```


## <span class="org-todo todo WAIT">WAIT</span> NETEASE-CLOUD-MUSIC :tangle ~/emacs-babel.el {#netease-cloud-music-tangle-emacs-babel-dot-el}

```emacs-lisp
(use-package netease-cloud-music
  :ensure t
  :after async
  :config
  (require 'netease-cloud-music)
  (require 'netease-cloud-music-ui)       ;If you want to use the default TUI, you should add this line in your configuration.
  (require 'netease-cloud-music-comment)  ;If you want comment feature：评论功能需要额外安装 async 包
  )
```


## JS :tangle ~/.emacs.d/emacs-babel-js.el {#js-tangle-dot-emacs-dot-d-emacs-babel-js-dot-el}


### <span class="org-todo todo TODO">TODO</span> SKEWER-MODE {#skewer-mode}

Dependencies:

-   [simple-httpd](<https://github.com/skeeto/emacs-http-server>) (available on MELPA)
-   [js2-mode](<https://github.com/mooz/js2-mode>) (available on ELPA)

<https://github.com/search?q=skewer-mode>

```emacs-lisp
(use-package skewer-mode
  :ensure t
  :config
   (require 'skewer-mode))
```


### JS-COMINT {#js-comint}

<https://github.com/search?q=js-comint>

```emacs-lisp
(use-package js-comint
  :ensure t
  :config
   (require 'js-comint))
;(setq js-comint-program-command "C:/Program Files/nodejs/node.exe")
```


### INDIUM {#indium}

```emacs-lisp
(use-package indium
  :ensure t
  :config
  ;;(require 'indium)
  (add-hook 'js-mode-hook #'indium-interaction-mode))
```


## plot :tanlge ~/.emacs.d/emacs-babel-plot.el {#plot-tanlge-dot-emacs-dot-d-emacs-babel-plot-dot-el}


### GNUPLOT 绘图 {#gnuplot-绘图}

[gnuplot](https://github.com/emacs-gnuplot/gnuplot) 插件可以让 Emacs 通过 gnuplot 绘图。

```emacs-lisp
(use-package gnuplot
  :ensure t
  :mode ("\\.gp$" . gnuplot-mode)
  :init
  (add-to-list 'org-src-lang-modes '("gnuplot" . gnuplot))
  ;;(org-babel-do-load-languages 'org-babel-load-languages
  ;;(append org-babel-load-languages
  ;;'((gnuplot . t))))
  :config
  ;; (add-to-list 'auto-mode-alist '("\\.gp$" . gnuplot-mode))
  (setq org-babel-default-header-args:gnuplot
        '((:exports . "results")
          (:results . "file"))))
```


### <span class="org-todo todo TODO">TODO</span> PLANTUML-MODE 绘图 {#plantuml-mode-绘图}

[plantuml](https://plantuml.com/zh/) 可以让我们在 Org mode 里通过纯文本画各种图，具体参考：[PlantUML integration with Emacs](https://plantuml.com/zh/emacs)。

需要提前通过 `brew install plantuml` 安装 `plantuml` 。

```emacs-lisp
(use-package plantuml-mode
  :ensure t
  :mode ("\\.plantuml\\'" . plantuml-mode)
  :init
  ;; enable plantuml babel support
  (add-to-list 'org-src-lang-modes '("plantuml" . plantuml))
  ;;(org-babel-do-load-languages 'org-babel-load-languages
                               ;;(append org-babel-load-languages
                                       ;;'((plantuml . t))))
  :config
  (setq org-plantuml-exec-mode 'plantuml)
  (setq org-plantuml-executable-path "plantuml")
  (setq plantuml-executable-path "plantuml")
  (setq plantuml-default-exec-mode 'executable)
  ;; set default babel header arguments
  (setq org-babel-default-header-args:plantuml
        '((:exports . "results")
          (:results . "file"))))
```


## ox-export ~/.emacs.d/emacs-babel-ox-export.el {#ox-export-dot-emacs-dot-d-emacs-babel-ox-export-dot-el}


### OX 文件导出通用设置 {#ox-文件导出通用设置}

下面是 org 文件导出的通用设置。

```emacs-lisp
(use-package ox
  :ensure nil
  :custom
  (org-export-with-toc t)
  (org-export-with-tags 'not-in-toc)
  (org-export-with-drawers nil)
  (org-export-with-priority t)
  (org-export-with-footnotes t)
  (org-export-with-smart-quotes t)
  (org-export-with-section-numbers t)
  (org-export-with-sub-superscripts '{})
  ;; `org-export-use-babel' set to nil will cause all source block header arguments to be ignored This means that code blocks with the argument :exports none or :exports results will end up in the export.
  ;; See:
  ;; https://stackoverflow.com/questions/29952543/how-do-i-prevent-org-mode-from-executing-all-of-the-babel-source-blocks
  (org-export-use-babel t)
  (org-export-headline-levels 9)
  (org-export-coding-system 'utf-8)
  (org-export-with-broken-links 'mark)
  (org-export-default-language "zh-CN")) ; 默认是en
  ;; (org-ascii-text-width 72)
```


### OX-EXTRA {#ox-extra}

```emacs-lisp
;; export extra
(use-package ox-extra
  :ensure nil
  :config
  (ox-extras-activate '(ignore-headlines)))
```


### OX-HTML org 导出后端设置 {#ox-html-org-导出后端设置}

导出 HTML 设置
我们先来对 HTML 导出做一个基本设置：

```emacs-lisp
(use-package ox-html
  :ensure nil
  :init
  ;; add support for video
  (defun org-video-link-export (path desc backend)
    (let ((ext (file-name-extension path)))
      (cond
       ((eq 'html backend)
        (format "<video width='800' preload='metadata' controls='controls'><source type='video/%s' src='%s' /></video>" ext path))
       ;; fall-through case for everything else
       (t
        path))))
  (org-link-set-parameters "video" :export 'org-video-link-export)
  :custom
  (org-html-doctype "html5")
  (org-html-html5-fancy t)
  (org-html-checkbox-type 'unicode)
  (org-html-validation-link nil))
```


### HTMLIZE {#htmlize}

```emacs-lisp
(use-package htmlize
  :ensure t
  :custom
  (htmlize-pre-style t)
  (htmlize-output-type 'inline-css))
```


### OX-LATEX 导出 PDF 设置 {#ox-latex-导出-pdf-设置}

`ox-latex` 是 Org mode 自带的功能，可以将 Org 文件导出为 latex 文件和 PDF 文件。

```emacs-lisp
(use-package ox-latex
  :after ox
  :ensure nil
  :defer t
  :config
  (add-to-list 'org-latex-classes
               '("cn-article"
                 "\\documentclass[UTF8,a4paper]{article}"
                 ("\\section{%s}" . "\\section*{%s}")
                 ("\\subsection{%s}" . "\\subsection*{%s}")
                 ("\\subsubsection{%s}" . "\\subsubsection*{%s}")
                 ("\\paragraph{%s}" . "\\paragraph*{%s}")
                 ("\\subparagraph{%s}" . "\\subparagraph*{%s}")))

  (add-to-list 'org-latex-classes
               '("cn-report"
                 "\\documentclass[11pt,a4paper]{report}"
                 ("\\chapter{%s}" . "\\chapter*{%s}")
                 ("\\section{%s}" . "\\section*{%s}")
                 ("\\subsection{%s}" . "\\subsection*{%s}")
                 ("\\subsubsection{%s}" . "\\subsubsection*{%s}")))
  (setq org-latex-default-class "cn-article")
  (setq org-latex-image-default-height "0.9\\textheight"
        org-latex-image-default-width "\\linewidth")
  (setq org-latex-pdf-process
    '("xelatex -interaction nonstopmode -output-directory %o %f"
      "bibtex %b"
      "xelatex -interaction nonstopmode -output-directory %o %f"
      "xelatex -interaction nonstopmode -output-directory %o %f"
      "rm -fr %b.out %b.log %b.tex %b.brf %b.bbl auto"
      ))
  ;; 使用 Listings 宏包格式化源代码(只是把代码框用 listing 环境框起来，还需要额外的设置)
  (setq org-latex-listings t)
  ;; mapping jupyter-python to Python
  (add-to-list 'org-latex-listings-langs '(jupyter-python "Python"))
  ;; Options for \lset command（reference to listing Manual)
  (setq org-latex-listings-options
        '(
          ("basicstyle" "\\small\\ttfamily")       ; 源代码字体样式
          ("keywordstyle" "\\color{eminence}\\small")                 ; 关键词字体样式
          ;; ("identifierstyle" "\\color{doc}\\small")
          ("commentstyle" "\\color{commentgreen}\\small\\itshape")    ; 批注样式
          ("stringstyle" "\\color{red}\\small")                       ; 字符串样式
          ("showstringspaces" "false")                                ; 字符串空格显示
          ("numbers" "left")                                          ; 行号显示
          ("numberstyle" "\\color{preprocess}")                       ; 行号样式
          ("stepnumber" "1")                                          ; 行号递增
          ("xleftmargin" "2em")                                       ;
          ;; ("backgroundcolor" "\\color{background}")                   ; 代码框背景色
          ("tabsize" "4")                                             ; TAB 等效空格数
          ("captionpos" "t")                                          ; 标题位置 top or buttom(t|b)
          ("breaklines" "true")                                       ; 自动断行
          ("breakatwhitespace" "true")                                ; 只在空格分行
          ("showspaces" "false")                                      ; 显示空格
          ("columns" "flexible")                                      ; 列样式
          ("frame" "tb")                                              ; 代码框：single, or tb 上下线
          ("frameleftmargin" "1.5em")                                 ; frame 向右偏移
          ;; ("frameround" "tttt")                                       ; 代码框： 圆角
          ;; ("framesep" "0pt")
          ;; ("framerule" "1pt")                                         ; 框的线宽
          ;; ("rulecolor" "\\color{background}")                         ; 框颜色
          ;; ("fillcolor" "\\color{white}")
          ;; ("rulesepcolor" "\\color{comdil}")
          ("framexleftmargin" "5mm")                                  ; let line numer inside frame
          ))
  )
```


### OX-REVEAL 导出幻灯片设置 {#ox-reveal-导出幻灯片设置}

我们可以 [ox-reveal](https://github.com/hexmode/ox-reveal) 插件，将 org 文件导出为漂亮的幻灯片。需要提前 [安装reveal.js](https://revealjs.com/installation/)：

```emacs-lisp
(use-package ox-reveal
  :ensure t
  :after ox
  :config
  (setq org-reveal-hlevel 1)
  ;; Avalable themes: night, black, white, league, beige, sky, serif, simple, solarized, blood, moon
  (setq org-reveal-theme "moon")
  ;; can also set root to a CDN cloud: https://cdn.jsdelivr.net/npm/reveal.js
  ;;(setq org-reveal-root (expand-file-name "reveal.js" user-emacs-directory))
  (setq org-reveal-root "https://cdn.jsdelivr.net/npm/reveal.js")
  (setq org-reveal-mathjax t)
  (setq org-reveal-ignore-speaker-notes t)
  ;; original title font size is TOO large!
  (setq org-reveal-title-slide "<h1><font size=\"8\">%t</font></h1><h2><font size=\"6\">%s</font></h2><p><font size=\"5\">%a</font><br/><font size=\"5\">%d</font></p>")
  ;; don't load highlight, use htmlize instead. If you want to add line-number, add -n in src block header
  ;;不要加载突出显示，而是使用 htmlize。如果要添加行号，请在 src 块头中添加 -n
  (setq org-reveal-plugins '(markdown zoom notes search))
  (setq org-reveal-klipsify-src 'on)
  (setq org-reveal-extra-css (expand-file-name "reveal.js/css/extra.css" user-emacs-directory)))

;;:ensure ox-reveal
;;#+REVEAL_ROOT: https://cdn.jsdelivr.net/npm/reveal.js
;;http://cdn.jsdelivr.net/reveal.js/3.0.0/
```


### OX-PANDOC 导出各种格式设置 {#ox-pandoc-导出各种格式设置}

[ox-pandoc](https://github.com/kawabata/ox-pandoc) 可以将 org 文件导出为各种格式的文件，需要提前安装 `brew install pandoc` 。

```emacs-lisp
(use-package ox-pandoc
  :ensure t)
;;:custom
;; special extensions for markdown_github output
;;(org-pandoc-format-extensions '(markdown_github+pipe_tables+raw_html))
;;(org-pandoc-command "/usr/local/bin/pandoc")
```


### OX-PUBLISH 导出静态站点设置 {#ox-publish-导出静态站点设置}

```emacs-lisp
(use-package ox-publish
  :ensure nil
  :commands (org-publish org-publish-all)
  :config
  (setq org-export-global-macros
        '(("timestamp" . "@@html:<span class=\"timestamp\">[$1]</span>@@")))

  ;; sitemap 生成函数
  (defun my/org-sitemap-date-entry-format (entry style project)
    "Format ENTRY in org-publish PROJECT Sitemap format ENTRY ENTRY STYLE format that includes date."
    (let ((filename (org-publish-find-title entry project)))
      (if (= (length filename) 0)
          (format "*%s*" entry)
        (format "{{{timestamp(%s)}}} [[file:%s][%s]]"
                (format-time-string "%Y-%m-%d"
                                    (org-publish-find-date entry project))
                entry
                filename))))

  ;; 设置 org-publish 的项目列表
  (setq org-publish-project-alist
        '(
          ;; 笔记部分
          ("org-notes"
           :base-directory "~/org/"
           :base-extension "org"
           :exclude "\\(tasks\\|test\\|scratch\\|diary\\|capture\\|mail\\|habits\\|resume\\|meetings\\|personal\\|org-beamer-example\\)\\.org\\|test\\|article\\|roam\\|hugo"
           :publishing-directory "~/public_html/"
           :recursive t                 ; include subdirectories if t
           :publishing-function org-html-publish-to-html
           :headline-levels 6
           :auto-preamble t
           :auto-sitemap t
           :sitemap-filename "sitemap.org"
           :sitemap-title "Sitemap"
           :sitemap-format-entry my/org-sitemap-date-entry-format)

          ;; 静态资源部分
          ("org-static"
           :base-directory "~/org/"
           :base-extension "css\\|js\\|png\\|jpg\\|gif\\|pdf\\|mp3\\|ogg\\|swf\\|mov"
           :publishing-directory "~/public_html/"
           :recursive t
           :publishing-function org-publish-attachment)

          ;; 项目集合
          ("org"
           :components ("org-notes" "org-static")))))
```


### OX-HUGO 导出博客设置 {#ox-hugo-导出博客设置}

[ox-hugo](https://github.com/kaushalmodi/ox-hugo) 插件可以将 org 文件导出为 [hugo](https://gohugo.io/) 需要的 Markdown 文件，并快速通过 hugo 进行博客的生成和发布。

```emacs-lisp
(use-package ox-hugo
  :ensure t
  :config
  (with-eval-after-load 'org-capture
    (defun org-hugo-new-subtree-post-capture-template ()
      "Returns `org-capture' template string for new Hugo post.
See `org-capture-templates' for more information."
      (let* ((title (read-from-minibuffer "Post Title: ")) ; Prompt to enter the post title
             (fname (org-hugo-slug title)))
        (mapconcat #'identity
                   `(
                     ,(concat "* TODO " title)
                     ":PROPERTIES:"
                     ,(concat ":EXPORT_FILE_NAME: " fname)
                     ":END:"
                     "%?\n")          ; Place the cursor here finally
                   "\n")))

    (add-to-list 'org-capture-templates
                 '("h"                ; `org-capture' binding + h
                   "Hugo post"
                   entry
                   ;; It is assumed that below file is present in `org-directory'
                   ;; and that it has a "Blog Ideas" heading. It can even be a
                   ;; symlink pointing to the actual location of capture.org!
                   (file+olp "capture.org" "Notes")
                   (function org-hugo-new-subtree-post-capture-template)))))
```


### <span class="org-todo todo WAIT">WAIT</span> OX-GFM 导出 Markdown 设置 {#ox-gfm-导出-markdown-设置}

我们通过 [ox-gfm](https://github.com/larstvei/ox-gfm) 插件来导出 Github 样式的 Markdown 文件。

```emacs-lisp
(use-package ox-gfm
  :ensure t
  :after ox)
```


## DIRED ~/.emacs.d/emacs-babel-dired.el {#dired-dot-emacs-dot-d-emacs-babel-dired-dot-el}


### DIRED 基础配置 Emacs 文件管理设置。 {#dired-基础配置-emacs-文件管理设置}

```emacs-lisp
(use-package dired
  :ensure nil
  :bind (:map dired-mode-map
              ("C-<return>" . xah-open-in-external-app)
              ("W" . dired-copy-path)
              )
  :config
  ;; Enable the disabled dired commands
  (put 'dired-find-alternate-file 'disabled nil)

  ;; open files via external program based on file types, See:
  ;; https://emacs.stackexchange.com/questions/3105/how-to-use-an-external-program-as-the-default-way-to-open-pdfs-from-emacs
  (defun xdg-open (filename)
    (interactive "fFilename: ")
    (let ((process-connection-type))
      (start-process "" nil (cond ((eq system-type 'gnu/linux) "xdg-open")
                                  ((eq system-type 'darwin) "open")
                                  ((eq system-type 'windows-nt) "start")
                                  (t "")) (expand-file-name filename))))
  ;; open files via external program when using find-file
  (defun find-file-auto (orig-fun &rest args)
    (let ((filename (car args)))
      (if (cl-find-if
           (lambda (regexp) (string-match regexp filename))
           '(
             ;; "\\.html?\\'"
             "\\.xlsx?\\'"
             "\\.pptx?\\'"
             "\\.docx?\\'"
             "\\.mp4\\'"
             "\\.app\\'"
             ))
          (xdg-open filename)
        (apply orig-fun args))))
  (advice-add 'find-file :around 'find-file-auto)

  (defun dired-copy-path ()
    "In dired, copy file path to kill-buffer.
At 2nd time it copy current directory to kill-buffer."
    (interactive)
    (let (path)
      (setq path (dired-file-name-at-point))
      (if (string= path (current-kill 0 1)) (setq path (dired-current-directory)))
      (message path)
      (kill-new path)))

  (defun xah-open-in-external-app (&optional @fname)
    "Open the current file or dired marked files in external app.
The app is chosen from your OS's preference.

When called in emacs lisp, if @fname is given, open that.

URL `http://ergoemacs.org/emacs/emacs_dired_open_file_in_ext_apps.html'
Version 2019-11-04"
    (interactive)
    (let* (
           ($file-list
            (if @fname
                (progn (list @fname))
              (if (or (string-equal major-mode "dired-mode")
                      (string-equal major-mode "dirvish-mode"))
                  (dired-get-marked-files)
                (list (buffer-file-name)))))
           ($do-it-p (if (<= (length $file-list) 5)
                         t
                       (y-or-n-p "Open more than 5 files? "))))
      (when $do-it-p
        (cond
         ((string-equal system-type "windows-nt")
          (mapc
           (lambda ($fpath)
             (w32-shell-execute "open" $fpath)) $file-list))
         ((string-equal system-type "darwin")
          (mapc
           (lambda ($fpath)
             (shell-command
              (concat "open " (shell-quote-argument $fpath))))  $file-list))
         ((string-equal system-type "gnu/linux")
          (mapc
           (lambda ($fpath) (let ((process-connection-type nil))
                              (start-process "" nil "xdg-open" $fpath))) $file-list))))))
  :custom
  ;; (dired-recursive-deletes 'always)
  (delete-by-moving-to-trash t)
  (dired-dwim-target t)
  (dired-bind-vm nil)
  (dired-bind-man nil)
  (dired-bind-info nil)
  (dired-auto-revert-buffer t)
  (dired-hide-details-hide-symlink-targets nil)
  (dired-kill-when-opening-new-dired-buffer t)
  (dired-listing-switches "-AFhlv"))
```


### DIRED-AUX {#dired-aux}

```emacs-lisp
(use-package dired-aux
  :after dired
  :ensure nil
  :bind (:map dired-mode-map
              ("C-c +" . dired-create-empty-file))
  :config
  ;; with the help of `evil-collection', P is bound to `dired-do-print'.
  (define-advice dired-do-print (:override (&optional _))
    "Show/hide dotfiles."
    (interactive)
    (if (or (not (boundp 'dired-dotfiles-show-p)) dired-dotfiles-show-p)
        (progn
          (setq-local dired-dotfiles-show-p nil)
          (dired-mark-files-regexp "^\\.")
          (dired-do-kill-lines))
      (revert-buffer)
      (setq-local dired-dotfiles-show-p t)))
  :custom
  (dired-isearch-filenames 'dwim)
  (dired-create-destination-dirs 'ask)
  (dired-vc-rename-file t))
```


### DIRED-X {#dired-x}

```emacs-lisp
(use-package dired-x
  :after dired
  :ensure nil
  :hook (dired-mode . dired-omit-mode)
  :init
  (setq dired-guess-shell-alist-user `((,(rx "."
                                             (or
                                              ;; Videos
                                              "mp4" "avi" "mkv" "flv" "ogv" "ogg" "mov"
                                              ;; Music
                                              "wav" "mp3" "flac"
                                              ;; Images
                                              "jpg" "jpeg" "png" "gif" "xpm" "svg" "bmp"
                                              ;; Docs
                                              "pdf" "md" "djvu" "ps" "eps" "doc" "docx" "xls" "xlsx" "ppt" "pptx")
                                             string-end)
                                        ,(cond ((eq system-type 'gnu/linux) "xdg-open")
                                               ((eq system-type 'darwin) "open")
                                               ((eq system-type 'windows-nt) "start")
                                               (t "")))))
  :custom
  (dired-omit-verbose nil)
  (dired-omit-files (rx string-start
                        (or ".DS_Store"
                            ".cache"
                            ".vscode"
                            ".ccls-cache" ".clangd")
                        string-end))
  ;; Dont prompt about killing buffer visiting delete file
  (dired-clean-confirm-killing-deleted-buffers nil))
```


### DIREDFL 多彩美化 {#diredfl-多彩美化}

默认的 Dired 只有两种颜色以区分文件和文件夹，我们可以使用 [diredfl](https://github.com/purcell/diredfl) 插件让 Dired 变得更加多彩一些：

```emacs-lisp
(use-package diredfl
  :ensure t
  :hook (dired-mode . diredfl-mode))
```


### DIRVISH 文件管理 [dirvish](https://github.com/alexluigit/dirvish) 是在 Dired 基础之上的文件管理增强插件。 {#dirvish-文件管理-dirvish-是在-dired-基础之上的文件管理增强插件}

需要安装 `poppler` 来预览 PDF；安装 `ffmpegthumbnailer` 来预览视频。

```emacs-lisp
(use-package dirvish
  :ensure t
  :hook (after-init . dirvish-override-dired-mode)
  :bind (:map dired-mode-map
         ("TAB" . dirvish-toggle-subtree)
         ("SPC" . dirvish-show-history)
         ("*"   . dirvish-mark-menu)
         ("r"   . dirvish-roam)
         ("b"   . dirvish-goto-bookmark)
         ("f"   . dirvish-file-info-menu)
         ("M-n" . dirvish-go-forward-history)
         ("M-p" . dirvish-go-backward-history)
         ("M-s" . dirvish-setup-menu)
         ("M-f" . dirvish-toggle-fullscreen)
         ([remap dired-sort-toggle-or-edit] . dirvish-quicksort)
         ([remap dired-do-redisplay] . dirvish-ls-switches-menu)
         ([remap dired-summary] . dirvish-dispatch)
         ([remap dired-do-copy] . dirvish-yank-menu)
         ([remap mode-line-other-buffer] . dirvish-other-buffer))
  :config
  (dirvish-peek-mode)
  (setq dirvish-hide-details t)
  ;; open mp4 file via external program which is mpv here.
  (add-to-list 'mailcap-mime-extensions '(".mp4" . "video/mp4"))
  (add-list-to-list 'dirvish-open-with-programs '(
                                                  (("html") . ("open" "%f"))
                                                  (("xlsx") . ("open" "%f"))
                                                  (("pptx") . ("open" "%f"))
                                                  (("docx") . ("open" "%f"))
                                                  (("md")   . ("open" "%f"))
                                                  ))
  :custom
  (dirvish-menu-bookmarks '(("h" "~/"             "Home")
                            ("d" "~/Downloads/"   "Downloads")
                            ("e" "~/.emacs.d.default/"    "Emacs")
                            ("o" "~/org/"         "org")
                            ("i" "~/iCloud/"      "iCloud")
                            ;; ("t" "~/.local/share/Trash/files/" "TrashCan")
                            ))
  (dirvish-mode-line-format '(:left
                              (sort file-time " " file-size symlink) ; it's ok to place string inside
                              :right
                              ;; For `dired-filter' users, replace `omit' with `filter' segment defined below
                              (omit yank index)))
  (dirvish-attributes '(subtree-state
                        file-size
                        vc-state
                        git-msg
                        file-time
                        ;; all-the-icons
                        ))
  )
```


### ALL-THE-ICONS-DIRED 图标美化 {#all-the-icons-dired-图标美化}

我们通过 [all-the-icons-dired](https://github.com/jtbm37/all-the-icons-dired) 插件给 Dired 添加好看的图标。

```emacs-lisp
(use-package all-the-icons-dired
  :ensure t
  :hook (dired-mode . all-the-icons-dired-mode))
```


## <span class="org-todo todo WAIT">WAIT</span> CONFUSED ORIGINAL TAB COPILOT {#confused-original-tab-copilot}

```emacs-lisp
;;(define-key copilot-completion-map (kbd "<tab>") 'copilot-accept-completion)
;;(define-key copilot-completion-map (kbd "TAB") 'copilot-accept-completion)
```

```emacs-lisp
(lambda ()
(when (and (org-in-src-block-p)
(not (org-between-regexps-p "^[ \t]*#\\+begin_" "^[ \t]*#\\+end_")))
(message "Mismatched source block at line %d" (line-number-at-pos))))
```

这个代码会在每个 \`#+begin_src\` 和 \`#+end_src\` 之间执行一个 Lambda 函数。当它发现一个没有匹配的 \`#+begin_src\` 或 \`#+end_src\` 时，它会在消息区域中显示一个错误消息，指出这个错误发生在哪一行。


## <span class="org-todo todo TODO">TODO</span> CORFU :tangle ~/emacs-babel.el {#corfu-tangle-emacs-babel-dot-el}

[corfu](https://github.com/minad/corfu) 通过弹窗进行补全。

```emacs-lisp
(use-package corfu
  :ensure t
  ;; 有了补全company
  ;;:hook (after-init . global-corfu-mode)
  :bind
  (:map corfu-map
        ("SPC" . corfu-insert-separator)    ; configure space for separator insertion
        ("M-q" . corfu-quick-complete)      ; use C-g to exit
        ("TAB" . corfu-next)
        ([tab] . corfu-next)
        ("S-TAB" . corfu-previous)
        ([backtab] . corfu-previous)
        ("C-j" . corfu-next)
        ("C-k" . corfu-previous)
        ("C-S-j" . corfu-insert)
        ;;:load-path "your-corfu-autoload-path"
        )
  :config
  ;; TAB cycle if there are only few candidates
  (setq completion-cycle-threshold 0)
  (setq tab-always-indent 'complete)

  (defun corfu-enable-always-in-minibuffer ()
    "Enable Corfu in the minibuffer if Vertico/Mct are not active."
    (unless (or (bound-and-true-p mct--active)
                (bound-and-true-p vertico--input))
      ;; (setq-local corfu-auto nil) Enable/disable auto completion
      (corfu-mode 1)))
  (add-hook 'minibuffer-setup-hook #'corfu-enable-always-in-minibuffer 1)

  ;; enable corfu in eshell
  (add-hook 'eshell-mode-hook
            (lambda ()
              (setq-local corfu-auto nil)
              (corfu-mode)))
  :custom
  (corfu-cycle t)                ;; Enable cycling for `corfu-next/previous'

  ;; For Eshell
  ;; ===========
  ;; avoid press RET twice in Eshell
  (defun corfu-send-shell (&rest _)
    "Send completion candidate when inside comint/eshell."
    (cond
     ((and (derived-mode-p 'eshell-mode) (fboundp 'eshell-send-input))
      (eshell-send-input))
     ((and (derived-mode-p 'comint-mode)  (fboundp 'comint-send-input))
      (comint-send-input))))
  ;; (advice-add #'corfu-insert :after #'corfu-send-shell)

  :custom
  (corfu-cycle t)                ;; Enable cycling for `corfu-next/previous'
  ;; 使用minibuffer显示corfu的显示
  ;;(corfu-auto-implict-commit nil)
  ;;(corfu-auto-delay 0)
  :init
  (setq corfu-cycle t)
  ;;(setq corfu-height 30)
  ;; (setq corfu-auto t)
  ;;(setq corfu-quit-at-boundary t)
  )
```

```elisp
;;定义函数以将company-mode和corfu-mode的显示合并到单个窗口
;;(defun my/company-corfu-completion ()
;;  (interactive)
;;  (if (bound-and-true-p corfu-mode)
;;      (progn
;;        (corfu-next)
;;        (setq completion-at-point-functions '(company-capf))))
;;  (company-complete-common))

;;;;将按键绑定到总函数以启用其自动完成
;;(global-set-key (kbd "M-/") 'my/company-corfu-completion)
;;(global-set-key (kbd "<C-tab>") 'corfu-cycle)
;;(global-set-key (kbd "<backtab>") 'company-capf)
;;设置corfu窗口的大小 将company-backends添加到corfu变量中
;; (add-hook 'corfu-mode-hook
;;     (lambda ()
;;       (setq corfu--company-backend company-backends)))
;;按键绑定以在`corfu`和`company`之间切换
;;  执行以上代码后, 当您通过M-x corfu-mode或M-x company-mode或在emacs中设置自动加载这些模式后，即可在编辑器中使用corfu-mode和company-mode进行自动完成。
;;默认情况下，corfu-mode的窗口出现在光标下方。要将其移动到上方，添加以下代码：
(setq corfu-position 'top)
;;如果您想更改corfu-mode窗口和文本区之间的间距，请添加以下代码：
(setq corfu-margin 80)
;;如果您想隐藏company-mode的提示窗口并只显示corfu-mode，请添加以下代码：
(setq corfu-auto-delay 0.2)
;;(setq completion-at-point-functions '(company-capf))
```


## CAPE :tangle ~/emacs-babel.el {#cape-tangle-emacs-babel-dot-el}

[Cape](https://github.com/minad/cape) 提供了一系列开箱即用的补全后端，跟 corfu 联合使用。

```emacs-lisp
(use-package cape
  :ensure t
  :init
  ;; Add `completion-at-point-functions', used by `completion-at-point'.
  (add-to-list 'completion-at-point-functions #'cape-file)
  (add-to-list 'completion-at-point-functions #'cape-dabbrev)
  (add-to-list 'completion-at-point-functions #'cape-keyword)  ; programming language keyword
  (add-to-list 'completion-at-point-functions #'cape-ispell)
  (add-to-list 'completion-at-point-functions #'cape-dict)
  (add-to-list 'completion-at-point-functions #'cape-symbol)   ; elisp symbol
  (add-to-list 'completion-at-point-functions #'cape-line)

  :config
  (setq cape-dict-file (expand-file-name "etc/hunspell_dict.txt" user-emacs-directory))

  ;; for Eshell:
  ;; ===========
  ;; Silence the pcomplete capf, no errors or messages!
  (advice-add 'pcomplete-completions-at-point :around #'cape-wrap-silent)

  ;; Ensure that pcomplete does not write to the buffer
  ;; and behaves as a pure `completion-at-point-function'.
  (advice-add 'pcomplete-completions-at-point :around #'cape-wrap-purify))
```


## <span class="org-todo done NEXT">NEXT</span> MULTIPLE-CURSORS 多光标编辑 {#multiple-cursors-多光标编辑}

[multiple-cursors](https://github.com/magnars/multiple-cursors.el) 插件能让 Emacs 实现多光标编辑和移动。

```emacs-lisp
(use-package multiple-cursors
  :ensure t
  :bind-keymap ("C-c o" . multiple-cursors-map)
  :bind (("C-`"   . mc/mark-next-like-this)
         ("C-\\"  . mc/unmark-next-like-this)
         :map multiple-cursors-map
         ("SPC" . mc/edit-lines)
         (">"   . mc/mark-next-like-this)
         ("<"   . mc/mark-previous-like-this)
         ("a"   . mc/mark-all-like-this)
         ("n"   . mc/mark-next-like-this-word)
         ("p"   . mc/mark-previous-like-this-word)
         ("r"   . set-rectangular-region-anchor)
         )
  :config
  (defvar multiple-cursors-map nil "keymap for `multiple-cursors")
  (setq multiple-cursors-map (make-sparse-keymap))
  (setq mc/list-file (concat user-emacs-directory "/etc/mc-lists.el"))
  (setq mc/always-run-for-all t)
  )
```


## <span class="org-todo done ___">已过期</span> ORG-BULLETS TABBAR Avy {#org-bullets-tabbar-avy}

```elisp
(use-package org-bullets
  :ensure t
  :config
  (add-hook 'org-mode-hook (lambda () (org-bullets-mode 1))))
;;(add-hook 'org-mode-hook (lambda () (org-bullets-mode 1)))

(package-install lorem-ipsum)
(use-package avy
  :ensure t ;;try 是一个特殊的关键字，它告诉 use-package 在安装失败时不要抛出错误，而是在 *Messages* 缓冲区中显示一条警告消息。这样可以避免因为包没有安装而导致整个 Emacs 配置失败。 (c-h l view-lossage) input ? brings up help
  :bind("M-s" . avy-goto-char))
```


## LAST_最后阶段，设置文件尾，和各个大佬配置 {#last-最后阶段-设置文件尾-和各个大佬配置}


### <span class="org-todo todo TODO">TODO</span> straight {#straight}

```emacs-lisp
(setq straight-base-dir "~/begin_new_life/my-packages/")
(defvar bootstrap-version)
(let ((bootstrap-file
       (expand-file-name "straight/repos/straight.el/bootstrap.el" user-emacs-directory))
      (bootstrap-version 6))
  (unless (file-exists-p bootstrap-file)
    (with-current-buffer
        (url-retrieve-synchronously
         "https://raw.githubusercontent.com/radian-software/straight.el/develop/install.el"
         'silent 'inhibit-cookies)
      (goto-char (point-max))
      (eval-print-last-sexp)))
  (load bootstrap-file nil 'nomessage))
(straight-use-package 'use-package)
(use-package use-package
  :custom
  (use-package-always-defer t)
  (use-package-always-ensure t))
;;(straight-use-package 'company-tabnine)
```


### <span class="org-todo todo TODO">TODO</span> 加载模块化配置 {#加载模块化配置}

```emacs-lisp
;; 将lisp目录放到加载路径的前面以加快启动速度
(let ((dir (locate-user-emacs-file "~")))
  (add-to-list 'load-path (file-name-as-directory dir)))

;; 加载各模块化配置
;; 不要在`*message*'缓冲区显示加载模块化配置的信息
(with-temp-message ""
  (require 'emacs-babel)
  )
```

```elisp
(require 'init-base)                  ; 一些基本配置
(require 'init-ui)                    ; UI交互
(require 'init-edit)                  ; 编辑行为
(require 'init-org)                   ; org相关设置
(require 'init-completion)            ; 补全系统
(require 'init-dired)                 ; 文件管理
(require 'init-tools)                 ; 相关工具
(require 'init-dev)                   ; 开发相关配置
(require 'init-mail)                  ; 邮件配置
(require 'init-rss)                   ; RSS配置
(require 'init-shell)                 ; Shell配置
(require 'init-browser)               ; 浏览器配置
```


### centaur {#centaur}

Windows is too slow to use
Add or change the configurations in custom.el, then restart Emacs.
C:\Users\wacl\\.emacs.d.centaur\custom.el

```org
#+TITLE:: 这一行填这个文档的标题 Emacs配置文件
#+AUTHOR:: 这一行填这个文档的作者
#+DATE:: 这一行填这个文档的创建时间
#+STARTUP: overview  这一行表示每次打开这个org文档的时候，默认把所有的标题行折叠起来（Overview）
```

custom.emcas

```emacs-lisp
;;; custom.el --- user customization file    -*- lexical-binding: t no-byte-compile: t -*-
;;; Commentary:
;;;       Add or change the configurations in custom.el, then restart Emacs.
;;;       Put your own configurations in custom-post.el to override default configurations.
;;; Code:

;; (setq centaur-logo nil)                        ; Logo file or nil (official logo)
;; (setq centaur-full-name "user name")           ; User full name
;; (setq centaur-mail-address "user@email.com")   ; Email address
;; (setq centaur-proxy "127.0.0.1:7890")          ; HTTP/HTTPS proxy
;; (setq centaur-socks-proxy "127.0.0.1:7890")    ; SOCKS proxy
;; (setq centaur-server nil)                      ; Enable `server-mode' or not: t or nil
;; (setq centaur-icon nil)                        ; Display icons or not: t or nil
;; (setq centaur-package-archives 'melpa)         ; Package repo: melpa, emacs-cn, bfsu, netease, sjtu, tencent, tuna or ustc
;; (setq centaur-theme 'auto)                     ; Color theme: auto, random, system, default, pro, dark, light, warm, cold, day or night
;; (setq centaur-completion-style 'minibuffer)    ; Completion display style: minibuffer or childframe
;; (setq centaur-dashboard nil)                   ; Display dashboard at startup or not: t or nil
;; (setq centaur-restore-frame-geometry nil)      ; Restore the frame's geometry at startup: t or nil
;; (setq centaur-lsp 'eglot)                      ; Set LSP client: lsp-mode, eglot or nil
;; (setq centaur-lsp-format-on-save t)            ; Auto format buffers on save: t or nil
;; (setq centaur-lsp-format-on-save-ignore-modes '(c-mode c++-mode python-mode markdown-mode)) ; Ignore format on save for some languages
;; (setq centaur-tree-sitter nil)                 ; Enable tree-sitter or not: t or nil. Only available in 29+.
;; (setq centaur-chinese-calendar t)              ; Support Chinese calendar or not: t or nil
;; (setq centaur-player t)                        ; Enable players or not: t or nil
;; (setq centaur-prettify-symbols-alist nil)      ; Alist of symbol prettifications. Nil to use font supports ligatures.
;; (setq centaur-prettify-org-symbols-alist nil)  ; Alist of symbol prettifications for `org-mode'

;; For Emacs devel
;; (setq package-user-dir (locate-user-emacs-file (format "elpa-%s" emacs-major-version)))
;; (setq desktop-base-file-name (format ".emacs-%s.desktop" emacs-major-version))
;; (setq desktop-base-lock-name (format ".emacs-%s.desktop.lock" emacs-major-version))

;; Fonts
(defun centaur-setup-fonts ()
  "Setup fonts."
  (when (display-graphic-p)
    ;; Set default font

    (cl-loop for font in '("Cascadia Code" "Fira Code" "Jetbrains Mono"
                           "SF Mono" "Hack" "Source Code Pro" "Menlo"
                           "Monaco" "DejaVu Sans Mono" "Consolas")
             when (font-installed-p font)
             return (set-face-attribute 'default nil
                                        :family font
                                        :height (cond (sys/macp 130)
                                                      (sys/win32p 110)
                                                      (t 100))))

    ;; Set mode-line font
    (cl-loop for font in '("Menlo" "SF Pro Display" "Helvetica")
             when (font-installed-p font)
             return (progn
                      (set-face-attribute 'mode-line nil :family font :height 120)
                      (when (facep 'mode-line-active)
                        (set-face-attribute 'mode-line-active nil :family font :height 120))
                      (set-face-attribute 'mode-line-inactive nil :family font :height 120)))

    ;; Specify font for all unicode characters
    (cl-loop for font in '("Segoe UI Symbol" "Symbola" "Symbol")
             when (font-installed-p font)
             return (if (< emacs-major-version 27)
                        (set-fontset-font "fontset-default" 'unicode font nil 'prepend)
                      (set-fontset-font t 'symbol (font-spec :family font) nil 'prepend)))

    ;; Emoji
    (cl-loop for font in '("Noto Color Emoji" "Apple Color Emoji" "Segoe UI Emoji")
             when (font-installed-p font)
             return (cond
                     ((< emacs-major-version 27)
                      (set-fontset-font "fontset-default" 'unicode font nil 'prepend))
                     ((< emacs-major-version 28)
                      (set-fontset-font t 'symbol (font-spec :family font) nil 'prepend))
                     (t
                      (set-fontset-font t 'emoji (font-spec :family font) nil 'prepend))))

    ;; Specify font for Chinese characters
    (cl-loop for font in '("WenQuanYi Micro Hei" "PingFang SC" "Microsoft Yahei" "STFangsong")
             when (font-installed-p font)
             return (progn
                      (setq face-font-rescale-alist `((,font . 1.3)))
                      (set-fontset-font t '(#x4e00 . #x9fff) (font-spec :family font))))))

(centaur-setup-fonts)
(add-hook 'window-setup-hook #'centaur-setup-fonts)
(add-hook 'server-after-make-frame-hook #'centaur-setup-fonts)

;; Mail
;; (setq message-send-mail-function 'smtpmail-send-it
;;       smtpmail-starttls-credentials '(("smtp.gmail.com" 587 nil nil))
;;       smtpmail-auth-credentials '(("smtp.gmail.com" 587
;;                                    user-mail-address nil))
;;       smtpmail-default-smtp-server "smtp.gmail.com"
;;       smtpmail-smtp-server "smtp.gmail.com"
;;       smtpmail-smtp-service 587)

;; Calendar
;; Set location , then press `S' can show the time of sunrise and sunset
(setq calendar-location-name "Chengdu"
      calendar-latitude 30.67
      calendar-longitude 104.07)
(load "~/emacs-babel.el")

;; Misc.
;; (setq confirm-kill-emacs 'y-or-n-p)

;; Enable proxy
;; (proxy-http-enable)
;; (proxy-socks-enable)

;; Display on the specified monitor
;; (when (and (> (length (display-monitor-attributes-list)) 1)
;;            (> (display-pixel-width) 1920))
;;   (set-frame-parameter nil 'left 1920))

(custom-set-variables)
;; custom-set-variables was added by Custom.
;; If you edit it by hand, you could mess it up, so be careful.
;; Your init file should contain only one such instance.
;; If there is more than one, they won't work right.


(custom-set-faces)
;; custom-set-faces was added by Custom.
;; If you edit it by hand, you could mess it up, so be careful.
;; Your init file should contain only one such instance.
;; If there is more than one, they won't work right.


;;; custom.el ends here
```


### purcell {#purcell}

To add your own customization, use M-x customize, M-x customize-themes etc. and/or create a file  which looks like this:
:LOGGING:  wrong can't use
~/.emacs.d/lisp/init-local.el

```emacs-lisp
(load "~/emacs-babel.el")
```


### doomemacs {#doomemacs}

<https://github.com/doomemacs/doomemacs/blob/master/docs/getting_started.org#writing-your-own-modules>


#### ~\\.doom.d\init.el {#dot-doom-dot-d-init-dot-el}

```emacs-lisp
;;; init.el -*- lexical-binding: t; -*-

;; This file controls what Doom modules are enabled and what order they load
;; in. Remember to run 'doom sync' after modifying it!

;; NOTE Press 'SPC h d h' (or 'C-h d h' for non-vim users) to access Doom's
;;      documentation. There you'll find a link to Doom's Module Index where all
;;      of our modules are listed, including what flags they support.

;; NOTE Move your cursor over a module's name (or its flags) and press 'K' (or
;;      'C-c c k' for non-vim users) to view its documentation. This works on
;;      flags as well (those symbols that start with a plus).
;;
;;      Alternatively, press 'gd' (or 'C-c c d') on a module to browse its
;;      directory (for easy access to its source code).

(doom! :input
       ;;bidi              ; (tfel ot) thgir etirw uoy gnipleh
       chinese
       japanese
       layout            ; auie,ctsrnm is the superior home row

       :completion
       company           ; the ultimate code completion backend
       ;;helm              ; the *other* search engine for love and life
       ido               ; the other *other* search engine...
       ivy               ; a search engine for love and life
       vertico           ; the search engine of the future

       :ui
       deft              ; notational velocity for Emacs
       doom              ; what makes DOOM look the way it does
       doom-dashboard    ; a nifty splash screen for Emacs
       doom-quit         ; DOOM quit-message prompts when you quit Emacs
       (emoji +unicode)  ; ??
       hl-todo           ; highlight TODO/FIXME/NOTE/DEPRECATED/HACK/REVIEW
       hydra
       indent-guides     ; highlighted indent columns
       ligatures         ; ligatures and symbols to make your code pretty again
       minimap           ; show a map of the code on the side
       modeline          ; snazzy, Atom-inspired modeline, plus API
       nav-flash         ; blink cursor line after big motions
       neotree           ; a project drawer, like NERDTree for vim
       ophints           ; highlight the region an operation acts on
       (popup +defaults)   ; tame sudden yet inevitable temporary windows
       tabs              ; a tab bar for Emacs
       treemacs          ; a project drawer, like neotree but cooler
       unicode           ; extended unicode support for various languages
       (vc-gutter +pretty) ; vcs diff in the fringe
       vi-tilde-fringe   ; fringe tildes to mark beyond EOB
       window-select     ; visually switch windows
       workspaces        ; tab emulation, persistence & separate workspaces
       zen               ; distraction-free coding or writing

       :editor
       (evil +everywhere); come to the dark side, we have cookies
       ;;file-templates    ; auto-snippets for empty files
       fold              ; (nigh) universal code folding
       (format +onsave)  ; automated prettiness
       ;;god               ; run Emacs commands without modifier keys
       lispy             ; vim for lisp, for people who don't like vim
       multiple-cursors  ; editing in many places at once
       ;;objed             ; text object editing for the innocent
       parinfer          ; turn lisp into python, sort of
       rotate-text       ; cycle region at point between text candidates
       ;; snippets          ; my elves. They type so I don't have to
       word-wrap         ; soft wrapping with language-aware indent

       :emacs
       dired             ; making dired pretty [functional]
       electric          ; smarter, keyword-based electric-indent
       ibuffer         ; interactive buffer management
       undo              ; persistent, smarter undo for your inevitable mistakes
       vc                ; version-control and Emacs, sitting in a tree

       :term
       eshell            ; the elisp shell that works everywhere
       shell             ; simple shell REPL for Emacs
       term              ; basic terminal emulator for Emacs
       vterm             ; the best terminal emulation in Emacs

       :checkers
       syntax              ; tasing you for every semicolon you forget
       (spell +flyspell) ; tasing you for misspelling mispelling
       grammar           ; tasing grammar mistake every you make

       :tools
       ansible
       biblio            ; Writes a PhD for you (citation needed)
       debugger          ; FIXME stepping through code, to help you add bugs
       direnv
       docker
       editorconfig      ; let someone else argue about tabs vs spaces
       ein               ; tame Jupyter notebooks with emacs
       (eval +overlay)     ; run code, run (also, repls)
       gist              ; interacting with github gists
       lookup              ; navigate your code and its documentation
       lsp               ; M-x vscode
       magit             ; a git porcelain for Emacs
       make              ; run make tasks from Emacs
       pass              ; password manager for nerds
       pdf               ; pdf enhancements
       prodigy           ; FIXME managing external services & code builders
       rgb               ; creating color strings
       taskrunner        ; taskrunner for all your projects
       terraform         ; infrastructure as code
       tmux              ; an API for interacting with tmux
       tree-sitter       ; syntax and parsing, sitting in a tree...
       upload            ; map local to remote projects via ssh/ftp

       :os
       (:if IS-MAC macos)  ; improve compatibility with macOS
       tty               ; improve the terminal Emacs experience

       :lang
       agda              ; types of types of types of types...
       beancount         ; mind the GAAP
       (cc +lsp)         ; C > C++ == 1
       clojure           ; java with a lisp
       common-lisp       ; if you've seen one lisp, you've seen them all
       coq               ; proofs-as-programs
       crystal           ; ruby at the speed of c
       csharp            ; unity, .NET, and mono shenanigans
       data              ; config/data formats
       (dart +flutter)   ; paint ui and not much else
       dhall
       elixir            ; erlang done right
       elm               ; care for a cup of TEA?
       emacs-lisp        ; drown in parentheses
       erlang            ; an elegant language for a more civilized age
       ess               ; emacs speaks statistics
       factor
       faust             ; dsp, but you get to keep your soul
       fortran           ; in FORTRAN, GOD is REAL (unless declared INTEGER)
       fsharp            ; ML stands for Microsoft's Language
       fstar             ; (dependent) types and (monadic) effects and Z3
       gdscript          ; the language you waited for
       (go +lsp)         ; the hipster dialect
       (graphql +lsp)    ; Give queries a REST
       (haskell +lsp)    ; a language that's lazier than I am
       hy                ; readability of scheme w/ speed of python
       idris             ; a language you can depend on
       json              ; At least it ain't XML
       (java +lsp)       ; the poster child for carpal tunnel syndrome
       javascript        ; all(hope(abandon(ye(who(enter(here))))))
       julia             ; a better, faster MATLAB
       kotlin            ; a better, slicker Java(Script)
       latex             ; writing papers in Emacs has never been so fun
       lean              ; for folks with too much to prove
       ledger            ; be audit you can be
       lua               ; one-based indices? one-based indices
       ;;markdown          ; writing docs for people to ignore
       nim               ; python + lisp at the speed of c
       nix               ; I hereby declare "nix geht mehr!"
       ocaml             ; an objective camel
       org               ; organize your plain life in plain text
       php               ; perl's insecure younger brother
       plantuml          ; diagrams for confusing people more
       purescript        ; javascript, but functional
       python            ; beautiful is better than ugly
       qt                ; the 'cutest' gui framework ever
       racket            ; a DSL for DSLs
       raku              ; the artist formerly known as perl6
       rest              ; Emacs as a REST client
       rst               ; ReST in peace
       (ruby +rails)     ; 1.step {|i| p "Ruby is #{i.even? ? 'love' : 'life'}"}
       (rust +lsp)       ; Fe2O3.unwrap().unwrap().unwrap().unwrap()
       scala             ; java, but good
       (scheme +guile)   ; a fully conniving family of lisps
       sh                ; she sells {ba,z,fi}sh shells on the C xor
       sml
       solidity          ; do you need a blockchain? No.
       swift             ; who asked for emoji variables?
       terra             ; Earth and Moon in alignment for performance.
       web               ; the tubes
       yaml              ; JSON, but readable
       zig               ; C, but simpler

       :email
       (mu4e +org +gmail)
       ;;notmuch
       (wanderlust +gmail)

       :app
       calendar
       emms
       ;;everywhere        ; *leave* Emacs!? You must be joking
       irc               ; how neckbeards socialize
       (rss +org)        ; emacs as an RSS reader
       twitter           ; twitter client https://twitter.com/vnought

       :config
       ;;literate
       (default +bindings +smartparens))
```


#### ~\\.doom.d\packages.el {#dot-doom-dot-d-packages-dot-el}

```text
(mapcar (lambda (pkg) `(package! ,pkg)) (split-string "beacon cal-china-x calfw calfw-org cnfonts"))
;;(mapcar (lambda (pkg) `(package! ,pkg)) (split-string "<<package_like>>"))
;(defmacro my-packages! (&rest packages)
;  `(progn
;     ,@(mapcar (lambda (p) `(package! ,p)) packages)))
;(doom! :packages
;       ;; Other package configurations...
;       (my-packages! <<package_like>>))

 :results output :exports none :var result=my_function :var packages=<<package_like>>
 begin_src emacs-lisp :tangle ~/.doom.d\packages.el :noweb yes ::var result=my_packages
 #+NAME: my_packages
 #+begin_src emacs-lisp :results output :noweb yes
 (mapcar (lambda (pkg) `(package! ,pkg)) (split-string "<<package_like>>"))
 #+end_src
 #+RESULTS: my_packages

```

```emacs-lisp
;; -*- no-byte-compile: t; -*-
  ;;; $DOOMDIR/packages.el

;;(package! doom-snippets :ignore t)
;; If you want to replace it with yasnippet's default snippets
;;(package! notmuch)
;;(package! ol-notmuch)

;; To install a package with Doom you must declare them here and run 'doom sync'
;; on the command line, then restart Emacs for the changes to take effect -- or
;; use 'M-x doom/reload'.

;; To install SOME-PACKAGE from MELPA, ELPA or emacsmirror:
;; To install a package directly from a remote git repo, you must specify a
;; `:recipe'. You'll find documentation on what `:recipe' accepts here:
;; https://github.com/radian-software/straight.el#the-recipe-format
                                        ;(package! another-package
                                        ;  :recipe (:host github :repo "username/repo"))

;; If the package you are trying to install does not contain a PACKAGENAME.el
;; file, or is located in a subdirectory of the repo, you'll need to specify
;; `:files' in the `:recipe':

;;(package!  org-modern
;;:recipe (:host github :repo "minad/org-modern"
;;:files ("org-modern.el" "src/lisp/*.el")))

;; If you'd like to disable a package included with Doom, you can do so here
;; with the `:disable' property:
                                        ;(package! builtin-package :disable t)

;; You can override the recipe of a built in package without having to specify
;; all the properties for `:recipe'. These will inherit the rest of its recipe
;; from Doom or MELPA/ELPA/Emacsmirror:
                                        ;(package! builtin-package :recipe (:nonrecursive t))
                                        ;(package! builtin-package-2 :recipe (:repo "myfork/package"))

;; Specify a `:branch' to install a package from a particular branch or tag.
;; This is required for some packages whose default branch isn't 'master' (which
;; our package manager can't deal with; see radian-software/straight.el#279)
                                        ;(package! builtin-package :recipe (:branch "develop"))

;; Use `:pin' to specify a particular commit to install.
                                        ;(package! builtin-package :pin "1a2b3c4d5e")


;; Doom's packages are pinned to a specific commit and updated from release to
;; release. The `unpin!' macro allows you to unpin single packages...
                                        ;(unpin! pinned-package)
;; ...or multiple packages
                                        ;(unpin! pinned-package another-pinned-package)
;; ...Or *all* packages (NOT RECOMMENDED; will likely break things)
                                        ;(unpin! t)
```


#### ~\\.doom.d\config.el {#dot-doom-dot-d-config-dot-el}

```emacs-lisp
;;; $DOOMDIR/config.el -*- lexical-binding: t; -*-

;; Place your private configuration here! Remember, you do not need to run 'doom
;; sync' after modifying this file!
;; Some functionality uses this to identify you, e.g. GPG configuration, email
;; clients, file templates and snippets. It is optional.
(setq user-full-name "John Doe"
      user-mail-address "john@doe.com")

;; add to ~/.doom.d/config.el

(setq doom-snippets-dir "~/.emacs.d/snippets/")
;;(use-package doom-snippets
;;  :ensure nil
;;  :load-path "~/.emacs.d.doomemacs/.local/straight/build-28.2/doom-snippets/"
;;  :after yasnippet
;;  :config
;;  (require 'doom-snippets))

;;(setq +snippets-dir "~/.emacs.d/snippets/")

;;(setq +file-templates-dir "~/.doom.d/templates/" max-specpdl-size 10000)
;;(setq +file-templates-dir "~/.doom.d/templates/")

;;(setq +snippets-enabled nil)
(load "~/emacs-babel.el")

;;(set-face-attribute 'mode-line nil :font
;;(format   "%s:size=%d"  "Hack" 32))
;;(set-face-attribute 'mode-line-inactive nil :font
;;(format   "%s:size=%d"  "Hack" 32))

;; Doom exposes five (optional) variables for controlling fonts in Doom:
;;
;; - `doom-font' -- the primary font to use
;; - `doom-variable-pitch-font' -- a non-monospace font (where applicable)
;; - `doom-big-font' -- used for `doom-big-font-mode'; use this for
;;   presentations or streaming.
;; - `doom-unicode-font' -- for unicode glyphs
;; - `doom-serif-font' -- for the `fixed-pitch-serif' face
;;
;; See 'C-h v doom-font' for documentation and more examples of what they
;; accept. For example:
;;
;;(setq doom-font (font-spec :family "Fira Code" :size 12 :weight 'semi-light)
;;      doom-variable-pitch-font (font-spec :family "Fira Sans" :size 13))
;;
;; If you or Emacs can't find your font, use 'M-x describe-font' to look them
;; up, `M-x eval-region' to execute elisp code, and 'M-x doom/reload-font' to
;; refresh your font settings. If Emacs still can't find your font, it likely
;; wasn't installed correctly. Font issues are rarely Doom issues!

;; There are two ways to load a theme. Both assume the theme is installed and
;; available. You can either set `doom-theme' or manually load a theme with the
;; `load-theme' function. This is the default:
;;(setq doom-theme 'doom-one)

;; This determines the style of line numbers in effect. If set to `nil', line
;; numbers are disabled. For relative line numbers, set this to `relative'.
(setq display-line-numbers-type `relative)

;; If you use `org' and don't want your org files in the default location below,
;; change `org-directory'. It must be set before org loads!

;;(setq org-directory "~/begin_new_life/org/")

;; Whenever you reconfigure a package, make sure to wrap your config in an
;; `after!' block, otherwise Doom's defaults may override your settings. E.g.
;;
;;   (after! PACKAGE
;;     (setq x y))
;;
;; The exceptions to this rule:
;;
;;   - Setting file/directory variables (like `org-directory')
;;   - Setting variables which explicitly tell you to set them before their
;;     package is loaded (see 'C-h v VARIABLE' to look up their documentation).
;;   - Setting doom variables (which start with 'doom-' or '+').
;;
;; Here are some additional functions/macros that will help you configure Doom.
;;
;; - `load!' for loading external *.el files relative to this one
;; - `use-package!' for configuring packages
;; - `after!' for running code after a package has loaded
;; - `add-load-path!' for adding directories to the `load-path', relative to
;;   this file. Emacs searches the `load-path' when you load packages with
;;   `require' or `use-package'.
;; - `map!' for binding new keys
;;
;; To get information about any of these functions/macros, move the cursor over
;; the highlighted symbol at press 'K' (non-evil users must press 'C-c c k').
;; This will open documentation for it, including demos of how they are used.
;; Alternatively, use `C-h o' to look up a symbol (functions, variables, faces,
;; etc).
;;
;; You can also try 'gd' (or 'C-c c d') to jump to their definition and see how
;; they are implemented.
```


#### ~/.doom.d/custom.el {#dot-doom-dot-d-custom-dot-el}

```emacs-lisp

```


### spacemacs {#spacemacs}

<https://github.com/doomemacs/doomemacs/blob/master/docs/getting_started.org#writing-your-own-modules>


#### SPC f e d 快捷键打开 dotspacemacs/user-config 函数所在的文件。在该文件中，找到 dotspacemacs/user-config 函数，并在其中添加 (load "my-file") {#spc-f-e-d-快捷键打开-dotspacemacs-user-config-函数所在的文件-在该文件中-找到-dotspacemacs-user-config-函数-并在其中添加--load-my-file}

dotspacemacs-install-packages
在 dotspacemacs-additional-packages 中加入 , SPC f e R 自动下载安装.

```emacs-lisp
  ;; -*- mode: emacs-lisp; lexical-binding: t -*-
;; This file is loaded by Spacemacs at startup.
;; It must be stored in your home directory.

(defun dotspacemacs/layers ()
  "Layer configuration:
This function should only modify configuration layer settings."
  (setq-default
   ;; Base distribution to use. This is a layer contained in the directory
   ;; `+distribution'. For now available distributions are `spacemacs-base'
   ;; or `spacemacs'. (default 'spacemacs)
   dotspacemacs-distribution 'spacemacs

   ;; Lazy installation of layers (i.e. layers are installed only when a file
   ;; with a supported type is opened). Possible values are `all', `unused'
   ;; and `nil'. `unused' will lazy install only unused layers (i.e. layers
   ;; not listed in variable `dotspacemacs-configuration-layers'), `all' will
   ;; lazy install any layer that support lazy installation even the layers
   ;; listed in `dotspacemacs-configuration-layers'. `nil' disable the lazy
   ;; installation feature and you have to explicitly list a layer in the
   ;; variable `dotspacemacs-configuration-layers' to install it.
   ;; (default 'unused)
   dotspacemacs-enable-lazy-installation 'unused

   ;; If non-nil then Spacemacs will ask for confirmation before installing
   ;; a layer lazily. (default t)
   dotspacemacs-ask-for-lazy-installation t

   ;; List of additional paths where to look for configuration layers.
   ;; Paths must have a trailing slash (i.e. "~/.mycontribs/")
   dotspacemacs-configuration-layer-path '()

   ;; List of configuration layers to load.
   dotspacemacs-configuration-layers
   '(
     ;; ----------------------------------------------------------------
     ;; Example of useful layers you may want to use right away.
     ;; Uncomment some layer names and press `SPC f e R' (Vim style) or
     ;; `M-m f e R' (Emacs style) to install them.
     ;; ----------------------------------------------------------------
      auto-completion
      better-defaults
      rust
      python
      sql
      emacs-lisp
      docker
      git
      ;; helm
      lsp
      ;; markdown
      multiple-cursors
      (shell :variables
             shell-default-height 30
             shell-default-position 'bottom
             shell-default-shell 'eshell)

      spell-checking
      syntax-checking
      version-control
      (go :variables
          go-tab-width 4
          gofmt-command "goimports"
          go-backend 'lsp)
      ;;(markdown :variables markdown-live-preview-engine 'vmd)
      (auto-completion :variables
       auto-completion-enable-snippets-in-popup t)
      org
      (org :variables
           org-enable-valign t
           org-enable-github-support t
           org-enable-reveal-js-support t)

      vimscript
      search-engine
      themes-megapack
     treemacs)


   ;; List of additional packages that will be installed without being wrapped
   ;; in a layer (generally the packages are installed only and should still be
   ;; loaded using load/require/use-package in the user-config section below in
   ;; this file). If you need some configuration for these packages, then
   ;; consider creating a layer. You can also put the configuration in
   ;; `dotspacemacs/user-config'. To use a local version of a package, use the
   ;; `:location' property: '(your-package :location "~/path/to/your-package/")
   ;; Also include the dependencies as they will not be resolved automatically.
   dotspacemacs-additional-packages '(
                                      js2-mode simple-httpd all-the-icons all-the-icons-completion all-the-icons-dired async autorevert avy beacon bind-key cal-china-x calfw calfw-org calibredb cape capf-autosuggest cnfonts company company-tabnine consult consult-notes corfu counsel counsel-projectile crux delsel denote diff-hl diminish diredfl dirvish djvu doom-modeline doom-themes editorconfig ef-themes elfeed elfeed-goodies elfeed-org embark embark-consult eshell eshell-git-prompt eshell-syntax-highlighting eshell-up evil evil-collection evil-goggles evil-matchit evil-traces eww exec-path-from-shell expand-region fanyi fontaine gnuplot go-translate helm helpful htmlize hydra indium ivy ivy-yasnippet js-comint keycast ledger-mode magit magit-delta marginalia minions multiple-cursors nerd-icons netease-cloud-music no-littering nov ob-http orderless org-ai org-appear org-auto-tangle org-contrib org-modern org-roam org-transclusion ox-gfm ox-hugo ox-pandoc ox-reveal pangu-spacing paren pass pinyinlib plantuml-mode popper posframe projectile py-autopep8 pyim pyim-basedict python quelpa quelpa-use-package rainbow-delimiters request sdcv sendmail sh-script shackle shr shrink-path skewer-mode smartparens super-save toc-org try undo-tree use-package use-package-ensure-system-package valign vertico web-mode which-key xr yasnippet yasnippet-snippets)

   ;; A list of packages that cannot be updated.
   dotspacemacs-frozen-packages '()

   ;; A list of packages that will not be installed and loaded.
   dotspacemacs-excluded-packages '()

   ;; Defines the behaviour of Spacemacs when installing packages.
   ;; Possible values are `used-only', `used-but-keep-unused' and `all'.
   ;; `used-only' installs only explicitly used packages and deletes any unused
   ;; packages as well as their unused dependencies. `used-but-keep-unused'
   ;; installs only the used packages but won't delete unused ones. `all'
   ;; installs *all* packages supported by Spacemacs and never uninstalls them.
   ;; (default is `used-only')
   dotspacemacs-install-packages 'used-but-keep-unused))

(defun dotspacemacs/init ()
  "Initialization:
This function is called at the very beginning of Spacemacs startup,
before layer configuration.
It should only modify the values of Spacemacs settings."
  ;; This setq-default sexp is an exhaustive list of all the supported
  ;; spacemacs settings.
  (setq-default
   ;; If non-nil then enable support for the portable dumper. You'll need to
   ;; compile Emacs 27 from source following the instructions in file
   ;; EXPERIMENTAL.org at to root of the git repository.
   ;;
   ;; WARNING: pdumper does not work with Native Compilation, so it's disabled
   ;; regardless of the following setting when native compilation is in effect.
   ;;
   ;; (default nil)
   dotspacemacs-enable-emacs-pdumper nil

   ;; Name of executable file pointing to emacs 27+. This executable must be
   ;; in your PATH.
   ;; (default "emacs")
   dotspacemacs-emacs-pdumper-executable-file "emacs"

   ;; Name of the Spacemacs dump file. This is the file will be created by the
   ;; portable dumper in the cache directory under dumps sub-directory.
   ;; To load it when starting Emacs add the parameter `--dump-file'
   ;; when invoking Emacs 27.1 executable on the command line, for instance:
   ;;   ./emacs --dump-file=$HOME/.emacs.d/.cache/dumps/spacemacs-27.1.pdmp
   ;; (default (format "spacemacs-%s.pdmp" emacs-version))
   dotspacemacs-emacs-dumper-dump-file (format "spacemacs-%s.pdmp" emacs-version)

   ;; If non-nil ELPA repositories are contacted via HTTPS whenever it's
   ;; possible. Set it to nil if you have no way to use HTTPS in your
   ;; environment, otherwise it is strongly recommended to let it set to t.
   ;; This variable has no effect if Emacs is launched with the parameter
   ;; `--insecure' which forces the value of this variable to nil.
   ;; (default t)
   dotspacemacs-elpa-https t

   ;; Maximum allowed time in seconds to contact an ELPA repository.
   ;; (default 5)
   dotspacemacs-elpa-timeout 5

   ;; Set `gc-cons-threshold' and `gc-cons-percentage' when startup finishes.
   ;; This is an advanced option and should not be changed unless you suspect
   ;; performance issues due to garbage collection operations.
   ;; (default '(100000000 0.1))
   dotspacemacs-gc-cons '(100000000 0.1)

   ;; Set `read-process-output-max' when startup finishes.
   ;; This defines how much data is read from a foreign process.
   ;; Setting this >= 1 MB should increase performance for lsp servers
   ;; in emacs 27.
   ;; (default (* 1024 1024))
   dotspacemacs-read-process-output-max (* 1024 1024)

   ;; If non-nil then Spacelpa repository is the primary source to install
   ;; a locked version of packages. If nil then Spacemacs will install the
   ;; latest version of packages from MELPA. Spacelpa is currently in
   ;; experimental state please use only for testing purposes.
   ;; (default nil)
   dotspacemacs-use-spacelpa nil

   ;; If non-nil then verify the signature for downloaded Spacelpa archives.
   ;; (default t)
   dotspacemacs-verify-spacelpa-archives t

   ;; If non-nil then spacemacs will check for updates at startup
   ;; when the current branch is not `develop'. Note that checking for
   ;; new versions works via git commands, thus it calls GitHub services
   ;; whenever you start Emacs. (default nil)
   dotspacemacs-check-for-update nil

   ;; If non-nil, a form that evaluates to a package directory. For example, to
   ;; use different package directories for different Emacs versions, set this
   ;; to `emacs-version'. (default 'emacs-version)
   dotspacemacs-elpa-subdirectory 'emacs-version

   ;; One of `vim', `emacs' or `hybrid'.
   ;; `hybrid' is like `vim' except that `insert state' is replaced by the
   ;; `hybrid state' with `emacs' key bindings. The value can also be a list
   ;; with `:variables' keyword (similar to layers). Check the editing styles
   ;; section of the documentation for details on available variables.
   ;; (default 'vim)
   dotspacemacs-editing-style 'vim

   ;; If non-nil show the version string in the Spacemacs buffer. It will
   ;; appear as (spacemacs version)@(emacs version)
   ;; (default t)
   dotspacemacs-startup-buffer-show-version t

   ;; Specify the startup banner. Default value is `official', it displays
   ;; the official spacemacs logo. An integer value is the index of text
   ;; banner, `random' chooses a random text banner in `core/banners'
   ;; directory. A string value must be a path to an image format supported
   ;; by your Emacs build.
   ;; If the value is nil then no banner is displayed. (default 'official)
   dotspacemacs-startup-banner 'official

   ;; Scale factor controls the scaling (size) of the startup banner. Default
   ;; value is `auto' for scaling the logo automatically to fit all buffer
   ;; contents, to a maximum of the full image height and a minimum of 3 line
   ;; heights. If set to a number (int or float) it is used as a constant
   ;; scaling factor for the default logo size.
   dotspacemacs-startup-banner-scale 'auto

   ;; List of items to show in startup buffer or an association list of
   ;; the form `(list-type . list-size)`. If nil then it is disabled.
   ;; Possible values for list-type are:
   ;; `recents' `recents-by-project' `bookmarks' `projects' `agenda' `todos'.
   ;; List sizes may be nil, in which case
   ;; `spacemacs-buffer-startup-lists-length' takes effect.
   ;; The exceptional case is `recents-by-project', where list-type must be a
   ;; pair of numbers, e.g. `(recents-by-project . (7 .  5))', where the first
   ;; number is the project limit and the second the limit on the recent files
   ;; within a project.
   dotspacemacs-startup-lists '((recents . 5)
                                (projects . 7))

   ;; True if the home buffer should respond to resize events. (default t)
   dotspacemacs-startup-buffer-responsive t

   ;; Show numbers before the startup list lines. (default t)
   dotspacemacs-show-startup-list-numbers t

   ;; The minimum delay in seconds between number key presses. (default 0.4)
   dotspacemacs-startup-buffer-multi-digit-delay 0.3

   ;; If non-nil, show file icons for entries and headings on Spacemacs home buffer.
   ;; This has no effect in terminal or if "all-the-icons" package or the font
   ;; is not installed. (default nil)
   dotspacemacs-startup-buffer-show-icons nil

   ;; Default major mode for a new empty buffer. Possible values are mode
   ;; names such as `text-mode'; and `nil' to use Fundamental mode.
   ;; (default `text-mode')
   dotspacemacs-new-empty-buffer-major-mode 'text-mode

   ;; Default major mode of the scratch buffer (default `text-mode')
   dotspacemacs-scratch-mode 'text-mode

   ;; If non-nil, *scratch* buffer will be persistent. Things you write down in
   ;; *scratch* buffer will be saved and restored automatically.
   dotspacemacs-scratch-buffer-persistent nil

   ;; If non-nil, `kill-buffer' on *scratch* buffer
   ;; will bury it instead of killing.
   dotspacemacs-scratch-buffer-unkillable nil

   ;; Initial message in the scratch buffer, such as "Welcome to Spacemacs!"
   ;; (default nil)
   dotspacemacs-initial-scratch-message nil

   ;; List of themes, the first of the list is loaded when spacemacs starts.
   ;; Press `SPC T n' to cycle to the next theme in the list (works great
   ;; with 2 themes variants, one dark and one light)
   dotspacemacs-themes '(spacemacs-dark
                         spacemacs-light)

   ;; Set the theme for the Spaceline. Supported themes are `spacemacs',
   ;; `all-the-icons', `custom', `doom', `vim-powerline' and `vanilla'. The
   ;; first three are spaceline themes. `doom' is the doom-emacs mode-line.
   ;; `vanilla' is default Emacs mode-line. `custom' is a user defined themes,
   ;; refer to the DOCUMENTATION.org for more info on how to create your own
   ;; spaceline theme. Value can be a symbol or list with additional properties.
   ;; (default '(spacemacs :separator wave :separator-scale 1.5))
   dotspacemacs-mode-line-theme '(spacemacs :separator wave :separator-scale 1.5)

   ;; If non-nil the cursor color matches the state color in GUI Emacs.
   ;; (default t)
   dotspacemacs-colorize-cursor-according-to-state t

   ;; Default font or prioritized list of fonts. The `:size' can be specified as
   ;; a non-negative integer (pixel size), or a floating-point (point size).
   ;; Point size is recommended, because it's device independent. (default 10.0)
   dotspacemacs-default-font '("Source Code Pro"
                               :size 18.0
                               :weight normal
                               :width normal)

   ;; The leader key (default "SPC")
   dotspacemacs-leader-key "SPC"

   ;; The key used for Emacs commands `M-x' (after pressing on the leader key).
   ;; (default "SPC")
   dotspacemacs-emacs-command-key "SPC"

   ;; The key used for Vim Ex commands (default ":")
   dotspacemacs-ex-command-key ":"

   ;; The leader key accessible in `emacs state' and `insert state'
   ;; (default "M-m")
   dotspacemacs-emacs-leader-key "M-m"

   ;; Major mode leader key is a shortcut key which is the equivalent of
   ;; pressing `<leader> m`. Set it to `nil` to disable it. (default ",")
   dotspacemacs-major-mode-leader-key ","

   ;; Major mode leader key accessible in `emacs state' and `insert state'.
   ;; (default "C-M-m" for terminal mode, "<M-return>" for GUI mode).
   ;; Thus M-RET should work as leader key in both GUI and terminal modes.
   ;; C-M-m also should work in terminal mode, but not in GUI mode.
   dotspacemacs-major-mode-emacs-leader-key (if window-system "<M-return>" "C-M-m")

   ;; These variables control whether separate commands are bound in the GUI to
   ;; the key pairs `C-i', `TAB' and `C-m', `RET'.
   ;; Setting it to a non-nil value, allows for separate commands under `C-i'
   ;; and TAB or `C-m' and `RET'.
   ;; In the terminal, these pairs are generally indistinguishable, so this only
   ;; works in the GUI. (default nil)
   dotspacemacs-distinguish-gui-tab nil

   ;; Name of the default layout (default "Default")
   dotspacemacs-default-layout-name "Default"

   ;; If non-nil the default layout name is displayed in the mode-line.
   ;; (default nil)
   dotspacemacs-display-default-layout nil

   ;; If non-nil then the last auto saved layouts are resumed automatically upon
   ;; start. (default nil)
   dotspacemacs-auto-resume-layouts nil

   ;; If non-nil, auto-generate layout name when creating new layouts. Only has
   ;; effect when using the "jump to layout by number" commands. (default nil)
   dotspacemacs-auto-generate-layout-names nil

   ;; Size (in MB) above which spacemacs will prompt to open the large file
   ;; literally to avoid performance issues. Opening a file literally means that
   ;; no major mode or minor modes are active. (default is 1)
   dotspacemacs-large-file-size 1

   ;; Location where to auto-save files. Possible values are `original' to
   ;; auto-save the file in-place, `cache' to auto-save the file to another
   ;; file stored in the cache directory and `nil' to disable auto-saving.
   ;; (default 'cache)
   dotspacemacs-auto-save-file-location 'cache

   ;; Maximum number of rollback slots to keep in the cache. (default 5)
   dotspacemacs-max-rollback-slots 5

   ;; If non-nil, the paste transient-state is enabled. While enabled, after you
   ;; paste something, pressing `C-j' and `C-k' several times cycles through the
   ;; elements in the `kill-ring'. (default nil)
   dotspacemacs-enable-paste-transient-state nil

   ;; Which-key delay in seconds. The which-key buffer is the popup listing
   ;; the commands bound to the current keystroke sequence. (default 0.4)
   dotspacemacs-which-key-delay 0

   ;; Which-key frame position. Possible values are `right', `bottom' and
   ;; `right-then-bottom'. right-then-bottom tries to display the frame to the
   ;; right; if there is insufficient space it displays it at the bottom.
   ;; (default 'bottom)
   dotspacemacs-which-key-position 'bottom

   ;; Control where `switch-to-buffer' displays the buffer. If nil,
   ;; `switch-to-buffer' displays the buffer in the current window even if
   ;; another same-purpose window is available. If non-nil, `switch-to-buffer'
   ;; displays the buffer in a same-purpose window even if the buffer can be
   ;; displayed in the current window. (default nil)
   dotspacemacs-switch-to-buffer-prefers-purpose nil

   ;; If non-nil a progress bar is displayed when spacemacs is loading. This
   ;; may increase the boot time on some systems and emacs builds, set it to
   ;; nil to boost the loading time. (default t)
   dotspacemacs-loading-progress-bar t

   ;; If non-nil the frame is fullscreen when Emacs starts up. (default nil)
   ;; (Emacs 24.4+ only)
   dotspacemacs-fullscreen-at-startup nil

   ;; If non-nil `spacemacs/toggle-fullscreen' will not use native fullscreen.
   ;; Use to disable fullscreen animations in OSX. (default nil)
   dotspacemacs-fullscreen-use-non-native nil

   ;; If non-nil the frame is maximized when Emacs starts up.
   ;; Takes effect only if `dotspacemacs-fullscreen-at-startup' is nil.
   ;; (default t) (Emacs 24.4+ only)
   dotspacemacs-maximized-at-startup t

   ;; If non-nil the frame is undecorated when Emacs starts up. Combine this
   ;; variable with `dotspacemacs-maximized-at-startup' to obtain fullscreen
   ;; without external boxes. Also disables the internal border. (default nil)
   dotspacemacs-undecorated-at-startup nil

   ;; A value from the range (0..100), in increasing opacity, which describes
   ;; the transparency level of a frame when it's active or selected.
   ;; Transparency can be toggled through `toggle-transparency'. (default 90)
   dotspacemacs-active-transparency 90

   ;; A value from the range (0..100), in increasing opacity, which describes
   ;; the transparency level of a frame when it's inactive or deselected.
   ;; Transparency can be toggled through `toggle-transparency'. (default 90)
   dotspacemacs-inactive-transparency 90

   ;; A value from the range (0..100), in increasing opacity, which describes the
   ;; transparency level of a frame background when it's active or selected. Transparency
   ;; can be toggled through `toggle-background-transparency'. (default 90)
   dotspacemacs-background-transparency 90

   ;; If non-nil show the titles of transient states. (default t)
   dotspacemacs-show-transient-state-title t

   ;; If non-nil show the color guide hint for transient state keys. (default t)
   dotspacemacs-show-transient-state-color-guide t

   ;; If non-nil unicode symbols are displayed in the mode line.
   ;; If you use Emacs as a daemon and wants unicode characters only in GUI set
   ;; the value to quoted `display-graphic-p'. (default t)
   dotspacemacs-mode-line-unicode-symbols t

   ;; If non-nil smooth scrolling (native-scrolling) is enabled. Smooth
   ;; scrolling overrides the default behavior of Emacs which recenters point
   ;; when it reaches the top or bottom of the screen. (default t)
   dotspacemacs-smooth-scrolling t

   ;; Show the scroll bar while scrolling. The auto hide time can be configured
   ;; by setting this variable to a number. (default t)
   dotspacemacs-scroll-bar-while-scrolling t

   ;; Control line numbers activation.
   ;; If set to `t', `relative' or `visual' then line numbers are enabled in all
   ;; `prog-mode' and `text-mode' derivatives. If set to `relative', line
   ;; numbers are relative. If set to `visual', line numbers are also relative,
   ;; but only visual lines are counted. For example, folded lines will not be
   ;; counted and wrapped lines are counted as multiple lines.
   ;; This variable can also be set to a property list for finer control:
   ;; '(:relative nil
   ;;   :visual nil
   ;;   :disabled-for-modes dired-mode
   ;;                       doc-view-mode
   ;;                       markdown-mode
   ;;                       org-mode
   ;;                       pdf-view-mode
   ;;                       text-mode
   ;;   :size-limit-kb 1000)
   ;; When used in a plist, `visual' takes precedence over `relative'.
   ;; (default nil)
   dotspacemacs-line-numbers 'relative

   ;; Code folding method. Possible values are `evil', `origami' and `vimish'.
   ;; (default 'evil)
   dotspacemacs-folding-method 'evil

   ;; If non-nil and `dotspacemacs-activate-smartparens-mode' is also non-nil,
   ;; `smartparens-strict-mode' will be enabled in programming modes.
   ;; (default nil)
   dotspacemacs-smartparens-strict-mode nil

   ;; If non-nil smartparens-mode will be enabled in programming modes.
   ;; (default t)
   dotspacemacs-activate-smartparens-mode t

   ;; If non-nil pressing the closing parenthesis `)' key in insert mode passes
   ;; over any automatically added closing parenthesis, bracket, quote, etc...
   ;; This can be temporary disabled by pressing `C-q' before `)'. (default nil)
   dotspacemacs-smart-closing-parenthesis nil

   ;; Select a scope to highlight delimiters. Possible values are `any',
   ;; `current', `all' or `nil'. Default is `all' (highlight any scope and
   ;; emphasis the current one). (default 'all)
   dotspacemacs-highlight-delimiters 'all

   ;; If non-nil, start an Emacs server if one is not already running.
   ;; (default nil)
   dotspacemacs-enable-server nil

   ;; Set the emacs server socket location.
   ;; If nil, uses whatever the Emacs default is, otherwise a directory path
   ;; like \"~/.emacs.d/server\". It has no effect if
   ;; `dotspacemacs-enable-server' is nil.
   ;; (default nil)
   dotspacemacs-server-socket-dir nil

   ;; If non-nil, advise quit functions to keep server open when quitting.
   ;; (default nil)
   dotspacemacs-persistent-server nil

   ;; List of search tool executable names. Spacemacs uses the first installed
   ;; tool of the list. Supported tools are `rg', `ag', `pt', `ack' and `grep'.
   ;; (default '("rg" "ag" "pt" "ack" "grep"))
   dotspacemacs-search-tools '("rg" "ag" "pt" "ack" "grep")

   ;; Format specification for setting the frame title.
   ;; %a - the `abbreviated-file-name', or `buffer-name'
   ;; %t - `projectile-project-name'
   ;; %I - `invocation-name'
   ;; %S - `system-name'
   ;; %U - contents of $USER
   ;; %b - buffer name
   ;; %f - visited file name
   ;; %F - frame name
   ;; %s - process status
   ;; %p - percent of buffer above top of window, or Top, Bot or All
   ;; %P - percent of buffer above bottom of window, perhaps plus Top, or Bot or All
   ;; %m - mode name
   ;; %n - Narrow if appropriate
   ;; %z - mnemonics of buffer, terminal, and keyboard coding systems
   ;; %Z - like %z, but including the end-of-line format
   ;; If nil then Spacemacs uses default `frame-title-format' to avoid
   ;; performance issues, instead of calculating the frame title by
   ;; `spacemacs/title-prepare' all the time.
   ;; (default "%I@%S")
   dotspacemacs-frame-title-format "%I@%S"

   ;; Format specification for setting the icon title format
   ;; (default nil - same as frame-title-format)
   dotspacemacs-icon-title-format nil

   ;; Color highlight trailing whitespace in all prog-mode and text-mode derived
   ;; modes such as c++-mode, python-mode, emacs-lisp, html-mode, rst-mode etc.
   ;; (default t)
   dotspacemacs-show-trailing-whitespace t

   ;; Delete whitespace while saving buffer. Possible values are `all'
   ;; to aggressively delete empty line and long sequences of whitespace,
   ;; `trailing' to delete only the whitespace at end of lines, `changed' to
   ;; delete only whitespace for changed lines or `nil' to disable cleanup.
   ;; (default nil)
   dotspacemacs-whitespace-cleanup nil

   ;; If non-nil activate `clean-aindent-mode' which tries to correct
   ;; virtual indentation of simple modes. This can interfere with mode specific
   ;; indent handling like has been reported for `go-mode'.
   ;; If it does deactivate it here.
   ;; (default t)
   dotspacemacs-use-clean-aindent-mode t

   ;; Accept SPC as y for prompts if non-nil. (default nil)
   dotspacemacs-use-SPC-as-y nil

   ;; If non-nil shift your number row to match the entered keyboard layout
   ;; (only in insert state). Currently supported keyboard layouts are:
   ;; `qwerty-us', `qwertz-de' and `querty-ca-fr'.
   ;; New layouts can be added in `spacemacs-editing' layer.
   ;; (default nil)
   dotspacemacs-swap-number-row nil

   ;; Either nil or a number of seconds. If non-nil zone out after the specified
   ;; number of seconds. (default nil)
   dotspacemacs-zone-out-when-idle nil

   ;; Run `spacemacs/prettify-org-buffer' when
   ;; visiting README.org files of Spacemacs.
   ;; (default nil)
   dotspacemacs-pretty-docs nil

   ;; If nil the home buffer shows the full path of agenda items
   ;; and todos. If non-nil only the file name is shown.
   dotspacemacs-home-shorten-agenda-source nil

   ;; If non-nil then byte-compile some of Spacemacs files.
   dotspacemacs-byte-compile nil))

(defun dotspacemacs/user-env ()
  "Environment variables setup.
This function defines the environment variables for your Emacs session. By
default it calls `spacemacs/load-spacemacs-env' which loads the environment
variables declared in `~/.spacemacs.env' or `~/.spacemacs.d/.spacemacs.env'.
See the header of this file for more information."
  (spacemacs/load-spacemacs-env))


(defun dotspacemacs/user-init ()
  "Initialization for user code:
This function is called immediately after `dotspacemacs/init', before layer
configuration.
It is mostly for variables that should be set before packages are loaded.
If you are unsure, try setting them in `dotspacemacs/user-config' first.")



(defun dotspacemacs/user-load ()
  "Library to load while dumping.
This function is called only while dumping Spacemacs configuration. You can
`require' or `load' the libraries of your choice that will be included in the
dump.")



(defun dotspacemacs/user-config ()
  "Configuration for user code:
This function is called at the very end of Spacemacs startup, after layer
configuration.
Put your configuration code here, except for variables that should be set
before packages are loaded."
  (load "~/emacs-babel.el"))



;; Do not write anything past this comment. This is where Emacs will
;; auto-generate custom variable definitions.
```


#### help {#help}

```elisp
;;for cnfonts
;; Chinese and English fonts alignment
(use-package cnfonts
 :config
 (cnfonts-enable)
 (setq cnfonts-use-face-font-rescale t))
;;中英文之间自动插入空格
(use-package pangu-spacing
 :config
 (global-pangu-spacing-mode 1)
  ;; only insert real whitespace in some specific mode, but just add virtual space
 in other mode
 (add-hook 'org-mode-hook #'valign-mode)
 (add-hook 'org-mode-hook
  '(lambda ()
    (set (make-local-variable 'pangu-spacing-real-insert-separtor) t))))

(setq python-shell-interpreter "c:\\Python311\\python.exe"
      org-babel-python-command "c:\\Python311\\python.exe")

(setq latex-shell-interpreter "d:/texlive/2022/bin/win32"
      org-babel-latex-command "d:/texlive/2022/bin/win32")


;;'(define-key e 'evil-motion-state-map "j" 'evil-next-visual-line)
;;(set (setq org-image-actual-width (/ (display-pixel-width) 3)))
(eval-after-load 'org
 (setq org-image-actual-width 960))
  ;;(set (setq org-image-actual-width (/ (display-pixel-width) 3))

;;(set (setq org-image-actual-width 960)
;; (let* ((no-ssl (and (memq system-type '(windows-nt ms-dos))
;; (not (gnutls-available-p))))
;; (proto (if no-ssl "http" "https")))
;; (when no-ssl (warn "\
;; Your version of Emacs does not support SSL connections,
;; which is unsafe because it allows man-in-the-middle attacks.
;; There are two things you can do about this warning:
;; 1. Install an Emacs version that does support SSL and be safe.
;; 2. Remove this warning from your init file so you won't see it again."))
;; (add-to-list 'package-archives (cons "melpa" (concat proto "://melpa.org/packages/")) t)
;; ;; Comment/uncomment this line to enable MELPA Stable if desired. See `package-archive-priorities`
;; ;; and `package-pinned-packages`. Most users will not need or want to do this.
;; ;;(add-to-list 'package-archives (cons "melpa-stable" (concat proto "://stable.melpa.org/packages/")) t)
;; )

  ;;; flymake-tldr-lint.el --- A TlDr Flymake backend powered by tldr-lint -*- lexical-binding: t; -*-

;;(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/") t)
;;(package-initialize)
;; The extension file content with all comments removed can be placed here.
(add-hook 'markdown-mode-hook 'flymake-tldr-lint-load)
(custom-set-variables
 '(package-selected-packages '(markdown-mode)))
(custom-set-faces)
;; custom-set-faces was added by Custom.

;;(use-package org-latex-impatient
;; :defer t
;; :hook (org-mode . org-latex-impatient-mode)
;; :init
;; (setq org-latex-impatient-tex2svg-bin
;; ;; location of tex2svg executable
;; "c:/users/wacl/node_modules/mathjax-node-cli/bin/tex2svg"))
(custom-set-variables
;; custom-set-variables was added by Custom.
;; If you edit it by hand, you could mess it up, so be careful.
;; Your init file should contain only one such instance.
;; If there is more than one, they won't work right.
 '(electric-pair-mode nil)
      ;;;!!!!!!!!!!!!!!!!!!!!!!!!!!!!!重要
 '(ignored-local-variable-values '((git-commit-major-mode . git-commit-elisp-text-mode)))
 '(org-latex-image-default-height ".98\\lineheight")
 '(org-latex-image-default-width ".98\\linewidth")
 '(package-selected-packages '(bui yaml markdown-mode)))


;; proxy连接国外的软件源由于网络问题, 通常较慢. 可以在 dotspacemacs/user-init 中设置代理
(setq url-proxy-services '(("no_proxy" . "127.0.0.1")
                           ("http" . "127.0.0.1:1087")
                           ("https" . "127.0.0.1:1087")))
```


### redguardtoo {#redguardtoo}

```emacs-lisp
(load "~/emacs-babel.el")
```

(defun my/disable-modes ()
  "Disable god-mode, ivy-mode, and helm-mode."
  (god-mode -1)
  (ivy-mode -1)
  (helm-mode -1))
  (add-hook 'after-init-hook #'my/disable-modes)


### 剪贴文件尾 {#剪贴文件尾}

```emacs-lisp
(set-clipboard-coding-system 'euc-cn)
(provide 'emacs-babel)
```
