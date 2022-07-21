<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
            <div class="addition">
               <input
                                type="text"
                                v-model="task"
                                placeholder="Enter task"
                                s
                            />
            <button class="btn btn-warning" @click="submitTask">
                                Add <span>+</span>
                            </button>
                            </div>
                            <div class="top-part">
                <div class="card-main" v-for="(task, index) in tasks" :key="index" >
                   
                            
                        <div class="inner-cont">
                            <div  class="upper-part">
                                <div>
                                     <h2>  {{ task.name }}</h2>
                                </div>
                                <div @click="deleteTask(index)" class="delete">delete</div>
                            </div>

                            <div class="bottm-part">
                              <div class="addition new-add" >
                                 <button class="btn btn-warning" @click="$vm2.open('modal-3')">
                                Add more
                              </button>

                               </div>
                              <div class="direct">
                            
                                <a href="#">></a>
                                    <div id="app">
                   
                        <modal-vue
                            @on-close="$vm2.close('modal-3')"
                            name="modal-3"
                            noHeader
                            :footerOptions="{
                                btn1: 'Cancel',
                                btn2: 'Add',
                                btn2Style: {
                                    backgroundColor: 'red',
                                },
                                btn2OnClick: () => {
                                    doAdd();
                                },
                                btn1OnClick: () => {
                                    $vm2.close('modal-3');
                                },
                            }"
                        >
                            <div>
                                <p>Add title</p>
                                <input placeholder="Add title" />
                                  <input placeholder="Add Description" />
                            </div>
                        </modal-vue>
                    </div>
                              </div>
                            </div>

                     
                        </div>

                    </div>

                   
                </div>
                </div>
                 
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "HelloWorld",
    props: {
        msg: String,
    },
    data() {
        return {
            task: "",
            editedTask: null,
            tasks: [{
                name:"title"
            }
      
            ],
        };
    },
    methods: {
        deleteTask(index) {
            this.tasks.splice(index, 1);
        },
        submitTask() {
            if (this.task.length === 0) return;
            /* We need to update the task */
            if (this.editedTask != null) {
                this.tasks[this.editedTask].name = this.task;
                this.editedTask = null;
            } else {
                /* We need to add new task */
                this.tasks.push({
                    name: this.task,
                    status: "todo",
                });
            }
            this.task = "";
        },
        doAdd() {
            alert("Data Added");
            this.$vm2.close("modal-1");
            this.$vm2.close("modal-3");
        },
        close() {
            this.$vm2.close("modal-4");
        },
    },
};
</script>
<style>
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

.upper-part {
    background: #fb7d44;
    display: flex;
    align-items: center;
    padding: 5px 20px;
}

.upper-part h2 {
    font-size: 20px;
    margin: 0;
    text-transform: capitalize;
    font-weight: 700;
}

.delete {
    margin-left: auto;
}

.bottm-part p {
    margin: 0;
}

.bottm-part {
    text-align: left;
    padding: 10px 20px;
    background: #f2f2f2;
    border-bottom: 1px solid #fff;
    display:flex;
    align-items:center;
}

.direct {
    margin-left: auto;
    font-weight: 700;
}


.bottm-part:hover .direct a{
    display:block;
}

.direct a {
    text-decoration: none;
    display:none;
}

.card {
    max-width: 400px;
    width: 100%;
    margin: 0px 2px;
}



.input-part button {
    font-size: 13px;
    border-radius: 5px;
}

.input-part {
    display: flex;
    align-items: center;
    padding: 10px 20px;
}

.input-part .form-control {
    margin-right: 10px;
    font-size: 13px;
}

.bottm-part:last-child {
    border: 0px;
}

.bottm-part h5 {
    margin: 0;
}

.card-main {
    margin: 10px 0px 0px;
    height: 500px;
    overflow: auto;
    width: 300px;
    
    
}

.card-main{
margin-right: 20px;
}

.card-main:last-child {
    margin-right: 0;
}

.top-part {
    display: flex;
    align-items: center;
}

.addition {
    text-align: left;
}

.new-add {
    margin-top: 15px;
}

div#app {
    margin: 30px 0px 0px;
}

.addition button {
    box-shadow: none!important;
}

.addition input {
    height: 35px;
    font-size: 13px;
}



@media (max-width:767px){
    .card-main{display:block;}
    .inner-cont {margin:0px auto; max-width:100%;}

    .addition {
    margin-bottom: 20px;
}
}

</style>
