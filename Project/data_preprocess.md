# Data Preprocessing Plan

## Phase 1: 数据导入与合并 (Data Ingestion)

**目标**：将散落在 12 个 CSV 文件中的数据整合成一个 R 数据框。

1.  读取文件
    -   使用 `list.files` 获取文件夹下所有 `.csv` 路径。\
    -   使用 `purrr::map_dfr` 或 `lapply` + `bind_rows` 批量读取并合并。
2.  标准化列名
    -   确保列名统一且便于引用，可用 `janitor::clean_names()`（例如 `pm25`, `temp`, `pres`），保持关键字段一致性。

## Phase 2: 基础清洗与时间处理 (Basic Cleaning)

**目标**：处理缺失值并建立标准时间索引。

1.  时间轴构建
    -   原始数据有 `year`, `month`, `day`, `hour`，使用 `lubridate::make_datetime()` 合并为 `datetime` (POSIXct)，设定时区 Asia/Shanghai。
2.  缺失值处理
    -   对目标变量 `PM2.5` 和主要气象变量：线性插值 `zoo::na.approx`，`maxgap = 6`，超过 6 小时的连续缺口保留 NA 或按需剔除。\
    -   插值前按 `station, datetime` 排序，避免跨站点插值。对尖峰敏感的变量（如 `RAIN`、`SO2`）可考虑更保守的填补方式。

## Phase 3: 核心特征工程 (Paper-Driven Features)

**目标**：构建论文强调的“季节年”和风场特性。

1.  季节年 (Seasonal Year)
    -   `season`: 3–5 春，6–8 夏，9–11 秋，12–2 冬。\
    -   `season_year`: 如果月份为 1 或 2，年份减 1；否则保持不变。
2.  风向简化
    -   将 16 方位归类为 5 类：NW (N, NNW, NW, WNW)、NE (NE, NNE, ENE, E)、SE (SE, SSE, ESE, S)、SW (SW, SSW, WSW, W)、CV (静风/不定风/其他)。保留原始 `wd` 以备回溯。
3.  累积风速 (CWS)
    -   按简化风向的连续段累加 `WSPM`，风向改变即重置。`WSPM` 缺失时用 0 或跳过以避免中断。可选地再计算短窗版本（如过去 6 小时累积）。
4.  累积降水 (可选)
    -   类似逻辑，可用整段或滚动窗口（如 24 小时和），避免跨季节累积失真。

## Phase 4: 空间与健康特征 (Spatial & Health Features)

**目标**：为区域对比和健康等级分析做准备。

1.  空间分组
    -   创建 `region`：Suburban (North) = Dingling, Huairou, Changping, Shunyi；Urban (Central) = 其余站点。若有坐标，可基于距离或官方划分调整。
2.  健康等级分箱
    -   基于 PM2.5 使用国标或 WHO 阈值，用 `cut` 生成有序因子 `aqi_level`（Good, Moderate, Unhealthy 等），明确区间闭合方式。

## Phase 5: 最终检查与导出 (Validation & Export)

1.  完整性检查
    -   确认无重复 `station + datetime`；使用 `summary()`/`skimr::skim()` 检查异常值和剩余 NA；验证 `season_year` 范围（2013–2016，2017 前两个月并入 2016 季节年）。
2.  导出
    -   保存清洗加工后的数据为 `beijing_air_processed.csv`，并记录处理元数据（插值参数、日期范围、特征说明）。
