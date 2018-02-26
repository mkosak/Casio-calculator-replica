<template>
    <div class="calc">
        <div class="logo"></div>
        <div class="sunpower">
            <div class="sunpower__battery"></div>
        </div>

        <div class="display">
            <div class="display__result" v-bind:class="{ display_on: power }">
                <span class="display__result__value">{{ display }}</span>
            </div>
        </div>

        <div class="modelname">SL-300SV</div>

        <div class="keyboard">
            <div class="keyboard__row">
                <div class="to_right">
                    <div class="keyboard_button_wrap">
                        <button @click=""
                                class="keyboard__button button_font_m button_size_s button_disabled">
                            <span>√</span></button>
                    </div>
                    <div class="keyboard_button_wrap">
                        <button @click="turnOff()"
                                class="keyboard__button button_font_s button_size_s"><span>OFF</span></button>
                    </div>
                </div>
            </div>
            <div class="keyboard__row">
                <div class="keyboard_button_wrap">
                    <button @click="memory('MC')"
                            class="keyboard__button button_font_m button_disabled"><span>MC</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="memory('MR')"
                            class="keyboard__button button_font_m button_disabled"><span>MR</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="memory('M-')"
                            class="keyboard__button button_font_m button_disabled"><span>M-</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="memory('M+')"
                            class="keyboard__button button_font_m button_disabled"><span>M+</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="operator('/')"
                            class="keyboard__button"><span>÷</span></button>
                </div>
            </div>
            <div class="keyboard__row">
                <div class="keyboard_button_wrap">
                    <button @click=""
                            class="keyboard__button button_font_m button_disabled"><span>%</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="number('7')"
                            class="keyboard__button"><span>7</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="number('8')"
                            class="keyboard__button"><span>8</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="number('9')"
                            class="keyboard__button"><span>9</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="operator('x')"
                            class="keyboard__button"><span>x</span></button>
                </div>
            </div>
            <div class="keyboard__row">
                <div class="keyboard_button_wrap">
                    <button @click=""
                            class="keyboard__button button_font_m button_disabled"><span>+/–</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="number('4')"
                            class="keyboard__button"><span>4</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="number('5')"
                            class="keyboard__button"><span>5</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="number('6')"
                            class="keyboard__button"><span>6</span></button>
                </div>
                <div class="keyboard_button_wrap">
                    <button @click="operator('-')"
                            class="keyboard__button"><span>-</span></button>
                </div>
            </div>
            <div class="keyboard__left">
                <div class="keyboard__row">
                    <div class="keyboard_button_wrap">
                        <button @click="clearLast()"
                                class="keyboard__button button_color_red button_font_m"><span>C</span></button>
                    </div>
                    <div class="keyboard_button_wrap">
                        <button @click="number('1')"
                                class="keyboard__button"><span>1</span></button>
                    </div>
                    <div class="keyboard_button_wrap">
                        <button @click="number('2')"
                                class="keyboard__button"><span>2</span></button>
                    </div>
                    <div class="keyboard_button_wrap">
                        <button @click="number('3')"
                                class="keyboard__button"><span>3</span></button>
                    </div>
                </div>
                <div class="keyboard__row">
                    <div class="keyboard_button_wrap">
                        <button @click="onAndClear()"
                                class="keyboard__button button_color_red button_font_m"><span>AC</span></button>
                        <div class="on">ON</div>
                    </div>
                    <div class="keyboard_button_wrap">
                        <button @click="number('0')"
                                class="keyboard__button"><span>0</span></button>
                    </div>
                    <div class="keyboard_button_wrap">
                        <button @click="number('.')"
                                class="keyboard__button"><span>.</span></button>
                    </div>
                    <div class="keyboard_button_wrap">
                        <button @click="equal()"
                                class="keyboard__button"><span>=</span></button>
                    </div>
                </div>
            </div>
            <div class="keyboard__right">
                <div class="keyboard_button_wrap">
                    <button @click="operator('+')"
                            class="keyboard__button button_size_l"><span>+</span></button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Calculator',
    data: function () {
        return {
            power: false,
            curOperator: '',
            prevResult: '',
            display: '',
            waitingOperand: false
        }
    },
    watch: {
        display: function() {
            if (this.power) {
                if (this.display === '') {
                    this.display = '0';
                }
				let displayString = this.display.toString();
				if (displayString.length > 9) {
					displayString = displayString.substring(0, 9); 					
					this.display = displayString;
				}
            } else {
                this.display = '';
            }
        }
    },
    methods: {
        operator: function(param) {
            this.curOperator = param;
            if (this.prevResult) {
                let res = eval(parseFloat(this.prevResult) + this.curOperator + parseFloat(this.display));
                this.prevResult = res;
            } else {
                this.prevResult = this.display;
            }
            this.waitingOperand = true;

        },
        number: function(param) {
            if (this.waitingOperand) {
                this.display = param;
                this.waitingOperand = false;
            } else {
                if (param == '.') {
                    this.display = this.display + param;
                } else {
                    this.display = this.display === '0' ? param : this.display + param;
                }

            }
        },
        equal: function() {
            switch (this.curOperator) {
                case '+':
                    this.display = parseFloat(this.prevResult) + parseFloat(this.display);
                    break;
                case '-':
                    this.display = parseFloat(this.prevResult) - parseFloat(this.display);
                    break;
                case 'x':
                    this.display = parseFloat(this.prevResult) * parseFloat(this.display);
                    break;
                case '/':
                    this.display = parseFloat(this.prevResult) / parseFloat(this.display);
                    break;
                case '.':
                    this.display = parseFloat(this.prevResult) / parseFloat(this.display);
                    break;
            }

            this.curOperator = '';
            this.prevResult = '';
            this.waitingOperand = true;
        },
        clearLast: function() {
            if (this.power) {
                if (this.display.length > 1) {
                    let clear = this.display.substring(0, this.display.length - 1);
                    this.display = clear;
                } else {
                    this.onAndClear();
                }
                this.curOperator = '';
            }

        },
        onAndClear: function() {
            if (this.power == false) {
                this.power = true;
                this.display = '0';
            } else {
                this.display = '0';
            }
            this.curOperator = '';

        },
        turnOff: function() {
            this.display = '';
            this.power = false;
        }
    }
}
</script>
