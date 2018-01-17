<template>
    <div id="seMap">
        <iframe :src='newSta' frameborder="0" scrolling="no"></iframe>
        <!-- <iframe src='https://www.baidu.com' sandbox="allow-forms allow-same-origin allow-scripts" frameborder="0" scrolling="no" target='_self'></iframe> -->
    </div>
</template>

<script>
export default {
    data() {
        return {
            newSta: document.location.protocol + "//uri.amap.com/marker?position=" + this.$route.query.lginfo.split(',')[1] + "," + this.$route.query.lginfo.split(',')[0] + "&name=" + this.$route.query.lgname + "&src=17hua&coordinate=gaode&callnative=0"
        }
    },
}

</script>

<style>
#seMap {
    width: 100%;
}

#seMap iframe {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    border: 0;
    height: 100%;
}
</style>
