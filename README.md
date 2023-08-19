# 澳洲财务数据收集  

本项目旨在收集澳洲的财务数据，并进行数据处理和验证。在此过程中，我们将使用Compustat Global的财务数据和股价数据，并将数据合并为一个统一的数据集。最终，我们将输出处理后的数据以及验证结果。

## 安装

1. 克隆项目到本地：

```shell
git clone https://github.com/djh592/AusDataProject.git
```
或使用 SSH：

```shell
git clone git@github.com:djh592/AusDataProject.git
```
2. 进入项目目录：

```shell
cd AusDataProject
```
3. 创建并激活虚拟环境：
```shell
python -m venv venv
source venv/bin/activate (Linux/Mac)
venv\Scripts\activate (Windows)
```
4. 安装依赖包：
```shell
pip install -r requirements
```

## 运行

1. 在激活的虚拟环境中，运行 Jupyter Notebook：
```shell
jupyter notebook
```

2. 打开 code.ipynb 文件。

3. 按照 Notebook 中的说明，运行代码并处理 CSV 数据。

4. 输出的 JSON 文件将保存在 `output/` 目录中。

## 项目过程

### 1、数据下载  

查询可能的数据来源：读论文、wrds、ChatGPT   

依次访问每个数据库：  
- 澳交所官网(ASX)：需要订阅费  
- Compustat Global：清华订阅，财务数据较全，时间范围1987至今  
- Worldscope：清华未订阅，很多顶刊文章使用  

ESG数据：
- Refinitiv ESG：未订阅  
- MSCI：未订阅  
- Sustainalytics：未订阅  
- ISS ESG：未订阅  
- FTSE ESG：通过wind查询  

[MSCI官网](https://www.msci.com/our-solutions/esg-investing/esg-ratings-climate-search-tool)和[新浪财经](https://finance.sina.com.cn/esg/grade.shtml)可查当前评级数据，无法下载历史数据。

目前，可用且比较全的数据来源只有Compustat Global，下载1987-6--2023-8的数据，国家：AUS。  
Fundamentals Annual包含变量：  
- International Security ID (ISIN)
- SEDOL (SEDOL)
- ISO Country Code - Incorporation (FIC)
- City (CITY)
- Company Legal Name (CONML)
- Standard Industry Classification Code (SIC)
- Data Year - Fiscal (FYEAR)
- Assets - Total (AT)
- Capital Expenditures (CAPX)
- Common/Ordinary Equity - Total (CEQ)
- Cash and Short-Term Investments (CHE)
- Debt in Current Liabilities - Total (DLC)
- Long-Term Debt - Total (DLTT)
- Earnings Before Interest (EBITDA)
- Property, Plant and Equipment - Total (Net) (PPENT)
- Interest and Related Expense - Total (XINT)
- Com Shares Outstanding - Issue (CSHOI)
- Net Income (Loss) - Consolidated (NICON)
- Treasury Stock - Number of Common Shares - Issue (TSTKNI)
- Research and Development Expense (XRD)   

可在 `data/wovz34pxq1fe4z1r.csv` 中找到该数据。

Security Monthly包含变量：
- International Security Identification Number (isin)
- Security Monthly (datadate)
- Close - Monthly (prccm)

可在 `data/ifl6dljmnzoyi6g6.csv` 中找到该数据。



### 2、处理数据  

代码见 `aus_code.ipynb`，数据文件在 `data/`

在数据处理阶段，我们会使用 `pd.merge` 函数将财务数据和价格数据进行合并。合并的规则是按照 isin 和 datadate 的年和月进行匹配，并将价格数据合并到财务数据表中。

在处理数据时，我们会保留缺失值，并将计算过程中的 `np.inf` 替换为 `np.nan`，以便于输出统计结果。

需要额外验证 TobinQ 公式中的 MKTVAL 是利用股价数据计算得出的。

最终，我们将输出处理后的数据和验证结果。
- 位置：`output/`
- 格式：JSON，按照 LiYan 学长提供的样例数据格式
- JSON 文件命名：`aus_变量名`


### 3、验证

由于澳洲历史时间跨度较大的研究文章较少，我们会参考一些时间跨度不那么长，或者是全球数据的研究文章，以验证我们收集的数据的大致范围是否正确。

## 文件结构

```plaintext
- AusDataProject/
  - data/
    - wovz34pxq1fe4z1r.csv
    - ifl6dljmnzoyi6g6.csv
  - output/
  - venv/
  - aus_code.ipynb
  - README.md
  - requirements
```
