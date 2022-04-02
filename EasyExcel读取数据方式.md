>读取`xlsx`文件方式`EasyExcel`

1. 直接读取，创建sheet页，从文件中直接读取数据并写入
2. 创建一个继承`AnalysisEventListener` 的监听类，在获取到数据时对数据解析进行业务处理。

>直接读取
1. 实体类

   `@ExcelProperty`注解有`index` 、`value`和`converter`三种属性，`index`对应读取文件中的列数，`value`对应列标题的名称。`converter`指明该字段读写按照对应的`converter`规则处理。

```java
@Data
@TableName("tb_covered")
@EqualsAndHashCode(callSuper = true)
@ApiModel(value = "室外覆盖结果表")
public class Covered extends Model<Covered> {
private static final long serialVersionUID = 1L;

    /**
     * 主键
     */
    @TableId(type = IdType.AUTO)
    @ApiModelProperty(value="主键")
    private Integer id;
    /**
     * 序号
     */
    @ApiModelProperty(value="序号")
    @ExcelProperty("序号")
    private Integer num;
    /**
     * 省份
     */
    @ApiModelProperty(value="省份")
    @ExcelProperty("省份")
    private String province;
    /**
     * 地市
     */
    @ApiModelProperty(value="城市")
    @ExcelProperty("城市")
    private String city;
    /**
     * 区域
     */
    @ApiModelProperty(value="区域")
    @ExcelProperty("区域")
    private String region;
    /**
     * 行政区
     */
    @ApiModelProperty(value="行政区")
    @ExcelProperty("行政区")
    private String district;
    /**
     * 场景
     */
    @ApiModelProperty(value="场景")
    @ExcelProperty("场景")
    private String scene;
    /**
     * 问题说明
     */
    @ApiModelProperty(value="问题说明")
    @ExcelProperty("问题说明")
    private String question;
    /**
     * 距离
     */
    @ApiModelProperty(value="弱覆盖里程（km)")
    @ExcelProperty(value = "弱覆盖里程（km)", converter = CustomDisConverter.class)
    private Double distance = 0d;
    /**
     * 电平
     */
    @ApiModelProperty(value="仿真电平")
    @ExcelProperty("仿真电平")
    private String level;
    /**
     * 原因
     */
    @ApiModelProperty(value="原因")
    @ExcelProperty("原因")
    private String reason;
    /**
     * 建议
     */
    @ApiModelProperty(value="建议")
    @ExcelProperty("建议")
    private String suggest;
    /**
     * 经度
     */
    @ApiModelProperty(value="经度")
    @ExcelProperty("中心经度")
    private Double lng;
    /**
     * 纬度
     */
    @ApiModelProperty(value="纬度")
    @ExcelProperty("中心纬度")
    private Double lat;
    }

```

2. 读取

```java
 List<Covered> list = EasyExcel.read(file.getInputStream()).head(Covered.class).sheet().doReadSync();
```

`list`中保存了从文件中读取的数据量，对`list`进行对应的业务处理即可。



>`listener`监听读取

1. `listener`监听类

```java
@Slf4j
@Component
public class CoveredExcelListener extends AnalysisEventListener<Covered> {

    @Resource
    private CoveredService coveredService;

    /**
     * 批处理阈值
     */
    private static final int BATCH_COUNT = 10;
    List<Covered> list = new ArrayList<>(BATCH_COUNT);

    @Override
    public void invoke(Covered covered, AnalysisContext analysisContext) {
        log.info("解析到一条数据:{}", JSON.toJSONString(covered));
        // 判断数据库是否存在该数据
        this.truncate(covered);
        if (list.size() >= BATCH_COUNT) {
            log.info("{}条数据，开始存储数据库！", list.size());
            coveredService.saveBatch(list);
            log.info("存储数据库成功，当前缓存数据：{}条", list.size());
            list.clear();
        }
    }

    private void truncate(Covered covered) {
        LambdaQueryWrapper<Covered> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(Covered::getCity, covered.getCity())
                .eq(Covered::getDistance, covered.getDistance())
                .eq(Covered::getDistrict, covered.getDistrict())
                .eq(Covered::getLevel, covered.getLevel())
                .eq(Covered::getLat, covered.getLat())
                .eq(Covered::getLng, covered.getLng())
                .eq(Covered::getNum, covered.getNum())
                .eq(Covered::getProvince, covered.getProvince())
                .eq(Covered::getQuestion, covered.getQuestion())
                .eq(Covered::getReason, covered.getReason())
                .eq(Covered::getRegion, covered.getRegion())
                .eq(Covered::getScene, covered.getScene())
                .eq(Covered::getSuggest, covered.getSuggest());
        List<Object> coveredList = coveredService.listObjs(queryWrapper);
        if (coveredList.size() > 1) {
            log.info("该数据{}已存在，无需重复插入！",coveredList);
            return;
        }
        list.add(covered);
    }

    @Override
    public void doAfterAllAnalysed(AnalysisContext analysisContext) {
        log.info("所有数据解析完成！");
    }

}
```

2. 读取

```java
 EasyExcel.read(file.getInputStream(), coveredExcelListener).head(Covered.class).sheet().doReadSync();
```

将读取业务逻辑写入`listener`中，对数据进行对应处理
