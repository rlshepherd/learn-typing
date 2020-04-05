<template>
    <div class="progress-card">
        <div class="progress-bar">
            <progress
                v-bind:value="wordCount"
                v-bind:max="totalWords"
            >
            </progress>
            {{wordCount}} / {{totalWords}}
        </div>
        <div class="statistics">
            <!-- Words Per Minute: {{wordsPerMinute}}
            Total Errors: {{errors}}
            Errors per words: {{errors / totalWords}} -->
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProgressCard',
    props : {
        wordCount: Number,
        totalWords: Number,
        // errors: Number,
        // wordsPerMinute: Number,
    }
}
</script>