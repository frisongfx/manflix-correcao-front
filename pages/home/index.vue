<template>
    <main class="main">

        <section class="banner">

            <div class="banner-info">
                <div class="info-content">
                    <img :src="banner.logo" alt="Movie Logo">
                    <div class="rating">
                        <i class="pi pi-star-fill star1"></i>
                        <i class="pi pi-star-fill star1"></i>
                        <i class="pi pi-star-fill star1"></i>
                        <i class="pi pi-star-fill star1"></i>
                        <i class="pi pi-star-fill star1"></i>
                        <span class="inf">
                            {{ banner.ano }} - {{ banner.temps }} temporada{{ banner.temps==1? '':'s' }}
                        </span>
                    </div>
                    <p>{{ banner.desc }}</p>
                </div>
            </div>

            <img src="manflix.png" alt="Logo Manflix" class="manflix-logo">

            <div class="banner-image">
                <img :src="banner.image" alt="Movie Image">
            </div>

        </section>

    <!-- --------------------------------------------------------------------- -->

        <section class="movies">
            
            <div class="movies-category" :style="'grid-template-columns: repeat('+nuSection+', 100%);'">

                <section
                    v-for="index in nuSection"
                    :key="index"
                    :id="'section' + index"
                    :style="'grid-template-columns: repeat('+nuItems+', auto);'"
                >

                    <a :href="'#section' + (index - 1 <= 0 ? nuSection : index - 1)">←</a>

                    <div
                        class="item"
                        v-for="i in nuItems"
                        :key="i"
                        @click="()=>{
                            x = ((index - 1) * nuItems) + (i - 1);
                            banner.image = $store.state.BASE_URL + movies_data[3].filmes[x].banner;
                            banner.logo = $store.state.BASE_URL + movies_data[3].filmes[x].logo;
                            banner.desc = movies_data[3].filmes[x].descricao;
                            banner.ano = movies_data[3].filmes[x].ano;
                            banner.temps = movies_data[3].filmes[x].temps;
                        }"
                    >

                        <img
                            v-if="movies_data[3] !== undefined"
                            :src="$store.state.BASE_URL + movies_data[3].filmes[
                                ((index - 1) * nuItems) + (i - 1)
                            ].foto"
                        >

                    </div>

                    <a :href="'#section' + (index + 1 > nuSection? 1 : index + 1)">→</a>

                </section>

                <!-- <section id="section2">

                    <a href="#section1">←</a>

                    <div class="item">
                        <img src="https://conteudo.imguol.com.br/c/entretenimento/58/2022/05/12/novo-poster-da-quarta-temporada-de-stranger-things-1652368177430_v2_3x4.jpg">
                    </div>
                    <div class="item">
                        <img src="https://conteudo.imguol.com.br/c/entretenimento/58/2022/05/12/novo-poster-da-quarta-temporada-de-stranger-things-1652368177430_v2_3x4.jpg">
                    </div>
                    <div class="item">
                        <img src="https://conteudo.imguol.com.br/c/entretenimento/58/2022/05/12/novo-poster-da-quarta-temporada-de-stranger-things-1652368177430_v2_3x4.jpg">
                    </div>
                    <div class="item">
                        <img src="https://conteudo.imguol.com.br/c/entretenimento/58/2022/05/12/novo-poster-da-quarta-temporada-de-stranger-things-1652368177430_v2_3x4.jpg">
                    </div>

                    <a href="#section3">→</a>

                </section>

                <section id="section3">

                    <a href="#section2">←</a>

                    <div class="item">
                        <img src="https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/e60d59103015959.5f43f7d599ad9.jpg">
                    </div>
                    <div class="item">
                        <img src="https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/e60d59103015959.5f43f7d599ad9.jpg">
                    </div>
                    <div class="item">
                        <img src="https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/e60d59103015959.5f43f7d599ad9.jpg">
                    </div>
                    <div class="item">
                        <img src="https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/e60d59103015959.5f43f7d599ad9.jpg">
                    </div>

                    <a href="#section1">→</a>

                </section> -->

            </div>

        </section>

    </main>
</template>

<script>
export default {
    name: 'home',
    // middleware: "auth",

    data() {
        return {
            banner: {
                image: 'https://wallpapercave.com/wp/wp5289118.jpg',
                logo: 'https://occ-0-2794-2219.1.nflxso.net/dnm/api/v6/LmEnxtiAuzezXBjYXPuDgfZ4zZQ/AAAABTS9DrEkcZ3gvuwJpxTs0fJ4Zc6Pa4EQlSNxuIsAVLMYR4vrqN0a9AL4M09qV5-2z39WIFiZICx_DjeF78Xh_tOEsHwk45yDpRGtm_Rdqac1.png?r=e75',
                desc: 'Uma notória gangue da Inglaterra de 1919 é liderada pelo cruel Tommy Shelby, um criminoso disposto a subir na vida a qualquer preço.',
                ano: '2016',
                temps: '6',
            },
            movies_data: [],
            category_data: [],
            nuSection: 4,
            nuItems: 3,
        }
    },

    methods:{
        getCategories: async function(){
            await this.$axios.get(this.$store.state.BASE_URL + '/categoria')
            .then((response) =>{
                this.category_data = response.data;
                console.log("CATEGORIAS:", this.category_data);
            })
            .catch((error) =>{
                console.log("CATEGORIA ERRO!", error);
            })
        },

        getMovie: async function(){

            this.category_data.map((category)=>{

                this.$axios.get(this.$store.state.BASE_URL + '/filmes?categoria=' + category.id)
                .then((response) =>{
                    this.movies_data.push({
                        categoria: category,
                        filmes: response.data
                    });
                    console.log("FILME:", this.movies_data);
                })
                .catch((error) =>{
                    console.log("FILME ERRO!", error);
                })

            })

        }
    },

    async created(){
        await Promise.all([
            this.getCategories()
        ])
        this.getMovie()
    }
}
</script>

<style lang="scss" scoped>
$height-banner: 52vh;
$width-banner: 58vw;

.main{
    box-sizing:  border-box;
    width: 100vw;
    height: auto;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    background-color: var(--cor-fundo-banner);

    .banner{
        width: 100vw;
        height: $height-banner;
        min-height: $height-banner;
        background-color: var(--cor-fundo-banner);
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;

        .manflix-logo{
            width: 15%;
            position: absolute;
            top: 10px;
            right: 45vw;
            z-index: 500;
        }

        .banner-info{
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100%;
            width: calc(100vw - $width-banner);

            .info-content{
                display: flex;
                flex-direction: column;
                align-items: flex-start;
                justify-content: flex-start;
                width: 75%;
                
                // background-color: blue;  

                img{
                    width: 75%;
                }

                .rating{
                    display: flex;
                    flex-direction: row;
                    margin-top: 7px;
                    margin-bottom: 7px;

                    .star1{
                        width: auto;
                        margin: 3px;
                        color: red;
                        font-size: 20px;
                    }

                    .inf{
                        margin: 3px;
                        color: var(--cor-font);
                        font-size: 20px;
                    }
                }

                p{
                    color: var(--cor-font);
                    font-size: 24px;
                }
            }
        }

        .banner-image{
            height: 100%;
            max-height: 100%;
            width: $width-banner;
            img{
                width: 100%;
                height: 120%;
                -webkit-mask-image: linear-gradient(to right, transparent 0%, black 15%);
                mask-image: linear-gradient(to right, transparent 0%, black 15%);
            }
        }
    }

    .movies{
        $item-grow: 1.05;
        $duration: 0.3s;
        
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        justify-content: center;
        width: 100vw;
        height: auto;
        min-height: calc(100vh - $height-banner);
        z-index: 1000;
        background-color: var(--cor-fundo-banner);
        scroll-behavior: smooth;

        .movies-category{
            display: grid;
            overflow: hidden;
            scroll-behavior: smooth;

            section{
                width: 100vw;
                position: relative;
                display: grid;
                margin: 20px 0;

                .item{
                    padding: 0 2px;
                    transition: $duration all;
                    cursor: pointer;
                    &:hover{
                        margin: 0 25px;
                        transform: scale($item-grow);
                    }
                }

                a{
                    position: absolute;
                    color: white;
                    text-decoration: none;
                    font-size: 4rem;
                    width: 80px;
                    padding: 20px;
                    text-align: center;
                    z-index: 1;

                    &:nth-of-type(1){
                        top: 0;
                        bottom: 0;
                        left: 0;
                    }

                    &:nth-of-type(2){
                        top: 0;
                        bottom: 0;
                        right: 0;
                    }
                }
            }
        }
    }
}
</style>