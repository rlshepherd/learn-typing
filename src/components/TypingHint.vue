<template>
    <div class="typing-hint">
        <transition name="hintfade">
            <img
                key=1
                :src="hintImageURI()"
                class="keyboard"
                v-if="needsHint"
            >
        </transition>
        <transition name="nohintfade">
            <img
                key=2
                src="../assets/letters/full-color-keyboard.png"
                class="keyboard"
                v-if="!needsHint"
                >
        </transition>
    </div>
</template>

<script>
export default {
    name: 'TypingHint',
    props: {
        nextLetter: String,
        start: Number,
        timeout: Number,
        timeFromLastKeystroke: Number,
    },
    created () {
        this.gettimeFromLastKeystroke()
        setInterval(this.gettimeFromLastKeystroke, 1000)
    },
    destroyed () {
        clearInterval(this.gettimeFromLastKeystroke)
    },
    methods: {
        gettimeFromLastKeystroke () {
            this.timeFromLastKeystroke = Date.now() - this.start;
        },
        hintImageURI: function() {
            var images = require.context('../assets/letters', false, /\.png$/)
            if (this.needsHint) {
                return images('./' + this.nextLetter + ".png")
            }
            return false;
        }
    },
    computed: {
        needsHint: function() {
            return this.timeFromLastKeystroke > this.timeout;
        }
    }
}
</script>
<style scoped>
.typing-hint {
    position: relative;
    margin-top: 5%;
    padding-left: 20px;
    width: 70%;
    display: flex;
}
.keyboard {
    position: absolute;
    max-width: 100%;
    width: inherit;
    left: 17.5%;
}
.hintfade-leave-active, .nohintfade-enter-active {
  transition: opacity 0.5s;
}
.nohintfade-leave-active {
  transition: opacity 1.5s;
}
.hintfade-enter, .hintfade-leave-to, .nohintfade-enter, .nohintfade-leave-to {
  opacity: 0;
}

</style>