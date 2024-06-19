# End-to-end-Medical-Chatbot-using-Llama2

<font color="red">Lecture 12</font>

> 在 GitHub 建立 repository ，預先新增三個檔案
> > `.gitignore` 選擇 Python
> >
> > `LICENSE` 選擇 MIT
> >
> > `README.md`

* 在 local 執行
>```bash
>E:\gitstore>git clone https://github.com/henrykohl/End-to-end-Medical-Chatbot-using-Llama2.git
>
>E:\gitstore>cd End-to-end-Medical-Chatbot-using-Llama2
>
>E:\gitstore\End-to-end-Medical-Chatbot-using-Llama2>code .
>```

## 以下步驟都在 VS Code 執行
> 因為在 本機 Windows 7 環境下，`CTransformers`無法成功呼叫，所以實際上是在 [github.dev](https://github.dev/henrykohl/End-to-end-Medical-Chatbot-using-Llama2) 下實做。

* 開啟 terminal 後，在 *Git bash* 建立 virtual environment，並啟動
> virtualenv 方式
> >`virtualenv mchatbot --python=python3.8`
>
> >`source ./mchatbot/Scripts/activate`
>
> conda 方式 (-y表示遵循默認配置)
> >`conda create -n mchatbot python=3.8 -y`
>
> >`conda activate mchatbot`

* 建立 `requirements.txt`,並安裝(要花一些時間)
> `ipywidgets` 與 `langchain_community`是自行加入，Lecture並不包含此倆，加入`langchain_community`後，`langchain==0.0.225`不能列在`requirements.txt`中，需要移除，或是改成`langchain`，`langchain`也不是需要的，可以不需要列在`requirements.txt`
>
> <font color="red">注意`langchain==0.0.225`與`langchain_community`不能放在同一個**requirements.txt**，事實上安裝`langchain_community`就不需要安裝`langchain==0.0.225`，否則在執行`Pinecone.from_texts`或是`RetrievalQA.from_chain_type`時，會出現錯誤</font>
> > 也就是不能執行
> > ```bash
> > pip install langchain==0.0.225 langchain_community
> > ```
> > 但可以執行 
> > ```bash
> > pip install langchain==0.0.225
> > pip install langchain_community
> > ```
>
> *commit* "requirements added"
>
> `pip install -r requirements.txt`
>
> `pip install langchain-huggingface` (langchain-huggingface若至於`requirements.txt`，執行`pip install`時，會出現 dependency ERROR)
> > ERROR : `langchain-huggingface` 需要 `sentence-transformers>=2.6.0`

* 建立 `model/instruction.txt`
> *commit* "model instruction added"
> 下載 [llama-2-7b-chat.ggmlv3.q4_0.bin](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/resolve/main/llama-2-7b-chat.ggmlv3.q4_0.bin?download=true) 模型 (3.79GB)

* 建立 `trails.ipynb`，選擇 mchatbot 環境下的 python kernel

* 建立 `/data`，將`Medical_book.pdf`至於此資料夾
> *commit* "data added"

* 在 Pinecone 建立 index
> Name: `medical-chatbot`
>
> Dimension: `384` (根據 [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) 模型)

* 用 `wget https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/resolve/main/llama-2-7b-chat.ggmlv3.q4_0.bin --quiet` 下載
> 當 llama-2-7b-chat.ggmlv3.q4_0.bin 下載完成後，放至於 /model 中
>
> 在 `.gitignore` 中新增 model/llama-2-7b-chat.ggmlv3.q4_0.bin
>
> *commit* "data loader added"

* <font color="red">資料夾`.vscode`中settings.json用來解決Visual Studio Code Pylance (reportMissingImports)的問題</font>

---
<font color="red">Lecture 13</font>

* 建立 `template.py`，且執行

* 把`trails.ipynb`移至`/research`下 (`/research`中原本空的`trails.ipynb`要被取代掉)

* *commit* "folder structure added"

* 完成`setup.py`，在`requirements.txt`中加入`-e .`
> 若沒有`setup.py`，安裝`requirements.txt`時，執行到`-e`會產生錯誤

* 執行 `pip install -r requirements.txt`再一次
> `/Medical_Chatbot.egg-info`會被自動建立

* *commit* "setup file added"

* 完成 `.env` (寫入 `PINECONE_API_KEY = '...'`)

* 完成 `helper.py`
> 完成後，在Pinecone DB的 Indexes 中，建立 `medical-bot` (<font color="red">Lecture 11</font>中用`trails.ipynb`建立的 Index 是 `medical-chatbot`)，

* 完成 `store_index.py`
> 執行 `python store_index.py` 測試，且建立 Pinecone DB 的 `medical-bot` Index

* *commit* "store index added"

* 完成 `src/prompt.py`

* 開始編譯 `app.py`

* 開始編譯 `templates/chat.html`
> 先參考 [How To Create a Coming Soon Page](https://www.w3schools.com/howto/howto_css_coming_soon.asp)

>```html
><div class="bgimg">
>  <div class="topleft">
>    <p>Logo</p>
>  </div>
>  <div class="middle">
>    <h1>COMING SOON</h1>
>    <hr>
>    <p>35 days</p>
>  </div>
>  <div class="bottomleft">
>    <p>Some text</p>
>  </div>
></div>
>```
> 正式修改後，其中`<img src="https://www.prdistribution.com/spirit/uploads/pressreleases/2019/newsreleases/d83341deb75c4c4f6b113f27b1e42cd8-chatbot-florence-already-helps-thousands-of-patients-to-remember-their-medication.png" class="rounded-circle user_img">`可以被取代為想要的 Image/Symbol 地址

* 將 `static/.gitkeep` 改名成 `static/style.css` 並開始編譯

* *commit* "templates added"

* 完成`app.py`後，執行 `python app.py`。可以測試三個問題
> What is Acne?
>
> hello
>
> """tell me about Acetaminophen?"""


* *commit* "completed"



