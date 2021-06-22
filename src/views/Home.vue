<template>
  <div class="home">
    g2测试
    <div id="container"> </div>
    <div id="pie"> </div>
    <div id="line"> </div>
    <div id="basic-line"> </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import {Chart} from '@antv/g2';
export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  data(){
    return{

    }
  },
  mounted(){

    //柱状图
    this.setZhu();
    //饼图
    this.setPie();
    //折线图
    this.setLine();
    //basicLine
    this.setBasicLine();


  },
  methods:{
    setZhu(){
      const data = [
        { year: '1951 年', sales: 38},
        { year: '1952 年', sales: 52},
        { year: '1952 年', sales: 52},
        { year: '1956 年', sales: 61},
        { year: '1957 年', sales: 145},
        { year: '1958 年', sales: 48},
        { year: '1959 年', sales: 38},
        { year: '1960 年', sales: 38},
        { year: '1962 年', sales: 38},
      ];
      //chat初始化
      const chart = new Chart({
        container: 'container',
        autoFit: false,//如果给true，可以不给宽度
        width:500,
        height: 500,
      });
      //设置数据
      chart.data(data);


      //使用图形语法进行图表的绘制
      //这部分实际上就像提供了一堆管道符一样
      //axis的控制是由scala度量控制的 
      chart.scale('sales', {
        alias: '销售量',
        //纵轴label本身是默认的，但是我们也可以自己设置
        ticks:[0, 20, 40, 60, 80, 100, 120, 140, 160, 180, ],
        //label可以设置展示样式
        formatter:(val)=>`￥${val}`,
        //还可以设置坐标轴数值范围
        min:0,
        max:200,
        tickCount: 10,//刻度线个数，貌似没啥用
        tickInterval:180,//刻度线间距
      });
      //横轴 
      chart.scale('year',{
        //两端留白
        range:[0.1,0.9]
      });

      //图例的设置
      //隐藏全部图例
      /* chart.legend(false); */
      //只隐藏x轴对应的图例
      // chart.legend("year",false);
      // 图例在特定位置显示
      chart.legend("year",{
        position:"right"
      })

      //这部分相当于标签的出现，上边的是配置
      chart.axis('sales', {
        title: {},
      });

      //提示信息
      chart.tooltip({
        showMarkers: false
      });


    
      chart.interaction('active-region');

      //可以对一维坐标、二维坐标或者三维坐标进行处理
      //chart.interval().position('year*sales').color("year");
      //对二维进行处理
      chart.interval()
      .position('year*sales')
      .color('year*sales',(year,sales)=>{
        if(year == "1951 年"){
          return "blue";
        }
        if(sales == 38){
          return "red"
        }
        //剩下的返回紫色
        return "purple"
      })


      //end：最后画图
      chart.render();
    },
    setPie(){
      const data = [
        { item: '事例一', count: 40, percent: 0.4 },
        { item: '事例二', count: 21, percent: 0.21 },
        { item: '事例三', count: 17, percent: 0.17 },
        { item: '事例四', count: 13, percent: 0.13 },
        { item: '事例五', count: 9, percent: 0.09 },
      ];

      const chart = new Chart({
        container: 'pie',
        autoFit: true,
        height: 500,
      });

      //坐标系的选择，决定了它以怎样的图显示
      //默认react
      chart.coordinate('theta', {
        radius: 0.75,
        //加内部半径就可以换程环状图，去掉就是饼图
        innerRadius: 0.6,
      });

      chart.data(data);

      chart.scale('percent', {
        formatter: (val) => {
          val = val * 100 + '%';
          return val;
        },
      });

      chart.tooltip({
        showTitle: false,
        showMarkers: false,
      });

      chart
        .interval()
        //一个一个
        .position('percent')
        .color('item')
        .label('percent', {
          content: (data) => {
            return `${data.item}: ${data.percent * 100}%`;
          },
        })
        .adjust('stack');

      chart.interaction('element-active');

      chart.render();

    },
    setLine(){
      const data = [
        { month: 'Jan', city: 'Tokyo', temperature: 7 },
        { month: 'Jan', city: 'London', temperature: 3.9 },
        { month: 'Feb', city: 'Tokyo', temperature: 6.9 },
        { month: 'Feb', city: 'London', temperature: 4.2 },
        { month: 'Mar', city: 'Tokyo', temperature: 9.5 },
        { month: 'Mar', city: 'London', temperature: 5.7 },
        { month: 'Apr', city: 'Tokyo', temperature: 14.5 },
        { month: 'Apr', city: 'London', temperature: 8.5 },
        { month: 'May', city: 'Tokyo', temperature: 18.4 },
        { month: 'May', city: 'London', temperature: 11.9 },
        { month: 'Jun', city: 'Tokyo', temperature: 21.5 },
        { month: 'Jun', city: 'London', temperature: 15.2 },
        { month: 'Jul', city: 'Tokyo', temperature: 25.2 },
        { month: 'Jul', city: 'London', temperature: 17 },
        { month: 'Aug', city: 'Tokyo', temperature: 26.5 },
        { month: 'Aug', city: 'London', temperature: 16.6 },
        { month: 'Sep', city: 'Tokyo', temperature: 23.3 },
        { month: 'Sep', city: 'London', temperature: 14.2 },
        { month: 'Oct', city: 'Tokyo', temperature: 18.3 },
        { month: 'Oct', city: 'London', temperature: 10.3 },
        { month: 'Nov', city: 'Tokyo', temperature: 13.9 },
        { month: 'Nov', city: 'London', temperature: 6.6 },
        { month: 'Dec', city: 'Tokyo', temperature: 9.6 },
        { month: 'Dec', city: 'London', temperature: 4.8 },
      ];

      const chart = new Chart({
        container: 'line',
        autoFit: false,
        width:500,
        height: 500,
      });

      chart.data(data);
      chart.scale({
        month: {
          range: [0, 1],
        },
        temperature: {
          nice: true,
        },
      });

      chart.tooltip({
        showCrosshairs: true,
        shared: true,
      });

      chart.axis('temperature', {
        label: {
          formatter: (val) => {
            return val + ' °C';
          },
        },
      });
      //这个画线
      chart
        .line()
        //两个作为坐标，剩下一个就是具体数据了
        .position('month*temperature')
        .color('city')
        .shape('smooth');
      //这个画点
      chart
        .point()
        .position('month*temperature')
        .color('city')
        .shape('circle');

      chart.render();

    },
    setBasicLine(){
      const data = [
        { year: '1991', value: 3 },
        { year: '1992', value: 4 },
        { year: '1993', value: 3.5 },
        { year: '1994', value: 5 },
        { year: '1995', value: 4.9 },
        { year: '1996', value: 6 },
        { year: '1997', value: 7 },
        { year: '1998', value: 9 },
        { year: '1999', value: 13 },
      ];
      const chart = new Chart({
        container: 'basic-line',
        autoFit: false,
        width:500,
        height: 500,
      });

      chart.data(data);
      chart.scale({
        year: {
          range: [0, 1],
        },
        value: {
          min: 0,
          nice: true,
        },
      });

      chart.tooltip({
        showCrosshairs: true, // 展示 Tooltip 辅助线
        shared: true,
      });

      chart.line().position('year*value').label('value');
      chart.point().position('year*value');

      chart.render();


    }

  },




}
</script>
