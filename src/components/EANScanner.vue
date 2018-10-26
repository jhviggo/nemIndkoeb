<template>
    <div class="position-relative">
        <div id="item"></div>
        <p id="EAN">Leder efter EAN...</p>
    </div>
</template>

<script>
/* eslint-disable */
import HelloWorld from '@/components/HelloWorld.vue'
import Quagga from 'quagga'

export default {
    name: 'home',
    components: {
        HelloWorld
    },
    mounted() {
        Quagga.init({
                inputStream : {
                    name : "Live",
                    type : "LiveStream",
                    target: document.querySelector('#item'),
                    patchSize: "x-large",
                    frequency: 10,
                    constraints: {
                        width: 320,
                        height: 320
                    },
                    area: {
                        top: '0%',
                        bottom: '0%',
                        left: '0%',
                        right: '0%',
                    },
                },
                decoder : {
                    readers : ["ean_reader"]
                }},
            function(err) {
                if (err) {
                    console.log(err);
                    return
                }
                Quagga.start();
                console.log('Quagga has started!');
            }
        );

        Quagga.onProcessed(function(result) {
            var drawingCtx = Quagga.canvas.ctx.overlay,
                drawingCanvas = Quagga.canvas.dom.overlay;

            if (result) {
                if (result.boxes) {
                    drawingCtx.clearRect(0, 0, parseInt(drawingCanvas.getAttribute("width")), parseInt(drawingCanvas.getAttribute("height")));
                    result.boxes.filter(function (box) {
                        return box !== result.box;
                    }).forEach(function (box) {
                        Quagga.ImageDebug.drawPath(box, {x: 0, y: 1}, drawingCtx, {color: "green", lineWidth: 2});
                    });
                }

                if (result.box) {
                    Quagga.ImageDebug.drawPath(result.box, {x: 0, y: 1}, drawingCtx, {color: "#00F", lineWidth: 2});
                }

                if (result.codeResult && result.codeResult.code) {
                    Quagga.ImageDebug.drawPath(result.line, {x: 'x', y: 'y'}, drawingCtx, {color: 'red', lineWidth: 3});
                }
            }
        });

        Quagga.onDetected(result => {
            if (result.codeResult && document.getElementById('EAN')){
                document.getElementById('EAN').innerText = result.codeResult.code;
                this.$emit('input', result.codeResult.code);
            }
        });
    }
}
</script>
