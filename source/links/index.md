---
title: Links
date: 2022-09-16 17:55:35
type: "Links"
---
# 友情链接

<div id="qexo-friends"></div>
<link rel="stylesheet" href="https://unpkg.com/qexo-static@1.6.0/hexo/friends.css"/>

<script src="https://unpkg.com/qexo-static@1.6.0/hexo/friends.js"></script>
<script>loadQexoFriends("qexo-friends", "https://admin.skysun0311.top")</script>

# 申请友链

<link rel="stylesheet" href="https://unpkg.com/apursuer-qexo-friend-links@1.0.2/apursuer-hexo-friend-links.css"/>

<article class="message is-info">
    <div class="message-header">
       申请友链
    </div>
    <div class="message-body">
        <div class="form-ask-friend">
            <div class="field">
                <label class="label">名字</label>
                <div class="control has-icons-left">
                    <input class="input" type="text" placeholder="Your site name" id="friend-name" required>
                    <span class="icon is-small is-left">
                        <i class="fas fa-signature"></i>
                    </span>
                </div>
            </div>
            <div class="field">
                <label class="label">链接</label>
            <div class="control has-icons-left">
                <input class="input" type="url" placeholder="A link to your site's homepage" id="friend-link" required>
                <span class="icon is-small is-left">
                    <i class="fas fa-link"></i>
                </span>
            </div>
            <p class="help ">请确保您的网址是有效的</p>
            </div>
            <div class="field">
                <label class="label">图标</label>
                <div class="control has-icons-left">
                    <input class="input" type="url" placeholder="Your website icon (as round as possible)" id="friend-icon" required>
                    <span class="icon is-small is-left">
                        <i class="fas fa-image"></i>
                    </span>
                </div>
            </div>
            <div class="field">
                <label class="label">网站描述</label>
                <div class="control has-icons-left">
                    <input class="input" type="text" placeholder="Please describe your site in one sentence." id="friend-des" required>
                    <span class="icon is-small is-left">
                        <i class="fas fa-info"></i>
                    </span>
                </div>
            </div>
            <div class="field">
                <div class="control">
                    <label class="checkbox">
                        <input type="checkbox" id="friend-check"/>我不会提交无意义的信息
                    </label>
                </div>
            </div>
            <div class="field is-grouped">
                <div class="control">
                    <button class="button is-info" type="submit" onclick="askFriend(event)">Apply</button>
                </div>
            </div>
        </div>
    </div>
</article>
<script src="https://recaptcha.net/recaptcha/api.js?render=6Ldnn3cUAAAAANS1T-9rfBL7z6lDnaZj5RXdhApc"></script>
<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
<script>
function TestUrl(url) {
    var Expression=/http(s)?:\/\/([\w-]+\.)+[\w-]+(\/[\w- .\/?%&=]*)?/;
    var objExp=new RegExp(Expression);
    if(objExp.test(url) != true){
        return false;
    }
    return true;
}
function askFriend (event) {
    let check = $("#friend-check").is(":checked");
    let name = $("#friend-name").val();
    let url = $("#friend-link").val();
    let image = $("#friend-icon").val();
    let des = $("#friend-des").val();
    if(!check){
        alert("Please check \"I am not submitting nonsense information\"");
        return;
    }
    if(!(name&&url&&image&&des)){
        alert("信息不完整 ");
        return;
    }
    if (!(TestUrl(url))){
        alert("URL 格式错误!需要包含 HTTP 协议头!");
        return;
    }
    if (!(TestUrl(image))){
        alert("切片URL格式错误!它需要包含HTTP协议头!");
        return;
    }
    event.target.classList.add('is-loading');
    grecaptcha.ready(function() {
          grecaptcha.execute('6Ldnn3cUAAAAANS1T-9rfBL7z6lDnaZj5RXdhApcssd', {action: 'submit'}).then(function(token) {
              $.ajax({
                type: 'get',
                cache: false,
                url: url,
                dataType: "jsonp",
                async: false,
                processData: false,
                //timeout:10000, 
                complete: function (data) {
                    if(data.status==200){
                    $.ajax({
                        type: 'POST',
                        dataType: "json",
                        data: {
                            "name": name,
                            "url": url,
                            "image": image,
                            "description": des,
                            "verify": token,
                        },
                        url: 'https://admin.skysun0311.top/pub/ask_friend/',
                        success: function (data) {
                            alert(data.msg);
                        }
                    });}
                    else{
                        alert("The URL cannot be reached!");
                    }
                    event.target.classList.remove('is-loading');
                }
          });
        });
    });
}
</script>

