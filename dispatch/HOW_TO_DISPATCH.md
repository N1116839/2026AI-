# AI 派工架構 — 操作指南

## 角色分工

| 角色 | 工具 | 用途 |
|---|---|---|
| 總指揮（軍師）| Claude Code（你我） | 規劃、思考、審查、決策 |
| 士兵 | OpenCode CLI | 出工執行，寫程式/生成內容 |

## 可用士兵模型

> 免費路線：NVIDIA 開發者帳號（build.nvidia.com）申請免費 API Key，
> 在 OpenCode 接「nvidia」供應商。免費、不用綁卡；限制約 40 次/分鐘，人多會排隊。

```
nvidia/deepseek-ai/deepseek-v4-flash       ← 主力：百萬上下文，寫程式/生成文字
nvidia/moonshotai/kimi-k2.6                ← 多模態（看得懂圖片）/ 需要推理時使用
nvidia/nvidia/nemotron-3-ultra-550b-a55b   ← 大型推理備選
```

## 標準派工流程

### 1. 由 Claude Code 寫好計畫
Claude Code 會把任務規格寫進 `dispatch/task_<名稱>.md`

### 2. 你在終端機執行派工指令
```powershell
cd "G:\我的雲端硬碟\2026AI大軍"
opencode run --model nvidia/deepseek-ai/deepseek-v4-flash --dangerously-skip-permissions --dir . "請讀取 dispatch/task_<名稱>.md，完成任務後把結果摘要寫進 dispatch/result_<名稱>.md"
```

### 3. Claude Code 讀取 result.md（不讀全部對話）
只讀結果，不把 OpenCode 的整個對話塞進 context，節省 Token。

## 平行派工（7倍速）

同時開 7 個 PowerShell 視窗，各自執行不同單元的任務：

```powershell
# 視窗1
opencode run -m nvidia/deepseek-ai/deepseek-v4-flash --dangerously-skip-permissions "讀 dispatch/task_unit1.md，結果寫 dispatch/result_unit1.md"

# 視窗2
opencode run -m nvidia/deepseek-ai/deepseek-v4-flash --dangerously-skip-permissions "讀 dispatch/task_unit2.md，結果寫 dispatch/result_unit2.md"

# ...以此類推
```

## Token 節省原則

- OpenCode 執行中：Claude Code 不需要盯著看
- OpenCode 完成後：Claude Code 只讀 `result_*.md`，不讀 OpenCode 的對話歷史
- 大量輸出（寫程式）：交給 OpenCode；規劃審查：Claude Code 自己來
