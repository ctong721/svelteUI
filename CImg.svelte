<script>
    import {onMount} from 'svelte';
    import {downLoadJPG} from './Unit.svelte'
    
    export let w = 100;
    export let h = 100;

    let c = undefined;
    let ctx = undefined;
    let c1 = undefined;
    let ctx1 = undefined;
    
    let updata = undefined;
    let sf = undefined;
    let img = undefined;
    let img1 = undefined;

    let w0=0;
    let h0=0;
    let imgw;
    let imgh;
    let scale = 1;
    let rotate = 0;
    
    onMount(() => {
        ctx = c.getContext("2d");
    });
    function getImg(){
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
    function changeScale(){
        imgw = w0 * scale;
        imgh = h0 * scale;
        let x = c.width/2 - imgw/2;
        let y = c.height/2 - imgh/2;
        ctx.clearRect(0 , 0 , c.width , c.height);
        ctx.drawImage(img1, x, y, imgw, imgh);
    }

    function downImg(){
        c1 = document.createElement("canvas");
        ctx1 = c1.getContext("2d");
        c1.width=imgw;
        c1.height=imgh;
        ctx1.drawImage(img1,0,0,imgw,imgh);
        downLoadJPG(c1);
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
<p>需要处理的图片: <input type="file" bind:this={updata} on:change="{getImg}"></p>
<p>
    调节缩放比: <input bind:this={sf} type=range bind:value={scale} min=0 max=2 step="0.01" on:change="{changeScale}" on:click={changeScale}>
    <img alt="" bind:this={img} width="30"/>
</p>
<p>
    <input type="button" value="下载处理后图片" on:click={downImg}>
</p>
</div>
<canvas bind:this={c} width="{w}" height="{h}"></canvas>
</div>