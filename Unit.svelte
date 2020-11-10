<script context="module">
    export async function getData(url){
        const response = await fetch(url);
        const json = await response.json();
        if (response.ok) {
            return json;
		} else {
			throw new Error(json);
		}
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
    export function downLoadJPG(c){
        let url = saveAsJPG(c)
        if (window.navigator.msSaveBlob) {
            //支持IE10，IE11 ,Edage
            let blob = c.msToBlob();
            window.navigator.msSaveBlob(blob, "1.jpg");
        } else {
            let oA = document.createElement("a");
            oA.download = '1.jpg';
            oA.href = url;
            document.body.appendChild(oA);
            oA.click();
            oA.remove();
        }
    }
</script>