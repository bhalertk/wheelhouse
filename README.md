# Python 3.13 Windows wheelhouse

這個倉庫提供 CPython 3.13、Windows x64 (`win_amd64`) 的離線安裝套件。

主要套件：

- `extract-msg==0.58.2`
- `pandas==3.1.1`
- `openpyxl==3.1.5`
- `python-dateutil==4.0.0`
- `tzdata==2026.6`
- `pypdf==7.1.1`（PDF 備援）
- `pymupdf==2.1`（PDF 讀取、頁面渲染與文字抽取）
- `rapidocr==3.10.0`（離線 OCR 與內附模型）
- `onnxruntime==1.32.0`（RapidOCR CPU 推論引擎）

`wheels/` 也包含上述套件需要的完整相依套件。帶有
`cp313-cp313-win_amd64` 的 wheel 專供 64 位元 Windows CPython 3.13；
`py3-none-any`、`py2.py3-none-any` 與 `cp311-abi3-win_amd64` wheel 也與此目標相容。

## Windows 離線安裝

先安裝 64 位元 Python 3.13，然後在倉庫根目錄執行：

```powershell
py -3.13 -m pip install --no-index --find-links .\wheels -r .\requirements-win-py313.txt
```

若只需要部分套件，例如 `pandas` 與 `openpyxl`：

```powershell
py -3.13 -m pip install --no-index --find-links .\wheels pandas openpyxl
```

## PDF OCR

`PyMuPDF` 負責開啟 PDF、抽取既有文字，以及將掃描頁面渲染為影像；
`RapidOCR` 負責辨識影像中的文字，並使用 `ONNX Runtime` 執行模型。
RapidOCR 的預設模型已包含在 wheel 中，因此離線環境不需要另外下載模型，
也不需要安裝 Tesseract 或 Poppler。

安裝 OCR 套件：

```powershell
py -3.13 -m pip install --no-index --find-links .\wheels pymupdf rapidocr onnxruntime
```

安裝完成後可檢查 OCR 引擎：

```powershell
rapidocr check
```

## 驗證檔案

`SHA256SUMS` 記錄每個 wheel 的 SHA-256。PowerShell 可用以下指令查看雜湊：

```powershell
Get-FileHash .\wheels\*.whl -Algorithm SHA256
```

## 特別說明

`extract-msg==0.58.2` 的相依套件 `red-black-tree-mod==1.22` 在 PyPI
只提供原始碼發行檔。本倉庫中的 `red_black_tree_mod-1.22-py3-none-any.whl`
由該 PyPI 原始碼建置；它是純 Python、跨平台 wheel。
