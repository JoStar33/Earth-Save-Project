<template>
    <div class="ThirdApplication">
        <v-layout row="row" wrap="wrap" width="100%" justify-center="justify-center">
            <v-card-actions class="justify-center" max-width="70%">
                <v-card>
                    <v-toolbar class="title" color="primary" dark="dark">
                        <v-layout
                            width="100%"
                            text-xs-center="text-xs-center"
                            justify-center="justify-center">
                            <v-toolbar-title>오늘 너가 지구를 얼마나 지켰는지 확인해봐!</v-toolbar-title>
                        </v-layout>
                    </v-toolbar>
                    <v-list v-for="(item, index) in items" :key="item.title">
                        <v-list-item>
                            <v-list-item-icon v-for="icons in item.icon" :key="icons">
                                <v-icon v-text="icons" size="30px"></v-icon>
                            </v-list-item-icon>
                            <v-list-item-content>
                                <v-list-item-title v-html="item.title"></v-list-item-title>
                            </v-list-item-content>
                            &nbsp;&nbsp;
                            <v-card-actions>
                                <v-btn @click="item.count = subCount(item.count)">-</v-btn>
                                &nbsp;&nbsp;
                                <!--{{item.count}}-->
                                <!--<input v-bind:value="inputText[item.id]" v-on:input="updateInput"
                                style="width: 30px">-->
                                <v-text-field class="text-field" v-model="item.count"></v-text-field>
                                &nbsp;&nbsp;
                                <v-btn @click="item.count = plusCount(item.count)">+</v-btn>
                            </v-card-actions>
                        </v-list-item>
                        <v-divider v-if="index < items.length - 1" :key="index"></v-divider>
                    </v-list>
                </v-card>
            </v-card-actions>
        </v-layout>
        <v-btn width="30%" color="primary">
            <router-link to="/plastic_consume1" class="link">Previous Step</router-link>
        </v-btn>
        <v-btn class="ForBtnClass" @click="pushData" width="30%" color="primary">
            Next Step
        </v-btn>
    </div>
</template>

<script>
    import JsonData from "../assets/save_earth2.json"
    export default {
        data: () => ({lackItems: [], Counter: 0, items: JsonData.items, inputText: []}),
        created() {
            this.Counter += parseInt(this.$route.query.items);
            for(let lackItem of this.$route.query.lackItems){
                this.lackItems.push(String(lackItem));
            }
            console.log(this.Counter);
            console.log(this.lackItems);
        },
        methods: {
            subCount(count) {
                if (count > 0) {
                    --count;
                } else {
                    alert("이런! 0에서 더빼는건 불가능해!");
                }
                return count;
            },
            plusCount(count) {
                ++count;
                return count;
            },
            //이용수치를 값으로 바꿔주는 함수. 추후 쿼리를 이용해서 Result.vue에 넘겨준다.
            CalculateData() {
                let shampooCount = this.items.find(item => item.title.includes('샴푸')).count;
                let publictransportCount = this.items.find(item => item.title.includes('대중교통')).count * -3;
                let plasticCount = this.items.find(item => item.title.includes('비닐봉투')).count * 3;
                let cupCount = this.items.find(item => item.title.includes('컵')).count;
                let bowlCount = this.items.find(item => item.title.includes('용기')).count * 2;
                this.Counter += (shampooCount + publictransportCount + plasticCount + cupCount + bowlCount);
            },
            //초과이용되었다고 판단되는 값들을 검출해서 배열로 넘기기 위한 함수
            PutLackItems() {
                if(this.items.find(item => item.title.includes('샴푸')).count >= 3){
                    this.lackItems.push("샴푸");
                }
                if(this.items.find(item => item.title.includes('대중교통')).count === 0){
                    this.lackItems.push("대중교통");
                }
                if(this.items.find(item => item.title.includes('비닐봉투')).count >= 4){
                    this.lackItems.push("비닐봉투");
                }
                if(this.items.find(item => item.title.includes('컵')).count >= 2){
                    this.lackItems.push("컵");
                }
                if(this.items.find(item => item.title.includes('용기')).count >= 3){
                    this.lackItems.push("용기");
                }
            },
            pushData() {
                this.CalculateData();
                this.PutLackItems();
                console.log(this.Counter);
                this
                .$router
                .push({
                    path: "/result",
                    query: {
                        name: "두번째 페이지 결과",
                        items: this.Counter, //배열을 넘기는것도 가능하다는 점 참고!
                        lackItems: this.lackItems
                    }
                });
            }
        }
    }
</script>

<style>
    .title {
        font-weight: 900 !important;
        text-align: center !important;
    }
    .ThirdApplication {
        display: block;
        text-align: center;
        margin-top: 5%;
        margin-bottom: 5%;
    }
    .ForBtnClass {
        color: black !important;
        text-decoration: none !important;
    }
    .link {
                width: 100% !important;
        height: 100% !important;
        color: black !important;
        text-decoration: none !important;
    }
    i {
        color: black !important;
    }
    .text-field {
        width: 50px;
    }
    .Calculator {
        float: left !important;
        text-align: center !important;
    }
    input {
        text-align: center !important;
    }
</style>