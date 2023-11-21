+++
author = ["wacl"]
draft = false
+++

## hugo {#hugo}

调整 Docker Desktop 的 vDisk 大小一个手误直接全没了

```shell
docker run -d --restart=unless-stopped --name=hugo -p 1313:1313 -v /Users/Shared/docker/hugo/src/quickstart:/src klakegg/hugo:latest server
\\192.168.2.1\Macintosh HD\Users\Shared\docker\hugo\src\quickstart
docker run --rm -v /Users/Shared/docker/hugo/site:/src klakegg/hugo:latest new site .
# 创建一个新的 hugo 站点
docker run --rm -it \
   -v /Users/Shared/docker/hugo/src:/src \
   klakegg/hugo:latest-ext \
   new site . --force

docker run --rm -it \
   -v /Users/Shared/docker/hugo/src:/src \
   klakegg/hugo:latest-ext \
   new posts/first_post.md

# 启动服务
docker run --rm -it \
  -v /Users/Shared/docker/hugo/src:/src \
  -p 1313:1313 \
  klakegg/hugo:latest-ext \
  server


docker run -v /Users/Shared/docker/hugo/src:/src \
  -p 1313:1313 \
   klakegg/hugo:latest-ext \

docker run --rm -it \
  -v /Users/Shared/docker/hugo/src:/src \
  -p 1313:1313 \
  klakegg/hugo:latest-ext \
  shell

docker run -d --name hugo-server -p 1313:1313 -v /Users/Shared/docker/hugo/src:/src klakegg/hugo:latest server
```


## rsshub {#rsshub}

```shell
docker run -d --restart=unless-stopped --name rsshub -p 1200:1200 diygod/rsshub:latest
```


## qd {#qd}

```shell
docker cp qd:/usr/src/app/config/database.db .
dockercompose
```


## aliyundrive-webdav {#aliyundrive-webdav}

```shell
docker run -d --name=aliyundrive-webdav --restart=unless-stopped -p 8800:8080 \
  -v /Users/Shared/docker/aliyundrive-webdav/:/etc/aliyundrive-webdav/ \
  -e REFRESH_TOKEN='eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIyMDZjYWNkMDQyNGQ0ODUwOTNmZjIyMzg0MWFiMWFmNSIsImF1ZCI6IjczZTYxMTgzMWE3YzRkODdhYzQ5Yzg0ODFiZjlmMmM0IiwiZXhwIjoxNzAzNDkzMDEyLCJpYXQiOjE2OTU3MTcwMTIsImp0aSI6IjFjMWRiNmEyYzE3YjQ1ZjliOWJhMjZhY2Q2Mjg4ZWEzIn0.bsOmZZ4VznVoKWahxZzwzaGng7FPliezbcbp1PSpjen5yDSVxhXhHPV9-5oyQ1eCtL2Un6se52idkGcaVWk0yg' \
  -e WEBDAV_AUTH_USER=188 \
  -e WEBDAV_AUTH_PASSWORD=ejx4swIG6QP \
  messense/aliyundrive-webdav
```


## alist {#alist}

```shell
docker run -d --restart=unless-stopped -v /Users/Shared/docker/alist:/opt/alist/data -p 5244:5244 --name="alist" xhofe/alist:latest
```


## push news {#push-news}

```json
@BotFather
Done! Congratulations on your new bot. You will find it at t.me/NEKOT_TOB_GTbot. You can now add a description, about section and profile picture for your bot, see /help for a list of commands. By the way, when you've finished creating your cool bot, ping our Bot Support if you want a better username for it. Just make sure the bot is fully operational before you do this.

Use this token to access the HTTP API:
6486677760:AAE8HKNJPcoC75ejtpcY8c3Uxh_5O0mNH0c
Keep your token secure and store it safely, it can be used by anyone to control your bot.

For a description of the Bot API, see this page: https://core.telegram.org/bots/api
## 下方填写 @getuseridbot 中获取到的纯数字ID
export TG_USER_ID="1250408886"

企业ID
ww48e8bbe6bc3608f0
PushMe
rmRIzHvES7WrT5jNDVzT
Pushdeer
PDU25814T07ZgO0CHtUw9Fgv16mIwWh19Ne0Qdozo


https://wechat.aibotk.com/user/info
APIKEY: 9eac95e02d0856f261f0baf72b109fc808fb5a5a
APISECRET： e4d931669d42959397ed8e12422a51cb72926925
pushplus
fc893f79f2b746099597703cc653e1b0
```


## <span class="org-todo todo WAIT">WAIT</span> webdav {#webdav}

```shell
docker run -d --name webdav -p 80:80 \
-v /我的/共享文件夹路径:/var/lib/dav/data \
--restart=unless-stopped \
-e AUTH_TYPE=Digest -e USERNAME=myusername -e PASSWORD=mypassword -e ANONYMOUS_METHODS=ALL \
bytemark/webdav
docker run --name nextcloud-mysql  --restart=unless-stopped -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```


## <https://github.com/search?q=qinglong> {#https-github-dot-com-search-q-qinglong}

docker exec -it qinglong bash -c "$(curl -fsSL <https://ghproxy.com/https://raw.githubusercontent.com/FlechazoPh/QLDependency/main/Shell/XinQLOneKey.sh> | sh)"

cd _ql/data/scripts_ &amp;&amp; apk add --no-cache build-base g++ cairo-dev pango-dev giflib-dev &amp;&amp; npm i &amp;&amp; npm i -S ts-node typescript @types/node date-fns axios png-js canvas --build-from-source

?WARN? deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See <https://v8.dev/blog/math-random> for details.

docker exec -it qinglong bash -c "$(curl -fsSL <https://ghproxy.com/https://raw.githubusercontent.com/FlechazoPh/QLDependency/main/Shell/QLOneKeyDependency.sh> | sh)"

docker exec -it qinglong bash -c "$(curl -fsSL <https://ghproxy.com/https://raw.githubusercontent.com/FlechazoPh/QLDependency/main/Shell/XinQLOneKey.sh> | sh)"

```shell
# curl -sSL get.docker.com | sh
docker run -dit \
    -v /Users/Shared/docker/ql/data:/ql/data \
    -p 5700:5700 \
    -e QlBaseUrl="/" \
    -e QlPort="5700" \
    --name qinglong \
    --hostname qinglong \
    --restart unless-stopped \
    whyour/qinglong:latest

# curl -sSL get.docker.com | sh
docker run -dit \
    -v $PWD/ql/data:/ql/data \
    # 冒号后面的 5700 为默认端口，如果设置了 QlPort, 需要跟 QlPort 保持一致
    -p 5700:5700 \
        # 部署路径非必须，以斜杠开头和结尾，比如 /test/
    -e QlBaseUrl="/" \
        # 部署端口非必须，当使用 host 模式时，可以设置服务启动后的端口，默认 5700
    -e QlPort="5700" \
        --name qinglong \
        --hostname qinglong \
        --restart unless-stopped \
        whyour/qinglong:latest
```

changed 1 package in 15s
Writing to _root_.config/pip/pip.conf
Lockfile is up to date, resolution step is skipped

?WARN? deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See <https://v8.dev/blog/math-random> for details.

_root_.local/share/pnpm/global/5:

-   tsx 3.12.10
-   tsx 3.13.0

Done in 28.8s
---&gt; 1. 开始检测配置文件

---&gt; 配置文件检测完成

---&gt; 2. 开始安装青龙依赖

安装 /ql 依赖包...

devDependencies: skipped

&gt; @ postinstall /ql
&gt; max setup 2&gt;/dev/null || true

Done in 531ms
---&gt; 青龙依赖安装完成

---&gt; 3. 开始安装脚本依赖

安装 /ql/data/scripts 依赖包...

Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: _ql/data_.pnpm-store/v3
  Virtual store is at:             node_modules/.pnpm

dependencies:

-   got 11.5.1 (13.0.0 is available)
-   nodemailer 6.8.0 (6.9.5 is available)
-   tough-cookie 4.0.0 (4.1.3 is available)
-   tunnel 0.0.6
-   ws 7.4.3 (8.14.2 is available)

devDependencies: skipped

Done in 5.3s
---&gt; 脚本依赖安装完成

---&gt; 1. 复制通知文件

---&gt; 复制一份 /ql/sample/notify.py 为 /ql/data/scripts/notify.py

'/ql/sample/notify.py' -&gt; '/ql/data/scripts/notify.py'

---&gt; 复制一份 /ql/sample/notify.js 为 /ql/data/scripts/sendNotify.js

'/ql/sample/notify.js' -&gt; '/ql/data/scripts/sendNotify.js'
---&gt; 通知文件复制完成

---&gt; 2. 复制 nginx 配置文件

'/ql/docker/nginx.conf' -&gt; '/etc/nginx/nginx.conf'
'/ql/docker/front.conf' -&gt; '/etc/nginx/conf.d/front.conf'
---&gt; 配置文件复制完成

=====&gt; 检测 nginx 服务
  120 nginx: master process nginx -c /etc/nginx/nginx.conf
  121 nginx: worker process
  122 nginx: worker process
  123 nginx: worker process
  125 nginx: worker process

=====&gt; nginx 服务正常

---&gt; pm2 日志
2023-09-27T12:07:21: ?? DB loaded
2023-09-27T12:07:21: ?? Init file down
2023-09-27T12:07:21: ?? Sentry loaded
2023-09-27T12:07:22: ?? Dependency Injector loaded
2023-09-27T12:07:22: ?? Express loaded
2023-09-27T12:07:22: ?? init data loaded
2023-09-27T12:07:22: ?? link deps loaded
2023-09-27T12:07:22: ?? init task loaded
2023-09-27T12:07:22: ?? 后端服务启动成功！
2023-09-27T12:16:49: ValidationError: The Express 'trust proxy' setting is true, which allows anyone to trivially bypass IP-based rate limiting. See <https://express-rate-limit.github.io/ERR_ERL_PERMISSIVE_TRUST_PROXY/> for more information.
2023-09-27T12:16:49:     at Object.trustProxy (_ql/node_modules_.pnpm/express-rate-limit@7.0.0_express@4.18.2/node_modules/express-rate-limit/dist/index.cjs:148:13)
2023-09-27T12:16:49:     at Object.wrappedValidations.&lt;computed&gt; [as trustProxy] (_ql/node_modules_.pnpm/express-rate-limit@7.0.0_express@4.18.2/node_modules/express-rate-limit/dist/index.cjs:324:22)
2023-09-27T12:16:49:     at Object.keyGenerator (_ql/node_modules_.pnpm/express-rate-limit@7.0.0_express@4.18.2/node_modules/express-rate-limit/dist/index.cjs:582:20)
2023-09-27T12:16:49:     at _ql/node_modules_.pnpm/express-rate-limit@7.0.0_express@4.18.2/node_modules/express-rate-limit/dist/index.cjs:633:32
2023-09-27T12:16:49:     at processTicksAndRejections (node:internal/process/task_queues:95:5)
2023-09-27T12:16:49:     at _ql/node_modules_.pnpm/express-rate-limit@7.0.0_express@4.18.2/node_modules/express-rate-limit/dist/index.cjs:615:5 {
2023-09-27T12:16:49:   code: 'ERR_ERL_PERMISSIVE_TRUST_PROXY',
2023-09-27T12:16:49:   help: 'https://express-rate-limit.github.io/ERR_ERL_PERMISSIVE_TRUST_PROXY/'
2023-09-27T12:16:49: }

=====&gt; 检测后台

{"code":200,"data":{"isInitialized":true,"version":"2.16.3","publishTime":1695441600,"branch":"master","changeLog":"1. 定时任务支持设置多定时规则、任务执行前和执行后命令

1.  增加启动环境变量 QlPort, 支持修改 hosts 模式青龙启动端口
2.  消息通知 Bark 增加额外参数: 时效性通知、跳转 Url
3.  修复调试脚本日志丢失
4.  环境变量名称增加复制功能
5.  修复系统设置展示版本失败
6.  其他 bug 修复

","changeLogLink":"<https://t.me/jiao_long/394>"}}


## nextcloud {#nextcloud}

```shell
docker run -d --name=nextcloud --restart=unless-stopped -v /Users/Shared/docker/nextcloud:/var/www/html -p 8080:80 --link nextcloud-mysql:mysql nextcloud
docker run -d -v /home/nextcloud:/var/www/html -p 8000:80 nextcloud
```

<http://192.168.2.1:8080/remote.php/dav/files/nextclouemacmini>
