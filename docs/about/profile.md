---
title: 个人主页
---
<center><font  color= #518FC1 size=6 class="ml3">AstonWang‘s Blog</font></center>
<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/2.0.2/anime.min.js"></script>

 
<center><font  color= #518FC1 size=3 class="ml4">一切看似逝去的，都不曾离开，你所给予的爱与温暖，让我执着地守护在这里</font></center>
<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/2.0.2/anime.min.js"></script>

<img class="img1" src='../pics/coffee.jpg'>

!!! pied-piper1 "About Site"
    因为平时热衷喝咖啡和听音乐，所以将本站取名为IceCoffee[空谷幽兰]，IceCoffee的意为冰咖啡，《空谷幽兰》则是取自音乐诗人许巍的歌曲；
    
    回望走过的这些年，无论是走过的路还是做过的事，总会在无声的时间里被慢慢沉淀，于此同时，记忆也在慢慢的被时间稀释，突然的某个瞬间，发现仅靠记忆去记录的生活，过去的很多光影都已搁浅在记录里，任凭自己怎么努力回想，能找回的也只有零星点点...

    于是乎，决定为未来的自己做一点有迹可循的事，因此便有了这个站点IceCoffee[空谷幽兰]，往后的日子希望自己能做一个长期主义者，坚持写下去...

!!! pied-piper1 "About Me"
    - [x] Hey, I'm AstonWang! 一个咖啡重度爱好者...
    - [x] 目前主要从事后端+ModelOps方向的工作...
    - [x] 喜欢研究新技术，热衷在Github闲逛,力争做一个全沾手艺人...


!!! pied-piper1 "About 一言"    
    - 我认清时间的时候，只剩下一寸光阴，这是我的个人主页，到处是岁月痕迹...
    - 日出未必意味着光明，太阳也无非是一颗晨星，只有在我们醒着时，才是真正的破晓...

<!-- <style>
.skill {
  margin-bottom: 10px;
}

.skill-name {
  font-weight: bold;
  margin-bottom: 5px;
}

.skill-bar {
  background-color: #ddd;
  height: 20px;
  border-radius: 5px;
}

.skill-level {
  background-color: #4CAF50;
  height: 100%;
  border-radius: 5px;
}
</style>

<div class="skill">
  <div class="skill-name">Python</div>
  <div class="skill-bar">
    <div class="skill-level" style="width: 75%;"></div>
  </div>
</div>

<div class="skill">
  <div class="skill-name">Go</div>
  <div class="skill-bar">
    <div class="skill-level" style="width: 50%;"></div>
  </div>
</div>

<div class="skill">
  <div class="skill-name">数据分析</div>
  <div class="skill-bar">
    <div class="skill-level" style="width: 50%;"></div>
  </div>
</div>

<div class="skill">
  <div class="skill-name">机器学习</div>
  <div class="skill-bar">
    <div class="skill-level" style="width: 35%;"></div>
  </div>
</div>

<div class="skill">
  <div class="skill-name">Shell</div>
  <div class="skill-bar">
    <div class="skill-level" style="width: 25%;"></div>
  </div>
</div> -->


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

[Send Email :fontawesome-solid-paper-plane:](mailto:sswss5@aliyun.com>){.md-button}