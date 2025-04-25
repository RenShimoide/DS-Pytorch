DockerfileでPythonのアップデートをしたほうが良い、Jupyter Labのポートを開けているためセキュリティに気を付けたい。要勉強
compose.yml内でコンテナネーム変更、GPUのメモリ割り当て変更
大学内で利用するときは追加でプロキシを通す
rootで入っているので適宜変更もしたほうが良いかも

配置図
my-python-project/
├── .devcontainer/
│   ├── jupyter
│   │   ├── Dockerfile
│   │   └── requirements.txt          # Dockerfile と同階層
│   ├── compose.yml
│   └── devcontainer.json      
├── .vscode/
│   └── launch.json             # エディタ設定
├── src/
│   ├── MNIST.ipynb
│   ├── MNIST.py
│   └── test.py
└── README.md
