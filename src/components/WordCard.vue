<template>
    <div class="word-card">
        <h1>{{ word }}</h1>
        <input
            id="mainInput"
            type="text"
            v-model="inputText"
        >
    </div>
</template>

<script>
export default {
    name: 'WordCard',
    props: {
        word: String,
    },
    data() {
        return {
            inputText: '',
        }
    },
    computed: {
            spelledCorrectly: function() {
                return this.inputText === this.word;
            },
            matchingSoFar: function() {
                return this.word.slice(0, this.inputText.length) === this.inputText;
            }, 
            nextLetter: function() {
                let index = this.inputText.length;
                return this.word.slice(index, index + 1);
            },
            positiveFeedbackRule: function () {
                return this.inputText.length > 0 && (this.matchingSoFar || this.spelledCorrectly);
            },
            negativeFeedbackRule: function () {
                return !this.matchingSoFar; 
            }
        },
        watch: {
            inputText: function() {
                let mainInputClasses = document.getElementById("mainInput").classList;
                
                // remove any existing feedback.
                if (mainInputClasses.contains('negative-feedback')) {
                    mainInputClasses.remove('negative-feedback');
                }
                if (mainInputClasses.contains('positive-feedback')) {
                    mainInputClasses.remove('positive-feedback');
                }

                // add new feedback.
                if (this.negativeFeedbackRule) {
                    mainInputClasses.add('negative-feedback')
                    setTimeout(function(){
                        mainInputClasses.remove('negative-feedback')
                    }, 300); // animation duration should match CSS animation duration.
                } else if (this.positiveFeedbackRule) {
                    mainInputClasses.add('positive-feedback')
                    setTimeout(function(){
                        mainInputClasses.remove('positive-feedback')
                    }, 300);
                }

                if (this.spelledCorrectly){
                    this.$emit('spelled-correctly');
                    this.inputText = '';
                }
            }
        },
}
</script>

<style scoped>

/* TODO make animation duration a global variable shared between CSS and JS */

#mainInput {
    border-radius: 25px;
    border: 2px solid #609;
    padding: 20px; 
    width: 400px;
    height: 15px;    
    background-color: #fff;
    font-size:24px;
}

#mainInput:focus{
    outline: none;
}

.negative-feedback{ 
    animation-name: negative-feedback-animation;
    animation-duration: 300ms;
    animation-timing-function: ease-in-out;
}

@keyframes negative-feedback-animation {
    from {
        box-shadow: none;
    }

    50% {
        box-shadow:
            0 0 10px 0px #fff,  /* inner white */
            0 0 50px 40px #f9f, /* middle magenta */
            0 0 1px 1px #0ff; /* outer cyan */
    }

}

.positive-feedback{ 
    animation-name: positive-feedback-animation;
    animation-duration: 300ms;
    animation-timing-function: ease-in-out;
}

@keyframes positive-feedback-animation {
    from {
        box-shadow: none;
    }

    50% {
        box-shadow:
            0 0 10px 0px #fff,  /* inner white */
            0 0 50px 40px rgb(186, 255, 201), /* middle magenta */
            0 0 1px 1px rgb(169, 226, 176); /* outer cyan */
    }

}
/* .positivefeedback{
    box-shadow:
        0 0 60px 30px #fff,  /* inner white 
        0 0 100px 60px rgb(0, 191, 6), /* middle magenta 
        0 0 140px 90px rgb(204, 255, 145); /* outer cyan 
    transition: box-shadow 0.3s ease-in-out;
} */

</style>
