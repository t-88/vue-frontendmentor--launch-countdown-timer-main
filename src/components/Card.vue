<template>
    <div class="Card-comp" >
        <div class="card">
            <Icon name="card" _class="card"/>
            <h1 class="number">{{ parseNumber }}</h1>
        </div>
        <!--<h4 class="label">{{ label }}</h4> -->
    </div>
</template>

<script>
import Icon from "./Icon";
export default {
    name: "Card",
    components: {
        Icon,
    },
    props: {
        number: { type: [String,Number], default: "08" },
        label: { type: String, default: "DAYS" },
        _ref: String,
    },
    data() {
        return {

        }
    },
    computed: {
        parseNumber() {
            if (this.number < 10 && this.number >= 0 ) {
                return `0${this.number}`
            } else if (this.number > 60) {
                return "00"
            } else if (this.number < 60){
                return this.number
            }
        }
    },
}
</script>

<style lang="scss">
.Card-comp {
    position: relative;
    text-align: center;
    overflow: hidden;
    .overlay {
        position: absolute;
        top: 0;
        left: 4px; //show make size bigger witch need offset 
        z-index: 1;
    }
    .number {
        z-index: 0;
        color: hsl(345, 95%, 68%);
        height: 0;
        font-size: 55px;
        transform: translate(0, -105px);
    }
    .label {
        letter-spacing: 4px;
        font-size: 12px;
        opacity: 0.6;
        font-weight: 700;
    }
}
</style>