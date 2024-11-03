---
title: Welcome Home...
date: 2024-06-28 00:20:30

---

<center><font  color= #518FC1 size=6 class="ml3">AstonWang‘s Blog</font></center>
<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/2.0.2/anime.min.js"></script>

<!-- 可选一言 -->

<center>
<font  color= #608DBD size=5>
<p id="hitokoto">
  <a href="#" id="hitokoto_text" target="_blank"></a>
</p>
<script>
  fetch('https://v1.hitokoto.cn')
    .then(response => response.json())
    .then(data => {
      const hitokoto = document.querySelector('#hitokoto_text')
      hitokoto.href = `https://hitokoto.cn/?uuid=${data.uuid}`
      hitokoto.innerText = data.hitokoto
    })
    .catch(console.error)
</script>
</font>
</center>


<div id="rcorners2" >
  <div id="rcorners1">
    <i class="fa fa-calendar" style="font-size:100"></i>
    <body>
      <font color="#4351AF">
      编程是一种技艺，一种需要用心学习的技艺
        <p class="p1"></p>
<script defer>
    //格式：2020年04月12日 10:20:00 星期二
    function format(newDate) {
        var day = newDate.getDay();
        var y = newDate.getFullYear();
        var m =
            newDate.getMonth() + 1 < 10
                ? "0" + (newDate.getMonth() + 1)
                : newDate.getMonth() + 1;
        var d =
            newDate.getDate() < 10 ? "0" + newDate.getDate() : newDate.getDate();
        var h =
            newDate.getHours() < 10 ? "0" + newDate.getHours() : newDate.getHours();
        var min =
            newDate.getMinutes() < 10
                ? "0" + newDate.getMinutes()
                : newDate.getMinutes();
        var s =
            newDate.getSeconds() < 10
                ? "0" + newDate.getSeconds()
                : newDate.getSeconds();
        var dict = {
            1: "一",
            2: "二",
            3: "三",
            4: "四",
            5: "五",
            6: "六",
            0: "天",
        };
        //var week=["日","一","二","三","四","五","六"]
        return (
            y +
            "年" +
            m +
            "月" +
            d +
            "日" +
            " " +
            h +
            ":" +
            min +
            ":" +
            s +
            " 星期" +
            dict[day]
        );
    }
    var timerId = setInterval(function () {
        var newDate = new Date();
        var p1 = document.querySelector(".p1");
        if (p1) {
            p1.textContent = format(newDate);
        }
    }, 1000);
</script>
      </font>
    </body>
  </div>
</div> 

<!-- *** -->
<!-- - 一切看似逝去的，都不曾离开，你所给予的爱与温暖，让我执着地守护着这里 ！     -->
<!-- - All problems in computer science can be solved by another level of indirection ! -->
<!-- *** -->

<!-- 
!!! info
    有人说：种一颗树，最好的时间是十年前，其次是现在... -->

=== "2024   &nbsp; &nbsp; &nbsp; (共 10 篇)"
    - 11/03 [常见的Python版本和包管理工具汇总](./pynotes/04.env_package_selection.md)
    - 10/24 [常用的Chrome插件](./toolbox/plugins/chrome_plugins.md)
    - 09/03 [Kserve(模型服务器)的安装部署](./notehub/modelops/kserve_deploy.md)
    - 08/23 [k8s储存组件Longhorn部署与使用](./notehub/cloud_native/k8s/longhorn_deploy.md)
    - 08/21 [Docker镜像无法拉取问题的解决方案](./notehub/cloud_native/docker/docker_image_pull_problem_for_solution.md)
    - 08/18 [k8s包管理工具Helm的使用](./notehub/cloud_native/k8s/helm_usage.md)
    - 07/17 [k8s开发工具之KtConnect](./notehub/cloud_native/k8s/ktconnect_usage.md)
    - 06/29 [常用的Pypi源汇总及使用](./pynotes/03.pypi_source_usage.md)
    - 06/28 [Python中的那些编码规范](./pynotes/02.py_coding_standards.md)
    - 06/28 [Hello Python](./pynotes/01.hello_python.md)
    

