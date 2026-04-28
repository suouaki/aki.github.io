## 问题
免费存储图片太难了，上传速度慢，甚至可能会遇到流量限制，尤其是部分图床服务在国内经常被墙，导致图片无法正常加载。
为了解决这一问题，本文将介绍两种高效、免费的图床搭建方法：

GitHub 图床 + Cloudflare 加速：通过 CDN 优化 GitHub 图床外链速度，解决图片被墙问题。


 ## 问题背景：GitHub 图床的局限性
- GitHub 图床免费且稳定，但其图片外链 raw.githubusercontent.com 经常在国内访问缓慢，甚至被墙，影响图片加载速度。
- Cloudflare R2 对象存储图床：提供更高效的大容量图片存储和全球加速服务。

## 创建 GitHub 图床仓库
1.登录 [GitHub](https://github.com/)
2.创建一个新的存储仓库
3.输入任意存储库名称,现在建立仓库（例如 hans-img）
4. GitHub 生成Personal access token(个人令牌)
   - 点击右上角用户头像，选择 Settings（设置）
   - 在页面左侧菜单栏最后，选择 Developer settings（开发人员设置）
   - 然后点击 Personal access tokens（个人[访问令牌）,选择 Token(classic)
   - 点击 Generate new token 开始创建一个新的令牌，注意一定要选择 classic 方式
   - 输入个人令牌名称(自定义),Expiration选择No expiratio(永久)然后repo全部打勾,其他保持默认
   - 保存生成的tokens
 ```
 注:要保存个人令牌,页面关闭后就无法查看!
 ```
## PicGo 配置
1.打开PicGo,点击图床设置,找到GitHub
2.图床设置GitHub
  1.图床配置名:自定义,
  2.仓库名:GitHub账号名/仓库名称
  3.设定Token:填写GitHub个人令牌
  4.自定义域:https://+Workers绑定的二级域名;
```
注:可以参考我的模版,替换你的信息,点击确定,然后设为默认图床;
```

## 完成图床配置
#点击上传区,图片上传确认图床名称,然后通过拖拽图片/选择图片/剪贴板图片上传即可#
```
图床部署完成GitHub存储库验证
```
|优点                     | 缺点                                         |
|-------------------|----------------------------------|
| 免费且易于配置  | 私有仓库的容量限制为 1GB    | 
|稳定性较高          | 适合中小型图床使用场景        | 

```
注:每个私有仓库的容量限制为1GB,超过再建新的存储库就可以了
```

## Cloudflare 加速 GitHub 图床
1.登录 [Cloudflare](https://dash.cloudflare.com/)
2.左侧菜单栏找到Workers和Pages
3.创建一个新的Workers,输入项目名称,然后部署
4.打开Workers项目,找到右上方 编辑代码
5.清空编辑器原来代码,复制粘贴下面代码

```githud
// Website you intended to retrieve for users.
const upstream = "raw.githubusercontent.com";

// Custom pathname for the upstream website.
// (1) 填写代理的路径，格式为 /<用户>/<仓库名>/<分支>
const upstream_path = "/hansvlss/PicGo-img/master";

// github personal access token.
// (2) 填写github令牌
const github_token = "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";

// Website you intended to retrieve for users using mobile devices.
const upstream_mobile = upstream;

// Countries and regions where you wish to suspend your service.
const blocked_region = [];

// IP addresses which you wish to block from using your service.
const blocked_ip_address = ["0.0.0.0", "127.0.0.1"];

// Whether to use HTTPS protocol for upstream address.
const https = true;

// Whether to disable cache.
const disable_cache = false;

// Replace texts.
const replace_dict = {
  $upstream: "$custom_domain",
};

addEventListener("fetch", (event) => {
  event.respondWith(fetchAndApply(event.request));
});

async function fetchAndApply(request) {
  const region = request.headers.get("cf-ipcountry")?.toUpperCase();
  const ip_address = request.headers.get("cf-connecting-ip");
  const user_agent = request.headers.get("user-agent");

  let response = null;
  let url = new URL(request.url);
  let url_hostname = url.hostname;

  if (https == true) {
    url.protocol = "https:";
  } else {
    url.protocol = "http:";
  }

  if (await device_status(user_agent)) {
    var upstream_domain = upstream;
  } else {
    var upstream_domain = upstream_mobile;
  }

  url.host = upstream_domain;
  if (url.pathname == "/") {
    url.pathname = upstream_path;
  } else {
    url.pathname = upstream_path + url.pathname;
  }

  if (blocked_region.includes(region)) {
    response = new Response(
      "Access denied: WorkersProxy is not available in your region yet.",
      {
        status: 403,
      }
    );
  } else if (blocked_ip_address.includes(ip_address)) {
    response = new Response(
      "Access denied: Your IP address is blocked by WorkersProxy.",
      {
        status: 403,
      }
    );
  } else {
    let method = request.method;
    let request_headers = request.headers;
    let new_request_headers = new Headers(request_headers);

    new_request_headers.set("Host", upstream_domain);
    new_request_headers.set("Referer", url.protocol + "//" + url_hostname);
    new_request_headers.set("Authorization", "token " + github_token);

    let original_response = await fetch(url.href, {
      method: method,
      headers: new_request_headers,
      body: request.body,
    });

    connection_upgrade = new_request_headers.get("Upgrade");
    if (connection_upgrade && connection_upgrade.toLowerCase() == "websocket") {
      return original_response;
    }

    let original_response_clone = original_response.clone();
    let original_text = null;
    let response_headers = original_response.headers;
    let new_response_headers = new Headers(response_headers);
    let status = original_response.status;

    if (disable_cache) {
      new_response_headers.set("Cache-Control", "no-store");
    } else {
      new_response_headers.set("Cache-Control", "max-age=43200000");
    }

    new_response_headers.set("access-control-allow-origin", "*");
    new_response_headers.set("access-control-allow-credentials", true);
    new_response_headers.delete("content-security-policy");
    new_response_headers.delete("content-security-policy-report-only");
    new_response_headers.delete("clear-site-data");

    if (new_response_headers.get("x-pjax-url")) {
      new_response_headers.set(
        "x-pjax-url",
        response_headers
          .get("x-pjax-url")
          .replace("//" + upstream_domain, "//" + url_hostname)
      );
    }

    const content_type = new_response_headers.get("content-type");
    if (
      content_type != null &&
      content_type.includes("text/html") &&
      content_type.includes("UTF-8")
    ) {
      original_text = await replace_response_text(
        original_response_clone,
        upstream_domain,
        url_hostname
      );
    } else {
      original_text = original_response_clone.body;
    }

    response = new Response(original_text, {
      status,
      headers: new_response_headers,
    });
  }
  return response;
}

async function replace_response_text(response, upstream_domain, host_name) {
  let text = await response.text();

  var i, j;
  for (i in replace_dict) {
    j = replace_dict[i];
    if (i == "$upstream") {
      i = upstream_domain;
    } else if (i == "$custom_domain") {
      i = host_name;
    }

    if (j == "$upstream") {
      j = upstream_domain;
    } else if (j == "$custom_domain") {
      j = host_name;
    }

    let re = new RegExp(i, "g");
    text = text.replace(re, j);
  }
  return text;
}

async function device_status(user_agent_info) {
  var agents = [
    "Android",
    "iPhone",
    "SymbianOS",
    "Windows Phone",
    "iPad",
    "iPod",
  ];
  var flag = true;
  for (var v = 0; v < agents.length; v++) {
    if (user_agent_info.indexOf(agents[v]) > 0) {
      flag = false;
      break;
    }
  }
  return flag;
}
```

*修改第6行、第10行,替换你自己的信息,然后点击右上角部署*
```
 注:const upstream_path填写GitHub仓库路径，格式为 /<用户>/<仓库名>/<分支>;const github_token填许GitHub创建的个人令牌;
```
6.添加你自己的域名到 Cloudflare
 - 点击设置 找到域和路由 点击添加 输入二级域名

**注:输入托管到Cloudflare的二级域名(域名前面添加一个名称就变成二级域名) ;**
