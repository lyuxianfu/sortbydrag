# SortByDrag

## example
```
<SortByDrag :data="list" :drag="drag" height="540rpx" :itemWidth="136" :itemHeight="162" @sort="handleSort">
    
    <template v-slot="{item, index}">
    
        ...你的item内容
    
    </template>
    
</SortByDrag>
```


## 参数说明

|参数| 说明      | 类型      | 默认值    |
|---|---------|---------|--------|
|drag| 是否允许拖拽  | Boolean | false  |
|data| 需要排列的列表 | Array   | [ ]    |
|itemWidth| 单项的宽    | Number  | 0      |
|itemHeight| 单项的高    | Number  | 0      |
|width| 整个区域的宽  | String  | 100vw  |
|height| 整个区域的高  | String  | 540rpx |


## 事件说明

| 事件   | 说明     | 回调            |
|------|--------|---------------|
| sort | 排序之后出发 | list(排序之后的数组) |




