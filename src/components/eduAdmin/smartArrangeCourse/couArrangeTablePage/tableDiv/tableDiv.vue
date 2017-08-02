<template>
    <div id="couArrangeTable_tableDiv">
      <div>
        <button class="amButtom" @click="DivCtl('five')"><img id="fiveArrow" class="iconImg" :src="arrowright"><span class="subtitle">五年制</span></button>
        <div v-for="(item,index) in classList" v-if="item.yearType=='5'" id="fiveDiv">
          <div v-if="item.yearType=='5'" :id="'5Div' + index" class="gradePlanDiv">
            <span><img :id="'Arrow' + index" class="gradePlanImg" @click="tableSlideToggle(index)" :src="arrowright"></span>
            <span class="gradePlanTitle1">{{item.className}}</span>
            <span class="gradePlanTitle2">{{item.classHeadmaster}}</span>
            <span class="gradePlanTitle3">{{item.classroom}}</span>
            <span><button class="gradeButton" @click="reset(index)" :id="'reset'+index" style="display: none">重置</button></span>
          </div>
          <div v-if="item.yearType=='5'" :id="'Table'+index">
              <table class="arrangeTable">
              <thead>
              <tr>
                <th width="10%">节次/周次</th>
                <th width="14.8%">星期一</th>
                <th width="14.8%">星期二</th>
                <th width="14.8%">星期三</th>
                <th width="14.8%">星期四</th>
                <th width="14.8%">星期五</th>
              </tr>
              </thead>
              <tbody>
              <tr>
                <td>第一~二节<br>8:30-10:05</td>
                <td v-html="1" draggable="true" @drop="drop($event,this)" @dragover="allowdrop($event)" @dragstart="drag($event,this)"></td>
                <td v-html="2" draggable="true" @drop="drop($event,this)" @dragover="allowdrop($event)" @dragstart="drag($event,this)"></td>
                <td v-html=""></td>
                <td v-html=""></td>
                <td v-html=""></td>
              </tr>
              <tr>
                <td>第三~四节<br>10:20-11:55</td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html=""></td>
                <td v-html=""></td>
              </tr>
              <tr>
                <td>第五~六节<br>14:30-15:15</td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="">班会</td>
              </tr>
              <tr>
                <td>第八~九节<br>14:30-15:15</td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
              </tr>
              <tr>
                <td>第十~十一节<br>14:30-15:15</td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
                <td v-html="" ></td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
        <Modal
            v-model="modal1"
            width="400"
            :mask-closable="false"
            id="modalBody"
            :styles="{top:'10rem'}">
            <!--对话框宽400px，显示隐藏绑定属性变量，不允许点击遮罩层关闭对话框，对话框距离页面顶端10rem-->
            <div style="font-size: 1.1rem;text-align: center;">
                <p>{{ errorMessage }}</p>
            </div>
            <div slot="footer" style="text-align: center">
                <button id="modalBtn" @click="modal2 = false">确定</button>
            </div>
        </Modal>
    </div>

</template>

<script>
  import arrowright from "./images/arrowright.png"
  import arrowdown from "./images/arrowdown.png"
    export default {
        name: 'couArrangeTable_tableDiv',
        data () {
            return {
              srcdom:'',
              srcev:'',
              arrowright:arrowright,
              arrowdown:arrowdown,
              checked: 0,
              modal1: false,
              classList:[
                  {classId:'8222',yearType:'5',className:'高2015',classHeadmaster:'高老师',classroom:'1-205'}
              ],
              errorMessage: ""
            }
        },
        beforeMount: function() {
            this.$http.post('./autoArrangeSeeCurriculum',{
                "yearSemester": "",
                "week": ""
            },{
                "Content-Type":"application/json"
            }).then(function(response){
                this.items = response.body;
//                获取课表
            },function(error){
                this.$Message.error("连接失败，请重试！");
            });
        }, //页面dom加载前获取后端数据
        methods:
          {
            drop:function (event,dom) {
              event.preventDefault();
              console.log(event.target);
            },
            allowdrop:function (event) {
                event.preventDefault();
              console.log(event.target);
            },
            drag:function (event,dom) {
                this.srcdom = dom;
                this.srcev = event;
              console.log(event.target);
            },
          }
    }
</script>

<style scoped>
    #couArrangeTable_tableDiv{
        position: relative;
    }
    #exchangeButton{
        /*调课按钮*/
        position: absolute;
        top: -3rem;
        right: 1.5rem;
    }
    #checkCourseTable{
        /*课表*/
        position: relative;
        margin: 0.5rem auto;
        width: 97%;
        border: thin solid #d4d4d9;
        border-collapse: collapse;
        text-align: center;
    }
    #checkCourseTable td{
        border: thin solid #d4d4d9;
        /*max-width: 3rem;*/
    }
    .headTr td{
        /*表头*/
        height: 1rem;
    }
    #checkCourseTable td input{
        /*课表内容*/
        outline: none;
        border: none;
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 0;
        text-align: center;
    }
    #checkCourseTable td:hover{
        cursor: pointer;
    }
    #headTdTime{
        /*表头"表头"*/
        text-align: right;
        border: none !important;
        width: 8.3rem;
    }
    #headTdCourse{
        /*表头"课程"*/
        border: none !important;
    }
    #headTdClass{
        /*表头"班级"*/
        padding-left: 0.5rem;
        text-align: left;
        border: none !important;
    }
    #headTdTime:after{
        /*表头分割线*/
        content: "";
        position: absolute;
        width: 0.05rem;
        height: 6rem;
        top: 0;
        left: 4rem;
        background-color: #d4d4d9;
        display: block;
        transform: rotate(-47deg);
        -ms-transform: rotate(-47deg);/* IE 9 */
        -webkit-transform: rotate(-47deg);/* Safari and Chrome */
        -o-transform: rotate(-47deg);/* Opera */
        -moz-transform: rotate(-47deg);/* Firefox */
        transform-origin: top;
    }
    #headTdClass:before{
        /*表头分割线*/
        content: "";
        position: absolute;
        width: 0.05rem;
        height: 8.75rem;
        top: 2rem;
        left:0;
        background-color: #d4d4d9;
        display: block;
        transform: rotate(-76deg);
        -ms-transform: rotate(-76deg);/* IE 9 */
        -webkit-transform: rotate(-76deg);/* Safari and Chrome */
        -o-transform: rotate(-76deg);/* Opera */
        -moz-transform: rotate(-76deg);/* Firefox */
        transform-origin: top;
    }
    #courseDetailP{
        padding-left: 1rem;
        margin-bottom: 0;
    }
    #courseDetailTable{
        position: relative;
        margin: 0.5rem auto;
        width: 97%;
        border-collapse: collapse;
        table-layout: fixed;
    }
    @media screen and (max-width:1025px) {
        #headTdTime:after{
            width: 0.1rem;
            height: 6.7rem;
            transform: rotate(-43deg);
            -ms-transform: rotate(-43deg);/* IE 9 */
            -webkit-transform: rotate(-43deg);/* Safari and Chrome */
            -o-transform: rotate(-43deg);/* Opera */
            -moz-transform: rotate(-43deg);/* Firefox */
        }
        #headTdClass:before{
            width: 0.1rem;
            height: 9rem;
            transform: rotate(-71.3deg);
            -ms-transform: rotate(-71.3deg);/* IE 9 */
            -webkit-transform: rotate(-71.3deg);/* Safari and Chrome */
            -o-transform: rotate(-71.3deg);/* Opera */
            -moz-transform: rotate(-71.3deg);/* Firefox */
        }
    }
    @media screen and (min-width:1025px) and (max-width:1173px) {
        #headTdTime:after{
            width: 0.1rem;
        }
        #headTdClass:before{
            width: 0.1rem;
        }
    }
</style>
