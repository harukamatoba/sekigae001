<template lang='pug'>
.Lottery
    v-layout(wrap row align-center justify-space-around)
        v-flex(xs6)
            v-select(v-model='tmp' :items="number" singlle-line click:prepend-inner='selectClick(input)'
            menu-props="auto" hide-details label="出席番号" return-object)
        //出席番号入力フォーム
        v-btn.mt-4.amber.accent-4.white--text(@click="slotStart" :disabled='btnDisable') 抽選
        //抽選ボタン　一回押すと押せなくなります

    v-layout(wrap row justify-space-around)
        v-responsive.mt-4(:aspect-ratio="1/1" max-width="40%" min-width="200px")
            v-card.blue(max-width="100%" min-width="200px" height="100%")
                v-layout(align-center max-width="100%" min-width="100%")
                    v-card-text.white--text.shuffleText.text-xs-center.px-0(width="100%") {{cardState.text}}
    //ここが生成された席番号を表示される

    v-card.ma-3.pa-1
            //教室の箱
            v-layout(justify-center)
                v-card.orange.lighten-3.ma-2(width='20vw' :aspect-ratio='9/16' @click='KyoudanClick')
                        v-card-text 教卓
                        //教卓

            v-layout(justify-center column)
                v-flex
                    v-layout(row wrap align-center justify-space-around)
                        v-card.primary.lighten-4.mb-2(width='calc(100vw/7.5)' :aspect-ratio='1' v-for='seki in 43' :key='seki' @click='sekiClick(i)')
                            .catch(v-if='randNum == seki')
                                v-card.red(width='calc(100vw/7.5)' :aspect-ratio='1')
                                    v-card-text HIT
                                    //生成された席番号と表示している席の番号が等しい場合赤い箱にしてメッセージを変える
                            .catch(v-else)
                                v-card-text {{seki}}
    p {{select}}}
</template>

<script lang='ts'>
import { Component, Vue } from 'vue-property-decorator';


@Component
export default class Lottery extends Vue {
    protected number = [
        1, 2, 3, 4, 5, 6, 7, 8, 9, 10,
    ];
    protected temp = 0;
    protected select ={number: -1, seki: -1};

    protected NGnum = [-1, 1, 40, 5, 33];
    protected btnDisable = false;
    protected cardState = {
        elevate: 0,             // アニメーション再生後、数字が確定すると浮き上がらせたい
        shuffleState: false,    // シャッフルする前はfalse、し始めるとtrue
        text: '? ?',
    };
    protected randNum = -1;
    protected slotStart() {
        this.cardState.shuffleState = true;     // とりあえず引いたよーって変数の中身を変更
        while(this.NGnum.indexOf(this.randNum) !== -1) {    // NGな配列の中に含まれている間乱数を生成し続ける
            this.randNum = Math.floor(Math.random() * 43) + 1;
        }
        this.cardState.text = Math.floor((this.randNum / 10)).toString() + ' ' + (this.randNum % 10).toString();
        this.btnDisable = true;

    }
    protected async KyoudanClick() {
        await this.$vdialog.alert({title: '警告', message:'そこ、野本さんの席だからぁ！！'}).promise;
    }
    // protected sekiClick(num: number) {

    // }
    protected selectClick(num: number) {
        // this.select.number = this.number;
    }
}
</script>

<style lang='stylus' scoped>
@require '~@/assets/styles/entry/_variable.styl';

.Lottery
    .shuffleText
        font-size: 20vw;


</style>
