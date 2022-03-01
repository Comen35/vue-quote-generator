<template>
    <div>
        <div class="loader" id="loader" v-if="loading"></div>
        <div class="quote-text" v-else>
            <div class="quote-text">
                <i class="fas fa-quote-left"></i>
                <span id="quote">{{ quote.quoteText }}</span>
                <i class="fas fa-quote-right"></i>
                <!-- Author -->
                <div class="quote-author">
                    <span id="author">{{ quote.authorText }}</span>
                </div>
                <!-- Buttons -->
                <div class="button-container">
                    <button
                        class="twitter-button"
                        id="twitter"
                        title="TweetThis!"
                        @click="tweetQuote"
                    >
                        <i class="fab fa-twitter"></i>
                    </button>
                    <button id="new-quote" @click="newQuote">New Quote</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            apiUrl: 'https://type.fit/api/quotes',
            apiQuotes: [],
            quote: {
                authorText: '',
                quoteText: '',
                quoteTextClass: 'long-quote',
                twitterUrl: ''

            },
            loading: true,
        }
    },
    methods: {
        newQuote() {
            const quote = this.apiQuotes[Math.floor(Math.random() * this.apiQuotes.length)]
            if (!quote.author) {
                this.quote.authorText = "Unknown";
            }
            else {
                this.quote.authorText = quote.author;
            }
            /* Check quote length to determine styling */
            if (quote.text.length > 120) {
                this.quote.quoteTextClass = 'long-quote'
            }
            else {
                this.quote.quoteTextClass = 'long-quote'
            }
            this.quote.quoteText = quote.text
        },
        tweetQuote() {
            this.twitterUrl = "https://twitter.com/intent/tweet?text=" + this.quote.quoteText + this.quote.authorText
            
            window.open(this.twitterUrl, '_blank')
        },
    },

    async mounted() {

        try {
            const response = await fetch(this.apiUrl);
            const data = await response.json();
            this.apiQuotes = data
            this.newQuote()

        } catch (error) {
            console.log(error);

        }
        this.loading = false
    },

}
</script>


<style>
.quote-container {
    width: auto;
    max-width: 900px;
    padding: 20px 30px;
    border-radius: 10px;
    background-color: rgba(204, 30, 30, 0.7);
    box-shadow: 0 10px 10px 10px rgba(0, 0, 0, 0.2);
}
.quote-text {
    font-size: 2.75rem;
    color: white;
    line-height: 4rem;
    text-shadow: 0 0.5rem rgba(0, 0, 0, 0.219);
}
.long-quote {
    font-size: 2rem;
}
.fa-quote-left,
.fa-quote-right {
    font-size: 3rem;
}
.quote-author {
    margin: 15px;
    font-size: 2rem;
    font-weight: 400;
    font-style: italic;
}
.button-container {
    margin-top: 15px;
    display: flex;
    justify-content: space-between;
}
button {
    cursor: pointer;
    font-size: 1.2rem;
    height: 2.5rem;
    border: none;
    border-radius: 10px;
    color: rgba(255, 255, 255, 0.8);
    background-color: #2306537c;
    outline: none;
    padding: 0.5rem 1.8rem;
    box-shadow: 0 0.5rem rgba(0, 0, 0, 0.219);
}
button:hover {
    filter: brightness(180%);
    color: rgba(204, 30, 30, 1);
    background-color: rgba(0, 0, 0, 1);
}
button:active {
    transform: translate(0, 0.3rem);
}
/* Loader */
.loader {
    border-top: 16px solid rgba(204, 30, 30, 1);
    border-right: 16px solid #3498db;
    border-bottom: 16px solid #fff;
    border-left: 16px solid #e0a709;
    border-radius: 50%;
    width: 120px;
    height: 120px;
    animation: spin 2s linear infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}
/* Media Query: Tablet or Smaller */
@media screen and (max-width: 1000px) {
    .quote-container {
        margin: auto 10px;
    }
    .quote-text {
        font-size: 2.5rem;
    }
}
</style>
