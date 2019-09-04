# Echarts

### 1.调整柱状图宽度以及之间的间距

```javascript
type: 'bar',
barWidth: '15%',//宽度
barGap:'10%',//间距
```
### 2.整个图表在容器内的位置

```javascript
grid: {
       left: '2%',
       right: '4%',
       bottom: '14%',
       top:'16%',
     },
```

### 3.图表上方标签参数

```javascript
 legend: {
     right: 10,//位置
     top:12,
     textStyle: {
         color: "#000"
     },
     itemWidth: 12,
     itemHeight: 10,
     itemGap: 35//间距
 }
```

### 4.图表Y轴刻度自适应，yAxis 设置为空即可。

```javascript
yAxis:{
    
},
 /*
 yAxis:{
   name: '单位（%）',//可设置y轴单位
 }
 */
```

