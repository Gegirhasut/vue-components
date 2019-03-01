<template>
    <div id="suggest-el">
        <input autocomplete="off" type="text" id="suggest-input" :name="name" v-model="searchText" @keyup="searchQualification" />
        <div id="suggestions" v-if="seen">
            <ul>
                <li v-for="option in options" v-bind:value="option.text" @click="clickSuggest(option.text)">
                    {{ option.text }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        name: "SuggestionsComponent",
        props: ['name'],
        data () {
            return {
                options: [ ],
                item: {
                    text: ''
                },
                searchText: '',
                seen: false
            }
        },
        methods: {
            clickSuggest (suggest) {
                this.searchText = suggest
                this.seen = false
            },
            searchQualification () {
                if (this.searchText.length <= 0) {
                    this.options = [];
                    this.seen = false
                } else {
                    ajaxFindQualifications(this.searchText).then(response => {
                        this.options = response;
                        this.seen = this.options.length > 0
                    })
                }
            }
        }
    }

    function ajaxFindQualifications (query) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                let qualifications = [
                    {text: 'Test message 1'},
                    {text: 'Other words here'},
                    {text: 'I dont know what is that'},
                    {text: 'It is a catastrophe'},
                ];
                const results = qualifications.filter((element, index, array) => {
                    return element.text.toLowerCase().includes(query.toLowerCase())
                });
                resolve(results)
            }, 100)
        })
    }
</script>

<style lang="scss">
    #suggest-el {
        position: relative;

        #suggest-input {
            outline: none;
            position: relative;
            display: block;
            border: 1px solid rgba(191, 191, 191, 0.87);
            padding: 5px 15px;
            width: 100%;
            box-sizing: border-box;
            -webkit-box-sizing: border-box;
            -moz-box-sizing: border-box;
            border-radius: 4px;
        }
        #suggestions {
            font-weight: 300;
            margin: 0;
            position: absolute;
            z-index: 10000001;
            width: 100%;
            border: 1px solid #e0e0e0;
            border-bottom-left-radius: 4px;
            border-bottom-right-radius: 4px;
            background: white;
            padding: 0px;
            max-height: 400px;
            ul {
                list-style: none;
                margin: 0px;
                padding: 0px;
                li {
                    padding: 5px 10px;
                }

                li:hover {
                    cursor: pointer;
                    background-color: #f6f6f6;
                }
            }
        }
    }
</style>