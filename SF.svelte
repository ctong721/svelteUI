<script>
    import { onMount } from 'svelte';
    
    let c = undefined;
    let ctx = undefined;
    let c1 = undefined;
    let ctx1 = undefined;
    export let w = 100;
    export let h = 100;
    let scale = 1;
    let updata;
    let sf;
    let w0=0;
    let h0=0;
    let img;
    let img1;
    let imgw;
    let imgh;
    
    onMount(() => {
        ctx = c.getContext("2d");
    });

    function change1(){
        imgw = w0 * scale;
        imgh = h0 * scale;
        let x = c.width/2 - imgw/2;
        let y = c.height/2 - imgh/2;
        ctx.clearRect(0 , 0 , c.width , c.height);
        ctx.drawImage(img1, x, y, imgw, imgh);
    }
    function t1(){
        let file = updata.files[0];
        //读取文件内容
        let reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = function (e) {
            img.src=reader.result;

            //加载图片
            img1 = new Image();
            img1.onload = function () {
                ctx.clearRect(0 , 0 , c.width , c.height);
                ctx.drawImage(img1, c.width/2-img1.width/2, c.height/2-img1.height/2, img1.width, img1.height);
                w0 = img1.width;
                h0 = img1.height;
            }
            img1.src = reader.result;
        }
        sf.value = 1;
    }
    // 保存成png格式的图片
    function saveAsPNG(canvas) {
        return canvas.toDataURL("image/png");
    }
    // 保存成jpg格式的图片
    function saveAsJPG(canvas) {
        return canvas.toDataURL("image/jpeg");
    }
    // 保存成bmp格式的图片  目前浏览器支持性不好
    function saveAsBMP(canvas) {
        return canvas.toDataURL("image/bmp");
    }
    function getDownImg(){
        c1 = document.createElement("canvas");
        ctx1 = c1.getContext("2d");
        c1.width=imgw;
        c1.height=imgh;
        ctx1.drawImage(img1,0,0,imgw,imgh);
    }
    function downLoad(c){
        let url = saveAsPNG(c)
        if (window.navigator.msSaveBlob) {
            //支持IE10，IE11 ,Edage
            let blob = c.msToBlob();
            window.navigator.msSaveBlob(blob, "1.jpg");
        } else {
            let oA = document.createElement("a");
            oA.download = '';
            oA.href = url;
            document.body.appendChild(oA);
            oA.click();
            oA.remove();
        }
    }
    function downImg(){
        getDownImg();
        downLoad(c1);
    }
</script>

<style>
div{
    border: 1px solid rgb(211, 247, 4);
    color: rgb(8, 93, 252);
}
</style>

<div style="width:{Number(w)+10}px">
<div>
<p>需要处理的图片: <input type="file" bind:this={updata} on:change="{t1}"></p>
<p>调节缩放比: <input bind:this={sf} type=range bind:value={scale} min=0 max=2 step="0.01" on:click={change1}>
<img alt="" bind:this={img} width="30"/></p>
<p><input type="button" value="下载处理后图片" on:click={downImg}></p>
</div>
<canvas bind:this={c} width="{w}" height="{h}"></canvas>
</div>