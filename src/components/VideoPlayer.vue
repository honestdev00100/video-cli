<template>
    <div class="video-tool">
        <!-- Only the video component should remain here -->
        <video class="video-js" ref="videoPlayer" />
        <!-- ejemplo de como debe de funcionar -->
        <!-- This whole section should work on the father, not the son. -->
        <pre>{{ setTimeValue }}</pre>
        <v-btn @click="playVideo()">Play</v-btn>
        <span>example of how it should work</span>
        <template v-for="(value, index) in setTimeValue">
            <div :key="index" class="button-player mb-4 mt-4">
                <v-btn @click="playVideo()">Play from {{ value.start }}seg until {{ value.end }}seg</v-btn>
            </div>
        </template>
    </div>
</template>

<script>
import videojs from 'video.js'
import 'video.js/dist/video-js.css'
import offset from 'videojs-offset/dist/videojs-offset'

export default {
    name: 'VideoPlayer',

    props: ['setTimeValue', 'aspectRatio', 'startTime', 'endTime', 'poster', 'sourceData'],

    data() {
        return {
            player: null,
            options: {
                aspectRatio: this.aspectRatio,
                playbackRates: [0.5, 1.0, 1.5, 2.0],
                autoplay: false,
                controls: true,
                controlBar: {
                    children: ['playToggle', 'volumePanel', 'playbackRateMenuButton', 'progressControl', 'remainingTimeDisplay', 'fullscreenToggle'],
                    pictureInPictureToggle: true,
                    volumePanel: {
                        inline: false
                    }
                },
                poster: this.poster,
                preload: 'auto',
                fluid: true,
                sources: this.sourceData
            }
        }
    },

    methods: {
        async playVideo() {
            // TODO not working
            // this.player.TimeRanges({
            //     start: 200,
            //     end: 300
            // })

            // TODO working, but not accept array types
            // this.player.offset({
            //     start: this.setTimeValue[0].start,
            //     end: this.setTimeValue[0].end,
            //     restart_beginning: false
            // })
            this.player.reset()
            this.player.src(this.sourceData)
            await this.player.play()
        }
    },

    mounted() {
        this.player = videojs(this.$refs.videoPlayer, this.options)
        console.log(this.setTimeValue)

        this.$emit('playVideo', this.setTimeValue)
    },

    beforeDestroy() {
        if (this.player) {
            this.player.dispose()
        }
    }
}
</script>

<style>
.video-js .vjs-big-play-button {
    border-radius: 50%;
    border: none;
    font-size: 36px;
    height: 80px;
    left: 50%;
    line-height: 2.2;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 80px;
}

.video-js .vjs-control-bar {
    background: #000;
    margin-bottom: 20px;
}

.vjs-menu li.vjs-selected {
    background: #000;
    color: #fff;
    padding-bottom: 5px;
    padding-top: 5px;
}
</style>