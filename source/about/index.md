<head>
<script>
window.onload = function(){ 
     var audio = document.getElementById('music');
         audio.pause();//打开页面时无音乐
}
function play() {
    var audio = document.getElementById('music');
    if (audio.paused) {
        audio.play();
        document.getElementById('musicImg').src="https://pan.johnsonran.cn/AliDrive/Blog-IMG/Main/%E3%82%8C%E3%82%93%E3%81%92.png";
    }else{
        audio.pause();
        audio.currentTime = 0;//音乐从头播放
        document.getElementById('musicImg').src="https://pan.johnsonran.cn/AliDrive/Blog-IMG/Main/%E3%82%8C%E3%82%93%E3%81%92.png";
    }
}
</script>
</head>
<audio id="music" src="https://pan.johnsonran.cn/AliDrive/Blog-IMG/Main/%E3%82%8C%E3%82%93%E3%81%92.mp3" loop="loop"></audio>
<img id="musicImg" src="https://pan.johnsonran.cn/AliDrive/Blog-IMG/Main/%E3%82%8C%E3%82%93%E3%81%92.png" onClick="play()" alt="musicImg"/>

<center>これで貴方は…もう私から、離れられないわね……<br>
<span>这样你…就再不能离开我了呢……</span><br>
</center>

<p>
"因果は巡る糸車 光と影の多面鏡  "<br>
<span>因果乃旋转纺车 光影之多面明镜</span><br>
<br>
"現世の一切は　一時の夢"<br>
<span>浮世苍茫　不过瞬逝幻梦</span><br>
<br>
"善も悪も　愛も嘘も　全て運命の輪の中で"<br>
<span>善恶爱诳　皆有定数于命运之轮中</span><br>
<br>
"常世の闇に飲まれゆく"<br>
<span>吞噬于黄泉之冥暗</span><br>
<br>
"嗚呼、我は夢の防人 うつろう恋の傍観者"<br>
<span>呜呼，吾乃梦之戍人 幻恋之观者</span><br>
<br>
"万華鏡の中で　永遠に生きるのみ"<br>
<span>唯于万华镜中　永世长存</span><br>
<br>
</p>
