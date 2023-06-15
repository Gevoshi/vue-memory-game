
<template>
   <div class="container" id="app">
        <h1 class="title">{{title}}</h1>
        <h3 class="count">Points : {{count}}</h3>
        <button 
            class="btn1" 
            @click="startTime"  
            :disabled="buttonDisabled"
            :class="{'disable-hover': buttonDisabled}">
            Start
        </button>
        <div class="place">
            <div 
                v-for="(box, index) in boxes" 
                :key="box" 
                class="box" 
                :class="{'flip' : box.show, 'box--disabled': !gameStatus}"
                @click="flip(index)">     
                <div class="front"></div>
                <div class="back" :class="box.value"></div>
            </div>
        </div>
        <h3 class="time"
            :class="{'littleTime' : timeColor}"> 
            {{timeValue}} : second
        </h3>
        <div class="timeSetter">
            <input 
                type="number" 
                min="10" 
                max="60"
                class="input" 
                v-model="inputValue" 
                @keyPress.enter="inputHandler" 
                :disabled="buttonDisabled">
            <button
                class="btn2"
                @click="inputHandler" 
                :disabled="buttonDisabled"
                :class="{'disable-hover': buttonDisabled}">
                Enter
            </button>
        </div>
    </div>
</template>

<script>

export default {

  data: () => ({
        title: 'Vue Memory Game',
        count: 0,
        timeValue: 30,
        timeColor: false,
        inputValue: 30,
        clickCount: 1,
        gameStatus: false,
        firstClick: '',
        secondClick: '',
        intervalId: '',
        checkTimeOut: '',
        buttonDisabled: false,
        boxes: Array(10).fill().map((_, idx) => {
            const key = (idx % 5) + 1
            return { value: `back${key}`, show: false, key }
        }).sort(() => Math.random() - .5)
    }), 

    methods: {
        inputHandler(){
        this.timeValue = parseInt(this.inputValue)
        }, 
            
        

        startTime(){
            this.gameStatus = true; 
            this.buttonDisabled = true;
            clearInterval(this.intervalId)
            this.intervalId = setInterval(() => {
                if(this.timeValue === 0){
                    clearInterval(this.intervalId);
                    this.endGame();
                    this.buttonDisabled = false;
                    this.timeValue = this.inputValue 
                }
                else{
                    this.timeValue--;
                    if(this.timeValue <= 5){
                        this.timeColor = true;
                    }
                }
            }, 1000);
        },

        flip(index){
            if(this.gameStatus ){
                if(this.boxes[index].show === false && this.clickCount === 1){
                    this.firstClick = this.boxes[index];
                    this.boxes[index].show = true
                    this.clickCount++
                }
                else if(this.boxes[index].show === false && this.clickCount === 2){
                    this.clickCount++;
                    this.secondClick = this.boxes[index]; 
                    this.boxes[index].show = true;
                    checkTimeOut = setTimeout( () => {
                        this.check()
                        clearTimeout(checkTimeOut)
                    }, 1000) ;
    
                }
            }
        },

        check(){
            if(this.firstClick.key === this.secondClick.key){
                this.count++;
                this.clickCount = 1;
                this.checkEnd()
            }
            else{
                this.firstClick.show =  !this.firstClick.show
                this.secondClick.show = !this.secondClick.show
                this.firstClick = '';
                this.secondClick = '';
                this.clickCount = 1;
            }
        },

        checkEnd(){
            if(this.count === 5){
                alert('You Win'),
                this.cleanToDefault()
            }
        },

        endGame(){
            if(this.count === 5){
                alert('You Win'),
                this.cleanToDefault()
            }
            else{
                alert('You Lose'),
                this.cleanToDefault()
            }
        },

        cleanToDefault(){
            this.count = 0,
            this.timeValue = 30,
            this.inputValue = 30,
            this.gameStatus = false,
            this.buttonDisabled = false,
            this.timeColor = false,
            this.firstClick = '';
            this.secondClick = '';
            this.boxes = Array(10).fill().map((_, idx) => {
                const key = (idx % 5) + 1
                return { value: `back${key}`, show: false, key }
            }).sort(() => Math.random() - .5),
            clearInterval(this.intervalId)

        }
    },

    watch: {
        inputValue(){
            if(this.inputValue > 60 || this.inputValue < 10 || this.inputValue === ''){
               parseInt(this.inputValue = 30);
            }
        }
    }
}
</script>

<style >
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: rgb(91, 133, 170);
}

.btn1 {
    margin: 0 auto;
    margin-bottom: 1rem;
    padding: .3rem 1rem;
    border-radius: 1rem;
    text-align: center;
    display: block;
    transition: all 300ms ease;
}

.btn1:hover {
    cursor: pointer;
    background-color: yellow;
    transition: all 300ms ease;
}

.disable-hover {
    pointer-events: none;
}

.container { 
    max-width: 100%;
    max-height: 100vh;
}

.place {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.box {
    width: 100px;
    height: 200px;
    margin: 5px;
    position: relative;
    transition: all 1000ms ease;
    transform-style: preserve-3d;
    cursor: pointer;
}

.flip {
    transform: rotateY(180deg);
    transition: all 1000ms ease;
}

.box--disabled {
    pointer-events: none;
    opacity: 0.5;
}

.front,
.back1,
.back2,
.back3,
.back4,
.back5 {
    width: 100%;
    height: 100%;
    border-radius: 1rem;
    box-shadow: 0 0 5px 2px rgba(50, 50, 50, 0.25);
    position: absolute;
    backface-visibility: hidden;
    cursor: pointer;
}

.front {
    background-image: url('pictures/3410304f458531e7854b615b0d94e65f.jpg');
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
}

.back {
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    transform: rotateY(180deg);
}

.back1 {
    background-image: url('randomPhoto/photo-1481349518771-20055b2a7b24.jpg');
}

.back2 {
    background-image: url('randomPhoto/istockphoto-1405528417-170667a.jpg');
}

.back3 {
    background-image: url('randomPhoto/interestingheader.jpg');
}

.back4 {
    background-image: url('randomPhoto/29140-interesting-business-image.jpg');
}


.back5 {
    background-image: url('randomPhoto/photo-1493612276216-ee3925520721.jpg');
}

.title {
    font-family: 'Courier New', Courier, monospace;
    text-align: center;
    color: #A9166C;
    padding: 3rem 0 1rem 0;
}

.count {
    text-align: center;
    color: #EBCF8D;
    padding-bottom: 2rem;
}

.time {
    margin-top: 1rem;
    text-align: center;
    color: rgb(125, 255, 125);
}

.littleTime {
    color: red;
}

.timeSetter {
    padding-top: 3rem;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: .2rem;
}

.input {
    padding: .3rem 3px;
    outline: none;
    border-radius: 1rem;
}

.btn2 {
    padding: .3rem 1rem;
    border-radius: 1rem;
    text-align: center;
    transition: all 300ms ease;
}

.btn2:hover {
    cursor: pointer;
    background-color: yellow;
    transition: all 300ms ease;
}
</style>
