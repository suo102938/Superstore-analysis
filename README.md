# Superstore-analysis
Superstore 折扣-利润分析 Notebook
# Superstore 折扣-利润分析

基于 Superstore 数据，围绕 **Discount（折扣）** 与 **Profit（利润）** 的关系，发现高折扣（如 45% OFF）导致部分品类/订单利润为负，并给出止损方案。

## 数据与文件
- Notebook：`Superstore-analysis-code.ipynb`
- 数据：`SampleSuperstore.csv`（与 Notebook 同级）
- 关键字段：`Order Date`, `Region`, `Category`, `Sub-Category`, `Sales`, `Quantity`, `Discount`, `Profit`

## 复现步骤
```bash
pip install pandas matplotlib seaborn
jupyter notebook Superstore-analysis-code.ipynb
```

## 分析流程
1) **预处理**
   - 读取 CSV（`windows-1252` → 失败回退 `utf-8`）
   - 列名去空格；`Order Date` 转 `datetime`（混合格式用 `dayfirst=True, errors='coerce'`）；衍生 `YearMonth`
   - 缺失：`Order Date` 有 271 行为空，可选择丢弃
2) **透视与下钻**
   - 透视：`Region × Category` 汇总 `Profit`，定位地区-品类差异
   - 子品类聚合：`Sales`、`Profit`、`Discount` 均值；算 `Profit Margin`
   - 亏损 Top：Tables（-17,725），Bookcases（-3,473），Supplies（-1,189）……
3) **折扣归因**
   - 散点：`Discount` vs `Profit`，折扣升高，利润快速转负
   - 折扣均值曲线：折扣 > 20% 后平均利润断崖下行
4) **趋势与相关性**
   - 月度销售趋势（`YearMonth` 折线）
   - 相关性热力图：`Discount` 与 `Profit` 显著负相关
5) **业务模拟（止损测算）**
   - 条件：`Discount > 0.2` 且 `Profit < 0`
   - 结果：发现 1,348 笔高折扣亏损订单；当前亏损 **$138,515.24**
   - 若回调/限制折扣至 ≤20%，可直接挽回 **$138,515.24**，整体利润提升约 **48.36%**

## 结论要点
- **折扣是亏损主因**：折扣超过 20% 时，平均利润率断崖式转负。
- **结构性亏损**：高销量子品类（如 Tables）在高折扣下整体亏损。
- **可执行收益**：收紧高折扣（>20%）的亏损订单，可直接止损约 13.9 万美元，净利提升 ~48%。

## 仓库结构
- `Superstore-analysis-code.ipynb`：核心分析 Notebook
- `SampleSuperstore.csv`：数据文件
- `README.md`：项目说明