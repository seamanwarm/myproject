<template>
    <div name="perosonmsg" class="person">
        <a href="#"><img class="small"  :src="userinfo.profile.avatarUrl?userinfo.profile.avatarUrl:'static/img/head.svg'"/></a>
        <router-link to="/login/" v-if="Nlogin"><span class="padding10" tabindex="1">{{userinfo.name?userinfo.name:"未登录"}}</span></router-link>
        <span v-else @click="display"     tabindex="1">{{userinfo.profile.nickname?userinfo.profile.nickname:''}}</span>
        <em  @click="display" @blur="display" tabindex="1"></em>
        <div v-show="show" name="downlist" id="downlist">
        

        <dl class="content">
            <dt>
                <img class="headpic" :src="userinfo.profile.avatarUrl?userinfo.profile.avatarUrl:'static/img/head.jpg'">
                <span>{{userinfo.profile.nickname?userinfo.profile.nickname:''}}</span>
                <button>签到</button>
                <div>
                    <span><em>{{userinfo.profile.eventCount?userinfo.profile.eventCount:""}}</em>动态</span>
                    <span><em>{{userinfo.profile.follows?userinfo.profile.follows:""}}</em>关注</span>
                    <span><em>{{userinfo.profile.followeds?userinfo.profile.followeds:""}}</em>粉丝</span>
                </div>
            </dt>
            <dd>
                <img src="../assets/member.png">
                <span>会员中心</span>
                <span class="intro">未订购</span>
            </dd>
            <dd>
                <img src="../assets/level.png">
                <span>等级</span>
                <span class="intro">Lv.{{userinfo.level?userinfo.level:""}}</span>
            </dd>
            <dd class="dd-bottom">
                <img src="../assets/buy.png">
                <span>积分商城</span>
                <span class="intro">12积分</span>
            </dd>
            <dd>
                <img src="../assets/setting.png">
                <span>个人信息设置</span>
            </dd>
            <dd>
                <img src="../assets/phone.png">
                <span>绑定社交账号</span>
            </dd>
            <dd class="dd-bottom">
                <img src="../assets/in.png">
                <span>导入歌单</span>
            </dd>
            <dd @click="quit" >
                <img  @click="quit"  src="../assets/end.png">
                <span @click="quit" >退出登录</span>
            </dd>
        </dl>
        </div>
    </div>
</template>
<script >
import { setCookie,getCookie,delCookie } from 'assets/js/cookie.js'
import Vue from 'vue'
export default{
  data () {
    return {
      app: '网易云音乐',
      show: false,
      userinfo:{
        name:"",
        headpic:"",
        active:"",
        attention:'',
        fans:"",
        profile:{
          avatarUrl:""
        }
      },
       Nlogin:true
    }
  },
   mounted(){
            /*页面挂载获取保存的cookie值，渲染到页面上*/
            let userid = getCookie('userid')
            if(userid){
                this.Nlogin=false
                Vue.http.get('newapi/user/detail?'+'uid='+userid).then(res => {
                  
                this.userinfo=JSON.parse(res.bodyText)
                 
              }) 
            }else
            {
               this.Nlogin=true 
            }
            
            /*this.name = uname*/
            /*如果cookie不存在，则跳转到登录页*/
           /* if(uname == ""){
                this.$router.push('/')
            }*/
        },
    methods:{
            quit(){
                /*删除cookie*/
                delCookie('userid')
                console.log("退出登录成功")
                 Vue.http.get("/newapi/login/refresh").then(res=>{
                    console.log(res)
                    this.$router.push('/')
                })
                 
            },
            display: function () {
               this.show = !this.show
          }
        }
}
</script>
<style scoped>
a, a:hover, a:visited, a:link {
    text-decoration: none;
    outline: none;
    color: #fff;
    
    
}
.padding10{
    padding: 10px;

}
*{
    box-sizing: border-box;
}
.person{
    width: 270px;
    font-size: 13px;
    display: inline-block;
    text-align: center;
    position: relative;
}

.small{
    width: 25px;
    height: 25px;
    border-radius: 25px;
    vertical-align: top;
}

.person > span{
    font-size: 13px;
    line-height: 25px;
    color: white;
    text-overflow: ellipsis;
    overflow: hidden;
    width: 75px;
    white-space: nowrap;
    display: inline-block;
    position: relative;
    outline: none;
    cursor: pointer;
}

.person > em{
    display: inline-block;
    height:0px; 
    width:0px;
    position: absolute;
    left: 200px;
    top: 10px;
    color: black;
    border-left: 6px solid transparent;
    border-right: 6px solid transparent;
    border-top: 5px solid white;
    box-sizing: content-box;
    outline: none;
    cursor: pointer;
}

#downlist{
    width: 100%;
    position: relative;
    z-index: 100;
    top: -5px;
}

#downlist > em{
    display: block;
    margin: 0 auto;
    width: 0px;
    border-bottom: 11px white solid;
    border-left: 10px transparent solid;
    border-right: 10px transparent solid;
}

.content{
    width: 100%;
    background-color: white;
    box-shadow: 0px 2px 4px rgb(225,225,226);
    border-radius: 5px;
}

dt{
    font-size: 15px;
    padding: 10px 20px;
    border-bottom: 1px solid rgb(233,233,233);
    text-align: left;
}

dd{
    font-size: 13px;
    color: black;
    padding: 8px 20px;
    height: 40px;
    text-align: left;
    line-height: 24px;
    position: relative;
}

dd:hover{
    background-color: rgb(236,237,238);
    cursor: pointer;
}

.headpic{
    width: 40px;
    height: 40px;
    border-radius: 40px;
    vertical-align: top;
}

dt > span{
    line-height: 40px;
    padding: 0 5px;
    width: 120px;
    overflow: hidden;
}

dt button{
    font-size: 12px;
    border: 1px solid rgb(236,237,238);
    background-color: white;
    text-align: right;
    width: 60px;
    padding: 4px 10px;
    background-image: url('../assets/sign.png');
    background-size: 15px 18px;
    background-repeat: no-repeat;
    background-position: 5px 2px;
    border-radius: 5px;
}

dt > div{
    overflow: hidden;
    text-align: center;
    margin: 10px 0;
}

dt > div span{
    width:33.3%;
    display: inline-block;
    float: left;
}

dt > div span:nth-child(2){
    border-left: 1px solid rgb(225,225,226);
    border-right: 1px solid rgb(225,225,226);
}

dt > div em{
    display: block;
    font-weight: bolder;
    margin: 5px;

}

dd > img{
    width: 20px;
    height: 20px;
    opacity: 0.5;
    vertical-align: top;
    margin-right: 10px;
}

dd:after{
    content: '';
    position: absolute;
    background-image: url('../assets/right.png');
    top: 15px;
    right: 15px;
    width: 10px;
    height: 10px;
    background-size: cover;
    opacity: 0.5;
}

.intro{
    display: inline-block;
    float: right;
    position: relative;
    right: 10px;
    color: rgb(136,136,136);
}

.dd-bottom{
    border-bottom: 1px solid rgb(233,233,233);
}
</style>
