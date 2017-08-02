<template>
  <div id="courseArrangeSettingDiv">
    <div class="blank">
      <div class="positionBar">
        <span>您的当前位置：</span>
        <span><a href="#/login/main/eduAdminHome" class="returnHome">首页</a></span>
        <span> > 智能排课 > 课程设置</span>
      </div>
    </div>
    <div class="dropDown">
      <div>
        <button class="amButtom" @click="DivCtl('five')"><img id="fiveArrow" class="iconImg" :src="arrowright"><span class="subtitle">五年制</span></button>
        <div v-for="(item,index) in classList" v-if="item.studyMode=='5'" id="fiveDiv">
          <div v-if="item.studyMode=='5'" :id="'5Div' + index" class="gradePlanDiv">
            <span><img :id="'Arrow' + index" class="gradePlanImg" @click="tableSlideToggle(index)" :src="arrowright"></span>
            <span class="gradePlanP">{{item.className}}</span>
            <span><button class="gradeButton" @click="allSave(index)" :id="'allSave'+index" style="display: none">全部保存</button></span>
          </div>
          <div v-if="item.studyMode=='5'" :id="'Table'+index" style="display: none">
            <table :id="index+'Table'">
              <thead>
              <tr class="headTr">
                <td>课程名称</td>
                <td>课程编号</td>
                <td>任课教师</td>
                <td>教师编号</td>
                <td>是否合课</td>
                <td>操作</td>
              </tr>
              </thead>
              <tbody v-for="(items,Index) in courseList[index].courses">
              <tr>
                <td v-html="items.courseName"></td>
                <td v-html="items.courseId"></td>
                <td>
                  <select :id="index+'sel1'+Index" v-model="items.teacherId" disabled>
                    <option value="" disabled>选择老师</option>
                    <option v-for="tea in teacherList" :value="tea.teacherId">{{tea.teacherName}}</option>
                  </select>
                </td>
                <td v-html="items.teacherId"></td>
                <td>
                  <select :id="index+'sel2'+Index" v-model="items.AllowCombineLesson" disabled>
                    <option disabled value="">是否合课</option>
                    <option value="是">是</option>
                    <option value="否">否</option>
                  </select>
                </td>
                <td class="operationTd">
                  <img :id="index+'EdImg'+Index" src="../../../../../assets/images/edit.png" @click="edit(index,Index)" title="编辑">
                  <!--编辑功能，初始显示，编辑时隐藏-->
                  <img :id="index+'SaImg'+Index" class="saveImg" src="../../../../../assets/images/save.png" @click="saveClick(index,Index)" title="保存">
                  <!--保存功能，初始隐藏，编辑时显示-->
                  <img :id="index+'ReImg'+Index" class="restoreImg" src="../../../../../assets/images/restore.png" @click="recoveryClick(index,Index)" title="取消">
                  <!--取消编辑并重置，初始隐藏，编辑时显示-->
                </td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div>
        <button class="amButtom" @click="DivCtl('three')"><img id="threeArrow" class="iconImg" :src="arrowright"><span class="subtitle">三年制</span></button>
        <div v-for="(item,index) in classList" v-if="item.studyMode=='3'" id="threeDiv">
          <div v-if="item.studyMode=='3'" :id="'3Div' + index" class="gradePlanDiv">
            <span><img :id="'Arrow' + index" class="gradePlanImg" @click="tableSlideToggle(index)" :src="arrowright"></span>
            <span class="gradePlanP">{{item.className}}</span>
            <span><button class="gradeButton" @click="allSave(index)" :id="'allSave'+index" style="display: none">全部保存</button></span>
          </div>
          <div v-if="item.studyMode=='3'" :id="'Table'+index" style="display: none">
            <table :id="index+'Table'">
              <thead>
              <tr class="headTr">
                <td>课程名称</td>
                <td>课程编号</td>
                <td>任课教师</td>
                <td>教师编号</td>
                <td>是否合课</td>
                <td>操作</td>
              </tr>
              </thead>
              <tbody v-for="(items,Index) in courseList[index].courses">
              <tr>
                <td v-html="items.courseName"></td>
                <td v-html="items.courseId"></td>
                <td>
                  <select :id="index+'sel1'+Index" v-model="items.teacherId" disabled>
                    <option value="" disabled>选择老师</option>
                    <option v-for="tea in teacherList" :value="tea.teacherId">{{tea.teacherName}}</option>
                  </select>
                </td>
                <td v-html="items.teacherId"></td>
                <td>
                  <select :id="index+'sel2'+Index" v-model="items.AllowCombineLesson" disabled>
                    <option disabled value="">是否合课</option>
                    <option value="是">是</option>
                    <option value="否">否</option>
                  </select>
                </td>
                <td class="operationTd">
                  <img :id="index+'EdImg'+Index" src="../../../../../assets/images/edit.png" @click="edit(index,Index)" title="编辑">
                  <!--编辑功能，初始显示，编辑时隐藏-->
                  <img :id="index+'SaImg'+Index" class="saveImg" src="../../../../../assets/images/save.png" @click="saveClick(index,Index)" title="保存">
                  <!--保存功能，初始隐藏，编辑时显示-->
                  <img :id="index+'ReImg'+Index" class="restoreImg" src="../../../../../assets/images/restore.png" @click="recoveryClick(index,Index)" title="取消">
                  <!--取消编辑并重置，初始隐藏，编辑时显示-->
                </td>
              </tr>
              </tbody>
            </table>
          </div>
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
        <p>您确定取消编辑并重置该课程信息吗?</p>
      </div>
      <div slot="footer" style="text-align: center">
        <button id="modalBtn" @click="recoveryOK()">确定</button>
        <button id="modalBtn" @click="modal1 = false">取消</button>
      </div>
    </Modal>
    <Modal
        v-model="modal2"
        width="400"
        :mask-closable="false"
        id="modalBody"
        :styles="{top:'10rem'}">
      <!--对话框宽400px，显示隐藏绑定属性变量，不允许点击遮罩层关闭对话框，对话框距离页面顶端10rem-->
      <div style="font-size: 1.1rem;text-align: center;">
        <p>您确定提交保存该课程信息吗？</p>
      </div>
      <div slot="footer" style="text-align: center">
        <button id="modalBtn" @click="saveOK()">确定</button>
        <button id="modalBtn" @click="modal2 = false">取消</button>
      </div>
    </Modal>
    <Modal
      v-model="modal3"
      width="400"
      :mask-closable="false"
      id="modalBody"
      :styles="{top:'10rem'}">
      <!--对话框宽400px，显示隐藏绑定属性变量，不允许点击遮罩层关闭对话框，对话框距离页面顶端10rem-->
      <div style="font-size: 1.1rem;text-align: center;">
        <p>您确定保存所有编辑项吗？</p>
      </div>
      <div slot="footer" style="text-align: center">
        <button id="modalBtn" @click="allsaveOK()">确定</button>
        <button id="modalBtn" @click="modal3 = false">取消</button>
      </div>
    </Modal>
  </div>
</template>

<script>
  import arrowright from "./images/arrowright.png"
  import arrowdown from "./images/arrowdown.png"
  export default {
    name: 'courseArrangeSettingDiv',
    data () {
      return {
        arrowright:arrowright,
        arrowdown:arrowdown,
        classList:[
//          {classId:'85114',className:'高2015级5班',studyMode:'5'},
//          {classId:'85114',className:'高2015级5班',studyMode:'3'}
              ],
        teacherList:[
//            {teacherId:'0110',teacherName:'李桂园'},
//          {teacherId:'0111',teacherName:'谭咏麟'},
//          {teacherId:'0114',teacherName:'别越塔'}
          ],
        courseList:[
//          {courses:[
//            {courseName:'1',courseId:'3',teacherName:'李桂园',teacherId:'0110',AllowCombineLesson:'否'},
//            {courseName:'2',courseId:'3',teacherName:'谭咏麟',teacherId:'0111',AllowCombineLesson:'否'},
//            {courseName:'1',courseId:'3',teacherName:'李桂园',teacherId:'0110',AllowCombineLesson:'否'},
//            {courseName:'2',courseId:'3',teacherName:'谭咏麟',teacherId:'0111',AllowCombineLesson:'否'},
//            {courseName:'1',courseId:'3',teacherName:'李桂园',teacherId:'0110',AllowCombineLesson:'否'},
//            {courseName:'2',courseId:'3',teacherName:'谭咏麟',teacherId:'0111',AllowCombineLesson:'否'}]
//          },
//          {courses:[
//            {courseName:'1',courseId:'3',teacherName:'李桂园',teacherId:'0110',AllowCombineLesson:'否'},
//            {courseName:'2',courseId:'3',teacherName:'谭咏麟',teacherId:'0111',AllowCombineLesson:'否'}]
//          },
        ],
        modal1: false,
        modal2: false,
        modal3: false,
        modal4: false,
        eindex:'',
        aindex:'',
        eIndex:'',
        ebuffer:[]
      }
    },
//    mounted: function(){
//      var importButton1 = document.getElementById("importButton1");
//      var importButton2 = document.getElementById("importButton2");
//      importButton1.className = "am-btn am-btn-success am-radius";
//      importButton2.className = "am-btn am-btn-success am-radius";
//    },
    beforeMount: function() {
      this.$http.post('./courseAssociationManege',{},{
        "Content-Type":"application/json"
      }).then(function(response){
          this.classList = response.body.classList;
          this.teacherList = response.body.teacherList;
          for(var i=0;i <this.classList.length;i++)
          {
             this.courseList.push({courses:[]});
          }
      },function(error){
        this.$Message.error("网络错误,请稍后重试！");
      });
    },
    methods:{
      DivCtl:function (msg) {
        var div = document.getElementById(msg+'Div');
        var img = document.getElementById(msg+'Arrow');
        //console.log(msg+'Div');
        if(div.style.display == "none")
          {
            img.src = this.arrowdown;
            div.style.display = "inline";
          }else{
          img.src = this.arrowright;
          div.style.display = "none";
          }
      },
      tableSlideToggle:function (index) {
        var table = document.getElementById('Table'+index);
        var img = document.getElementById('Arrow'+index);
        if(table.style.display == "none")
        {
          this.$Loading.start();
          this.$http.post('./courseAssociationManege/showLessons',{
            classId:this.classList[index].classId
          },{
            "Content-Type":"application/json"
          }).then(function(response){
            this.courseList[index].courses = response.body.courseAssociationListForClass;
            this.$Loading.finish();
          },function(error){
            this.$Loading.error();
            this.$Message.error("网络错误,请稍后重试！");
          });
          img.src = this.arrowdown;
          table.style.display = "inline";
        }else{
          img.src = this.arrowright;
          table.style.display = "none";
        }
      },
      edit:function (index,Index) {
        var EdImg = document.getElementById(index+'EdImg'+Index);
        var SaImg = document.getElementById(index+'SaImg'+Index);
        var ReImg = document.getElementById(index+'ReImg'+Index);
        var sel1 = document.getElementById(index+'sel1'+Index);
        var sel2 = document.getElementById(index+'sel2'+Index);
        var button = document.getElementById('allSave'+index);
        EdImg.style.display = "none";
        SaImg.style.display = "inline";
        ReImg.style.display = "inline";
        sel1.removeAttribute("disabled");
        sel2.removeAttribute("disabled");
        button.style.display = "inline";
        this.ebuffer.push({head:index+'@'+Index,teacherId:sel1.value,AllowCombineLesson:sel2.value});
      },
      saveClick:function (index,Index) {
        this.eindex = index;
        this.eIndex = Index;
        this.modal2 = true;
      },
      saveOK:function () {
        this.$http.post('./courseAssociationManege/update',{
          courseAssociationVo:this.courseList[this.eindex].courses[this.eIndex]
        },{
          "Content-Type":"application/json"
        }).then(function(response){
            if(response.body.result == 1)
            {
              var EdImg = document.getElementById(this.eindex+'EdImg'+this.eIndex);
              var SaImg = document.getElementById(this.eindex+'SaImg'+this.eIndex);
              var ReImg = document.getElementById(this.eindex+'ReImg'+this.eIndex);
              var sel1 = document.getElementById(this.eindex+'sel1'+this.eIndex);
              var sel2 = document.getElementById(this.eindex+'sel2'+this.eIndex);
              EdImg.style.display = "inline";
              SaImg.style.display = "none";
              ReImg.style.display = "none";
              sel1.disabled="true";
              sel2.disabled="true";
              this.$Message.success("保存成功！");
            }else
            {
                this.$Message.error("保存失败,请稍后重试！");
            }
        },function(error){
          this.$Message.error("网络错误,请稍后重试！");
        });
      },
      recoveryClick:function (index,Index) {
        this.eindex = index;
        this.eIndex = Index;
        this.modal1 = true;
      },
      recoveryOK:function () {
        var heads = this.eindex+'@'+this.eIndex;
        for(var i=0;i<this.ebuffer.length;i++)
        {
            if(this.ebuffer[i].head == heads)
            {
              this.courseList[this.eindex].courses[this.eIndex].teacherId = this.ebuffer[i].teacherId;
              this.courseList[this.eindex].courses[this.eIndex].AllowCombineLesson = this.ebuffer[i].AllowCombineLesson;
              var EdImg = document.getElementById(this.eindex+'EdImg'+this.eIndex);
              var SaImg = document.getElementById(this.eindex+'SaImg'+this.eIndex);
              var ReImg = document.getElementById(this.eindex+'ReImg'+this.eIndex);
              var sel1 = document.getElementById(this.eindex+'sel1'+this.eIndex);
              var sel2 = document.getElementById(this.eindex+'sel2'+this.eIndex);
              EdImg.style.display = "inline";
              SaImg.style.display = "none";
              ReImg.style.display = "none";
              sel1.disabled="true";
              sel2.disabled="true";
              this.modal1 = false;
              this.$Message.success("编辑项已还原！");
              break;
            }
        }
      },
      allSave:function (index) {
        this.aindex = index;
        this.modal3 = true;
      },
      allsaveOK:function () {
        var list = [];
        for(var i= 0;i < this.courseList[this.aindex].courses.length;i++)
        {
          var EdImg = document.getElementById(this.aindex+'EdImg'+i);
          if(EdImg.style.display == "none")
          {
              list.push(this.courseList[this.aindex].courses[i]);
          }
        }
        this.$http.post('./courseAssociationManege/updateAll',{
          courseAssociationList:list
        },{
          "Content-Type":"application/json"
        }).then(function(response){
            if(response.body.result == 1)
            {
                this.statusRecovery();
                this.$Message.success("保存成功！");
            }else
            {
              this.$Message.error("保存失败,请稍后重试！");
            }
        },function(error){
          this.$Message.error("网络错误,请稍后重试！");
        });
      },
      statusRecovery:function () {
        for(var i= 0;i < this.courseList[this.aindex].courses.length;i++)
        {
          var EdImg = document.getElementById(this.aindex+'EdImg'+i);
          if(EdImg.style.display == "none")
          {
            var SaImg = document.getElementById(this.aindex+'SaImg'+i);
            var ReImg = document.getElementById(this.aindex+'ReImg'+i);
            var sel1 = document.getElementById(this.aindex+'sel1'+i);
            var sel2 = document.getElementById(this.aindex+'sel2'+i);
            EdImg.style.display = "inline";
            SaImg.style.display = "none";
            ReImg.style.display = "none";
            sel1.disabled="true";
            sel2.disabled="true";
          }
        }
      }
    }
  }
</script>

<style scoped>
  /*最上层Div*/
  #courseArrangeSettingDiv{
    margin: 0 auto;
    background-color: #f3f3f3;
    min-height: 38.5rem;
  }
  .dropDown{
    /*页面主要内容*/
    margin: 0.5rem 5rem;
  }
  table{
    width: 100%;
    margin: 0 auto;
    border-collapse: collapse;
    table-layout: fixed;
    border-right: thin solid #E3E3E3;
    border-left: thin solid #E3E3E3;
  }
  td{
    border-bottom: thin solid #E3E3E3;
    height: 2.5rem;
    text-align: center;
  }
  tbody td{
    height: 3.5rem;
  }
  img{
    position: relative;
    margin: 0.5rem 0.2rem;
    width: 1.5rem;
    height: 1.5rem;
    cursor: pointer;
  }
  input{
    outline:none;
    border: none;
    text-align: center;
  }
  .saveImg{
    /*保存功能图标*/
    display: none;
  }
  .restoreImg{
    /*重置功能图标*/
    display: none;
  }
  .operationTd{
    /*功能图标*/
    width: 15rem;
  }
  .portTd{
    /*下载和导入*/
    /*display: flex;*/
    padding: 0;
  }
  #upload{
    display: inline;
    float: right;
  }
  #downloadButton{
    /*五年制模版下载按钮*/
    height: 2.35rem;
  }
  #downloadButton3{
    /*三年制模版下载按钮*/
    height: 2.35rem;
  }
  .courseTd:hover > span{
    color: red;
  }
  .teacherTd:hover > span{
    color: red;
  }
  @media screen and (max-width: 1117px) {
    .am-btn{

    }
    #downloadButton{
      /*五年制模版下载按钮*/
      width: 4.5rem;
      padding: 0;
    }
    #downloadButton3{
      /*三年制模版下载按钮*/
      width: 4.5rem;
      padding: 0;
    }
  }
</style>
