<template>
    <section class="section-recover-identity bg-corner">
        <article>
            <header>
                <h1 class="page-title">
                    Recover your identity
                </h1>
            </header>

            <form class="form" v-bind:class="{'is-loading' : isLoading}">
                <fieldset>
                    <label class="ui-input-recover-label" for="file">Click to upload your identity</label>
                    <input class="ui-input-recover"
                           @change="restoreIdentity($event)"
                           type="file"
                           name="file"
                           id="file"/>
                </fieldset>
                <p class="page-subtitle">
                    or
                </p>
                <fieldset>
                    <label class="ui-label" for="privateKey">
                        Paste your private key
                    </label>
                    <input class="ui-input" type="text" v-model="privateKey"
                           name="privateKey"
                           id="privateKey">
                    <button class="alt ui-button"
                            @click="restoreIdentity">Restore
                    </button>
                    <br/>
                    {{errorMessage}}
                </fieldset>
            </form>
        </article>
    </section>
</template>

<script>
    import Api from '../services/api';

    export default {
        name: 'recover-identity',
        components: {},
        data() {
            return {
                privateKey: '',
                error: '',
                errorMessage: '',
                isLoading: false
            };
        },
        methods: {
            restoreIdentity(e) {
                e.preventDefault();
                if (event.target.files && event.target.files[0]) {
                    let file = event.target.files[0].path;
                    Api.recoverIdentityFile(file).then(resp => {
                        if (resp.status === 'OK') {
                            this.$router.push('loading-identity');
                        } else {
                            this.error = true;
                        }
                    });
                }
            }
        }
    };
</script>

<style scoped lang="scss">

    @import "../../src/assets/scss/colors";

    .section-recover-identity {
        align-items: center;
        justify-content: center;

        header {
            margin-bottom: 3em;
        }

        .page-subtitle {
            font-size: 1.5rem;
            margin: 1.5rem auto;
        }

        .form {
            text-align: center;
        }

        .ui-input {
            text-align: center;
            width: 100%;
            max-width: 580px;
            height: 60px;
            margin: 1.5rem 0 1rem;
        }

        .ui-button {
            font-size: 2.2rem;
            width: 100%;
            max-width: 420px;
            height: 65px;
            margin: 2.5rem auto 2rem;
        }

        .ui-input-recover {
            width: 0.1px;
            height: 0.1px;
            opacity: 0;
            overflow: hidden;
            position: absolute;
            z-index: -1;
        }

        .ui-input-recover {
            width: 100%;
            max-width: 580px;
            height: 60px;
            margin: 1rem auto 0;
            display: block;
        }

        p {
            font-size: 0.9rem;
        }

        .ui-input-recover-label {
            width: 100%;
            max-width: 580px;
            height: 60px;
            display: block;
            padding: 11px 20px 20px;
            margin: 0 auto;
            cursor: pointer;
            color: #333333;
            font-weight: bold;
            letter-spacing: 0.1rem;
            border: 0;
            border-radius: 2px;
            background-color: #FFFFFF;
            box-shadow: 0 0.3rem 1rem 0 rgba(0, 0, 0, 0.05);
            font-family: "Segoe UI Bold", Roboto, Oxygen-Sans, Ubuntu, Cantarell, Helvetica Neue, sans-serif;
            -webkit-font-smoothing: antialiased;
            text-rendering: optimizeLegibility;
            font-stretch: normal;
            line-height: normal;
            position: relative;

            &::after {
                content: "+";
                position: absolute;
                z-index: 1;
                width: 60px;
                height: 60px;
                background-color: #76be28;
                border-radius: 5rem;
                top: 0;
                right: -80px;
                color: #F4F7FE;
                font-family: "Segoe UI Bold", Roboto, Oxygen-Sans, Ubuntu, Cantarell, Helvetica Neue, sans-serif;
                font-size: 2.8rem;
                line-height: 50px;
            }
        }

        legend.header {
            font-size: 1.8rem;
            font-weight: normal;
            letter-spacing: 0.02rem;
            width: 100%;
            padding: 0px;
        }

        .is-loading {
            background: red;
        }
    }

</style>
