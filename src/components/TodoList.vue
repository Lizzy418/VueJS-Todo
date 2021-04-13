<template>   
    <section>
        <transition-group name="list" tag="ul">
            <li v-for="(item,index) in propsdata" :key = "item['t_key']" @dblclick="updateTodo(index)">
                <i class="checkBtn fas fa-check" aria-hidden="true"></i>
                <!-- 수정 상태이고,번호가 현재 인덱스와 같으면 수정 창을 띄웁니다. -->
                <span v-if="edit_num === index && edit_state === true"><input class="editLi" type="text" v-model="edit_text" @keyup.enter="updateTodo(index)" ref="editInput"></span>
                <span v-else>{{item['item']}}</span>
                <span class="editBtn" type="button" @click="updateTodo(index)">
                    <i class ="fas fa-edit" aria-hidden="true"></i>
                </span>
                <span class="removeBtn" type="button" @click="removeTodo(index)">
                    <i class="far fa-trash-alt" aria-hidden="true"></i>
                </span>
            </li>
        </transition-group>
    </section>
</template>

<script>
export default {
    props: ['propsdata'],
    methods: {
        removeTodo(index) {
            //console.log(todoItem, index);
            //localStorage.removeItem(todoItem);
            //this.todoItems.splice(index, 1);
            this.$emit('removeTodo', index);
        },
        //todo update
        //현재 아이템 배열의 index를 매개변수로 받음
        updateTodo(index) {
            if(this.edit_state == true) {
                //edit num => 인덱스 올리고
                //edit_text => 고칠 내용 올리고
                //edit_state => false로 만듦
                this.$emit('updateTodo', this.edit_num, this.edit_text)
                //this.$refs.mainInput[0].focus();
            }
            else {
                this.edit_text = this.propsdata[index]['item'];
            }
            //고쳐야 할 곳의 인덱스를 현재 인덱스로
            this.edit_num = index;
            //현재 수정 상태 -> 일반 상태, 일반 상태 -> 수정 상태
            this.edit_state = !this.edit_state
        }
    },
    updated(){
        // 이벤트 업데이트 이후, 수정 창이 활성화 되어 있으면 포커스를 줍니다.
        if(this.edit_state === true) {
            this.$refs.editInput[0].focus();
        }
    },
    data() {
        return {
            edit_num : -1,
            edit_state: false,
            edit_text: '',
        }
    },

}
</script>

<style scoped>
    .list-enter-active, .list-leave-active {
        transition: all 1s;
    }
    .list-enter, .list-leave-to {
        opacity: 0;
        transform: translateY(30px);
    }
    ul {
        list-style-type: none;
        padding-left: 0px;
        margin-top: 0;
        text-align: left;
    }

    li {
        position: relative;
        display: flex;
        min-height: 50px;
        height: 50px;
        line-height: 50px;
        margin: 0.5rem 0;
        padding: 0 0.9rem;
        /* background: #ABD0CE; */
        color: #7C7877;
        background: linear-gradient(to right, #ABD0CE, #88b8b4);
        border-radius: 5px;
        font-family: 'Poor Story', cursive;
        font-weight: bold;

        width: 600px;
        margin-left:auto; margin-right:auto;
    }
    li:hover {
        background: rgb(115, 181, 184);
    }
    
    .editLi {
        border-style: none;
        height: auto;
        line-height: normal;
        padding: .6em .5em;
        font-family: 'Poor Story', cursive;
        font-weight: bold;
    }

    .editLi:focus {
        outline: 2px solid cadetblue;
    }

    .editBtn {
        position: absolute;
        right: 40px;
    }

    
    .checkBtn {
        line-height: 45px;
        color: #FCDF8A;
        margin-right: 5px;
    }
    .removeBtn {
        margin-left: auto;
        color: #cb7575;
        /* color: #de4343; */
    }
</style>