# **Foundational Generative AI**
--([learning page](https://learn.ineuron.ai/course/foundational-generative-ai/656d8f170af8644aac926376))--

```diff
@@ End-to-end-Medical-Chatbot-using-Llama2 @@
```

## Lecture 12



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

### 以下步驟都在 VS Code 環境中執行
> 因為在 本機 Windows 7 環境下，`CTransformers`無法成功呼叫，所以實際上是在 [github.dev](https://github.dev/henrykohl/End-to-end-Medical-Chatbot-using-Llama2) 下實做。

* 開啟 terminal 後，在 *Git bash* 建立 virtual environment，並啟動
> virtualenv 方式
> >```bash
> >virtualenv mchatbot --python=python3.8
> >source ./mchatbot/Scripts/activate
> >```
> conda 方式 (-y表示遵循默認配置)
> >```bash
> >conda create -n mchatbot python=3.8 -y
> >conda activate mchatbot
> >```

* 建立 `requirements.txt`,並安裝(要花一些時間)
```text
ctransformers==0.2.5
sentence-transformers==2.2.2
pinecone-client
langchain==0.0.225
flask
pypdf
python-dotenv
```

> 還需加入 (Lecture並不包含) `ipywidgets` 與 `langchain_community` 等兩個包，加入`langchain_community`後，(1) `langchain==0.0.225`不能列在`requirements.txt`中，需要移除，(2)或是改成`langchain`，(3)又或是`langchain`也可以不需要列在 `requirements.txt`
>
> ```diff
> + 注意`langchain==0.0.225`與`langchain_community`不能放在同一個**requirements.txt**，也就是安裝`langchain_community`，
> + 就不需要安裝`langchain==0.0.225`，否則在執行`Pinecone.from_texts`或是`RetrievalQA.from_chain_type`時，會出現錯誤
> ```
> > 也就是不能這樣執行 packages 安裝
> > ```bash
> > pip install langchain==0.0.225 langchain_community
> > ```
> > 但可以如此執行 packages 安裝
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

* 建立 `trails.ipynb`，選擇 mchatbot 環境下的 python kernel

* 建立 `/data`，將`Medical_book.pdf`至於此資料夾
> *commit* "data added"

* 在 Pinecone 建立 index
> Name: `medical-chatbot`
>
> Dimension: `384` (根據 [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) 模型)

* 下載 [llama-2-7b-chat.ggmlv3.q4_0.bin](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/resolve/main/llama-2-7b-chat.ggmlv3.q4_0.bin?download=true) 模型 (3.79GB)
> 第一種方式：從網頁上直接下載
> 
> 第二種方式：用指令下載
> ```diff
> - wget https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/resolve/main/llama-2-7b-chat.ggmlv3.q4_0.bin --quiet
> ```
> > 當 llama-2-7b-chat.ggmlv3.q4_0.bin 下載完成後，放至於 /model 中
>
> 在 `.gitignore` 中新增 model/llama-2-7b-chat.ggmlv3.q4_0.bin
>
> *commit* "data loader added"

* 資料夾`.vscode`中settings.json用來解決Visual Studio Code Pylance (reportMissingImports)的問題</font>

---

## Lecture 13

* 建立 `template.py`，且執行 `python template.py` 以建立必要的 資料夾 和 檔案
> 在 `template.py` 中，不要建立空資料夾，所以資料夾 `static` 要改成 `static/.gitkeep` (比要好)

* 把`trails.ipynb`移至`/research`下 (`/research`中原本空的`trails.ipynb`要被取代掉)
> ```diff
> ! 注意，要執行 trails.ipynb 時，因為檔案路徑關係，不要在 /research 之下直接執行，要將 trails.ipynb 複製到`/research`的上一層目錄才行
> ```

* *commit* "folder structure added"

* 完成`setup.py`，在`requirements.txt`中加入`-e .`
> 若沒有`setup.py`，安裝`requirements.txt`時，執行到`-e`會產生錯誤

* 執行 `pip install -r requirements.txt`再一次
> `/Medical_Chatbot.egg-info`會被自動建立

* *commit* "setup file added"

* 完成 `.env` (寫入 `PINECONE_API_KEY = '...'`)
> ```diff
> - 當執行 clone 新的 repository 時，也要建立一個新的 `.env`
> ```

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

* **注意** 若 clone 此 repository後（在本機 或 在雲端--github.dev , aws  ），有兩個檔 (repository並沒有包含) 要加上 (對應前面兩項紅字說明)
> 下載 `llama-2-7b-chat.ggmlv3.q4_0.bin` 
> 
> 建立 `.env`


---

## 補充： 在 AWS 上的步驟

```bash
>sudo apt update

>sudo apt-get update

>sudo apt upgrade -y

>sudo apt install git curl unzip tar make sudo vim wget -y
```
由於建立的 EC2 使用 micro 等級，只有 1GB 的 memory ，在 install package 時，很容易造成 low memory的 issue ，進而整個 EC2 instance 都當住，解決方式參考 [Running out of RAM while doing pip install on AWS EC2](https://stackoverflow.com/questions/78133882/running-out-of-ram-while-doing-pip-install-on-aws-ec2)

```bash
>sudo fallocate -l 1G ~/swapfile
>sudo dd if=/dev/zero of=~/swapfile bs=1024 count=1048576
>sudo chmod 600 ~/swapfile
>sudo mkswap ~/swapfile
>sudo swapon ~/swapfile
```
clone 在 Github 的 repository

```bash
>git clone "Your-repository"
```

EC2 已安裝 python 3.10，但之後要在建立的 virtual environment中安裝 python 3.8，所以需先安裝 python 3.8 ，參考[How to Install Python 3.8 on Ubuntu 22.04](https://linuxgenie.net/how-to-install-python-3-8-on-ubuntu-22-04/)
```bash
>sudo add-apt-repository ppa:deadsnakes/ppa

>sudo apt install python3.8 -y

>python3.8 --version # check the version information
```

[create a venv having python3.8 as python3 On Ubuntu](https://stackoverflow.com/questions/62773433/python3-8-venv-not-working-with-python3-8-m-venv-env)
```bash
>cd End-to-end-Medical-Chatbot-using-Llama2/
>sudo apt install python3.8 python3.8-venv python3-venv
>python3.8 -m venv mchatbot
>source mchatbot/bin/activate
>python3 --version # check the version information in the virtual environment
```

在資料夾 /model 中，下載 ggml
```diff
cd  model
- wget https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/resolve/main/llama-2-7b-chat.ggmlv3.q4_0.bin --quiet
```

建立 `.env` (寫入 `PINECONE_API_KEY = '...'`)
```diff
- vi .env
```

執行
```bash
>pip install sentence-transformers==2.2.2
>pip install - r requirements.txt
```
**注意**，在AWS環境中 `requirements.txt` 不包含 `sentence-transformers==2.2.2` 




