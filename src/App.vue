<template>
   
    <div class="container">
       
        <div class="row">
           
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
               
                <h1>Animations</h1>
                
                <hr>
                
                    <select v-model="alertAnimation" class="form-control">

                        <option value="fade">Fade</option>

                        <option value="slide">Slide</option>

                    </select>

                    <br>

                    <button class="btn btn-primary" @click="show = !show">Show Alert</button>

                    <br><br>

                        <transition :name="alertAnimation">

                            <div class="alert alert-info" v-if="show">This is a fade transition</div>

                        </transition>

                        <transition name="slide" type="animation">

                            <div class="alert alert-info" v-if="show">This is a slide transition</div>

                        </transition>

                        <transition name="fade" >

                            <div class="alert alert-info" v-if="show">This is a fade transition with "appear"</div>

                        </transition>

                        <transition enter-active-class="animated bounce" leave-active-class="animated shake">

                            <div class="alert alert-info" v-if="show">This is a bounce then shake transition with animate.css "animated" class</div>

                        </transition>

                        <transition :name="alertAnimation" mode="out-in">

                            <div class="alert alert-info" v-if="show" key="firstDiv">First Alert</div>

                            <div class="alert alert-warning" v-else key="secondDiv">Second Alert</div>

                        </transition>
                
                <br><br><br>
                
                <h3>Javascript Animations</h3>
                
                <hr>
                                
                    <button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>

                    <br><br>
                    
                        <transition

                           @before-enter="beforeEnter"
                           @enter="enter"
                           @after-enter="afterEnter"
                           @enter-cancelled="enterCancelled"

                           @before-leave="beforeLeave"
                           @leave="leave"
                           @after-leave="afterLeave"
                           @leave-cancelled="leaveCancelled"

                           :css="false"

                           >

                            <div style="width: 100px; height: 100px; background-color: lightgreen" v-if="load"></div>

                        </transition>
                  
                <br><br><br>
                      
                <h3>Dynamic Components (click button once then hit load above)</h3>
                
                <hr>
                
                   <button class="btn btn-primary" @click="selectedComponent == 'app-success-alert' ? selectedComponent = 'app-danger-alert' : selectedComponent = 'app-success-alert'">Toggle Components</button>
                   
                   <br><br>
                   
                   <transition name="fade" mode="out-in">
                       
                       <component :is="selectedComponent"></component>
                       
                   </transition>
                   
                <br><br><br>
                
                <h3>Transition Groups</h3>
                
                <hr>
                
                   <button class="btn btn-primary" @click="addItem">Add Item</button>
                   
                   <br><br>
                   
                   <ul class="list-group">
                   
                       <transition-group name="slide">

                           <li

                                class="list-group-item"
                                v-for="(number, index) in numbers"
                                @click="removeItem(index)"
                                style="cursor: pointer"
                                :key="number">

                                {{ number }}

                           </li>

                       </transition-group>
                   
                   </ul>
                   
                   <br><br><br>  

            </div>
            
        </div>
        
    </div>
    
</template>

<script>
    
    import DangerAlert from './DangerAlert.vue';
    import SuccessAlert from './SuccessAlert.vue';
    
    export default {
        
        data() {
            
            return {
                
                show: false,
                load: true,
                alertAnimation: 'fade',
                elementWidth: 100,
                selecetedComponent: 'app-success-alert',
                numbers: [1,2, 3, 4, 5]

            }
        },
        
        methods: {
            
            beforeEnter(el) {
                
                console.log('Before Enter');
                this.elementWidth = 100;
                el.style.width = this.elementWidth + 'px';
                
            },
            
            enter(el, done) {
                
                console.log('Enter');
                let round = 1;
                const interval = setInterval( () => { 
                    
                    el.style.width = (this.elementWidth + round * 10) + 'px';
                    round++;
                    
                    if (round > 20) { 
                        
                        clearInterval(interval);
                        
                        done();
                    
                    }
                
                }, 20);
                
            },
            
            afterEnter(el) {
                
                console.log('After Enter');
                
            },
            
            enterCanceled(el) {
                
                console.log('Enter Canceled');
                
            },
            
            beforeLeave(el) {
                
                console.log('Before Leave');
                this.elementWidth = 300;
                el.style.width = this.elementWidth + 'px';
                
            },
            
            leave(el, done) {
                
                console.log('Leave');
                let round = 1;
                const interval = setInterval( () => { 
                    
                    el.style.width = (this.elementWidth - round * 10) + 'px';
                    round++;
                    
                    if (round > 20) { 
                        
                        clearInterval(interval);
                        
                        done();
                    
                    }
                
                }, 20);
                
            },
            
            afterLeave(el) {
                
                console.log('After Leave');
                
            },
            
            leaveCanceled(el) {
                
                console.log('Leave Canceled');
                
            },
            
            addItem() {
                
                const pos = Math.floor(Math.random() * this.numbers.length);
                
                this.numbers.splice(pos, 0, this.numbers.length + 1);
                
            },
            
            removeItem(index) {
                
                this.numbers.splice(index, 1);
                
            }
            
        },
        
        components: {
            
            appDangerAlert: DangerAlert,
            appSuccessAlert: SuccessAlert
            
        }
    }
    
</script>

<style>
    
/*/// FADE TRANSITION ///*/
    
    .fade-enter {
        
        opacity: 0;
        
    }
    
    .fade-enter-active {
        
        transition: opacity 1s;
        
    }
    
    .fade-leave {
        
        /* opacity: 1; not necessary since 1 is the default for opacity */
        
    }
    
    .fade-leave-active {
        
        transition: opacity 1s;
        opacity: 0;
        
    }
    
    
/*/// SLIDE TRANSITION ///*/
    
    .slide-enter {
        
        opacity: 0;
        /* transform: translateY(20px); */ 
        
    }
    
    .slide-enter-active {
        
        animation: slide-in 1s ease-out forwards;
        transition: opacity .5s;
        
    }
    
    .slide-leave {}
    
    .slide-leave-active {
        
        animation: slide-out 1s ease-out forwards;
        transition: opacity 1s;
        opacity: 0;
        position: absolute;
        
    }
    
    .slide-move {
        
        transition: 1s;
        
    }
    
    @keyframes slide-in {
        
        from {
            
            transform: translateY(20px);
            
        }
        
        to {
            
            transform: translateY(0);
            
        }
        
    }
    
    @keyframes slide-out {
        
        from {
            
            transform: translateY(0);
            
        }
        
        to {
            
            transform: translateY(20px);
            
        }
        
    }

</style>