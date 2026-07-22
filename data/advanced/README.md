# 進階題庫：完整攻擊語料（3,602 題）

給想打更多、更難題目的進階學員。`attack-corpus-full.csv` 是本課多模型 ASR 評測（6 模型 × 3,602 題）所用的**完整攻擊語料**，欄位與 `data/` 的其他 CSV 一致，notebook 第 6 節的 `load_rows` 可直接讀。

## 怎麼用

在 notebook 第 7 節開頭，把攻擊題庫換成這一份：

```python
attack_rows = load_rows("attack-corpus-full.csv")   # 3,602 題，而非內嵌的 44 題切片
```

把檔案放到 notebook 當前目錄（或 `data/`），`load_rows` 會優先讀本機檔。挑不同 `ROW_INDEX`、不同 `source_file` 的題目，看你的手法在哪類題目、哪顆模型上撐得住。第 2 節的難度階梯（`8b` → `550b`）配這份語料就是完整的挑戰場。

## 欄位

與其他切片同一個 6 欄 schema，另加一欄 `id`：

| 欄位 | 意義 |
|---|---|
| `original_prompt` | 原始請求文字 |
| `label` | 全部 `1`（本語料只收有害 / 攻擊題） |
| `split` | 統一標 `database` |
| `source_file` | 這一列來自哪個 benchmark（見下方出處） |
| `prompt_category` | 該 benchmark 自己的風險 / 主題分類（各家 taxonomy 不同，別跨檔硬併） |
| `prompt_style` | 攻擊 / 變體 / 呈現風格 |
| `id` | 內容雜湊（去重、對照用） |

## 出處與歸屬（attribution）

本語料是**七個已發表的學術安全 / 越獄 benchmark** 中 `label==1`（有害意圖）題目的重組，取樣為決定性、可重現（每組取法見下表）。各 benchmark 的原始授權條款以其原始發佈為準，使用請遵循各自 license；此處為授權資安教育用途的彙整重散布。

| `source_file` | 題數 | benchmark | 出處 |
|---|---|---|---|
| `sorry_bench_202503` | 1,260 | SORRY-Bench (2025-03) | 21 種 surface-form 風格穩健度題組（每風格取 60，seed 42） |
| `hf_jailbreak_classification` | 666 | HF Jailbreak Classification | HuggingFace 社群 jailbreak-template 辨識集 |
| `fortress_dataset_english` | 500 | FORTRESS（拼合集） | ~20 個 HF / 社群來源的注入 / 越獄拼合池，取最長 500 題 |
| `harmbench_english` | 400 | HarmBench (Mazeika et al., 2024) | 標準 behaviors 集，簡潔祈使句式有害意圖 |
| `strongreject_english` | 313 | StrongREJECT (Souly et al., 2024) | 精選「可回答」的違規請求 |
| `deepset_prompt_injection_english` | 263 | deepset prompt-injections (HF `deepset/prompt-injections`, 2023) | 經典指令注入句式 |
| `jailbreakbench_judge_comparison_dataset` | 200 | JailbreakBench (Chao et al., NeurIPS 2024) | judge / classifier 判別子集的 unsafe 題 |

## 安全須知（讀完再用）

- 這是**授權的資安教育素材**。這份語料**未做溫和化過濾**，含完整分類（涵蓋 cybercrime、malware、terrorism 等重度類別），與 `data/attack-samples.csv`（刻意停在盜版等溫和等級的 44 題切片）不同。用途限於理解對齊模型的**拒絕機制**與評估攻防，**不是**產出真正可操作的有害內容。
- 這些是**已公開的研究 benchmark**；散布請保留上表歸屬並遵循各原始 license。
- **模型的有害完成內容不在此檔**（本檔只有攻擊 prompt）。評測時模型對這些 prompt 產生的有害回覆刻意不公開。
