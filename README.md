<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>稀土元素回收分析</title>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.3/dist/echarts.min.js"></script>
</head>
<body>
    <div id="main" style="width: 1000px;height:600px;"></div>
    <script type="text/javascript">
        var chartDom = document.getElementById('main');
        var myChart = echarts.init(chartDom);
        
        // 数据转换（范围取中间值，成熟度量化）
        const data = [
            { 
                name: '镧（La）',
                value: [10, 17.5, 7.5, 3],  // 回收成本、市场价格、利润空间、技术成熟度（高=3）
                app: '催化剂、玻璃添加剂'
            },
            { 
                name: '铈（Ce）',
                value: [12.5, 22.5, 10, 3],
                app: '抛光粉、储氢材料'
            },
            { 
                name: '锗（Pr）',
                value: [30, 65, 35, 2],      // 技术成熟度（中=2）
                app: '永磁体、特种合金'
            },
            { 
                name: '钕（Nd）',
                value: [35, 85, 50, 3],
                app: '永磁体（风电、电动车）'
            },
            { 
                name: '铕（Eu）',
                value: [90, 225, 135, 1],    // 技术成熟度（低=1）
                app: '荧光粉（LED、电视）'
            },
            { 
                name: '钇（Y）',
                value: [55, 165, 110, 2],
                app: '高温合金、陶瓷材料'
            }
        ];

        // 颜色配置（扩展自用户指定色系）
        const colors = ["#dd6b66", "#759aa0", "#e69d87", "#8dc1a9", "#ea7e53", "#eedd78"];

        option = {
            backgroundColor: "rgba(51,51,51,1)",
            title: {
                text: '稀土元素回收经济性分析',
                textStyle: { color: '#fff' },
                left: 'center'
            },
            tooltip: {
                trigger: 'item',
                formatter: params => {
                    return `${params.name}<br/>
                    回收成本: ${params.value[0]}美元/千克<br/>
                    市场价格: ${params.value[1]}美元/千克<br/>
                    利润空间: ${params.value[2]}美元/千克<br/>
                    技术成熟度: ${['低','中','高'][params.value[3]-1]}<br/>
                    主要应用: ${params.data.app}`;
                }
            },
            radar: {
                indicator: [
                    { name: '回收成本', max: 100 },    // 覆盖铕的最高成本90
                    { name: '市场价格', max: 250 },     // 覆盖铕的最高价格225
                    { name: '利润空间', max: 150 },     // 覆盖铕的最高利润135
                    { name: '技术成熟度', max: 3 }       // 高=3
                ],
                axisName: { 
                    textStyle: { 
                        color: '#fff',
                        fontSize: 12
                    } 
                },
                splitArea: { show: false }  // 优化视觉层次
            },
            series: [{
                type: 'radar',
                data: data.map((item, index) => ({
                    value: item.value,
                    name: item.name,
                    itemStyle: { color: colors[index] },
                    lineStyle: { 
                        color: colors[index],
                        width: 2
                    },
                    areaStyle: { 
                        opacity: 0.15,  // 半透明填充增强可读性
                        color: colors[index]
                    }
                }))
            }],
            legend: {
                data: data.map(item => item.name),
                textStyle: { color: '#fff' },
                orient: 'vertical',
                right: 30,
                top: 60,
                itemGap: 15
            }
        };

        myChart.setOption(option);
    </script>
</body>
</html>
