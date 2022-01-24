<template>
  <div>
    <el-row :span="10">
      <el-col :span="8">
        <br>
      <el-card >
        <div style="text-align: center;color: darkturquoise">
          指导老师：付冬
        </div>

      </el-card>
      </el-col>
      <el-col :span="8">
        <el-card>
          <div style="text-align: center;color: #409eff">
            课题：基于springboot的疫情管理系统
          </div>
        </el-card>
      </el-col>
      <el-col :span="8">
        <br>
        <el-card>
          <div style="text-align: center;color: blueviolet">
            课题人：谭霖庆
          </div>
        </el-card>
      </el-col>

    </el-row>
    <br><br><br><br><br><br><br><br>
    <el-row>
      <el-col :span="15">
        <div id="main" style="width: 500px; height: 400px"></div>
      </el-col>
      <el-col :span="9">
        <div id="pie" style="width: 500px; height: 400px"></div>
      </el-col>
    </el-row>

</div>
</template>

<script>
import * as echarts from 'echarts'
export default {
  name: "Home.vue",
  data(){
    return{

    }
  },
  mounted() {

    var option;
    option = {
      title:{
        text: "各个学院上报了健康填报人数",
        subtext: "实时更新",
        left:"center"

      },
      xAxis: {
        type: 'category',
        data: []
      },
      yAxis: {
        type: 'value'
      },
      series: [
        {
          data: [],
          type: 'line'
        },
        {
          data: [],
          type: 'bar'
        }
      ]
    };

    var chartDom = document.getElementById('main');
    var myChart = echarts.init(chartDom);
    this.request.get("http://localhost:9090/echarts/example").then(res=>{
      option.xAxis.data=res.data.x
      option.series[0].data=res.data.y
      option.series[1].data=res.data.y
      myChart.setOption(option);
    })


    //饼图

    var pieoption;

    pieoption = {
      title: {
        text: '健康人数比例',
        subtext: '实时更新',
        left: 'center'
      },
      tooltip: {
        trigger: 'item'
      },
      legend: {
        orient: 'vertical',
        left: 'left'
      },
      series: [
        {
          type: 'pie',
          radius: '60%',
          label:{            //饼图图形上的文本标签
            normal:{
              show:true,
              position:'inner', //标签的位置
              textStyle : {
                fontWeight : 300 ,
                fontSize : 14,    //文字的字体大小
                color: "#fff"
              },
              formatter:'{d}%'
            }
          },
          data: [

          ],
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }
      ]
    };
    var pieDom = document.getElementById('pie');
    var pieChart = echarts.init(pieDom);
    this.request.get("http://localhost:9090/echarts/health").then(res => {
      pieoption.series[0].data = [
        {name: "正常比例", value: res.data[0]},
        {name: "体温不正常比例（过高或者过低）", value: res.data[1]},
      ]
      pieChart.setOption(pieoption)
    })


  }

}
</script>

<style>

</style>